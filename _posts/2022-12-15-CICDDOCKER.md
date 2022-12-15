---
layout: post
title: CI/CD Docker
category: Programming
tag: Programming
---

Git CI/CD is baed on Docker image.

1. Firstly, Create Docker Image(OS, Library, Dependency, etc..)

2. with that Docker Image, start to CI/CD Process

3. create gitcicd.yaml and write run-process in shell script

(example)

```

# You can override the included template(s) by including variable overrides
# SAST customization: https://docs.gitlab.com/ee/user/application_security/sast/#customizing-the-sast-settings
# Secret Detection customization: https://docs.gitlab.com/ee/user/application_security/secret_detection/#customizing-settings
# Note that environment variables can be set in several places
# See https://docs.gitlab.com/ee/ci/variables/#priority-of-environment-variables
stages:
- build
- release

default:
  interruptible: true
  timeout: 10m
".ros_build_and_package_template":
  except:
    refs:
      - tags
  before_script:
    - VERSION=$(cat VERSION)
    - source /ros_entrypoint.sh
    - mkdir -p rosws/src
    - ln -s "$(pwd)" rosws/src/ros_package
    - cd rosws
    - '[ ! -f "devel/.rosinstall" ] && rosws init devel /opt/ros/$ROS_DISTRO'
    - cd -
    - curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
    - apt update && DEBIAN_FRONTEND=noninteractive apt install -y libbsd-dev
    - apt update
    - apt install -y git
    - git clone https://github.com/IntelRealSense/librealsense.git
    - cd librealsense
    - mkdir build && cd build
    - cmake ../ -DCMAKE_BUILD_TYPE=Release -DBUILD_EXAMPLES=true -DBUILD_WITH_TM2=true
    - make -j4 && make install
    # - apt-get install git git-doc gitweb git-gui gitk git-email git-svn
    # - apt-get -y install git libssl-dev libusb-1.0-0-dev pkg-config libgtk-3-dev
    # - apt-key adv --keyserver keyserver.ubuntu.com --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE || apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE
    # - apt-get install software-properties-common -y
    # - apt-get update
    # - add-apt-repository "deb https://librealsense.intel.com/Debian/apt-repo $(lsb_release -cs) main" -u
    # - apt-get install librealsense2-dkms -y
    # - apt-get install librealsense2-utils -y
    - apt-get install ros-$ROS_DISTRO-ddynamic-reconfigure -y
    # - apt-get --yes update && apt --yes upgrade
    # - modinfo uvcvideo | grep "version:"
    - cd ../..
  script:
    - cd rosws
    - '[ -d "./install" ] && rm -rf ./install'
    - catkin_make clean
    - catkin_make -DCATKIN_ENABLE_TESTING=False -DCMAKE_BUILD_TYPE=Release
    - catkin_make install
    - cd -
    - mkdir -p output
    - cd rosws/install
    - tar cvfz "../../output/$CI_PROJECT_NAME-$ROS_DISTRO.tar.gz" --exclude=local_setup.sh
      --exclude=local_setup.bash --exclude=local_setup.zsh --exclude=setup.sh --exclude=setup.bash
      --exclude=setup.zsh --exclude=env.sh --exclude=_setup_util.py --exclude=lib/pkgconfig
      --exclude=*/cmake *
    - cd -
    # - cd rootfs/
    # - tar cvfz ../output/$CI_PROJECT_NAME-rootfs.tar.gz *
    # - cd -
    - cd output/
    - tar cvfz "../$CI_PROJECT_NAME-$ROS_DISTRO-$VERSION.tar.gz" *
    - cd -

build_noetic:
  tags:
    - ros-noetic
  stage: build
  extends: ".ros_build_and_package_template"
  artifacts:
    paths:
      - "$CI_PROJECT_NAME-*.tar.gz"

release:
  stage: release
  tags:
    - smbclient
  needs:
    - job: build_noetic
      artifacts: true

  rules:
     # rules의 내용은 Array로 기입되어야 하며, 각각의 조건 중 하나라도 충족시 규칙이 True가 되어 작업(job)이 실행 됨
    # 아래의 조건은 특정 branch("release")에 VERSION 파일이 변경되었을 경우 또는 DO_RELEASE 변수 입력시에 만 'release' job 진행 함
    - if: '$CI_COMMIT_BRANCH == "master"'
      changes:
        - VERSION
      when: always
    - if: $DO_RELEASE
      when: always
    - when: never

  script:
    - VERSION=$(cat VERSION)
    - newline=$'\n'
    - RELEASE_NOTE="# ${VERSION}${newline}### Downloads${newline}"
    - |
      for file in $(ls $CI_PROJECT_NAME-*.tar.gz); do
        #if [ "$LOCATION" == "seoul" ]; then
        #  echo "$LOCATION"
          smbclient -U"pkgfile%ros" //xx.xx.xx.xxx/public/ -c "cd project/seoul/Packages; mkdir ${CI_PROJECT_NAME}; cd ${CI_PROJECT_NAME}; mkdir ${VERSION}; cd ${VERSION}; put ${file}"
          file_url=http://xx.xx.xx.xxx/project/seoul/Packages/${CI_PROJECT_NAME}/${VERSION}/${file}
          [ -n $file_url ] &&  RELEASE_NOTE="${RELEASE_NOTE}- [${file}](${file_url})${newline}"
        #elif [ "$LOCATION" == "busan" ]; then
        #  echo "$LOCATION"
          smbclient -U"pkgfile%rit" //xx.xx.xx.xxx/public/ -c "cd project/busan/Packages; mkdir ${CI_PROJECT_NAME}; cd ${CI_PROJECT_NAME}; mkdir ${VERSION}; cd ${VERSION}; put ${file}"
          file_url=http://xx.xx.xx.xxx/project/seoul/Packages/${CI_PROJECT_NAME}/${VERSION}/${file}
          [ -n $file_url ] &&  RELEASE_NOTE="${RELEASE_NOTE}- [${file}](${file_url})${newline}"
        #elif [ "$LOCATION" == "sokcho" ]; then
        #  echo "$LOCATION"
          smbclient -U"pkgfile%rit" //xx.xx.xx.xxx/public/ -c "cd project/sokcho/Packages; mkdir ${CI_PROJECT_NAME}; cd ${CI_PROJECT_NAME}; mkdir ${VERSION}; cd ${VERSION}; put ${file}"
          file_url=http://xx.xx.xx.xxx/project/sokcho/Packages/${CI_PROJECT_NAME}/${VERSION}/${file}
        [ -n $file_url ] &&  RELEASE_NOTE="${RELEASE_NOTE}- [${file}](${file_url})${newline}"
      done
    - >
      TAG_EXIST=$(curl -s --header "PRIVATE-TOKEN:${GITLAB_ACCESS_TOKEN}" "${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/repository/tags/v${VERSION}" | jq 'has("name")')
    - >
      if [ "$TAG_EXIST" == "true" ]; then
        curl --request DELETE --header "PRIVATE-TOKEN:${GITLAB_ACCESS_TOKEN}" "${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/protected_tags/v*"
        curl -s --request DELETE --header "PRIVATE-TOKEN:${GITLAB_ACCESS_TOKEN}" "${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/repository/tags/v${VERSION}"
        curl --request POST --header "PRIVATE-TOKEN:${GITLAB_ACCESS_TOKEN}" "${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/protected_tags?name=v*&create_access_level=40"
      fi
    - 'curl -s --request POST --header "PRIVATE-TOKEN:${GITLAB_ACCESS_TOKEN}" "${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/repository/tags?tag_name=v${VERSION}&ref=${CI_COMMIT_BRANCH}"'
    - >
      RN_EXIST=$(curl -s --header "PRIVATE-TOKEN:${GITLAB_ACCESS_TOKEN}" "${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/repository/tags/v${VERSION}" | jq -r '.["release"] | has("tag_name")')
    - '[ "$RN_EXIST" == "true" ] && curl -s --request DELETE --header "PRIVATE-TOKEN:${GITLAB_ACCESS_TOKEN}" "${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/releases/v${VERSION}"'
    - 'curl -s --request POST --header "PRIVATE-TOKEN:${GITLAB_ACCESS_TOKEN}" --data "description=${RELEASE_NOTE}" ${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/repository/tags/v${VERSION}/release'
```


## manage opensource as private in CI/CD

1. need to know where package locate in desire computer that we want to cover our private managing package’s “.so” and “include(header)” file over already existed files in computer.
2. make a folder refer to the path that we want to cover up.
3. use tar xf -C to your “.so” “ include(header file) pasted to the path.
4. with that, we can update our own without using apt update(get flexiibility to our project)

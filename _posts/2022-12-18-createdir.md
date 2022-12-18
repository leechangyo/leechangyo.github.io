---
layout: post
title: Create File or Dir in C++
category: C++
tag: C++
---

```c++
// create image save folder
            std::string image_save_folder_path = m_data_collect_folder_path + "/image";
            struct stat st = {0};
            if(stat(image_save_folder_path.c_str(), &st) == -1)
            {
                mkdir(image_save_folder_path.c_str(), 0777); // convert a C++ string to a C string
                std::cout << "directory does not exist, Create Directory" << std::endl;
            }
            /*  */
```

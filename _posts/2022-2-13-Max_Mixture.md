---
layout: post
title: Max Mixture와 Switchable constraints
category: SLAM
tag: SLAM
---

SLAM에서 최적화를 할때 Log LikelyHood를 통한 error Function(Object Function)을 만들고 미지수에 대해 최적화를 하여 값을 구한다.

각 노드간에 연결되어 있는 constraints에 대하여 Gaussian Distribution으로 나타내게 되면 unimodel형식이며

<a href="https://postimg.cc/fJBMMPgs"><img src="https://i.postimg.cc/XY0qkSfr/maxresdefault.jpg" width="700px" title="source: imgur.com" /><a>

이에 대한 각 노드간에 연결되어 있는 constraints값을 통해 error function을 정의하여서 Gaussian Newton이나 LM 방법을 사용을 하여서 Non Linear System에 대한 Optimization을 한다.

<a href="https://postimg.cc/KRqQBKbM"><img src="https://i.postimg.cc/zG5MmWP0/Screen-Shot-2022-02-12-at-6-50-12-PM.png" width="700px" title="source: imgur.com" /><a>

허나, GPS의 튀는 값으로 잘못된 edge가 연결이 되거나 Descriptor Matching을 통해 잘못 loop closure가 일어날 경우, 맵이 쉽게 일그러진다.

<a href="https://postimg.cc/478LCSJf"><img src="https://i.postimg.cc/rsTvrB3r/1-j-ZA9f5l1-Ra-EKv-Po-FMz5-BQ.png" width="700px" title="source: imgur.com" /><a>

즉, node간에 잘못된 constraints가 생기게 된다면, 위와 같이 multi model gaussian distributions 형상이 나오는데, 이에 대한 Object Function에 대해 Optimization를 하게 된다면, Error가 점점 커지기 때문이다.

이에 생겨난게 **Max Mixture** 이다.

### Max Mixture

Max Mixture는 이 두 multi model을 가지고 있는 Object function중에 Maximum LikelyHood를 가지고 있는 Constraints과 edge만 가지고 Least Square, Optimization을 하겠다라는 의미이다.

<a href="https://postimg.cc/RNwYhGdR"><img src="https://i.postimg.cc/QxzZ6yWR/Screen-Shot-2022-02-12-at-9-19-55-PM.png" width="700px" title="source: imgur.com" /><a>

#### Multi Model를 구하고 Object Function을 만드는 방법은 다음과 같다.

먼저 Multi model을 구하는 것은 아래와 같다.

<a href="https://postimg.cc/m14HwQzQ"><img src="https://i.postimg.cc/L5Y3mDm0/Screen-Shot-2022-02-12-at-9-24-12-PM.png" width="700px" title="source: imgur.com" /><a>

Multi Model을 만들고 error function을 만들어 Optimization을 해야하기에 log likleyhood를 사용하지만, Sum은 Log LikelyHood에 들어갈 수없는데, 그 이유는 값이 Negative(-) 값이 나오기 때문이다.

<a href="https://postimg.cc/RNG5R468"><img src="https://i.postimg.cc/JzCrRh11/Screen-Shot-2022-02-12-at-9-24-20-PM.png" width="700px" title="source: imgur.com" /><a>

이에 Sum of Gaussian Distribution이 아닌, Maximum of Gaussian을 하여서 Multi Model을 만든다.

<a href="https://postimg.cc/Mvy6yCqf"><img src="https://i.postimg.cc/SNPnp4Pd/Screen-Shot-2022-02-12-at-9-25-26-PM.png" width="700px" title="source: imgur.com" /><a>


<a href="https://postimg.cc/BXB6Wjtn"><img src="https://i.postimg.cc/xTWbzH2M/Screen-Shot-2022-02-12-at-9-25-47-PM.png" width="700px" title="source: imgur.com" /><a>


### Max Mixture Robust Pose Graph

이렇게 multi model을 구할 수 있지만, Max Mixture Robust Pose Graph에서는 전체 노드와 constraints중 Maximum LikelyHood를 가지고 있는 노드와 constraints을 가지고 object function을 써서 Optimization을 하는 것이다. Iteration을 할때 Maximum LikelyHood를 가지는 node들과 constraints의 object function으로 최적화를 한다.

코드는 아래와 같다.

```c++
void EdgeSE2Mixture::UpdateBelief(int i)
{
  //required for multimodal max-mixtures, for inlier/outliers the vertices would not change
  this->setVertex(0,allEdges[i]->vertex(0));
  this->setVertex(1,allEdges[i]->vertex(1));           

  double p[3];
  allEdges[i]->getMeasurementData(p);
  this->setMeasurement(g2o::SE2(p[0],p[1],p[2]));
  this->setInformation(allEdges[i]->information());  
}

//get the edge with max probability
void EdgeSE2Mixture::computeError()
{
  int best = -1;
  double minError = numeric_limits<double>::max();
  for(unsigned int i=0;i<numberComponents;i++){    
    double thisNegLogProb = getNegLogProb(i);
    if(minError>thisNegLogProb){
      best = i;
      minError = thisNegLogProb;
    }
  }

  bestComponent = best;
  UpdateBelief(bestComponent);
  //cerr << "\nBest component is "<<bestComponent<<"\n";

  //this will be used by the optimizer
  g2o::EdgeSE2::computeError();
}

double EdgeSE2Mixture::getNegLogProb(unsigned int c)
{  
  allEdges[c]->computeError();
  //cerr << "\nchi2 for " <<c<<" "<< allEdges[c]->chi2();
  return -log(weights[c]) +
	  0.5*log(determinants[c]) +
	  0.5*allEdges[c]->chi2();
}
```

<a href="https://postimg.cc/RNyCk2zX"><img src="https://i.postimg.cc/CxLZqVrT/Screen-Shot-2022-02-12-at-9-25-58-PM.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/jLF6Gq5m"><img src="https://i.postimg.cc/XYV2Bp0j/Screen-Shot-2022-02-12-at-9-26-02-PM.png" width="700px" title="source: imgur.com" /><a>

pick one well made unimodel Gaussian model, with that use it as object function(cost function) to do optimization iteratively.(Iteratively pick best uni-model Gaussian model)

#### Switchable constraints

Switchable constraints로 마찬가지다, node간에 잘못된 constraints을 무시를하여서 Optimization을 하는데 목적이다.

### Reference

[Least Square Deep Explain](http://jinyongjeong.github.io/2017/02/26/lec12_Least_square/)

[Robust Pose Graph](http://ais.informatik.uni-freiburg.de/teaching/ws12/mapping/pdf/slam18-robust.pdf)

[MAX-MIXTURE CODE](https://github.com/OpenSLAM-org/openslam_maxmixture)

[Switchable Constraint CODE](https://openslam-org.github.io/vertigo.html#:~:text=Vertigo%20is%20a%20C%2B%2B%20extension,Authors)

[OPENSLAM CODE](https://openslam-org.github.io/)

---
layout: post
title: Difference between Variance and Standard Deviation
category: calculus
tag: calculus
---


### BASIC

Standard deviation and variance are basic mathematical concepts that play important roles throughout the financial sector, including the areas of accounting, economics, and investing. In the latter, for example, a firm grasp of the calculation and interpretation of these two measurements is crucial to the creation of an effective trading strategy.

Standard deviation and variance are both determined by using the mean of a group of numbers in question. The mean is the average of a group of numbers, and the variance measures the average degree to which each number is different from the mean.

The extent of the variance correlates to the size of the overall range of numbers
  — meaning the variance is greater when there is a wider range of numbers in the group, and the variance is less when there is a narrower range of numbers.

-- Standard deviation looks at how spread out a group of numbers is from the mean, by looking at the **square root of the variance.**
-- The variance measures the average degree to which each point differs from the mean—the average of all data points.
-- The two concepts are useful and significant for traders, who use them to measure market volatility.


### STANDARD DEVIATION DEFINITION

Standard deviation is a statistic that looks at **how far from the mean a group of numbers** is, by using the square root of the variance.

The calculation of variance uses squares **because it weighs outliers more heavily than data closer to the mean.** This calculation also prevents differences above the mean from canceling out those below, which would result in a variance of zero.

Standard deviation is calculated as the square root of variance by figuring out the variation between each data point relative to the mean.

If the points are further from the mean, there is a higher deviation within the date; if they are closer to the mean, there is a lower deviation. So the more spread out the group of numbers are, the higher the standard deviation.


### Variance DEFINITION

The variance is the average of the squared differences from the mean. To figure out the variance, first calculate the difference between each point and the mean; then, square and average the results.

For example, if a group of numbers ranges from 1 to 10, it will have a mean of 5.5. If you square the differences between each number and the mean, and then find their sum, the result is 82.5﻿. To figure out the variance, divide the sum, 82.5, by N-1, which is the sample size (in this case 10) minus 1. The result is a variance of 82.5/9 = 9.17. Standard deviation is the square root of the variance so that the standard deviation would be about 3.03.

Because of this squaring, the variance is no longer in the same unit of measurement as the original data. Taking the root of the variance means the standard deviation is restored to the original unit of measure and therefore much easier to interpret.


### 정리
행렬로 이해하면, Matrix Determinant같다. 득 행렬의 특징을 구하는 Determinant가 여기에서 포인트와 mean 값이 상관관계를 나타내 주는 것이 Standard Deviation이라고 생각이 든다. 이에대한 이용은 아래 레퍼런스를 확인하면 좋을 것 같다. 특히 이것 https://www.mathsisfun.com/data/standard-deviation.html

**方差是总体所有变量值与其算术平均数偏差平方的平均值，它表示了一组数据分布的离散程度的平均值。标准差是方差的平方根，它表示了一组数据关于平均数的平均离散程度。**

Variance와 Standard Deviation에 Square를 하는 것은, 데이터에 대한 unique 값을 갖게 하려고 하는 것이다 이것도 https://www.mathsisfun.com/data/standard-deviation.html 을 보면 알 수 있다.

REFERENCE
https://www.investopedia.com/ask/answers/021215/what-difference-between-standard-deviation-and-variance.asp
https://blog.csdn.net/colorknight/article/details/9531461
https://www.cnblogs.com/lonelyshy/p/12555273.html
https://www.mathsisfun.com/data/standard-deviation.html

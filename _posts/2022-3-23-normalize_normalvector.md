---
layout: post
title: Vector Normalization(Covariance Matrix, mass points)
category: calculus
tag: calculus
---

정규화란 무언가를 표준화 시키거나 다른 것과 비교하기 쉽도록 바꾸는 것을 의미합니다

Vector Normalization는 벡터에 대한 크기를 구하는 것이다.

벡터에 대한 크기를 가지고 우리는 특정 데이터를 표준화 할 수 있기 때문에 좀더 정확한 데이터 처리에 대한 결과를 얻을 수 있다.

데이터에 대한 표준화를 하지 않는다면 연산량과 데이터에 대한 튀는 값이 발생할 수 있기 떄문에 표준화 작업이 필요하다.

즉, normalization을 한다는 것은 크기를 구하여서 표준화 작업을 하겠다라고 이해하면 좋겠다.

예제로 Covariance Matrix를 구할때 Normalization Covariance(Mass points -> vector between masspoints and point -> distance += norm(normal vector) -> avg_distance = distance/number of points - > ratio = sqrt(2)/avg_distance -> each point *= ratio -> build covarinace matrix -> SVD(PCA) -> eigen vector and eigen value  -> scale = norm(eigen vector -> eigen vactor = eigen_vector/norm)


[Normalization intuition](https://ko.khanacademy.org/computing/computer-programming/programming-natural-simulations/programming-vectors/a/vector-magnitude-normalization)

#### covariance normalization 관련 코드

```c++
// Apply Least-Squares plane fitting
void FitPlaneLSQ(MatrixReaderWriter& mrw,
	vector<int>& inliers,
	Mat& plane)
{
	vector<Point3d> normalizedPoints;
	normalizedPoints.reserve(inliers.size());

	// Calculating the mass point of the points
	Point3d masspoint(0, 0, 0);

	for (const auto& inlierIdx : inliers)
	{
		Point3d p;
		p.x = mrw.data[mrw.columnNum * inlierIdx];
		p.y = mrw.data[mrw.columnNum * inlierIdx + 1];
		p.z = mrw.data[mrw.columnNum * inlierIdx + 2];
		masspoint += p;
		normalizedPoints.emplace_back(p);
	}
	masspoint = masspoint * (1.0 / inliers.size());

	// Move the point cloud to have the origin in their mass point
	for (auto& point : normalizedPoints)
		point -= masspoint;

	// Calculating the average distance from the origin
	double averageDistance = 0.0;
	for (auto& point : normalizedPoints)
	{
		averageDistance += cv::norm(point);
		// norm(point) = sqrt(point.x * point.x + point.y * point.y + point.z * point.z)
	}

	averageDistance /= normalizedPoints.size();
	const double ratio = sqrt(2) / averageDistance;

	// Making the average distance to be sqrt(2)
	for (auto& point : normalizedPoints)
		point *= ratio;

	// Now, we should solve the equation.
	cv::Mat A(normalizedPoints.size(), 3, CV_64F);

	// Building the coefficient matrix
	for (size_t pointIdx = 0; pointIdx < normalizedPoints.size(); ++pointIdx)
	{
		const size_t& rowIdx = pointIdx;

		A.at<double>(rowIdx, 0) = normalizedPoints[pointIdx].x;
		A.at<double>(rowIdx, 1) = normalizedPoints[pointIdx].y;
		A.at<double>(rowIdx, 2) = normalizedPoints[pointIdx].z;
	}

	cv::Mat evals, evecs;
	cv::eigen(A.t() * A, evals, evecs);
	const cv::Mat& normal = evecs.row(2); // the normal of the line is the eigenvector corresponding to the smallest eigenvalue
	const double& a = normal.at<double>(0),
		& b = normal.at<double>(1),
		& c = normal.at<double>(2);
	Point3d normalP(a, b, c);
	const double d = -normalP.ddot(masspoint);

	plane.at<double>(0) = a;
	plane.at<double>(1) = b;
	plane.at<double>(2) = c;
	plane.at<double>(3) = d;
}
```

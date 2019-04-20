---
layout:     post
title:      Coursera《机器学习》(吴恩达)编程作业第八周(ex7)
subtitle:   吴恩达机器学习课程编程作业 Week8 /exercise7
date:       2019-03-05
author:     王沛
header-img: img/machine-leaining.jpg
catalog: true
tags:
    - 机器学习
    - MATLAB
---

### 目录
- 必做题
1. [pca.m](#1) - Perform principal component analysis
2. [projectData.m](#2) - Projects a data set into a lower dimensional space
3. [recoverData.m](#3) - Recovers the original data from the projection
4. [findClosestCentroids.m](#4) - Find closest centroids (used in K-means)
5. [computeCentroids.m](#5) - Compute centroid means (used in K-means)
6. [kMeansInitCentroids.m](#6) - Initialization for K-means centroids

Coursera[课程地址](https://www.coursera.org/learn/machine-learning/)  
本周作业的官方指导文件可以从这里[**下载pdf**](/download/machine-learning-ex7.pdf)
--
---


<h2 id="1">1. pca</h2>

{% highlight MATLAB %}
function [U, S] = pca(X)
%PCA Run principal component analysis on the dataset X
%   [U, S, X] = pca(X) computes eigenvectors of the covariance matrix of X
%   Returns the eigenvectors U, the eigenvalues (on diagonal) in S
%

% Useful values
[m, n] = size(X);

% You need to return the following variables correctly.
U = zeros(n);
S = zeros(n);

% ====================== YOUR CODE HERE ======================
% Instructions: You should first compute the covariance matrix. Then, you
%               should use the "svd" function to compute the eigenvectors
%               and eigenvalues of the covariance matrix. 
%
% Note: When computing the covariance matrix, remember to divide by m (the
%       number of examples).
%
Sigma=1/m*X'*X;
[U,S,V]=svd(Sigma);

% =========================================================================

end
{% endhighlight %}


<h2 id="2">2. projectData</h2>

{% highlight MATLAB %}
function Z = projectData(X, U, K)
%PROJECTDATA Computes the reduced data representation when projecting only 
%on to the top k eigenvectors
%   Z = projectData(X, U, K) computes the projection of 
%   the normalized inputs X into the reduced dimensional space spanned by
%   the first K columns of U. It returns the projected examples in Z.
%

% You need to return the following variables correctly.
Z = zeros(size(X, 1), K);

% ====================== YOUR CODE HERE ======================
% Instructions: Compute the projection of the data using only the top K 
%               eigenvectors in U (first K columns). 
%               For the i-th example X(i,:), the projection on to the k-th 
%               eigenvector is given as follows:
%                    x = X(i, :)';
%                    projection_k = x' * U(:, k);
%
U_reduce = U(:, 1:K);
Z=X*U_reduce;

% =============================================================

end
{% endhighlight %}

<h2 id="3">3. recoverData</h2>

{% highlight MATLAB %}
function X_rec = recoverData(Z, U, K)
%RECOVERDATA Recovers an approximation of the original data when using the 
%projected data
%   X_rec = RECOVERDATA(Z, U, K) recovers an approximation the 
%   original data that has been reduced to K dimensions. It returns the
%   approximate reconstruction in X_rec.
%

% You need to return the following variables correctly.
X_rec = zeros(size(Z, 1), size(U, 1));

% ====================== YOUR CODE HERE ======================
% Instructions: Compute the approximation of the data by projecting back
%               onto the original space using the top K eigenvectors in U.
%
%               For the i-th example Z(i,:), the (approximate)
%               recovered data for dimension j is given as follows:
%                    v = Z(i, :)';
%                    recovered_j = v' * U(j, 1:K)';
%
%               Notice that U(j, 1:K) is a row vector.
%               
X_rec=Z*U(:,1:K)';

% =============================================================

end
{% endhighlight %}

<h2 id="4">4. findClosestCentroids</h2>

{% highlight MATLAB %}
function idx = findClosestCentroids(X, centroids)
%FINDCLOSESTCENTROIDS computes the centroid memberships for every example
%   idx = FINDCLOSESTCENTROIDS (X, centroids) returns the closest centroids
%   in idx for a dataset X where each row is a single example. idx = m x 1 
%   vector of centroid assignments (i.e. each entry in range [1..K])
%

% Set K
K = size(centroids, 1);

% You need to return the following variables correctly.
idx = zeros(size(X,1), 1);

% ====================== YOUR CODE HERE ======================
% Instructions: Go over every example, find its closest centroid, and store
%               the index inside idx at the appropriate location.
%               Concretely, idx(i) should contain the index of the centroid
%               closest to example i. Hence, it should be a value in the 
%               range 1..K
%
% Note: You can use a for-loop over the examples to compute this.
%
minimum = zeros(K,1);
for i = 1:length(idx)
  for j=1:K
      minimum(j)=sum((X(i,:)-centroids(j,:)) .^ 2);
  end
  [val,ind]=min(minimum);
  idx(i)=ind;
  
end

% =============================================================

end
{% endhighlight %}

<h2 id="5">5. computeCentroids</h2>

{% highlight MATLAB %}
function [U, S] = pca(X)
%PCA Run principal component analysis on the dataset X
%   [U, S, X] = pca(X) computes eigenvectors of the covariance matrix of X
%   Returns the eigenvectors U, the eigenvalues (on diagonal) in S
%

% Useful values
[m, n] = size(X);

% You need to return the following variables correctly.
U = zeros(n);
S = zeros(n);

% ====================== YOUR CODE HERE ======================
% Instructions: You should first compute the covariance matrix. Then, you
%               should use the "svd" function to compute the eigenvectors
%               and eigenvalues of the covariance matrix. 
%
% Note: When computing the covariance matrix, remember to divide by m (the
%       number of examples).
%
Sigma=1/m*X'*X;
[U,S,V]=svd(Sigma);

% =========================================================================

end
{% endhighlight %}

<h2 id="6">6. kMeansInitCentroids</h2>

{% highlight MATLAB %}
function centroids = kMeansInitCentroids(X, K)
%KMEANSINITCENTROIDS This function initializes K centroids that are to be 
%used in K-Means on the dataset X
%   centroids = KMEANSINITCENTROIDS(X, K) returns K initial centroids to be
%   used with the K-Means on the dataset X
%

% You should return this values correctly
centroids = zeros(K, size(X, 2));

% ====================== YOUR CODE HERE ======================
% Instructions: You should set centroids to randomly chosen examples from
%               the dataset X
%
% Initialize the centroids to be random examples

% Randomly reorder the indices of examples 
randidx = randperm(size(X, 1)); 
% Take the first K examples as centroids 
centroids = X(randidx(1:K), :);

% =============================================================

end
{% endhighlight %}
---
layout:     post
title:      Coursera《机器学习》(吴恩达)编程作业第五周(ex4)
subtitle:   吴恩达机器学习课程编程作业 Week5 /exercise4
date:       2019-02-13
author:     王沛
header-img: img/machine-leaining.jpg
catalog: true
tags:
    - 机器学习
    - MATLAB
---

### 目录
- 必做题
1. [sigmoidGradient.m](#1) - Compute the gradient of the sigmoid function
2. [randInitializeWeights.m](#2) - Randomly initialize weights
3. [nnCostFunction.m](#3) - Neural network cost function


Coursera[课程地址](https://www.coursera.org/learn/machine-learning/)  
本周作业的官方指导文件可以从这里[**下载pdf**](/download/machine-learning-ex4.pdf)
--
---


<h2 id="1">1. sigmoidGradient</h2>

{% highlight MATLAB %}
function g = sigmoidGradient(z)
%SIGMOIDGRADIENT returns the gradient of the sigmoid function
%evaluated at z
%   g = SIGMOIDGRADIENT(z) computes the gradient of the sigmoid function
%   evaluated at z. This should work regardless if z is a matrix or a
%   vector. In particular, if z is a vector or matrix, you should return
%   the gradient for each element.

g = zeros(size(z));

% ====================== YOUR CODE HERE ======================
% Instructions: Compute the gradient of the sigmoid function evaluated at
%               each value of z (z can be a matrix, vector or scalar).

gz = 1./(1+exp(-z));
g = gz.*(1-gz);

% =============================================================

end
{% endhighlight %}


<h2 id="2">2. randInitializeWeights</h2>

{% highlight MATLAB %}
function W = randInitializeWeights(L_in, L_out)
%RANDINITIALIZEWEIGHTS Randomly initialize the weights of a layer with L_in
%incoming connections and L_out outgoing connections
%   W = RANDINITIALIZEWEIGHTS(L_in, L_out) randomly initializes the weights 
%   of a layer with L_in incoming connections and L_out outgoing 
%   connections. 
%
%   Note that W should be set to a matrix of size(L_out, 1 + L_in) as
%   the first column of W handles the "bias" terms
%
% You need to return the following variables correctly 
W = zeros(L_out, 1 + L_in);

% ====================== YOUR CODE HERE ======================
% Instructions: Initialize W randomly so that we break the symmetry while
%               training the neural network.
%
% Note: The first column of W corresponds to the parameters for the bias unit
%
% Randomly initialize the weights to small values
epsilon_init = 0.12;
W = rand(L_out, 1 + L_in) * 2 * epsilon_init - epsilon_init;

% =========================================================================

end
{% endhighlight %}

<h2 id="3">3. nnCostFunction</h2>

{% highlight MATLAB %}
function [J grad] = nnCostFunction(nn_params, ...
                                   input_layer_size, ...
                                   hidden_layer_size, ...
                                   num_labels, ...
                                   X, y, lambda)
%NNCOSTFUNCTION Implements the neural network cost function for a two layer
%neural network which performs classification
%   [J grad] = NNCOSTFUNCTON(nn_params, hidden_layer_size, num_labels, ...
%   X, y, lambda) computes the cost and gradient of the neural network. The
%   parameters for the neural network are "unrolled" into the vector
%   nn_params and need to be converted back into the weight matrices. 
% 
%   The returned parameter grad should be a "unrolled" vector of the
%   partial derivatives of the neural network.
%

% Reshape nn_params back into the parameters Theta1 and Theta2, the weight matrices
% for our 2 layer neural network
Theta1 = reshape(nn_params(1:hidden_layer_size * (input_layer_size + 1)), ...
                 hidden_layer_size, (input_layer_size + 1));

Theta2 = reshape(nn_params((1 + (hidden_layer_size * (input_layer_size + 1))):end), ...
                 num_labels, (hidden_layer_size + 1));

% Setup some useful variables
m = size(X, 1);
         
% You need to return the following variables correctly 
J = 0;
Theta1_grad = zeros(size(Theta1));
Theta2_grad = zeros(size(Theta2));

% ====================== YOUR CODE HERE ======================
% Instructions: You should complete the code by working through the
%               following parts.
%
% Part 1: Feedforward the neural network and return the cost in the
%         variable J. After implementing Part 1, you can verify that your
%         cost function computation is correct by verifying the cost
%         computed in ex4.m
%
% Part 2: Implement the backpropagation algorithm to compute the gradients
%         Theta1_grad and Theta2_grad. You should return the partial derivatives of
%         the cost function with respect to Theta1 and Theta2 in Theta1_grad and
%         Theta2_grad, respectively. After implementing Part 2, you can check
%         that your implementation is correct by running checkNNGradients
%
%         Note: The vector y passed into the function is a vector of labels
%               containing values from 1..K. You need to map this vector into a 
%               binary vector of 1's and 0's to be used with the neural network
%               cost function.
%
%         Hint: We recommend implementing backpropagation using a for-loop
%               over the training examples if you are implementing it for the 
%               first time.
%
% Part 3: Implement regularization with the cost function and gradients.
%
%         Hint: You can implement this around the code for
%               backpropagation. That is, you can compute the gradients for
%               the regularization separately and then add them to Theta1_grad
%               and Theta2_grad from Part 2.
%
X=[ones(m,1) X];
hx = sigmoid([ones(m,1) sigmoid(X*Theta1')]*Theta2');

yk=zeros(m,num_labels);
for i=1:m
  yk(i,y(i))=1;
end

J = sum( sum(-log(hx).*yk) - sum(log(1-hx).*(1-yk)) )/m + ( sum(sum(Theta1(:,2:end).^2))+sum(sum(Theta2(:,2:end).^2)) )*lambda/(2*m) ;

%BackPropagation
for ex=1:m
  a1=X(ex,:)';    %first example data 401*1
  z2=Theta1*a1;    %25*1
  a2=[1;sigmoid(z2)];     %26*1
  z3=Theta2*a2;    %10*1
  a3=sigmoid(z3);
  y=yk(ex,:);   %1*10
  delta3=a3-y'; %10*1
  delta2=Theta2(:,2:end)'*delta3.*sigmoidGradient(z2); %25*1  delta1===0
  
  Theta1_grad=Theta1_grad+delta2*a1';
  Theta2_grad=Theta2_grad+delta3*a2';
end
  
Theta1(:,1)=0;
Theta2(:,1)=0; %Regulization
Theta1_grad=Theta1_grad./m + lambda/m*Theta1 ;
Theta2_grad=Theta2_grad./m + lambda/m*Theta2 ;
% -------------------------------------------------------------

% =========================================================================

% Unroll gradients
grad = [Theta1_grad(:) ; Theta2_grad(:)];

end
{% endhighlight %}

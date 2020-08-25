# Linear Regression

## Linear Regression이란?

- Supervised learning
- Line fitting
- binary classification / multi classification
- return **value** not class



* Linear
  * y(x) = w<sup>T</sup>x+ *ε* 
  * 가장 적합한 w를 찾기
  
* Non-linear relationships

  * 가장 적합한 ∮를 찾기
  * Polynomial Regression
  * Multivariate linear regression

* Maximum likelihood estimation

* **Residual Sum of Squares**

  * To minimize NLL(Negative log likelihood), we minimize this term called residual sum of squares

    <img src='https://wikimedia.org/api/rest_v1/media/math/render/svg/1d02ea15d89dd2cfacc25842694462970df5e216'>

  * 예측된 y값과 실제 y값의 차이가 minimum하게 만드는 w를 찾는다. 위 수식에서는 ![\beta ](https://wikimedia.org/api/rest_v1/media/math/render/svg/7ed48a5e36207156fb792fa79d29925d2f7901e8)

* Ridge Regression

  * Regularization: overfitting 방지
  * RSS+람다: w값을 작게



## Regularization

* overfitting 방지
* l2 norm / l2 regularization
* degree가 커져도(too complex) **big data**인 경우 어느정도 괜찮은 결과를 낸다.



### 참고

* 오일석, '패턴인식', 교보문고(2016)
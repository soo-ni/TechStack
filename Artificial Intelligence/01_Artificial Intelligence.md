# Artificial Intelligence

## Machine Learning vs. ...

* Big Data
  * ML is one way to **analyze, understand**, and **predict** Big Data

* Data Mining
  * ML doesn't require **structured data**, while Data Mining does
  * ML: 비정형 데이터(images, texts)
  * Data Mining: 정형 데이터
* Artificial Intelligence
  * ML develops **data-dependent** solutions to the problems in AI
  * ML ⊂ AI
* Statistics
  * ML is depply rooted in, but **expands** the practical limitations of Statistics
  * 데이터의 양이 많고, noise가 많은 경우 ML이 통계학의 한계를 극복



## Major Problem Formulations

### Supervised

* Training data에 label이 있는 경우
* Classification model
  * Linear
  * Non-linear
    * Decision Trees: features extractions

### Unsupervised

* Training data에 label을 얻기 힘든 경우
* Algorithm
  * *k*-means clustering: 기준 점을 찾은 후 거리 계산
  * DB Scan: 임의의 점 하나에서 정해진 거리내에 있는 점을 모음

### Representation 

deep learning

* Deep Neural Network
* Neural Networks
  * **Reducing the demensionality** of data
  * 분류할 수 있는 의미있는 단위까지 reducing
  * ex. facial recognition
    * layer1(pixel:gradients) => layer2(lines) => layer3(shapes) => layer4(face)
    * low level feature => high level feature

Why now?

- **Complexity of model** is very high - requires <u>huge datasets</u>, advanced <u>hardware</u> with fast processors, large memory, high I/O speed
- Model is prone to overfitting - advanced algorithms to **overcome overfitting** are needed (dropped out)
- Parameter estimation is difficult - clever modifications to the model to the model, as well as advanced algorithms to estimate parameters more quickly

### Reinforcement



## Famous AI Systems

* IBM Chess machine
* Urban Challenge
* IBM Research Watson 2011
* DeepMind AlphaGo 2016
* DeepMind AlphaGo Zero 2017
* Google Duplex



## Major Domains within Artificial Intelligence & Corresponding Datasets

* Visual Intelligence
  * MNIST (basic)
  * ImageNet (hierarchical): accuracy, one-shot learning, transfer learning
* Language Intelligence
  * SQUAD: Stanford Question Answer
  * Machine Translation
  * GLUE Benchmark



### 참고

* 오일석, '패턴인식', 교보문고(2016)
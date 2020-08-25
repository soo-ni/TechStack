# Clustering

적절한 방식으로 특징 벡터 간의 거리를 정의할 수 있다면 유사한(즉 거리가 가까운) 샘플들을 모아 하나의 그룹으로 간주하는 것을 **군집(Cluster)**이라 하고, 이러한 작업을 **군집화(Clustering)**라 한다.

## Applications of Clustering

* 백화점 고객을 구매 상품에 따라서 클러스터링
* 추천 시스템에 의해 고객의 과거 패턴을 이용해서 클러스터링
* Gene 데이터를 유사도에 따라서 클러스터링
* 텍스트 문서들을 주제에 따라서 클러스터링
* Facebook에서 이미지들을 유사한 이미지들로 클러스터링
* Call center에서 고객과 통화한 내용을 텍스트로 변환하여 그 내용에 나타나는 단어나 어휘구나 어휘 절을 추출하고 이를 이용하여 각각의 통화 내역을 그룹으로 나누어서 회사에 걸려온 상담 내용을 카테고리 별로 나누어 보고 각 카테고리 별로 요약정보를 만듦

## Clustering Algorithm

### *k*-means Clustering

군집의 개수 k를 지정한 후 군집의 중심을 초기화한다. 샘플 각각에 대해 k개의 군집 중심 중 가장 가까운 것을 찾아 배정한 다음 각 군집의 중심을 샘플의 평균으로 대치한다. 이러한 과정을 반복할 때, 군집 배정의 결과가 이전과 같으면 멈춘다.

> 고려해야할 것
>
> 1. 군집의 중심을 어떻게 초기화 할 것인가?
> 2. 군집 중심과 샘플간의 거리 계산을 어떤 방법으로 할 것인가?

### *k*-medoids Clustering

k-means에서 outlier가 있는 경우, 새로운 군집 중심이 왜곡되는 단점이 있어 이러한 현상을 완화시키는 변형이 k-medoids 알고리즘이다. k-means와 같이 샘플의 평균을 구하는 것이 아닌 다른 샘플까지의 거리의 합이 최소가 되는 점을 대표로 삼아 그것을 군집 중심으로 한다.

### Hierarchical Clustering

계층 군집화(hierarchical clustering) 알고리즘은 포함 관계(⊂)를 군집화 결과로 출력한다. 계층 군집화 알고리즘은 작은 군집들에서 출발해 이들을 모아나가는 **응집(agglomerative)** 방식과 큰 군집에서 출발해 이들을 나누어 나가는 **분열(divisive)** 방식이 있다.

#### 응집 방식

> 1. 초기 입력: 각 샘플이 하나의 군집
>
> 2. 가장 가까운 쌍을 찾아(*D<sub>min</sub>*) 두 군집을 하나로 합친다.
>
> 3. 두 군집을 제거하고 새로운 군집을 추가한다.
>
> 4. 2-3 반복
>
> 5. 출력: 덴드로그램(**dendrogram**: 군집화 결과를 트리 형태로 표현)
>
>    
>
> - *D<sub>min</sub>*: Single-link
> - *D<sub>max</sub>*: Compelete-link
> - *D<sub>ave</sub>*: Average-link
> - *D<sub>mean</sub>*: Mean-link
> - *D<sub>cent</sub>*: Centroid-link

#### 분열 방식

> 1. 초기 입력: 각 샘플이 하나의 군집
> 2. 가장 먼 쌍을 찾아(*Dmin*) 두 군집을 하나로 합친다.
> 3. 두 군집을 제거하고 새로운 군집을 추가한다.
> 4. 2-3 반복
> 5. 출력: 덴드로그램

### DBSCAN Clustering

밀도기반 클러스터링(Density-based Spatial Clustering of Applications with Noise)은 K-means와 Hierarchical Clustering의 경우 군집간의 거리를 이용하여 클러스터링하는 것과 다르게 **밀도가 높은 부분**을 클러스터링 하는 방식이다.

점 p에서 거리 *ε* (epsilon) 내에 점이 m(minPts)개 있으면 하나의 군집으로 인식한다. 즉, 점 p를 중심으로 *ε* 반경 내에 m개 이상의 점이 있으면 해당 p는 core point가 되고, 군집에는 속하지만 스스로 **core point**가 될 수 없는 점을 **border point**라 한다. 어느 클러스터에도 속하지 않는 점은 **noise point**가 된다.

* Density-reachable
* Density-connected

### EM Clustering

#### Probabilistic Latent Semantic Indexing (PLSI)

Expectation-Maximization Clustering

* Expectation: 샘플이 어느 가우시언에 속하는지 추정하는 단계로 소속 정도를 확률로 표현한다.
* Maximization: 소속 값 추정을 마친 후 매개 변수 집합의 추정을 시작한다.
* E와 M 단계를 번갈아가며 반복하다가 수렴조건이 만족되면 멈춘다.

> 1. 샘플 각각을 가장 가까운 프로토타입에 할당한다.
> 2. 각 프로토타입을 자신에게 배정된 샘플의 평균으로 대치한다.
> 3. 로그 우도를 계산하고 이전 것과 비교하여 더 이상 좋아지지 않으면 수렴했다고 결정한다. (임계 값 설정)

### Matrix Factorization

추천 알고리즘에서 많이 쓰이는 알고리즘이다.



## 참고

* [DBSCAN] https://bcho.tistory.com/1205

* 오일석, '패턴인식', 교보문고(2016)
# Algorithm

## Sorting

|           | 시간                  | 공간 | 설명                                                         |
| --------- | --------------------- | ---- | ------------------------------------------------------------ |
| Bubble    | O(n^2)                | O(n) | * 서로 인접한 두 원소의 대소를 비교하고, 조건에 맞지 않으면 자리를 교환하며 정렬<br />* Stable Sort |
| Selection | O(n^2)                | O(n) | * 차례대로 자리를 선택하고 그 자리에 오는 값을 찾는 것을 의미한다<br />* Unstable Sort |
| Insertion | O(n^2)                | O(n) | * 두번째 원소부터 시작해서 그 앞의 원소들과 비교해 삽입할 위치를 지정한 다음 지정된 자리에 값을 삽입<br />* Unstable Sort |
| Quick     | O(nlog₂n)<br />O(n^2) | O(n) | * 분할 정복 (divide and conquer)<br />* 배열 가운데 임의로 Pivot을 고른 후 Pivot보다 작으면 앞으로 Pivot보다 크면 뒤로 이동<br />* Unstable Sort |
| Merge     | O(nlogn)              | O(n) | * 분할 정복 (divide and conquer)<br />* 배열을 나눌 수 없을 때까지 반으로 나눈 다음 배열간 비교를 통해 작은 값은 왼쪽, 큰 값은 오른쪽으로 옮긴 다음 합병시키며 정렬<br />* Stable Sort<br />* LinkedList가 좋음 |
| Heap      | O(nlogn)              | O(n) | * 완전 이진 트리 (Heap)<br />* 최대 혹은 최소 힙을 구성한 다음 가장 루트 노드에서 가장 최대 값 혹은 최소 값이 있으므로 해당 노드를 마지막으로 바꾸고 제거 => 다시 정렬 |

|           | img                                                          |
| --------- | ------------------------------------------------------------ |
| Bubble    | <img src="https://github.com/GimunLee/tech-refrigerator/raw/master/Algorithm/resources/bubble-sort-001.gif" width=300 /> |
| Selection | <img src="https://github.com/GimunLee/tech-refrigerator/raw/master/Algorithm/resources/selection-sort-001.gif" width=300 /> |
| Insertion | <img src="https://github.com/GimunLee/tech-refrigerator/raw/master/Algorithm/resources/insertion-sort-001.gif" width=300 /> |
| Quick     | <img src="https://github.com/GimunLee/tech-refrigerator/raw/master/Algorithm/resources/quick-sort-001.gif" width=300 /> |
| Merge     |                                                              |
| Heap      |                                                              |


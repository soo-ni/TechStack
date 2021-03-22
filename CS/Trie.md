# Trie

일반 트리 자료구조 중 하나로(N진 트리: N은 문자의 총 종류가 된다), **Digital Tree, Radix Tree, Prefix Tree**라고 불린다. 텍스트 자동 완성 기능과 같이 문자열을 저장하고 탐색하는 데 유용한 자료구조이다.

## Trie?

### 형태

각 노드는 <Key, Value> 값을 가지고 있다. Key는 하나의 알파벳이 되고, Value는 그 Key에 해당하는 자식 노드가 된다. 루트 노드는 특정 문자를 의미하지 않고 자식 노드만 가지고 있고, 노드의 자손들은 해당 노드와 공통 접두어(prefix)를 가진다.

이 때, 키워드 정보는 마지막 노드만 가지고 있고 나머지 노드들은 정보는 없고 링크만 가지게 된다. 

### 특징

* 정렬된 트리 구조이다.
* 자식 노드를 Map<Key, Value> 형태로 가지고 있다.
* 루트를 제외한 노드의 자손들은 해당 노드와 공통 접두어를 가진다.
* 루트 노드는 빈 문자와 연관 있다.

## 구현

* Node

  ```java
  public static class Node {
      char data;
      boolean isEnd;
      Node[] child;
      
      Node(char c) {
          data=c;
          child=new Node[27];
      }
      
      Node setChild(char c) {
          if(c=='\0') {
              isEnd=true;
              return null;
          }
          if(child[c-'A']==null) {
              child[c-'A']=new Node(c);
          }
          
          return child[c-'A'];
      }
  }
  ```

* Trie

  ```java
  public static class Trie {
      Node n;
      
      Trie() {
          n = new Node('\0');
      }
      
      void add(String s) {
          Node n = this.n;
          for(int i=0; i<s.length(); i++) {
              n = n.setChild(s.charAt(i));
          }
          n.setChild('\0');
      }
      
      boolean isContain(int length) {
          Node n = this.n;
          for(int i=0; i<length; i++) {
              if(n.child[string[i]-'A']==null) {
                  return false;
              }
              n = n.child[string[i]-'A'];
          }
          
          return n.isEnd;
      }
  }
  ```

  





## 참고

* https://the-dev.tistory.com/2
* https://gusdnd852.tistory.com/174
* https://m.blog.naver.com/javaking75/140211950640


# 알고리즘

## 1주차

**수학적 귀납법을 통해 재귀 증명**(매우중요)

==> 직관과 충돌하는 부분이 있음!!, 아주 쉬운 예를 가지고 따라 들어가지 않는 법을 연습한다.



우선 명제 P -> Q의 의미를 알아야 한다.

P가 참, Q가 참 -> 전체는 참
P가 참, Q가 거짓 -> 전체는 거짓
P가 거짓 -> 전체는 거짓

P가 거짓일때는 Vacuous라고 해서 항상 전체가 참인 경우이다. 이 부분이 직관과 충돌하지만 예시를 들어보면 편하다.

시험을 100점 받으면 치킨을 사준다(P->Q)에서

시험을 100점 받았는데도 치킨을 못 먹은 경우는 확실히 F이다.

하지만 시험을 못봤으면 치킨을 먹든 못 먹든 상관이 없어져서 이부분이 Vacuous라 하는데 False는 확실히 아니고 True라고 하기도 애매해서 Vacuous라 부른다. 근데 이 부분을 그냥 Vacuous True라고 불러서 True로 치는 것이다.

수학적 귀납법의 큰 틀은 

(1) Base : P(1)이 참이고

(2) Step : P(n-1) -> P(n)이 참이면

(3) Result : P(n)은 모든 자연수 n에 대해서 참이다.

**여기서 P(n-1)이 참이라고 가정하는 이유는 뭘까? 만약 거짓이면?**

이 부분에서 Vacuous True가 사용된다.

P(n-1)이 거짓일때는 전체가 참이므로 애초에 논의할 의미가 없다.

따라서 P(n-1)이 참인 경우에만 논의해야하며, 이를 참이라 믿는 것이다.

**총합을 구하는 재귀 함수**
```
int sum(int x){
    if(x==0){
        return 0;
    }
    return x + sum(x-1);
}
```
보통의 이러한 재귀 함수를 보면 그 다음단계로 호출하는 과정을 생각하며 계속 굴처럼 파고 들어가서 이해를 한다.

**1주차의 가장 큰 의미는 이러한 과정을 안하기 위해서 재귀호출이 그냥 정답을 return한다고 믿자는 것이다.**

사실 재귀함수가 조금만 복잡해져도 그 함수를 들어가서 이해하는 것은 매우 어려운 일에 해당하고 교수님 말씀으로는 이런식으로 이해하는 사람이 세상에 그렇게 많지 않다고 한다.. 일단 난 절대 아님 ㅠ

아무튼 저 코드에서 Sum(x-1)을 보면 함수를 따라 들어가지 말고 1부터x-1까지의 합을 return한다고 믿으면 된다.

그럼 결론적으로 return x + sum(x-1)은 
1부터 x까지의 합을 리턴한다.

**Binary Search**

```
int search(int a[], int n, int x){ //배열, size, 찾을 값
    int l, r, m;
    l=0; // 맨 왼쪽 인덱스
    r=n-1; // 맨 오른쪽 인덱스
    while(l<=r){
 
        m=(l+r)/2; // l<=m<=r

        if(a[m]==x){
            return m;
        }

        else if(a[m]>x){
            r=m-1;
        }

        else{
            l=m+1;
        }
        return -1;
    }
}
```

**핵심**

1) m을 보고 a[m]>x이면 찾는 값 보다 a[m]이 오른쪽에 있으므로 오른쪽은 찾을 필요가 없다.-> l부터 m-1까지만 조사하자

    a[m]<x이면 찾는 값보다 a[m]이 왼쪽에 있으므로 왼쪽은 찾을 필요가 없다. -> m+1부터 r까지만 조사하자.

2) r이 l보다 작을 경우.. => 서로 교차한 상황
=> l부터 r까지의 배열이 0개 => 0개 중에 x가 있다? => x가 없다.

    세 개의 박스중에 고양이가 있다면 한 곳에 있다 ==> 고양이가 있는지 없는지 모른다.

    박스가 없다, 고양이가 있다면 박스 중에 한 곳에 있다 => 박스가 없으므로 고양이가 없다.

**증명**

Invariant : 불변(코드에서 잘 찾아야 함)

a[i] = x라면 l<=i<=r 이 항상 성립한다.

=> 찾고자 하는 값이 있다면 l과 r 사이에 존재한다.(l과 r이 바뀔때 항상 조건이 성립한다.)

이걸 깨뜨리는 코드는 존재하지 않는다.(집합조건을 깨뜨리는 코드)

따라서 a[i]=x인 i가 없다면 -1이 리턴된다 -> 루프 안에서 반드시 리턴된다

**Recursive Binary Search**

```
int search(int a[], int n , int x){
    int m;
    if(n==0){ // 배열 사이즈가 0이하면 없다는 것 
        return -1;
    }
    m=n/2;
    if(a[m]==x){
        return m; 
    }
    else if(a[m] > x){
        return search(a, m, x); // m개(0부터 m-1까지의 개수)
    }
    else{
        r = search(a+m+1, n-m-1, x); // 전체 n개에서 m+1개가 빠짐 n-(m+1) == n-m-1
        if(r==-1){
            return -1;
        }
        else{
            return r + m + 1;
        }
    }
}
```

**증명**

Base : n이 0인 경우, Vacuous true로 만족한다.

Step:
    
- case 1 :
  
    a[m]==x인 경우, m을 리턴하므로 성립


- case 2 :

    a[m]>x인 경우, 오른쪽을 무시, 즉 a[m+1], a[m+2], ..., a[n]이 모두 x보다 크므로 a[i]=x인 경우가 있다면 i는 0,1, ..., m-1 중 하나이다.

    따라서 return(a,m,x)에서 재귀함수로 성립한다고 믿으면 위 주장이 성립한다.


- case 3 : case2와 동일



**Selection Sort**

```
int sort(int a[], int n){
    int i, j, m, t;
    for(i=0; i<n; i++){
        m=i;
        for(j=i; j<n; j++){
            if(a[m]>a[j]){
                m=j;
            }
            t=a[i]; //swap
            a[i]=a[m];
            a[m]=t;
        }
    }
}
```


Sorting이 됐다는 증명을 어떻게?

입력 : a[0], a[1], ...

Sorting이 완료된 후 다음이 만족되어야 함

1) Sorting전의 a[0], a[1], ... a[n-1](sorting 전 집합) = b[0], b[1], ..., b[n-1](Sorting 후 집합)

2) b[0] < b[1] < ... < b[n-1]

## proof by Invariant

집합 조건을 깰 수 있는 코드가 없음

k번째 루프가 끝난 후에

**Invariant**(찾는데는 창의력을 요구함)

(1) a[0] < a[1] < ... < a[k-1] -> a[0] < a[1] < ... < a[n-1]

(2) if x > k-1 a[k-1] < a[x]

위 두개의 Invariant가 항상 성립한다는걸 증명하면 집합 조건을 깰 수 있는 코드는 없다와 (1),(2)가 사실이므로 Sorting이 증명된다.

Invariant가 항상 성립한다는건 수학적 귀납법으로 증명한다.

Base : k=0, (1), (2)가 null condition으로 true이다.(아무 것도 없으면 true => vacuous true )

Step : k번째 루프가 끝났을 때 Invariant가 성립한다면, k+1번째 루프가 끝났을때도 Invariant가 성립한다.

Invariant : k번째 루프가 끝난 후

(1) : a[0] > a[1] < ... < a[k-1]

(2) : if x > k-1 a[k-1] < a[x]

**a[k], ..., a[n-1]중 최소값을 a[k]로 옮김**

Invariant : k+1번째 루프가 끝난 후

(1) : a[0] > a[1] < ... < a[k-1] < a[k] (a[k]가 뭐든지간에 위의 (2)에의해 성립)

(2) : if x > k a[k] < a[x]


**Recursive Selection Sort**

```
int sort(int a[], int n){
    int j, m, t;
    if(n<=1){
        return;
    }
    m=0;
    for(j=0; j<n; i++){
        if(a[m] > a[j]){
            m=j;
        }
    }
    t=a[0]; //swap
    a[0]-a[m];
    a[m]=t;

    sort(a+1, n-1); // a+1번째부터 n-1개의 배열을 sorting한다(고 믿자)
    return;
}
```

## proof by Invariant

집합 조건을 깰 수 있는 코드는 지금도 없음

Base : n=1 할 일이 없음 => True

Step : 

n-1일 때, sort함수가 성공한다면

=> 즉, 재귀 호출이 끝난 후 a[1] < a[2]< ... < a[n-1]이라면

n일때 sort함수가 성공한다.

=> 함수가 끝날 때 a[0] < a[1] < ... < a[n-1]

재귀 호출 전 a[0]은 MIN이고 a[0] < a[1] , ... , a[0]< a[n-1]이 성립한다.

**결론적으로 재귀 호출로 인해 1~n-1이 sort가 되어있고 a[0] < a[1]이 자명하므로(a[1] a[0]이 아닌 a[1] or a[2] or ,... a[n-1]이기 때문에)
0~n-1은 sort된다.**


## 2주차

## Merge Sort

```
int merge_sort(int a[], int n){
	int h;
	int b[n];
    if (n == 1)
        return;
	h = n / 2;
	//copy a[] to b[]
    merge_sort(b, h); //왼쪽 sort
	merge_sort(b+h, b-h); //오른쪽 sort
	// 왼쪽/오른쪽 sorting된 상태에서 둘을 합친다.
}
```

### Proof by Induction

- 집합 조건 깰 수 있는 코드 없음

- Base : n=1, 할 일이 없음

- Step : 

    - n/2일때 sort()함수가 성공한다면 => 즉, 재귀 호출이 끝난후 b[0] < b[2] < ... b[n/2-1] and b[n/2] < b[n/2+1] < ... < b[n-1] 이라면 (왼쪽 오른쪽 각각 sorting이 된다면!)

    - n일때 sort()함수가 성공한다.

        => 함수가 끝날 때 a[0] < a[1] < ... <a[n-1]이 성립

        => **Merge의 정확성에서 온다.**

- Merge의 정확성 : 합집합을 제대로 한다.

    a[]에 b[]로 복사가 될 때, 같은 원소가 그대로 넘어가고, sort함수에서도 집합 조건을 깨지 않는다면 재귀 호출이 끝난 후에 b에 있는 애들을 합집합을 하면 원래 입력에 있던 집합과 같다. ==> **집합 조건이 깨지지 않는다.**

# NlogN
 
 ### Quick Sort(처음에 보면 매우 어렵다)

 ```
 int qsort(int a[], int n){
	int d;
	int p = a[0]; // 피벗
	if (n <= 1) return; // base
	// Rearrange a[] so that
	// a[d] == p,
	// a[0], a[1], ..., a[d-1] < p
	// a[d+1], a[d+2], ..., a[n-1] > p
	// -> O(N) Partitioning
	qsort(a, d);
	qsort(a+d+1, n-d-1);
	return;
}
```

- 집합 조건 깰 수 있는 코드 없음

- Base : n=0 or 1, 할 일이 없음

- Step : 

    재귀 호출 전에 if(x<d) a[x] < a[d]와 if(y>d) a[y] > a[d]가 성립한다.

    - qsort(a, d-1)과 qsort(a+d+1, n-d-1)이 성공한다면 => 재귀 호출이 끝난 후, a[0] < a[1] < ... < a[d-1] and a[d+1] < a[n/2+1] < ... < a[n-1]이라면 (a[d]를 기준으로 왼쪽은 a[d]보다 작고, 오른쪽은 크다면)

    - n일때 qsort()함수가 성공한다. => 함수가 끝날 때 a[0] < a[1] < a[2] < ... < a[n-1] 성립

  
        => 안에는 a[d-1] < a[d] < a[d+1] < a[d+2] ... 이 숨겨져 있고 a[d-1] 전에는 모두 성립하고, a[d] < a[d+1]과 a[d] > a[d+1]이 성립하는지만 보이면 된다.

퀵소트는 최선이 NlogN, 최악은 N^2 Slection Sort와 비슷, 그럼에도 퀵소트를 가장 많이 사용하는 이유는...?


## Greedy Algorithms Minimum Spanning Tree

최적해를 찾은 후, 같은 방법으로 계속해서 해를 찾아나간다.

비교적 간단한 방법으로 선택하고 선택한 후 바꾸지 않는 알고리즘

### MST(Minimum Spanning Tree)

<img src="https://ifh.cc/g/GPOxO5.png">


단순히 Greedy를 쓰면 그 상황에서 최소를 찾아서 해결하면 답이 될 수 없다!! => 그럼 어떻게 찾지..? ==> 다른 Greedy를 사용해야 한다.

Tree : Cycle이 없는 연결된 그래프

**연결 그래프**

무방향 그래프에 있는 모든 정점 쌍에 대해서 항상 경로가 존재하는 그래프

즉 노드들이 하나도 빠짐없이 간선에 연결되어 있는 그래프로 트리(Tree) 가 대표적인 예이다.


- Cycle이 있는 연결그래프가 답이 되지 않는 이유


    만약에 답이 Cycle이 있는 연결 그래프 였는데 싸이클이 있으면 하나를 끊어도 여전히 연결 그래프다. ==> 그럼 가중치를 줄일 수 있으니까 **Cycle이 있으면 답이 아니다!**

**노드가 5개면 Edge를 4개를 고르면 된다. => 싸이클 안생김**


## Prim Algoritm

- 아이디어

    - 하나의 노드를 가진 트리에서 시작(아무 노드나)

    - 트리에 인접한 에지들 중 가장 작은 웨이트를 가진 에지를 추가(대신 Cycle 만들지 않아야 함)

    - Spanning Tree가 될 때 까지 반복

- 정확성 증명(어렵다)

    답이 여러개일 수 있고, 그 중에 하나를 찾는다.


    - Invariant
  
        **$T_{mst}$가 MST 중 한 트리라고 하자. Prim이 만들어가는 답 $T$가 항상 $T\subseteq T_{mst}$임을 보이면 됨(항상 유지)**

    - Base : |T| = 0, 아무것도 안넣었을때는 공집합, 공집합은 당연히 부분집합 => OK

    - Step : |T| = k(원소가 k개)일때, T<=$T_{mst}$

        <img src="https://ifh.cc/g/rY0yWx.png">

        Alg이 한번의 선택 진행

        $T'= T\cup\{e\}$

        case 1: $T' \subseteq T_{mst}$일 경우 성립한다.

        case 2: $T' \nsubseteq T_{mst}$일 경우, $e \notin T_{mst}$ 이다. ==> **다른 $T_{mst}$을 만족시킴을 보이면 된다.**

        **이 경우, $e$를 포함한 유일한 Cycle이 존재한다.**

        **Cycle에 $T'$에 속하지 않고, $T$에 인접한 edge가 존재한다..**

        e의 양쪽 노드 중 T에 인접한 것이 있고, 인접하지 않은 것이 있다.

        이중에 a를 인접해 있는 노드, b를 인접해 있지 않은 노드라 하자.

        만약 e의 양쪽 노드 둘다 T에 인접해있으면 Cycle이 생겨서 알고리즘에 추가하지 않는다.

        a에서 Cycle을 돌아 b로 오는 경우를 생각해보자.
        **그럼 반드시 인접한 노드에서 인접해 있지 않은 노드로 가는 경우가 존재한다.** ==> $e'$ 가 반드시 존재한다.

        <img src="https://ifh.cc/g/An7qln.png">

        $e'$도 e와 동일한 조건으로 트리에 인접한 노드와 인접해있지 않은 노드를 연결하므로 Cycle을 만들지 않는다 ==> **고려대상이 된다.**

        W(e) 와 W(e')의 가중치를 비교해보자!!

        case 1) 

        **w(e) < w(e')**

        $T_{mst}$ 에서 $e'$를 빼고 $e$를 넣은 것이 더 좋은 답이 된다.

        ==> **$T_{mst}$가 정답이 아니게 된다.**

        case 2)

        W(e) > W(e')

        => Algorithm이 $e'$를 고른다.

        case 3)

        W(e) == W(e')

        바꿔도 여전히 정답이 된다.

        $T_{mst}$ 에서 $e'$를 빼고 $e$를 넣은 것이 더 좋은 답이 된다.

        ==> 또한 정답이 된다.


    알고리즘이 정답에 없는걸 추가했더니 모순이 생겼다 => 일어날 수 없는 일이다.(안생기는 케이스)

## 3주차,4주차

### Kruskal Algorithm

MST 알고리즘, Prim과 매우 유사하지만 차이는 Prim은 근처에 있는 것 중에 작은걸 추가하며 넓혀가지만 Kruskal은

중구난방으로 Edge가 추가가 된다.

**크루스칼 선행 알고리즘**

  그래프 알고리즘에 속하며
  서로소 집합 알고리즘이라고도 한다.

  - Union(합침)

    값이 작은 쪽으로 부모를 바꿈.

  - Find(탐색)

    같은 부모를 가지고 있는지, 즉 같은 그룹에 속해있는지 알려주는 함수
    
    **재귀를 사용함.**

    ```
    package Kruskal;

    public class Kruskal {

      public static int getParent(int Parent[], int x) { // 재귀적으로 부모 노드를 찾음
        if (Parent[x] == x) {
          return x; // 자기 자신이 부모인경우
        }
        return Parent[x] = getParent(Parent, Parent[x]); // 자기 자신이 부모가 아닌 경우 재귀적 호출
      }

      public static void unionParent(int parent[], int a, int b) { // 두 부모 노드를 합침
        a = getParent(parent, a);
        b = getParent(parent, b);
        if (a < b) { // 작은 쪽의 부모로 합치기
          parent[b] = a;

        } else {
          parent[a] = b;
        }
      }

      public static int findParent(int parent[], int a, int b) { // 같은 부모를 가지는지 확인
        a = getParent(parent, a);
        b = getParent(parent, b);
        if (a == b) {
          return 1; // 같은 부모
        }
        return 0; // 다른 부모
      }

      public static void main(String[] args) {
        int[] parent = new int[11];
        for (int i = 1; i <= 10; i++) { // 부모를 자기 자신의 갚으로 초기화
          parent[i] = i;
        }

        unionParent(parent, 1, 2);
        unionParent(parent, 2, 3);
        unionParent(parent, 3, 4);
        unionParent(parent, 5, 6);
        unionParent(parent, 6, 7);
        unionParent(parent, 7, 8);
        System.out.print("1과 5는 연결되어 있나요 " + findParent(parent, 1, 5));
      }
        }
    ```

    Union-Find를 통해 어떤 덩어리당 번호가 생기고 구별이 가능하다.

    노드에게 물어보면 어떤 덩어리에 속해있는지 매우 빠른 속도로 확인 가능

    **==> 같은 덩어리에 속하면(같은 부모를 가지고 있으면) 연결하지 않는다.**

- 정확성 증명(어렵다)

    답이 여러개일 수 있고, 그 중에 하나를 찾는다.


    - Invariant

        **$T_{mst}$가 MST 중 한 트리라고 하자. Kruskal이 만들어가는 답 $T$가 항상 $T\subseteq T_{mst}$임을 보이면 됨(항상 유지)**

    - Base : |T| = 0, 아무것도 안넣었을때는 공집합, 공집합은 당연히 부분집합 => OK

    - Step : |T| = k(원소가 k개)일때, T<=$T_{mst}$
    
    
    <img src="https://ifh.cc/g/q6O69g.png">

    Alg이 한번의 선택 진행

    $T'= T\cup\{e\}$

    case 1: $T' \subseteq T_{mst}$일 경우 성립한다.

    case 2: $T' \nsubseteq T_{mst}$일 경우, $e \notin T_{mst}$ 이다. ==> **다른 $T_{mst}$을 만족시킴을 보이면 된다.**

    **이 경우, $e$를 포함한 유일한 Cycle이 존재한다.** ==> $T_{mst}\cup\{e\}$ 에 존재한다.

    **Cycle에 e말고 not in T인 edge가 존재한다.**

    그 중 하나 아무거나를 $e'$이라고 하자.

    <img src="https://ifh.cc/g/lC6TGp.png">

    1) W(e) < W(e')

        $T{mst}$에서 $e$를 빼고 $e'$를 넣은 것이 더 좋은 답이다. => 모순

    2) W(e) > W(e')

        $e'$이 이미 T에 원소로 있었어야 함 ==> 모순

        

    3) W(e) == W(e')

    
        $T{mst}$ 에서 $e'$를 빼고 $e$를 더한 것도 정답이 된다.==> 또다른 $T{mst}$에 해당.

    
  
### Prim과 Kruskal can find ANY Solution


- Stable Sort Example

    Sort시에 원래 순서가 유지되어야 한다.

    <img src="https://ifh.cc/g/XFrSFQ.png">

    답이 여러개일때 절대로 못 찾는 답이 있을수 있다.

- Prim이든 Kruskal 이든 ANY(어떤) 답이든 찾을 수 있다. => 같은 입력에 대해서 정답이 여러개 인데 모든 답을 찾을 수 있다.

    돌릴때 마다 답이 다름.


- Proof(Prim에 대한 증명, Kruskal도 비슷)

    - Prim 알고리즘에게 $T{mst}$를 정해서 찾으라고 한다.(Fix)

    - T를 따라 답을 찾아가던 중 $T{mst}$에 속하지 않는 e를 넣었다 치면 ${e'}$을 찾게되고, e와 e'의 가중치가 같을때 아무거나 고르게 된다.

    **Prim을 잘 조절하면 절대 못 찾는 답은 없다.**

- 모든 Weight가 다르면 Solution이 하나 밖에 없다?

    <img src="https://ifh.cc/g/JXnlas.png">

    이 경우에 1,2를 포함하는 경우가 있으므로 둘다 답이 아니라는 것을 보일 수 있다.

- **모든 Weight가 다르면 Kruskal은 유일한 답을 찾는다.** ==> option이 없기 때문(선택권이 없다).

    모든 답을 찾을 수 있는데 Weight가 다르면 e, e' 둘중 선택하는 것 없이 유일한 답을 찾게 되므로 유일한 답을 찾는다.

    
### Dijkstra Algorithm

여러 가지 버전이 있다. 어려움. 제대로 이해하고 있는 사람 별로 없음.


- 노드마다 Shortest Path 길이만 찾는 버전

    <많이 직관적이지 않아서 어려움>

- 각 노드에 대해서 실제 Path까지 찾는 버전

- Path가 여러개 있을수 있어서 그 여러개를 다 찾는 버전


워낙 어려우니 다른 방향으로도 설명함()

- As a Variation of BFS

- Select from Nearest to Furthest

    
### Only length

<img src="https://ifh.cc/g/0DKcpq.jpg">

G : 그래프

n : 노드 개수

v(0) : 소스 노드

$d{min}$ : 답

<img src="https://ifh.cc/g/TOwkt6.png">

한 놈에 대해서 Shortest Path 찾으면 다른놈에 대해서도 찾을 수 있음

==> 소스가 하나일 때, 따라서 타겟을 하나로 정할 필요가 없다. 한 놈에 대해서 찾는 시간이나 다 찾는 시간이나 거의 비슷하다.

V(0) 기준으로 Direct Edge(인접한 엣지)를 따라 잠정적인 답을 적은 다음에 그 답을 점점 더 정확한 답으로 바꿔나감.


d(min) 값에다 넣는다.


**빨간 애들은 답을 찾은 애들이다.**

**파란 애들은 잠정적인 답을 가지고 있는 애들이다.(not in R)**

시작 할때는 소스 노드만 Red이다. 

**잠정적인 답 중 가장 작은것이 그대로 Red, 즉, 최종 답이 된다.**

==> 15로 가는 또 다른 통로는 19를 거쳐와야 하는데 이미 19이므로 15보다 크므로 안되기 때문

1) 파란색 중 가장 작은 값을 빨간색으로 바꾼다.

2) 방금 바꾼 빨간색을 포함해 파란 애들을 업데이트 시킨다.

<img src="https://ifh.cc/g/5wDTt2.jpg">

**왜 파란색 중 가장 작은 값을 바로 빨간색으로 바꿔도 되는가?**


귀류법으로 증명한다.

빨간 것만 거쳐서 파란색으로 최종으로 오는 것이

만약 15가 답이 아니라면(15보다 더 좋은 것이 있다면), 빨간 것에서 파란색으로 거쳐서 나오는 순간이 있고, 또 파란색으로 가는 순간이 반드시 있어야 한다. 







        










  






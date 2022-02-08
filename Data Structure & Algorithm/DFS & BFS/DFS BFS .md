### DFS : Depth- first Search , 깊이 우선 탐색
![image](https://user-images.githubusercontent.com/89775352/152966777-b3c2a1dd-456d-405f-97e5-1423a5de3d9c.png)

🛕노드/정점 (vertex) : 도시 ---------🚄간선(edge) : 도로)🚄----------:노드 : 도시 🏯

그래프는 2가지 방식으로 표현 가능하다.
- 인접 행렬(Adjacency Matrix) : 2차원의 배열로 표현
- 인접 리스트(Adjacency List) : 리스트로 표현 

#연결되어있지 않은 오드끼리는 무한의 비용이라고 작성한다.

INF=9999999999999999
#2차원 리스트를 이용해 인접 행렬 표현 
```
graph= [
    [0,7,5],
    [7,0,INF],
    [5,INF,0]
]
print(graph)
```
#인접리스트

#행이 3개인 2차원 리스트로 인접 리스트 표현 
```
graph=[[] for _ in range(3)]

#노드 0에 연결된 노드 저보 저장 (노드,거리)
graph[0].append((1,7))
graph[0].append((2,5)) #(2,5)행렬을 메소드에서 넣어주라

#노드 1에 연결된~
graph[1].append((0,7))

#노드 2에 연결된~
graph[2].append((0,5))

print(graph)
```

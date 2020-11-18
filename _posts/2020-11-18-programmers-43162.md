---
title: "[Javascript] 프로그래머스 네트워크"
toc: true
category:
- Algorithm
tag:
- algorithm
- javascript
- DFS
- BFS
---

[https://programmers.co.kr/learn/courses/30/lessons/43162](https://programmers.co.kr/learn/courses/30/lessons/43162)

## 문제 설명
네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다. 
예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가 직접적으로 연결되어 있을 때 컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다. 
따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.

컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때, 네트워크의 개수를 return 하도록 solution 함수를 작성하시오.

**제한사항**
- 컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.
- 각 컴퓨터는 0부터 n-1인 정수로 표현합니다.
- i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.
- computer[i][i]는 항상 1입니다.


**입출력 예**

| n |	computers |	return |
| -------- | -------- | -------- |
| 3 |	[[1, 1, 0], [1, 1, 0], [0, 0, 1]]	| 2 |
| 3 |	[[1, 1, 0], [1, 1, 1], [0, 1, 1]] | 1 |

## 문제 풀이
DFS와 BFS 로 풀 수 있는 문제이다.

### 해결 1 : DFS (재귀함수) 사용
```javascript
function solution(n, computers) {
    let visited = new Array(n).fill(false); // 방문여부 체크, 초기값 false
    let cnt = 0;
    
    for(let i = 0; i < n; i++) {
        if(!visited[i]) { // 방문하지 않은 경우
            cnt++;
            dfs(i);
        }
    }
    
    function dfs(i) {
        visited[i] = true; // i를 방문여부 true 체크
        for(let j = 0; j < n; j++) {
            if(computers[i][j] && !visited[j]) { // i와 연결된 j가 있을 경우
                    // visited[j]===false일 경우 "j!==i" 임
                dfs(j); // 재귀하며 방문체크
            }
        }
    }

    return cnt;
}
```

### 해결 2 : BFS (큐) 사용
```javascript
function solution(n, computers) {
    let visited = new Array(n).fill(false); // 방문여부 체크, 초기값 false
    let queue = []; // 방문할 노드 저장을 위한 큐
    let cnt = 0;
    
    for(let i = 0; i < n; i++) {
        if(visited[i]) continue; // 이미 방문한 노드일 경우 넘어간다.
        
        cnt++; // 네트워크 +1
        visited[i] = true; // i 방문여부 true로 체크
        queue.push(i); // 큐에 i를 넣어준다.
        
        while(queue.length > 0) { // 큐에 저장된 값이 있을 경우
            let v = queue.shift(); // 큐에서 첫 번째 값을 꺼내 v로 선언
            for(let j = 0; j < n; j++) {
                if(computers[v][j] && !visited[j]) { // v와 연결된 j가 있을 경우
                    // visited[j]===false일 경우, "j!==v" 임
                    visited[j] = true; // j 방문여부 true로 체크
                    queue.push(j); // 큐에 j를 넣어준다.
                }
            }
        }
    }

    return cnt;
}
```

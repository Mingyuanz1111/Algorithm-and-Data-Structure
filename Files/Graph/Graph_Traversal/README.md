# Graph Traversal
## Contents
* [Depth First Search (DFS)](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Graph/Graph_Traversal#depth-first-search-dfs)
* [Breadth First Search (BFS)](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Graph/Graph_Traversal#breadth-first-search-bfs)

มีอยู่ 2 Algorithm หลักๆ คือ Depth First Search (DFS), Breadth First Search (BFS)

## Depth First Search (DFS)
* เป็นการ Traversal โดยเริ่มจาก vertex หนึ่ง และ search พุ่งลึกไปใน graph เรื่อยๆ จนกว่าจะตัน  
  แล้วจึง return ย้อนกลับมา search vertex ก่อนหน้านี้ ไปเรื่อยๆ จนกว่าจะ visit ครบหมดทุก vertex
* วิธีที่ Implement ง่ายที่สุด คือ visit vertex เริ่มต้น, **mark visited ไว้หลังจาก visit**, แล้ว recursive แต่ละ vertex รอบๆ ที่ยังไม่ถูก visit
* มีอีกวิธีคือการใช้ Stack แต่มันยุ่งยากกว่าการ recursive วิธีนี้จะใช้แทนการ recursive ในกรณีที่ recursive แล้ว memory limit exceed  
  แต่โจทย์ส่วนใหญ่ให้ memory พอต่อการ recursive อยู่แล้ว
* DFS สามารถ ทำให้ graph ใดๆ เป็น Rooted Tree ตามทางที่ search ไปได้ด้วย Tree นั้นเรียกว่า DFS Tree

> Example: DFS on Graph
<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/3e8ad4fe-24b0-4456-81fd-ba4ba0434d6a" width="800">

```c++
vector<int> graph[n]; //adjacency list of graph
int visited[n];
void dfs(int cur){ //dfs on graph

    //code before visit adjacent vertices

    visited[cur]=1; //visit node
    for(auto v: graph[cur]) if(!visited[v]) dfs(v); //loop all adjacent vertices

    //code after visit adjacent vertices

    return; //can return value to previous vertex
}
```

* ถ้า DFS บน Tree ไม่จําเป็นต้องเก็บค่า visited ก็ได้ แต่ recursive โดยส่งต่อ vertex ของ parent แทน  
  เพราะ Tree ไม่มี cycle จึงมีแค่ node parent ที่เคย visit ไปแล้ว

> Example: DFS on Tree
<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/a7066d90-7cda-4a32-bf42-d15b6f19fecb" width="800">

```c++
vector<int> graph[n]; //adjacency list of tree (no cycle)
void dfs(int cur,int prev){ //dfs on tree

    // code before visit adjacent vertices

    // loop all adjacent vertices
    for(auto v: graph[cur]) if(v!=prev) dfs(v,cur); 

    // code after visit adjacent vertices

    return; // can return value to previous vertex
}
```

## Breadth First Search (BFS)
* เป็นการ Traversal โดยเริ่มจาก vertex หนึ่ง และ search ทีละ level ความลึกของ graph จากจุดเริ่มต้นไปเรื่อยๆ จนกว่าจะ visit ครบหมดทุก vertex
* วิธีที่ Implement คือ visit vertex เริ่มต้น แล้ว push แต่ละ vertex รอบๆ ที่ยังไม่ถูก visit เข้าไป ใน Queue  
  แล้ว **set ค่า visited หลังจาก push เข้าไปใน Queue** จากนั้นก็ pop ตัวหน้าสุดของ queue ออก มา BFS ต่อเรื่อยๆ  
  จนกว่า queue จะว่าง แปลว่า BFS ครบทุก vertex แล้ว
* BFS สามารถ ทำให้ graph ใดๆ เป็น Rooted Tree ตามทางที่ search ไปได้ด้วย Tree นั้นเรียกว่า BFS Tree

> Example: BFS on Graph
<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/da065f38-5083-45ce-9fd8-9b225dd42de3" width="800">

```c++
vector<int> graph[n]; //adjacency list of graph
int visited[n];
int main(){
    //start bfs
    queue<int> q;
    q.push(src); //start from vertex that will be the root of the bfs tree
    visited[src]=1; //visited in bfs means the vertex was added to the queue already
    while(!q.empty()){ //empty queue means visited all nodes already
        int cur=q.front(); //get vertex at the front of queue
        q.pop(); //pop that vertex
        //code for visiting vertex v
        for(auto v: graph[cur]) if(!visited[v]){
            visited[v]=1; //visit vertex when add to queue
            q.push(v); //add to queue
        }
    }
    return 0;
}
```

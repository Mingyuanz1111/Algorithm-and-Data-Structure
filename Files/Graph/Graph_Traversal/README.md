# การสำรวจกราฟ (Graph Traversal)
## Contents
* [Depth First Search (DFS)](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Graph/Graph_Traversal#depth-first-search-dfs)
* [Breadth First Search (BFS)](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Graph/Graph_Traversal#breadth-first-search-bfs)

มีอยู่ 2 Algorithm หลักๆ ได้แก่ การค้นหาตามแนวลึก (Depth First Search, DFS), การค้นหาตามแนวกว้าง (Breadth First Search, BFS)

## การค้นหาตามแนวลึก (Depth First Search, DFS)
* เป็นการสำรวจกราฟที่มีแนวคิดโดยเริ่มต้นจาก vertex หนึ่งและสำรวจพุ่งลึกไปใน graph เรื่อยๆ จนกว่าจะตัน  
  แล้วจึง backtrack ย้อนกลับมา vertex ก่อนหน้านี้ และสำรวจต่อไปเรื่อยๆไปยัง vertex ที่ยังไม่ถูกเยี่ยม จนกว่าจะเยี่ยมครบทุก vertex
  
* วิธี implement ที่ง่ายที่สุด คือการเขียนฟังก์ชัน recursive เพื่อเรียกใช้จาก vertex เริ่มต้นอันหนึ่ง แล้ว recursive ไปหา vertex อื่นๆต่อเรื่อยๆ

> Code
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

* ในที่นี้จะใช้ adjacency list เพื่อเก็บโครงสร้างกราฟ และ visited เป็น array ที่เก็บค่าว่าแต่ละ vertex ถูกเยี่ยมไปแล้วหรือยัง ซึ่ง `visited[i]` จะมีค่าเป็น `true` เมื่อ vertex ที่ `i` เคย**ถูกเรียกฟังก์ชันไปหาแล้ว**
* ขั้นตอนแรกของ algorithm คือต้องกำหนด vertex เริ่มต้นใดก็ได้ แล้วเริ่มเรียกใช้ฟังก์ชันจาก vertex เริ่มต้นนั้น
เมื่อฟังก์ชันสำรวจไปถึง vertex หนึ่ง (พารามิเตอร์ `cur` ของฟังก์ชัน) ในฟังก์ชันจะทำขั้นตอนเรียงตามนี้
1) **จดไว้ว่า vertex `cur` (vertex ปัจจุบัน) ถูกเยี่ยมไปแล้วทันที**
2) เรียก recursive แต่ละ vertex รอบๆ cur ที่ยังไม่เคยถูกเยี่ยม

ฟังก์ชันจะทำงานเสร็จเมื่อไม่มี vertex ใดให้สำรวจต่อแล้ว  
สังเกตว่าเมื่อจด vertex ปัจจุบันว่า visited ไว้ก่อนที่จะสำรวจต่อไป ทำให้ระหว่างสำรวจต่อ จะไม่มีการวนกลับมาที่ vertex เดิมซ้ำอีกแล้ว

* DFS สามารถ ทำให้ graph ใดๆ เป็น rooted tree ตามทางที่ search ไปได้ด้วย และ tree นั้นเรียกว่า DFS tree

> Example: DFS on Graph
<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/3e8ad4fe-24b0-4456-81fd-ba4ba0434d6a" width="800">


* ถ้า DFS บน tree ไม่จําเป็นต้องเก็บค่า visited ใน array ก็ได้ แต่ recursive โดยส่งต่อเลข vertex ของ parent แทน  
  เพราะ tree ไม่มี cycle เมื่อสำรวจไปถึง vertex หนึ่ง จะการันตีได้ว่ามีแค่ node parent ที่เคยเยี่ยมไปแล้ว ขั้นตอน recursive จึงเรียกฟังก์ชันต่อทุก vertex ข้างๆมัน ยกเว้น parent ของมัน

```c++
vector<int> graph[n]; //adjacency list of tree (no cycle)
void dfs(int cur,int prv){ //dfs on tree

    // code before visit adjacent vertices

    // loop all adjacent vertices
    for(auto v: graph[cur]) if(v!=prv) dfs(v,cur); 

    // code after visit adjacent vertices

    return; // can return value to previous vertex
}
```

> Example: DFS on Tree
<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/a7066d90-7cda-4a32-bf42-d15b6f19fecb" width="800">

* มีอีกวิธีคือการใช้โครงสร้างข้อมูล stack มาช่วยในการจัดลำดับการสำรวจ เนื่องจากโครงสร้างนี้มีลำดับเหมือนกับการ recursive แต่มันไม่สะดวกต่อการเขียนเท่าการ recursive วิธีนี้จึงใช้แทนการ recursive เฉพาะตอนที่ใช้ recursive แล้ว memory limit exceeded  
  แต่โจทย์ส่วนใหญ่ให้ memory พอสำหรับการ recursive อยู่แล้ว

## การค้นหาตามแนวกว้าง (Breadth First Search, BFS)
* เป็นการสำรวจกราฟโดยเริ่มต้นจาก vertex หนึ่ง และสำรวจทีละความลึกของกราฟนับจาก vertex เริ่มต้นไปเรื่อยๆ จนกว่าจะเยี่ยมครบหมดทุก vertex

* วิธี implement คือการใช้โครงสร้างข้อมูล queue มาช่วยในการจัดลำดับการสำรวจ โดย queue นี้จะเก็บลำดับของ vertex ที่รอทำการสำรวจอยู่ เมื่อต้องการสำรวจ vertex ต่อไป ให้สำรวจ vertex ที่อยู่ตรงหัว queue ก่อน แล้ว pop มันออก และเมื่อเจอ vertex ที่ยังไม่เคยเจอ ให้ push เข้าคิวเพื่อรอสำรวจต่อไป

> Code
```c++
vector<int> graph[n]; //adjacency list of graph
int visited[n];
int main(){
    //start bfs
    queue<int> que;
    que.push(src); //start from vertex that will be the root of the bfs tree
    visited[src]=1; //visited in bfs means the vertex was added to the queue already
    while(!q.empty()){ //empty queue means visited all nodes already
        int cur=que.front(); //get vertex at the front of queue
        que.pop(); //pop that vertex
        //code for visiting vertex v
        for(auto v: graph[cur]) if(!visited[v]){
            que.push(v); //add to queue
            visited[v]=1; //visit vertex when add to queue
        }
    }
    return 0;
}
```

* ในที่นี้จะใช้ adjacency list เพื่อเก็บโครงสร้างกราฟ และมี array `visited` เช่นกัน แต่จะนิยามไว้**ต่าง**จากของ Depth First Search ว่า ค่า `visited[i]` จะมีค่าเป็น `true` เมื่อ vertex ที่ `i` เคย**ถูกเพิ่มใส่ queue ไปแล้ว**
* ขั้นตอนแรกของ algorithm คือต้องกำหนด vertex เริ่มต้นใดก็ได้ แล้วนำ vertex เริ่มต้นนั้นไป push ใส่ queue และจดว่า visited แล้ว (เพิ่มใส่ใน queue แล้ว)

หลังจากนั้นจะใช้ while loop เพื่อทำขั้นตอนเรียงตามนี้ในการ loop แต่ละครั้ง :
1) นำเลข vertex ที่อยู่หัว queue มาใส่ตัวแปร `cur` เพื่อความง่ายต่อการใช้งาน
2) pop หัว queue ออก เนื่องจากกำลังจะสำรวจ vertex นี้แล้ว
3) push แต่ละ vertex รอบๆ `cur` ที่ยังไม่เคยถูก visit เข้าไปใน queue และ**จด vertex เหล่านี้ว่า visited ไว้ทันทีหลังจากเพิ่มเข้า queue**  

loop จะจบลงเมื่อ queue ว่าง เพราะมันหมายความว่าไม่มี vertex ใดให้สำรวจต่อแล้ว  
สังเกตว่าเมื่อจด vertex ที่ถูกเพิ่มเข้าไปใน queue ไว้ว่า visited แล้ว ก่อนที่จะสำรวจต่อไป ทำให้ระหว่างสำรวจต่อ จะไม่มีการเพิ่ม vertex เดิมเข้าไปใน queue ซ้ำอีกแล้ว

* BFS สามารถ ทำให้ graph ใดๆ เป็น Rooted Tree ตามทางที่ search ไปได้ด้วย Tree นั้นเรียกว่า BFS Tree

> Example: BFS on Graph
<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/da065f38-5083-45ce-9fd8-9b225dd42de3" width="800">


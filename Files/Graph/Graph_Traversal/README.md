# การสำรวจกราฟ (Graph Traversal)
## Contents
* [Depth First Search (DFS)](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Graph/Graph_Traversal#depth-first-search-dfs)
* [Breadth First Search (BFS)](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Graph/Graph_Traversal#breadth-first-search-bfs)

การสำรวจกราฟ คือการ"เยี่ยม"ทุกๆ vertex ของกราฟตามลำดับการสำรวจ โดยมักจะเริ่มจาก vertex เริ่มต้นบางจุดแล้วสำรวจ vertex ข้างๆไปเรื่อยๆจนกว่าจะครบ

การสำรวจกราฟมีอยู่ 2 Algorithm หลักๆ ได้แก่ การค้นหาตามแนวลึก (Depth First Search, DFS), การค้นหาตามแนวกว้าง (Breadth First Search, BFS)
ซึ่งต่างก็มีประโยชน์และสมบัติที่แตกต่างกัน

## การค้นหาตามแนวลึก (Depth First Search, DFS)
* เป็นการสำรวจกราฟที่มีแนวคิดโดยเริ่มต้นจาก vertex หนึ่งและสำรวจพุ่งลึกไปใน graph เรื่อยๆ จนกว่าจะตัน  
  แล้วจึง backtrack ย้อนกลับมา vertex ก่อนหน้านี้ และสำรวจต่อไปเรื่อยๆไปยัง vertex ที่ยังไม่ถูกเยี่ยม จนกว่าจะเยี่ยมครบทุก vertex
  
* วิธี implement ที่ง่ายที่สุด คือการเขียนฟังก์ชัน recursive เพื่อเรียกใช้จาก vertex เริ่มต้นอันหนึ่ง แล้ว recursive ไปหา vertex อื่นๆต่อเรื่อยๆ

> Code
ตัวอย่างการนำเข้าข้อมูลของกราฟ และสำรวจโดยใช้ DFS เริ่มจาก vertex ที่ 0
```c++
#define MAXN 200001
vector<int> graph[MAXN]; // adjacency list ของกราฟที่จะสำรวจ
bool visited[MAXN]; // มีค่าเป็น false ทุกช่อง

void dfs(int cur){ // ฟังก์ชัน DFS ที่เรียกเพื่อเยี่ยม vertex ที่ cur

    // จดว่า cur ถูกเยี่ยมแล้ว
    visited[cur]=true;

    // loop ทุก vertex รอบๆ cur ที่ยังไม่เคยถูกเยี่ยม เพื่อ recursive ไปเยี่ยมต่อ
    for(auto v: graph[cur]) if(!visited[v]) dfs(v);
}

int main(){
    // นำเข้าข้อมูลของกราฟ
    int n,m,u,v;
    cin >> n >> m; // กราฟมี n vertices, m edges
    for(int i=0; i<m; i++){
        cin >> u >> v;
        graph[u].push_back(v);
        graph[v].push_back(u);
    }

    // กำหนด vertex เริ่มต้นการสำรวจ
    int src=0;

    // เรียกฟังก์ชัน DFS เพื่อเริ่มสำรวจจากจุดเริ่มต้น
    dfs(src);

    return 0;
}
```

* ในที่นี้จะใช้ adjacency list เพื่อเก็บโครงสร้างกราฟ และ visited เป็น array ที่เก็บค่าว่าแต่ละ vertex ถูกเยี่ยมไปแล้วหรือยัง ซึ่ง `visited[i]` จะมีค่าเป็น `true` เมื่อ vertex ที่ `i` เคย**ถูกเรียกฟังก์ชันไปหาแล้ว**
* ขั้นตอนแรกของ algorithm คือต้องกำหนด vertex เริ่มต้นใดก็ได้ แล้วเริ่มเรียกใช้ฟังก์ชันจาก vertex เริ่มต้นนั้น
เมื่อฟังก์ชันสำรวจไปถึง vertex หนึ่ง (พารามิเตอร์ `cur` ของฟังก์ชัน) ในฟังก์ชันจะทำขั้นตอนเรียงตามนี้
1) **จดไว้ว่า vertex `cur` (vertex ปัจจุบัน) ถูกเยี่ยมไปแล้วทันที**
2) เรียก recursive แต่ละ vertex รอบๆ cur ที่ยังไม่เคยถูกเยี่ยม

เมื่อเยี่ยม vertex หนึ่ง หากไม่มี vertex รอบๆเลย หรือทุก vertex รอบๆถูกเยี่ยมหมดแล้ว จะไม่มีการ recursive ต่อ

เมื่อฟังก์ชันนี้ถูกเรียก recursive ไปเรื่อยๆ จะมี vertex ในกราฟที่ถูกเยี่ยมมากขึ้นเรื่อยๆ จนสุดท้าย ฟังก์ชันจะทำงานเสร็จเมื่อทุก vertex ที่ไปหาได้จากจุดเริ่มต้นนั้นถูกเยี่ยมหมดแล้ว
สังเกตว่าเมื่อจด vertex ปัจจุบันว่า visited ไว้ก่อนที่จะสำรวจต่อไป ทำให้ระหว่างสำรวจต่อ จะไม่มีการวนกลับมาที่ vertex เดิมซ้ำอีกแล้ว

* DFS สามารถ ทำให้ graph ใดๆ เป็น rooted tree ตามทางที่ search ไปได้ด้วย และ tree นั้นเรียกว่า DFS tree

> Example: DFS on Graph
<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/3e8ad4fe-24b0-4456-81fd-ba4ba0434d6a" width="800">


* ถ้า DFS บน tree ไม่จําเป็นต้องเก็บค่า visited ใน array ก็ได้ แต่ recursive โดยส่งต่อเลข vertex ของ parent แทน  
  เพราะ tree ไม่มี cycle ดังนั้นเมื่อสำรวจไปถึง vertex หนึ่ง จะการันตีได้ว่ามีแค่ node parent ที่เคยเยี่ยมไปแล้ว ขั้นตอน recursive จึงเรียกฟังก์ชันต่อทุก vertex ข้างๆมัน ยกเว้น parent ของมัน

> Code
ตัวอย่างการนำเข้าข้อมูลของกราฟต้นไม้ และสำรวจโดยใช้ DFS เริ่มจาก vertex ที่ 0
```c++
#define MAXN 200001
vector<int> graph[MAXN]; // adjacency list ของกราฟต้นไม้ที่จะสำรวจ
void dfs(int cur,int prv){  // ฟังก์ชัน DFS ที่เรียกเพื่อเยี่ยม vertex ที่ cur โดย vertex ก่อนหน้านี้คือ prv

    // loop ทุก vertex รอบๆ cur ยกเว้น prv เพื่อ recursive ไปเยี่ยมต่อ
    for(auto v: graph[cur]) if(v!=prv) dfs(v,cur);
}

int main(){
    // นำเข้าข้อมูลของกราฟต้นไม้
    int n,m,u,v;
    cin >> n >> m; // กราฟมี n vertices, m edges
    for(int i=0; i<n-1; i++){
        cin >> u >> v;
        graph[u].push_back(v);
        graph[v].push_back(u);
    }

    // กำหนด vertex เริ่มต้นการสำรวจ ซึ่งมองว่าเป็น root ของ tree ก็ได้
    int src=0;

    // เรียกฟังก์ชัน DFS เพื่อเริ่มสำรวจจากจุดเริ่มต้น
    dfs(src,src); // ให้พารามิเตอร์ prv มีค่าเป็น src ก็ได้ หมายความว่าไม่มี vertex ก่อนหน้ามันเลย

    return 0;
}
```

> Example: DFS on Tree
<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/a7066d90-7cda-4a32-bf42-d15b6f19fecb" width="800">

* มีอีกวิธีในการ DFS คือการใช้โครงสร้างข้อมูล stack มาช่วยในการจัดลำดับการสำรวจ เนื่องจากโครงสร้างนี้มีลำดับเหมือนกับการ recursive แต่มันไม่สะดวกต่อการเขียนเท่าการ recursive วิธีนี้จึงใช้แทนการ recursive เฉพาะตอนที่ใช้ recursive แล้ว memory limit exceeded  
  แต่โจทย์ส่วนใหญ่ให้ memory เพียงพอสำหรับการ recursive อยู่แล้ว

* สำหรับกราฟที่ไม่เชื่อมต่อกันทั้งหมด การสำรวจโดยเริ่มจาก vextex เริ่มต้นเพียงจุดเดียวอาจสำรวจได้ไม่ครบทั้งกราฟ เพราะอาจมีบาง vertex ที่เข้าถึงจากจุดเริ่มต้นไม่ได้
ดังนั้น กรณีต้องการ DFS สำรวจทุก vertex ในกราฟประเภทดังกล่าว ต้องทำการ loop ทุกๆ vertex เริ่มต้นที่เป็นไปได้ และตรวจสอบว่า vertex นั้นถูกเยี่ยมไปหรือยัง ถ้ายังไม่ถูกเยี่ยมก็ให้ทำการ DFS เริ่มจาก vertex นั้นเลย

```c++
for(int src=0; src<n; src++) if(!visited[src]){
    // เริ่มทำการ DFS โดยเริ่มจาก vertex ที่ src
}
```

## การค้นหาตามแนวกว้าง (Breadth First Search, BFS)
* เป็นการสำรวจกราฟโดยเริ่มต้นจาก vertex หนึ่ง และสำรวจทีละความลึกของกราฟนับจาก vertex เริ่มต้นไปเรื่อยๆ จนกว่าจะเยี่ยมครบหมดทุก vertex

* วิธี implement คือการใช้โครงสร้างข้อมูล queue มาช่วยในการจัดลำดับการสำรวจ โดย queue นี้จะเก็บลำดับของ vertex ที่**รอทำการสำรวจ**อยู่ เมื่อต้องการสำรวจ vertex ต่อไป ให้สำรวจ vertex ที่อยู่ตรงหัว queue ก่อน แล้ว pop มันออก และเมื่อเจอ vertex ที่ยังไม่เคยเจอ ให้ push เข้าคิวเพื่อรอสำรวจต่อไป

* ที่ทำเช่นนี้แล้วสามารถสำรวจทีละความลึกของกราฟได้ เพราะว่า  
เมื่อเริ่มเยี่ยมจากใน queue มีเพียง vertex ที่ความลึก 0 (จุดเริ่มต้น) ก่อน หลังจากที่เยี่ยม vertex เหล่านั้นเสร็จ ใน queue จะมีเพียง vertex ที่ความลึก 1  
หลังจากนั้นจะเริ่มเยี่ยม vertex ใน queue ที่มีเพียง vertex ที่ความลึก 1 หลังจากที่เยี่ยม vertex เหล่านั้นเสร็จ ใน queue จะมีเพียง vertex ที่ความลึก 2  
เป็นเช่นนี้ไปเรื่อยๆนั่นเอง

> Code
ตัวอย่างการนำเข้าข้อมูลของกราฟ และสำรวจโดยใช้ BFS เริ่มจาก vertex ที่ 0
```c++
#define MAXN 200001
vector<int> graph[MAXN]; // adjacency list ของกราฟที่จะสำรวจ
bool visited[MAXN]; // มีค่าเป็น false ทุกช่อง

int main(){
    // นำเข้าข้อมูลของกราฟ
    int n,m,u,v;
    cin >> n >> m; // กราฟมี n vertices, m edges
    for(int i=0; i<m; i++){
        cin >> u >> v;
        graph[u].push_back(v);
        graph[v].push_back(u);
    }

    // กำหนด vertex เริ่มต้นการสำรวจ
    int src=0;

    // เริ่มทำการ BFS
    queue<int> que;
    que.push(src); // นำ vertex เริ่มต้น push ใส่ใน queue
    visited[src]=1; // จดว่า cur ถูกเยี่ยมแล้ว

    while(!q.empty()){

        // นำเลข vertex ที่อยู่หัว queue มาใส่ตัวแปร cur และ pop มันออก
        int cur=que.front();
        que.pop();

        // loop ทุก vertex รอบๆ cur ที่ยังไม่ถูกเพิ่มเข้า queue แล้วทำการเพิ่มเข้า queue ต่อ
        for(auto v: graph[cur]) if(!visited[v]){
            que.push(v);
            visited[v]=true; // จดว่า ถูกเพิ่มใส่ queue ไปแล้ว
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

* สำหรับกราฟที่ไม่เชื่อมต่อกันทั้งหมด การสำรวจโดยเริ่มจาก vextex เริ่มต้นเพียงจุดเดียวอาจสำรวจได้ไม่ครบทั้งกราฟ เพราะอาจมีบาง vertex ที่เข้าถึงจากจุดเริ่มต้นไม่ได้
ดังนั้น กรณีต้องการ BFS สำรวจทุก vertex ในกราฟประเภทดังกล่าว ต้องทำการ loop ทุกๆ vertex เริ่มต้นที่เป็นไปได้ และตรวจสอบว่า vertex นั้นถูกเยี่ยมไปหรือยัง ถ้ายังไม่ถูกเยี่ยมก็ให้ทำการ BFS เริ่มจาก vertex นั้นเลย

```c++
for(int src=0; src<n; src++) if(!visited[src]){
    // เริ่มทำการ BFS โดยเริ่มจาก vertex ที่ src
}
```

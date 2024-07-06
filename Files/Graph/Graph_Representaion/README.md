# Graph Representaion
## Contents
* [Adjacency Matrix](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Graph/Graph_Representaion#adjacency-matrix)
* [Adjacency List](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Tree/Heap#adjacency-list)
* [Edge List](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Tree/Heap#edge-list)

มีอยู่ 3 แบบ คือ Adjacency Matrix, Adjacency List, Edge List

## Adjacency Matrix
* เก็บกราฟใน `int graph[n][n]`
  * โดยที่ `n` เป็นจำนวน vertex
  * `graph[i][j]` (row `i`, column `j`) แสดงถึง edge จาก vertex `i` ไป vertex `j` โดยค่าที่เก็บไว้จะเป็น weight ของ edge นั้น
* ถ้าไม่มี edge จาก `i` ไป `j` ให้ใส่ค่า `INF` (บางครั้งอาจจะใส่ค่า `0` แล้วแต่จะ implement)

> Example:
<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/8ad1554f-a053-4f2f-a1b4-17093c01bea6" width="800">

* ถ้าจะเก็บ Unweighted Graph ให้ใส่ค่าเป็น 1 เมื่อมี edge และใส่ 0 ถ้าไม่มี edge (ใช้ `bool` ได้เหมือนกัน)
* Undirected Graph จะมี Adjacency Matrix ที่สมมาตรเสมอ

## Adjacency List
* เก็บกราฟใน `vector<int> graph[n]` โดยที่ `n` เป็นจำนวน vertex
* `graph [i]` เป็น vector<int> ที่เก็บ edge ทั้งหมด ที่ชี้ออกจาก graph `i` โดยเก็บเป็น เลข vertex ที่ edge ไปหา

> Example: Unweighted Graph
<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/946f5f8e-09a3-4511-9a34-b3417e8ee259" width="800">

* ถ้าจะเก็บ weighted graph ให้ ใช้เป็น `vector<pair<int, int>>` ซึ่ง pair เก็บ `{เลข vertex ที่ไปนา, weight ของ edge นั้น}`

> Example: Weighted Graph
<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/64270f22-1c7f-4a73-aff8-12e253a3aef2" width="800">

* ถ้าจะเก็บ Undirected Graph ให้ใส่ edge เดียวกันไปทั้ง 2 ด้าน  
  เช่น ตอนจะเพิ่ม edge ที่เชื่อม `a` กับ `b` ให้ `graph[a].push_back (b), graph [b].push_back(a);`

## Edge List
* เก็บกราฟใน `vector<pair<int, int>> graph`
* `graph[i]` เป็น `pair<int, int>` ที่เก็บ edge หนึ่งของกราฟ  
  โดยเก็บเป็น `{เลข vertex ตันทาง, เลข vertex ที่ edge ไปหา}`

> Example: Unweighted Graph
<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/23b1a03b-8242-45fb-a74f-00d105310e94" width="800">

* ถ้าจะเก็บ weighted graph ให้ ใช้เป็น `vector<pair<pair<int, int>, int>>`  
  ซึ่ง pair เก็บ `{{เลข vertex ต้นทาง, เลข vertex ที่ไปนา}, weight ของ edge นั้น}`

> Example: Weighted Graph
<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/f675e4b4-4f79-4909-a81e-7a9434a6e4b5" width="800">

* สําหรับ Edge List ถ้าเก็บ undirected graph ก็ไม่จําเป็นต้องสนใจลำดับของ vertex ใน pair ก็ได้ แล้วแต่จะ Implement

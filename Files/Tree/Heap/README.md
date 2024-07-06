# Heap
## Contents
* [Structure](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Tree/Heap#structure)
* [Operation](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Tree/Heap#operation)

## Structure
* หลักๆแล้ว Heap มีอยู่ 2 แบบ คือ Max Heap และ Min Heap
* Max Heap คือ Binary Tree ที่แต่ละ node จะมีค่าของ child น้อยกว่า ค่าใน node นั้นเสมอ เพราะฉะนั้น **root** ของ Max Heap จะเป็น node ที่มีค่า**สูงสุด**ของใน Heap เสมอ
* Min Heap คือ Binary Tree ที่แต่ละ node จะมีค่าของ child มากกว่า ค่าใน node นั้นเสมอ เพราะฉะนั้น **root** ของ Min Heap จะเป็น node ที่มีค่า**ต่ำสุด**ของใน Heap เสมอ
* เราจะใช้ Heap เมื่อต้องการหาค่าที่มากน้อยที่สุดในข้อมูลที่มี ใน Heap พร้อมกับสามารถดึงค่าที่มาก/น้อยสุดออกจาก Heap หรือเพิ่มค่าใหม่เข้าไปใน Heap ได้ โดยใช้เวลาเพียง `O(nlogn)` และยังคงสมบัติ ของ Heap ได้อยู่
* Heap จึงเป็น Data Structure ที่ใช้ในการทำ Priority Queue ซึ่งก็คือ Queue ที่จะ pop ข้อมูลที่มี priority สูงสุดในข้อมูลที่ใส่ไปใน Queue นั้นออกมา  
  (จากที่ปกติจะเป็นข้อมูลตัวแรกสุดตามลำกับที่ใส่เข้าไปใน Queue)
* ปกติแล้ว การ Implement Heap จะใช้ [Binary Tree ในรูปแบบของ array](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Tree/Binary_Tree#array-based-structure-for-binary-tree)

<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/710dbbe6-bec7-49ba-a26d-926a87b7dc04" width="800">

## Operation

ในที่นี้จะยกตัวอย่าง Operation และ Code สำหรับ Max Heap
ชนิดของ Heap สามารถเปลี่ยนได้ที่ function `heapify`

### heapify
* ใช้ maintain สมบัติของ Heap ที่ตำแหน่งที่ `i` โดย subtree ของ **child ทั้งคู่ต้องเป็น Heap อยู่แล้ว**
* เป็น Operation พื้นฐานสำคัญของ Heap ที่สามารถนำไปใช้ใน Opertion อื่น ของ Heap ได้
* ทำได้โดย หา Node ที่มีค่าสูงที่สุดระหว่าง Node ที่ `i` และ `child ของมันทั้งคู่` (ถ้ามี)
  * ถ้า node ที่มีค่ามากสุด เป็น `child ของ node นั้น` ให้สลับ node ที่ `i` กับ `child นั้น` แล้ว heapify node ในตำแหน่ง `child นั้น` ต่อ
  * ถ้า node ที่มีค่ามากสุด เป็น node ที่ `i` เอง ก็หยุด Heapify ได้ เพราะมันแปลว่า operation ไปคงสมบัติ Heap เรียบร้อยแล้ว

> Example: Heapify at index `2`
<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/b287588d-bf5d-4218-a0f4-b3a380341bd4" width="800">

```c++
void heapify(int *heap,int sz,int i){
     // check empty heap
    if(sz==0) return;
    
    int l=i*2,r=i*2+1; // child nodes
     // find maximum value between index i l r
     // comparator can be changed here to change the type of heap
    int mx;
    if(l<=sz && heap[i]<heap[l]) mx=l;
    else mx=i;
    if(r<=sz && heap[mx]<heap[r]) mx=r;
    
     // if child node has maximum value
    if(mx!=i){
        int temp=heap[i]; // swap node with child
        heap[i]=heap[mx];
        heap[mx]=temp;
        heapify(heap,sz,mx); // heapify at swapped child node
    }
}
```

### build
* สร้าง Heap จาก array ที่ยังไม่เป็น Heap เลย
* ทำได้โดย Heapify ที่ internal node ตั้งแต่ ล่างขึ้นบน ก็คือ เริ่ม Heapify ที่ index n/2 (คือ internal node ที่ index มากที่สุด) จนถึง index ที่ 1

> Example: Build Heap from Array
<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/ea1d9fb7-b03b-42db-95a2-871612a9961e" width="1200">

```c++
void build(int *heap,int sz){
     // heapify all internal nodes from bottom to top
    for(int i=sz/2; i>0; i--) heapify(heap,sz,i);
}
```

### push (insert)
* เพิ่มค่า val เข้าไปใน Heap
* ทำโดย เพิ่มขนาด Heap ไปอีก `1` และ น่า `val` ไปที่ index ตำแหน่งสุดท้ายที่เพิ่งเพิ่มเข้ามา และถ้า node นั้น มีค่ามากกว่า parent ให้สลับ node นั้นกับ parent ไปเรื่อยๆจนกว่า parent จะมีค่ามากกว่า จึงหยุด loop

> Example: Insert value `21` into the heap
<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/a11727a9-28c0-420b-8bf1-939e573ac55d" width="800">

```c++
void push(int *heap,int *sz,int val){
     // increase size and add val to last node
    heap[++*sz]=val;
    
     // swap added node with its parent
     // until value of parent is greater than that node
    int i=*sz;
    while(i>1 && heap[i/2]<heap[i]){
        int temp=heap[i/2];
        heap[i/2]=heap[i];
        heap[i]=temp;
        i/=2;
    }
}
```

### pop (delete)
* ลบค่า ที่อยู่ที่ root ของ Heap ออก
* ทำได้โดยสลับ node ที่ root กับ node ที่ index ท้ายสุด และ ลดขนาด Heap ไปอีก 1 แล้ว Heapify ที่ root ของ Heap

> Example: Pop `top` value of the Heap
<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/8150fa6f-51a9-4fa8-a73c-f193c776cf28" width="800">

```c++
int pop(int *heap,int *sz){
     // check empty heap
    if(*sz==0) return 0;
    
     // put the last node to the top and decrease size
    int mx=heap[1];
    heap[1]=heap[(*sz)--];
     // heapify the top node
    heapify(heap,*sz,1);
     // return maximum value in heap
    return mx;
}
```

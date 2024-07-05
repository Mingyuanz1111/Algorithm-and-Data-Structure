# Heap
## Contents
* [Structure](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Tree/Heap#structure)
* [Operation](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Tree/Heap#operation)

## Structure
* หลักๆแล้ว Heap มีอยู่ 2 แบบ คือ Max Heap และ Min Heap
* Max Heap คือ Binary Tree ที่แต่ละ node จะมีค่าของ child น้อยกว่า ค่าใน node นั้นเสมอ เพราะฉะนั้น **root** ของ Max Heap จะเป็น node ที่มีค่า**สูงสุด**ของใน Heap เสมอ
* Min Heap คือ Binary Tree ที่แต่ละ node จะมีค่าของ child มากกว่า ค่าใน node นั้นเสมอ เพราะฉะนั้น **root** ของ Min Heap จะเป็น node ที่มีค่า**ต่ำสุด**ของใน Heap เสมอ
* เราจะใช้ Heap เมื่อต้องการหาค่าที่มากน้อยที่สุดในข้อมูลที่มี ใน Heap พร้อมกับสามารถดึงค่าที่มาก/น้อยสุดออกจาก Heap หรือเพิ่มค่าใหม่เข้าไปใน Heap ได้ โดยใช้เวลาเพียง `O(nlogn)` และยังคงสมบัติ ของ Heap ได้อยู่
* ปกติแล้ว การ Implement Heap จะใช้ [Binary Tree ในรูปแบบของ array](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Tree/Binary_Tree#array-based-structure-for-binary-tree)

## Operation
### heapify
* ใช้ maintain สมบัติของ Heap ที่ตำแหน่งที่ i โดย subtree ของ child ทั้งคู่ต้องเป็น Heap อยู่แล้ว
* เป็น Operation พื้นฐานสำคัญของ Heap ที่สามารถนำไปใช้ใน Opertion อื่น ของ Heap ได้
* ทำได้โดย หา Node ที่มีค่าสูงที่สุดระหว่าง Node ที่ i และ child ของมันทั้งคู่ (ถ้ามี)
  * ถ้า node ที่มีค่ามากสุด เป็น child ของ node นั้น ให้สลับ node ที่ i กับ child นั้น แล้ว heapify node ในตำแหน่ง child นั่นต่อ
  * ถ้า node ที่มีค่ามากสุด เป็น node ที่ i เอง ก็หยุด Heapify ได้ เพราะมันแปลว่า operation ไปคงสมบัติ Heap เรียบร้อยแล้ว


### build
* สร้าง Heap จาก array ที่ยังไม่เป็น Heap เลย
* ทำได้โดย Heapify ที่ internal node ตั้งแต่ ล่างขึ้นบน ก็คือ เริ่ม Heapify ที่ index n/2 (คือ internal node ที่ index มากที่สุด) จนถึง index ที่ 1

### insert
* เพิ่มค่า val เข้าไปใน Heap
* ทำโดย เพิ่มขนาด Heap ไปอีก 1 และ น่า val ไปที่ index ตำแหน่งสุดท้ายที่เพิ่งเพิ่มเข้ามา และถ้า node นั้น มีค่ามากกว่า parent ให้สลับ node นั้นกับ parent ไปเรื่อยๆจนกว่า parent จะมีค่ามากกว่า จึงหยุด loop

### delete
* ลบค่า ที่อยู่ที่ root ของ Heap ออก (และ return ค่านั้น)
* ทำได้โดยสลับ Node ที่ root กับ Node ที่ index ท้ายสุด และ ลดขนาด Heap ไปอีก 1 แล้ว Heapify ที่ root ของ Heap

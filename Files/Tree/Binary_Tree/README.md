# Binary_Tree
## Contents
* [Structure](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Tree/Binary_Tree#structure)
* [Operation](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Tree/Binary_Tree#operation)
* [Array-based Structure for Binary Tree](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Tree/Binary_Tree#array-based-structure-for-binary-tree)

## Structure

เป็น Rooted Tree ที่มี child ของ node ใดๆ เพียง 0 นิ้ว 2 node เท่านั้น

structure ของ node จึงเหมือนกับ Rooted Tree ธรรมดา แต่มีเพียง 2 pointer ที่ใช้ชี้ไปหา child

ในแต่ละ node ของ Tree จะเก็บ
* `key` ข้อมูลของ node นั้น
* `par` pointer ที่ชี้ไปหา parent node 
* `lchd` pointer ที่ชี้ไปหา left child
* `rchd` pointer ที่ชี้ไปหา right child

ถ้า node ไหน ไม่มี parent หรือ child ให้ใส่ค่า `NULL` ไป

```c++
struct node{
    int key;
    struct node *par,*lchd=NULL,*rchd=NULL;
};
```

<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/568a04c0-bfd9-452d-aff7-38cecda780ae" width="500">

## Operation

### createnode
สร้าง node ที่มีข้อมูล `key` และมี parent เป็น `par`
```c++
struct node* createnode(int key,struct node *par,int side){
    struct node *node=(struct node*)malloc(sizeof(struct node));
    node->key=key; //set key
    node->par=par; //set parent
    if(par!=NULL){
        if(side) par->rchd=node; //link child to parent in the corresponding side
        else par->lchd=node; // side=0 is left, side=1 is right
    }
    return node;
}
```

### depth
หาความลึกจาก root ของโหนดหนึ่งใน Tree
```c++
int depth(struct node* node){
    int dep=0; //start with 0 at the node
    while(node->par!=NULL){
        node=node->par;
        dep++;
    } //add depth until reach the root
    return dep;
}
```

### preorder
Tree Traversal โดย visit node นั้นก่อน แล้วค่อย Traversal children ต่อ
```c++
void preorder(struct node* node){
    //code for visiting node here
    preorder(node->lchd); //preorder left child
    preorder(node->rchd); //preorder right child
}
```

### postorder
Tree Traversal โดย Traversal children ก่อน แล้วค่อย visit node นั้นทีหลัง
```c++
void postorder(struct node* node){
    postorder(node->lchd); //postorder left child
    postorder(node->rchd); //postorder right child
    //code for visiting node here
}
```

### inorder
Tree Traversal โดย Traversal Left Child ก่อน แล้วค่อย visit node นั้น แล้วต่อไปก็ Traversal Right Child
```c++
void inorder(struct node* node){
    inorder(node->lchd); //inorder left child
    //code for visiting node here
    inorder(node->rchd); //inorder right child
}
```

# Array-based Structure for Binary Tree

* Binary Tree สามารถเก็บใน array ได้ โดย root ของ Tree อยู่ที่ index `1`

* left child ของ node ที่ index `i` คือ node ที่ `2*i`

* right child ของ node ที่ index `i` คือ node ที่ `2*i+1`

* parent ของ node ที่ index `i` คือ node ที่ `i/2`

* full binary tree ที่มีขนาด ขนาด n node จึงต้องเก็บใน array ขนาด `n+1` และมีความสูง `⌊log2(n)⌋`

<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/baeaca39-632d-4066-ab0b-b51376f8dc72" width="500">

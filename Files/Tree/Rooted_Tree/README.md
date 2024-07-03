# Rooted_Tree
## Contents
* [Structure](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Tree/Rooted_Tree#structure)
* [Operation](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Tree/Rooted_Tree#operation)

## Structure

ในแต่ละ node ของ Tree จะเก็บ
* `key` ข้อมูลของ node นั้น
* `par` pointer ที่ชี้ไปหา parent node 
* `chd` container (vector) ที่เก็บ pointer ที่ชี้ไปหา child node ทั้งหมด

root ของ Tree จะมี `parent = NULL` และ leaf ของ Tree จะมี `chd.empty() = true`

```c++
struct node{
    int key;
    struct node *par;
    vector<struct node*> chd;
};
```

<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/ca783d69-aacc-4067-86b9-2b271b26a752" width="400">

## Operation

### createnode
สร้าง node ที่มีข้อมูล `key` และมี parent เป็น `par`
```c++
struct node* createnode(int key,struct node *par){
    struct node *node=new struct node; //only use new because struct has stl container
    node->key=key; //set key
    node->par=par; //set parent
    if(par!=NULL) par->chd.push_back(node); //link child to parent
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
        dep++; //add depth until reach the root
    }
    return dep;
}
```

### preorder
Tree Traversal โดย visit node นั้นก่อน แล้วค่อย Traversal children ต่อ
```c++
void preorder(struct node* node){
    //code for visiting node here
    for(auto ch: node->chd) preorder(ch); //preorder all child
}
```

### postorder
Tree Traversal โดย Traversal children ก่อน แล้วค่อย visit node นั้นทีหลัง
```c++
void postorder(struct node* node){
    for(auto ch: node->chd) postorder(ch); //postorder all child
    //code for visiting node here
}
```

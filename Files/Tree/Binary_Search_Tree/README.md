# Binary Search Tree
## Contents
* [Properties](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Tree/Binary_Search_Tree#properties)
* [Operation](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Tree/Binary_Search_Tree#operation)

## Properties

เป็น [Binary Tree](https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/tree/main/Files/Tree/Binary_Tree) ที่ node ใดๆ
* มีค่าของทุก node ใน subtree ของ left child **น้อยกว่า** ค่าของ node นั้น
* มีค่าของทุก node ใน subtree ของ right child **มากกว่า** ค่าของ node นั้น
* Binary Search Tree จึงใช้ในการ Binary Search หาข้อมูลหลายรอบ ในระหว่างที่มีการเพิ่มหรือลบข้อมูลอยู่บ่อยๆ
* ถ้า Inorder Traversal กับ Binary Search Tree จะได้ลำดับที่ sort แล้วออกมา

<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/2e864b76-e977-4787-a2ec-11993b16afb5" width="500">

## Operation

### maximum
ใช้หา node ที่มี `key` มากทีสุด ใน subtree ของ node นั้น  
หาโดย loop node ไปทาง **right child** จนกว่าจะมี right child เท่ากับ `NULL`
```c++
struct node* maximum(struct node *node){
    while(node->rchd!=NULL) node=node->rchd; //loop right child until NULL
    return node;
}
```

### minimum
ใช้หา node ที่มี `key` น้อยทีสุด ใน subtree ของ node นั้น  
หาโดย loop node ไปทาง **left child** จนกว่าจะมี left child เท่ากับ `NULL`
```c++
struct node* minimum(struct node *node){
    while(node->lchd!=NULL) node=node->lchd; //loop left child until NULL
    return node;
}
```

### searchtree
ใช้หา node ที่มี `key` ตามที่ต้องการจาก node ที่รับมา  
ถ้า key ที่ต้องการมีค่า**น้อยกว่า** `key` ของ node นั้น ให้ search ไปทาง**ซ้าย**  
ถ้า key ที่ต้องการมีค่า **มากกว่า** `key` ของ node นั่น ให้ search ไปทาง**ขวา**

> Example: Search for key 31

<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/292a2d06-3781-405b-a6ae-06b9e0668f6e" width="800">

```c++
struct node* searchtree(int key,struct node *node){
    while(node!=NULL && key!=node->key){ //loop until found node or NULL
        if(key<node->key) node=node->lchd; //search left
        else if(key>node->key) node=node->rchd; //search right
    }
    return node;
}
```

### successor
ใช้หา **successor** ของ node หนึ่ง ซึ่งก็คือ node ที่มี `key` **น้อยที่สุด** ที่มี `key` **มากกว่า** node นั้น  
ทำได้โดยหา node ที่มี `key` น้อยที่สุด ของ right subtree  
แต่ถ้าไม่มี right child ให้ loop หา ancestor ของ node นั้น ตัวแรกที่เจอที่มี `key` มากกว่า node นั้น คือ successor

> Example:

<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/3556f86e-df71-40f0-a51c-9970b2f1875b" width="500">

```c++
struct node* successor(struct node *node){
    //if have right child, find minimum node of right subtree
    if(node->rchd!=NULL) return minimum(node->rchd);
    //if not, loop ancestor and node to find one that the node is left child of ancestor
    struct node *ancestor=node->par;
    while(ancestor!=NULL && node==ancestor->rchd){
        node=ancestor;
        ancestor=node->par;
    }
    return ancestor;
}
```

### predecessor
ใช้หา **predecessor** ของ node หนึ่ง ซึ่งก็คือ node ที่มี `key` **มากที่สุด** ที่มี `key` **น้อยกว่า** node นั้น  
ทำได้โดยหา node ที่มี `key` มากที่สุด ของ left subtree  
แต่ถ้าไม่มี left child ให้ loop หา ancestor ของ node นั้น ตัวแรกที่เจอที่มี `key` น้อยกว่า node นั้น คือ predecessor

> Example:

<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/5bc3c7dd-cac0-4cf2-b0ce-451d37a03670" width="500">

```c++
struct node* predecessor(struct node *node){
    //if have left child, find maximum node of right subtree
    if(node->lchd!=NULL) return maximum(node->lchd);
    //if not, loop ancestor and node to find one that the node is left child of ancestor
    struct node *ancestor=node->par;
    while(ancestor!=NULL && node==ancestor->lchd){
        node=ancestor;
        ancestor=node->par;
    }
    return ancestor;
}
```

### insertnode
ใช้เพิ่ม node ที่มี `key` ที่ต้องการเพิ่มใน Tree ทำโดยใช้วิธีคล้ายกับการ search หาค่า  
แต่มีตัวแปลที่เก็บ parent ไล่ตามหลัง node ที่ search อยู่ด้วย  
ถ้า search ไปถึง `NULL` ให้เพิ่ม node เป็น child ของ parent ที่ตามหลังอยู่ตอนนั้น

> Example: Insert key 25

<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/45f3cd57-58ea-499e-8184-31dcf8fe85de" width="1200">

```c++
struct node* insertnode(int key,struct node *node){
    struct node *par=NULL; //this will follow the node down until the node is NULL
    while(node!=NULL){ //find the correct position for the new node
        par=node;
        if(key<=node->key) node=node->lchd;
        else node=node->rchd;
    } //when node is NULL, it is at the correct position
    node=createnode(key,par,(par==NULL || key<=par->key)?0:1); //add node to the parent
    return node;
}
```

### deletenode
ใช้ลบ node ที่มีค่า `key` นั้น ออกจาก tree และ return root node ตัวใหม่ (ถ้า root เปลี่ยน)  
ทำโดย recursive function ไปทางซ้ายและขวาเหมือนกับการ search  
และรอ update child จากการ return node กลับมาหลังการ recursive ต่อ (รอดูใน code น่าจะเข้าใจกว่า)  
ถ้าเจอ node ที่มีค่า `key` นั้นแล้ว ให้ check ทั้ง 3 case นี้ :  
  
#### Case 1: node ที่จะลบ ไม่มี child เลย
ให้ free pointer ของ node นั้นและ return ค่า NULL ให้กับ parent

> Example: Delete key 30

<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/4c781540-6bf6-46b9-8fe7-6c5272def7ab" width="1200">

#### Case 2: node ที่จะลบ มี child เพียง node เดียว

ให้ set ค่า parent ของ child นั้น เป็น parent ของ node ที่จะลบ (link ข้าม node ที่จะลบไป)  
แล้วทำการ free pointer ของ node ที่จะลบ  
แล้ว return pointer ของ child ให้กับ parent ใหม่ที่เพิ่ง set ค่าไป  

> Example: Delete key 5

<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/640a17aa-2b9e-426e-9852-e4743ea34ad3" width="1200">
   
> Example: Delete key 44

<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/4239bc7b-ee9f-42a1-bbfc-58d3988d1a86" width="1200">

#### Case 3 : node ที่จะลบ มี child 2 ตัว

ให้หา successor ของ node นั่น  
, set `key` ของ node นั้น เป็น `key` ของ successor  
, link parent ของ successor กับ child ของ successor (link ข้าม successor node ไป)  
, free pointer ของ successor แล้ว return node ที่เจอตอนแรก  

> Example: Delete key 12

<img src="https://github.com/Mingyuanz1111/Algorithm-and-Data-Structure/assets/174484621/8d3e04ed-f483-4689-aee4-dd79ed62d9c1" width="1200">

```c++
struct node *deletenode(int key,struct node *node){
    if(node==NULL) return NULL; //check for NULL node
    //find node with the key
    if(k<node->key){
        node->lchd=deletenode(key,node->lchd); //recursive to left child and update child
        return node;
    } else if(k>node->key){
        node->rchd=deletenode(key,node->rchd); //recursive to right child and update child
        return node;
    }
    //found node with the key
    if(node->rchd==NULL && node->lchd==NULL){ //if doesn't have both left and right child
        free(node); //delete the node
        return NULL; //update child of parent to NULL
    } else if(node->rchd==NULL){ //if doesn't have right child
        struct node *child=node->lchd;
        child->par=node->par; //update parent of child
        free(node); //delete the node
        return child; //update child link of the parent node after return
    } else if(node->lchd==NULL){ //if doesn't have left child
        struct node *child=node->rchd;
        child->par=node->par; //update parent of child
        free(node); //delete the node
        return child; //update child link of the parent node after return
    } else{ //if have both left and right child
        struct node *succ=node->rchd; //find successor of the node
        while(succ->lchd!=NULL) succ=succ->lchd;
        node->key=succ->key; //set key of the node to key of the successor
        //update parent and child of successor
        if(succ->par==node){ //this case happens when right child of the node has no left child
            node->rchd=succ->rchd;
            succ->rchd->par=node;
        } else{
            succ->par->lchd=succ->rchd;
            succ->lchd->par=succ->par;
        }
        free(succ); //delete the successor
        return node;
    }
}
```

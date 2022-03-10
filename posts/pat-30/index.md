# PAT-Advanced 堆



算法笔记 - Heap
<!--more-->

## 堆的自上而下的调整
```c++
void downadjust(int low, int high){
    int i = low, j = i*2;   //和两个子节点比较
    while(j <= high){
        if(j+1 <= high && heap[j+1] > heap[j]){
            j = j+1;    //挑选最大的子节点
        }
        if(heap[j] > heap[i]){
            wap(heap[j], heap[i]);
            i = j;
            j = i*2;    //交换,并递推处理交换的结点
        }
        else{
            break;
        }
    }
}
```

## 建堆
```c++
void createheap(){
    for(int i=n/2; i >= 1; i--){
        downadjust(i, n);
    }
}
```

## 删除堆顶元素
```c++
void deletetop(){
    heap[1] = heap[n--];
    downadjust(1, n);
}
```

## 向上调整
```c++
void upadjust(int low, int high){
    int i = high, j = i/2;  //和父节点比较
    while(j >= low){
        if(heap[j] < heap[i]){
            swap(heap[j], heap[i]);
            i = j;
            j = i/2;    //递推交换后的父节点
        }
        else{
            break;
        }
    }
}
```

## 添加元素
```c++
void insert(int x){
    heap[n++];
    upadjust(1, n);
}
```


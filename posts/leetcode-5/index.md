# Leetcode Part-5



链队列的实现方法<br>
102-二叉树的层序遍历
<!--more-->

## 链队列的实现方法
做题经常会遇到要自己写一个链队列,在此记录一下
 - struct QueueNode
 - struct LinkQueue
 - initQueue
 - enqueue
 - dequeue
 - destroyQueue
 - emptyQueue
一定要熟背鸭
```c
typedef struct QueueNode {
    struct TreeNode *data;
    struct QueueNode *next;
} QueueNode_t;

typedef struct LinkQueue {
    int count;
    QueueNode_t *front;
    QueueNode_t *rear;
} LinkQueue_t;

int initQueue(LinkQueue_t *queue) {
    queue->front = queue->rear = malloc(sizeof(QueueNode_t));
    if (NULL == queue->front)
        return -1;
    queue->front->next = NULL;
    queue->count = 0;
    return 0;
}

int enqueue(LinkQueue_t *queue, struct TreeNode *data) {
    QueueNode_t *newNode = malloc(sizeof(QueueNode_t));
    if (NULL == newNode) {
        return -1;
    }

    newNode->data = data;
    newNode->next = NULL;
    queue->rear->next = newNode;
    queue->rear = newNode;
    queue->count++;

    return 0;
}

int dequeue(LinkQueue_t *queue, struct TreeNode **data) {
    if (queue->front == queue->rear) {
        return -1;
    }

    QueueNode_t *denode = queue->front->next;
    *data = denode->data;
    queue->front->next = denode->next;

    if (denode == queue->rear) {
        queue->rear = queue->front;
    }

    free(denode);
    queue->count--;
    return 0;
}

void destroyQueue(LinkQueue_t *queue) {
    /*q->front 指向头node, queue->rear指向下一个节点，这里当临时指针用*/
    while (queue->front) {
        queue->rear = queue->front->next;
        free(queue->front);
        queue->front = queue->rear;
    }
}

int emptyQueue(LinkQueue_t *queue) {
    return queue->front == queue->rear ? 1 : 0;
}
```

## 102-二叉树的层序遍历
题目:给你一个二叉树，请你返回其按层序遍历得到的节点值。即逐层地，从左到右访问所有节点
解题思路:目前想到的是记录每一层的结点数,然后while(size--)循环,每次层节点访问后输出,本质就是广度优先搜索
以下题解使用顺序队列实现,这道题是中等题,对于我来说写出完整实现有些困难,但大体思想知道,要继续锻炼代码能力了
```c
#define MAX_SIZE 1000

typedef struct {
        int front;
        int rear;
        int size;
        struct TreeNode* data[MAX_SIZE];
} Queue;

void Init(Queue *q)
{
        q->front = -1;
        q->rear = -1;
        q->size = 0;
        memset(q->data, 0, sizeof(struct TreeNode*) * MAX_SIZE);
}

int Push(Queue *q, struct TreeNode* node)
{
        assert(q && node && q->size != MAX_SIZE);
        if (q == NULL || node == NULL || q->size == MAX_SIZE)
                return -1;

        q->rear++;
        q->rear %= MAX_SIZE;
        q->data[q->rear] = node;
        q->size++;
        return 0;
}

int Pop(Queue *q, struct TreeNode** node)
{
        assert(q && node && q->size != 0);
        if (q == NULL || node == NULL || q->size == 0)
                return -1;
        q->front++;
        q->front %= MAX_SIZE;
        *node = q->data[q->front];
        q->size--;
        return 0;
}

int GetTreeDepth(const struct TreeNode* root)
{
        if (root == NULL)
                return 0;

        int left = GetTreeDepth(root->left);
        int right = GetTreeDepth(root->right);

        return left > right ? left + 1 : right + 1;
}

int** levelOrder(struct TreeNode* root, int* returnSize, int** returnColumnSizes)
{
        if (root == NULL || returnSize == NULL || returnColumnSizes == NULL) {
            *returnSize = 0;
            *returnColumnSizes = (int *)malloc(sizeof(int) * 1);
            (*returnColumnSizes)[0] = 0;                
            return NULL;
        }
            
        Queue q;
        Init(&q);
        Push(&q, root);

        int depth = GetTreeDepth(root);
        *returnSize = depth;
        int **matrix = (int **)malloc(sizeof(int *) * depth);
        *returnColumnSizes = (int *)malloc(sizeof(int) * depth);
        if (matrix == NULL || returnColumnSizes == NULL)
                return NULL;

        int cur_depth = 0;
        while(q.size != 0) {
                int level_size = q.size;
                /* create raw */
                (*returnColumnSizes)[cur_depth] = level_size;
                matrix[cur_depth] = (int *)malloc(sizeof(int) * level_size);
                int cur = 0;
                while (level_size--) {
                        struct TreeNode* node;
                        Pop(&q, &node);
                        /* add node->val to res */
                        matrix[cur_depth][cur] = node->val;

                        if (node->left)
                                Push(&q, node->left);
                        if (node->right)
                                Push(&q, node->right);
                        cur++;
                }
                cur_depth++;
        }
        return matrix;
}
```


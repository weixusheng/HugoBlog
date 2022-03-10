# PAT-Advanced 41-44


1041-1044
<!--more-->

# 1041-Be Unique
快乐hash
```c++
#include<set>
#include<vector>
#include<iostream>

using namespace std;

int main(int argc, char const *argv[])
{
    int num;
    cin >> num;
    set<int> s1;
    vector<int> data(num);
    int shit[100000] = {0};
    int tmp;
    for(int i=0; i<num; i++){
        cin >> tmp;
        data[i] = tmp;
        shit[tmp]++;
    }
    int j;
    for(j=0; j<num; j++){
        if(shit[data[j]] == 1){
            printf("%d", data[j]);
            break;
        }   
    }
    if(j == num){
        printf("None");
    }
    return 0;
}
```

# 1042-Shuffling Machine
打扰了,这题没看懂啥意思
```c++
#include <cstdio>
using namespace std;
int main() {
    int cnt;
    scanf("%d", &cnt);
    int start[55], end[55], scan[55];
    for(int i=1; i<55; i++) {
        scanf("%d", &scan[i]);
        end[i] =i;
    }
    for(int i=0; i<cnt; i++) {
        for(int j=1; j<55; j++)
            start[j] =end[j];
        for(int k=1; k<55; k++)
            end[scan[k]] =start[k];
    }
    char c[6] = {"SHCDJ"};
    for(int i=1; i<55; i++) {
        end[i] =end[i] -1;
        printf("%c%d", c[end[i]/13], end[i]%13+1);
        if(i!=54) 
            printf(" ");    
    }
    return 0;
}
```

# 1043-Is It a Binary Search Tree
这道题考察二叉搜索树,并且让我引发了一些思考
对于给定树(搜索树)的遍历序列,可以直接建树,之后进行树的遍历
而如果直接对遍历序列进行操作,就会...没啥思路
并且搜索树的镜像树,就是对左右树的访问次序颠倒即可
```c++
#include<iostream>
#include<vector>

using namespace std;

struct node{
    int data;
    node *left, *right;
};

void insert(node* &root, int data){
    if(root == NULL){
        root = new node;
        root->data = data;
        root->left = root->right = NULL;
        return;
    }
    if(data < root->data){
        insert(root->left, data);
    }
    else{
        insert(root->right, data);
    }
}

void preorder(node* root, vector<int> &vi){
    if(root == NULL){
        return;
    }
    vi.push_back(root->data);
    preorder(root->left, vi);
    preorder(root->right, vi);
}

void preorder_mirro(node* root, vector<int> &vi){
    if(root == NULL){
        return;
    }
    vi.push_back(root->data);
    preorder_mirro(root->right, vi);
    preorder_mirro(root->left, vi);
}

void posterorder(node* &root, vector<int> &vi){
    if(root == NULL){
        return;
    }
    posterorder(root->left, vi);
    posterorder(root->right, vi);
    vi.push_back(root->data);
}

void posterorder_mirro(node* &root, vector<int> &vi){
    if(root == NULL){
        return;
    }
    posterorder_mirro(root->right, vi);
    posterorder_mirro(root->left, vi);
    vi.push_back(root->data);
}

vector<int> origin, pre, prem, post, postm;

int main(int argc, char const *argv[])
{
    int n,data;
    node* root = NULL;
    scanf("%d", &n);
    for(int i=0; i<n; i++){
        scanf("%d", &data);
        origin.push_back(data);
        insert(root, data);
    }
    preorder(root, pre);
    preorder_mirro(root, prem);
    if(origin == pre){
        printf("YES\n");
        posterorder(root, post);
        for(int i=0; i<post.size(); i++){
            printf("%d", post[i]);
            if(i < post.size()-1){
                printf(" ");
            }
        }
    }
    else if(origin == prem){
        printf("YES\n");
        posterorder_mirro(root, postm);
        for(int i=0; i<postm.size(); i++){
            printf("%d", postm[i]);
            if(i < postm.size()-1){
                printf(" ");
            }
        }
    }
    else{
        printf("NO\n");
    }
    return 0;
}
```

# 1044-Shopping in Mars
这道题需要解决的问题是,查找相加恰好为一定值的序列
即可构造一个递增的相加值序列,而对于这种递增序列查找某一符合的值,就可以用二分法
真的没想到二分法还能这么神奇...太强了
我爱甲级压轴题...(哭)
```c++
#include<iostream>
#include<vector>
using namespace std;

vector<int> ans, sum;
int n, m, tmpsum;

int func(int i, int &j){
    int left = i;
    int right = n;
    while(left < right){
        int mid = (left+right)/2;
        if(sum[mid]-sum[i-1] >= m){
            right = mid;
        }
        else{
            left = mid+1;
        }
    }
    j = right;
    int tsum = sum[j]-sum[i-1];
    return tsum;
}

int main(int argc, char const *argv[])
{
    scanf("%d %d", &n, &m);
    sum.resize(n+1);
    for(int i=0; i<n; i++){
        scanf("%d", &sum[i]);
        sum[i] += sum[i-1];
    }
    int maxshit = sum[n];
    for(int i=1; i<=n; i++){
        int j;
        tmpsum = func(i, j);
        if(tmpsum > maxshit){
            continue;
        }
        else if(tmpsum >= m){
            if(tmpsum < maxshit){
                ans.clear();
                maxshit = tmpsum;
            }
            ans.push_back(i);
            ans.push_back(j);
        }
    }
    for(int j=0; j<ans.size(); j+=2){
        printf("%d-%d\n", ans[j],ans[j+1]);
    }
    return 0;
}
```

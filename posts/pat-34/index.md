# PAT-Advanced 21



1021
<!--more-->

# 1021-Deepest Root
这道题主要解决思路为 图的深度遍历的应用-找到连通图.找到最高树
这道题感觉算是稍微吃透了
数组对于路径的保存有极好的作用,这里利用tmp保存尾端符合条件(maxheight边界条件)的尾端结点,在dijkstra也有类似的应用
```c++
#include<set>
#include<iostream>
#include<vector>
using namespace std;

vector<vector<int> > v;
bool shit[10010];
set<int> res;
vector<int> tmp;
int maxheight = 0, n;

void dfs(int node, int height){
    if(height > maxheight){
        tmp.clear();
        tmp.push_back(node);
        maxheight = height;
    }
    else if(height == maxheight){
        tmp.push_back(node);
    }
    shit[node] = true;
    for(int i=0; i<v[node].size(); i++){
        if(shit[v[node][i]] == false){
            dfs(v[node][i], height+1);
        }
    }
}

int main(int argc, char const *argv[])
{
    scanf("%d", &n);
    v.resize(n+1);
    int a, b, cnt=0, s;
    for(int i=0; i<n; i++){     //插入数据
        scanf("%d%d", &a, &b);
        v[a].push_back(b);
        v[b].push_back(a);
    }
    for(int i=1; i<=n; i++){
        if(shit[i] == false){
            dfs(i, 1);
            if(i == 1){
                if(tmp.size() != 0){
                    s = tmp[0];
                }
                for(int j=0; j<tmp.size(); j++){
                    res.insert(tmp[j]);
                }
            }
            cnt++;
        }
        if(cnt >= 2){
            printf("Error: %d components", cnt);
        }
        else{
            tmp.clear();
            maxheight = 0;
            fill(shit, shit+10010, false);
            dfs(s, 1);
            for(int i=0; i<tmp.size(); i++){
                res.insert(tmp[i]);
            }
            for(auto it = res.begin(); it != res.end(); it++){
                printf("%d\n", *it);
            }
        }
    }
    return 0;
}
```



# Leetcode Part-28



昨天刷了一些树的题,今天来刷图

<!--more-->

# 207(210).课程表
考察基本拓扑排序的解决,以及拓扑路径的寻找
判定拓扑排序的方法就是,寻找有向图中是否存在环,对于拓扑路径的寻找,有dfs和bfs两种
对于dfs 需要用到栈以及记录节点状态
对于bfs 需要用到队列,记录节点的出入度
**建议使用bfs,队列实现,思路清晰**
## bfs
```c++
class Solution {
private:
    vector<int> indig;
    vector<vector<int>> edges;
    vector<int> res;

public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        
        indig.resize(numCourses);
        edges.resize(numCourses);
        for(int i=0; i<prerequisites.size(); ++i){
            edges[prerequisites[i][1]].push_back(prerequisites[i][0]);
            ++indig[prerequisites[i][0]];
        }
        queue<int> q;
        for(int i=0; i<numCourses; ++i){
            if(indig[i] == 0){
                q.push(i);
            }
        }
        while(!q.empty()){
            int i = q.front();
            q.pop();
            res.push_back(i);
            for(int j=0; j<edges[i].size(); ++j){
                if(--indig[edges[i][j]] == 0){
                    q.push(edges[i][j]);
                }
            }
        }
        if(res.size() != numCourses){
            return {};
        }
        return res;
    }
};
```
## dfs
```c++
class Solution {
private:
    vector<vector<int> > edges;
    vector<int> visited;
    vector<int> res;
    bool valid = true;

public:
    void dfs(int v){
        visited[v] = 1;
        for(int i=0; i<edges[v].size(); i++){
            if(visited[edges[v][i]] == 0){
                dfs(edges[v][i]);
                if(!valid){
                    return;
                }
            }
            else if(visited[edges[v][i]] == 1){
                valid = false;
                return;
            }
        }
        visited[v] = 2;
        res.push_back(v);
    }

    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        visited.resize(numCourses);
        edges.resize(numCourses);
        for(int i=0; i<prerequisites.size(); i++){
            edges[prerequisites[i][1]].push_back(prerequisites[i][0]);
        }
        for(int i=0; i<numCourses && valid; i++){
            if(!visited[i]){
                dfs(i);
            }
        }
        if(!valid){
            return {};
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```

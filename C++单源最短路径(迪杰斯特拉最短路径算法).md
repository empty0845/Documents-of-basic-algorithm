C++单源最短路径(迪杰斯特拉最短路径算法)
===

```c++
#define INT_MAX 2147483647
#include <iostream>
#include <vector>
#include <queue>

std::vector<int> dijkstra(int start, const std::vector<std::vector<std::pair<int, int>>>& g, int n) {
    std::vector<int> distances(n, INT_MAX);
    distances[start] = 0;
    std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, std::greater<std::pair<int, int>>> pq;
    pq.push(std::pair<int, int>{0, start});
    while (!pq.empty()) {
        int node = pq.top().second;
        int distance = pq.top().first;
        pq.pop();
        if (distance > distances[node]) {
            continue;
        }
        distances[node] = distance;
        for (const auto& neighbor: g[node]) {
            int neighbor_node =  neighbor.first;
            int weight = neighbor.second;
            int cur_dis = distance + weight;
            if (cur_dis < distances[neighbor_node]) {
                distances[neighbor_node] = cur_dis;
                pq.push(std::pair<int, int>{cur_dis, neighbor_node});
            }
        }
    }
    return distances;
}

int main() {
    std::vector<std::vector<std::pair<int, int>>> graph1 = {  
        {{1, 1}, {2, 4}},  
        {{0, 1}, {3, 2}},  
        {{0, 4}, {4, 8}},  
        {{1, 2}, {2, 7}},  
        {{1, 8}, {3, 6}}  
    };
    int n1 = 5;
    int start1 = 0;
    std::vector<std::vector<std::pair<int, int>>> graph2 = {  
        {{1, 1}, {2, 4}},  
        {{0, 1}},  
        {{}, {3, 2}, {4, 5}},  
        {{2, 2}},  
        {{}, {1, 3}}  
    };
    int n2 = 5;
    int start2 = 0;
    std::vector<int> result1 = dijkstra(start1, graph1, n1);
    std::vector<int> result2 = dijkstra(start2, graph2, n2);
    std::cout << "graph1: " << std::endl << "node: " << start1 << std::endl;
    for (int i = 0; i < n1; ++i) {
        std::cout << "To node " << i << " : " << result1[i] << std::endl;
    }
    std::cout << std::endl;
    std::cout << "graph2: " << std::endl << "node: " << start2 << std::endl;
    for (int i = 0; i < n2; ++i) {
        std::cout << "To node " << i << " : " << result2[i] << std::endl;
    }
    return 0;
}
```
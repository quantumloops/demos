#include <iostream>
#include <vector>
#include <queue>
#include <climits>
#include <string>

#define INF INT_MAX
using namespace std;

typedef pair<int, int> P;

void dijkstras(const vector<vector<P>>& graph, int src) {
    int V = graph.size();
    vector<int> dist(V, INF);

    priority_queue<P, vector<P>, greater<P>> pq;
    pq.push({0, src});
    dist[src] = 0;

    while (!pq.empty()) {
        int u = pq.top().second;
        pq.pop();

        for (auto &edge : graph[u]) {
            int v = edge.first, weight = edge.second;
            if (dist[v] > dist[u] + weight) {
                dist[v] = dist[u] + weight;
                pq.push({dist[v], v});
            }
        }
    }

    cout << "\nVertex\tDistance from Source" << endl;
    for (int i = 0; i < V; i++) {
        cout << i << "\t" << (dist[i] == INF ? "Unreachable" : to_string(dist[i])) << endl;
    }
}

int main() {
    int V, E;
    cout << "Enter number of vertices: ";
    cin >> V;
    cout << "Enter number of edges: ";
    cin >> E;

    vector<vector<P>> graph(V);

    cout << "\nEnter edges (source ,destination, weight):" << endl;
    for (int i = 0; i < E; i++) {
        int src, dest, weight;
        cin >> src >> dest >> weight;
        graph[src].push_back({dest, weight});
        // For undirected graph, uncomment the following line:
        // graph[dest].push_back({src, weight});
    }

    int startNode;
    cout << "\nEnter starting node: ";
    cin >> startNode;

    dijkstras(graph, startNode);
    return 0;
}
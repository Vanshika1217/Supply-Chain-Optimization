#include <iostream>
#include <vector>
#include <algorithm>
#include <limits>
#include <fstream>
#include <queue>

using namespace std;

// Define structures for nodes, edges, and graph
struct Edge {
    int source, destination, weight;
};

struct Place {
    int id;
    string type;
    string name;
};

struct Node {
    Place place;
    vector<Edge> edges;
};

class Graph {


public:
vector<Node> nodes;
    void addNode(int id, const string& type, const string& name) {
        Place newPlace = {id, type, name};
        Node newNode = {newPlace};
        nodes.push_back(newNode);
    }

    void addEdge(int source, int dest, int weight) {
        Edge newEdge = {source, dest, weight};
        nodes[source].edges.push_back(newEdge);
        nodes[dest].edges.push_back(newEdge); // Assuming undirected graph
    }

    pair<vector<int>, vector<int>> dijkstraShortestPath(int start) {
        vector<int> dist(nodes.size(), numeric_limits<int>::max());
        vector<int> prevNode(nodes.size(), -1);
        vector<bool> visited(nodes.size(), false);
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;

        dist[start] = 0;
        pq.push({0, start});

        while (!pq.empty()) {
            int u = pq.top().second;
            pq.pop();

            if (visited[u]) continue;
            visited[u] = true;

            for (const auto &edge : nodes[u].edges) {
                int v = edge.source == u ? edge.destination : edge.source;
                int weight = edge.weight;

                if (!visited[v] && dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;
                    prevNode[v] = u;
                    pq.push({dist[v], v});
                }
            }
        }

        return {dist, prevNode};
    }

    vector<Edge> primMST() {
        vector<Edge> result;
        vector<bool> visited(nodes.size(), false);
        vector<int> parent(nodes.size(), -1);
        vector<int> minWeight(nodes.size(), numeric_limits<int>::max());
        int src;
        cout<<"Enter source ID: ";
        cin>>src;
        minWeight[src] = 0;
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        pq.push({src, 0});

        while (!pq.empty()) {
            int u = pq.top().second;
            pq.pop();

            if (visited[u]) continue;
            visited[u] = true;

            for (const auto &edge : nodes[u].edges) {
                int v = edge.source == u ? edge.destination : edge.source;
                int weight = edge.weight;

                if (!visited[v] && weight < minWeight[v]) {
                    parent[v] = u;
                    minWeight[v] = weight;
                    pq.push({weight, v});
                }
            }
        }

        for (int i = 1; i < nodes.size(); ++i) {
            result.push_back({parent[i], i, minWeight[i]});
        }

        return result;
    }
};

void printShortestPaths(const vector<int> &shortestPaths, const vector<int> &prevNode, const vector<Node>& nodes) {
    ofstream outputFile("output.txt", ios::app);

    if (!outputFile.is_open()) {
        cout << "Error opening file for writing." << endl;
        return;
    }

    outputFile << "Shortest Paths from src node:" << endl;

    int shortestCost = numeric_limits<int>::max();
    vector<int> shortestPath;

    for (int i = 0; i < shortestPaths.size(); ++i) {
        outputFile << "Node " << nodes[i].place.id << " (" << nodes[i].place.name << "): " << shortestPaths[i] << " | Path: ";
        cout << "Node " << nodes[i].place.id << " (" << nodes[i].place.name << "): " << shortestPaths[i] << " | Path: ";
        vector<int> path;
        int cur = i;
        while (cur != -1) {
            path.push_back(cur);
            cur = prevNode[cur];
        }
        //reverse(path.begin(), path.end());
        for (int j = 0; j < path.size(); ++j) {
            outputFile << nodes[path[j]].place.name;
            cout<< nodes[path[j]].place.name;
            if (j != path.size() - 1) {
                outputFile << " -> ";
                cout << " -> ";
            }
        }
        outputFile << endl;
        cout << endl;

        // Calculate shortest cost path
        if (shortestPaths[i] < shortestCost && shortestPaths[i]!=0) {
            shortestCost = shortestPaths[i];
            shortestPath = path;
        }
    }

    outputFile << "Shortest Cost Path: ";
    for (int i = 0; i < shortestPath.size(); ++i) {
        outputFile << nodes[shortestPath[i]].place.name;
        if (i != shortestPath.size() - 1) {
            outputFile << " -> ";
        }
    }
    cout << "Shortest Cost Path: ";
    for (int i = 0; i < shortestPath.size(); ++i) {
        cout << nodes[shortestPath[i]].place.name;
        if (i != shortestPath.size() - 1) {
           cout<< " -> ";
        }
    }
    cout << " | Cost: " << shortestCost << endl;
    outputFile << " | Cost: " << shortestCost << endl;

    outputFile.close(); // Close the output file
    cout << "Shortest paths and cost information saved to output.txt" << endl;
}

void printMSTEdges(const vector<Edge> &primMST, const vector<Node>& nodes) {
    cout << "\nEdges in Prim's Minimum Spanning Tree:" << endl;
    for (const auto &edge : primMST) {
        cout << nodes[edge.source].place.name << " - " << nodes[edge.destination].place.name << " : " << edge.weight << endl;
    }
}

void calculateProductionInventoryDeadline(const vector<int> &weights, const vector<int> &profits, int capacity) {
    int n = weights.size();
    vector<vector<int>> dp(n + 1, vector<int>(capacity + 1, 0));

    for (int i = 1; i <= n; ++i) {
        for (int w = 1; w <= capacity; ++w) {
            if (weights[i - 1] > w) {
                dp[i][w] = dp[i - 1][w];
            } else {
                dp[i][w] = max(dp[i - 1][w], profits[i - 1] + dp[i - 1][w - weights[i - 1]]);
            }
        }
    }

    vector<int> selectedItems;
    int remainingCapacity = capacity;

    for (int i = n; i > 0 && remainingCapacity > 0; --i) {
        if (dp[i][remainingCapacity] != dp[i - 1][remainingCapacity]) {
            selectedItems.push_back(i - 1);
            remainingCapacity -= weights[i - 1];
        }
    }

    cout << "\nItems selected for production scheduling based on deadline:" << endl;
    for (const auto &item : selectedItems) {
        cout << "Item " << item << " (Weight: " << weights[item] << ", Profit: " << profits[item] << ")" << endl;
    }
}

void calculateProductionInventoryCapacity(const vector<int> &weights, const vector<int> &profits, int capacity) {
    int n = weights.size();
    vector<vector<int>> dp(n + 1, vector<int>(capacity + 1, 0));

    for (int i = 1; i <= n; ++i) {
        for (int w = 1; w <= capacity; ++w) {
            if (weights[i - 1] > w) {
                dp[i][w] = dp[i - 1][w];
            } else {
                dp[i][w] = max(dp[i - 1][w], profits[i - 1] + dp[i - 1][w - weights[i - 1]]);
            }
        }
    }

    vector<int> selectedItems;
    int remainingCapacity = capacity;

    for (int i = n; i > 0 && remainingCapacity > 0; --i) {
        if (dp[i][remainingCapacity] != dp[i - 1][remainingCapacity]) {
            selectedItems.push_back(i - 1);
            remainingCapacity -= weights[i - 1];
        }
    }

    cout << "\nItems selected for production scheduling based on capacity:" << endl;
    for (const auto &item : selectedItems) {
        cout << "Item " << item << " (Weight: " << weights[item] << ", Profit: " << profits[item] << ")" << endl;
    }
}

int main() {
    Graph graph;

    // Add 10 nodes with IDs, types, and names
    graph.addNode(0, "Supplier", "Supplier A");
    graph.addNode(1, "Supplier", "Supplier B");
    graph.addNode(2, "Supplier", "Supplier C");
    graph.addNode(3, "Supplier", "Supplier D");
    graph.addNode(4, "Supplier", "Supplier E");
    graph.addNode(5, "Supplier", "Supplier F");
    graph.addNode(6, "Supplier", "Supplier G");
    graph.addNode(7, "Supplier", "Supplier H");
    graph.addNode(8, "Supplier", "Supplier I");
    graph.addNode(9, "Supplier", "Supplier J");

    // Add 15 edges between the nodes with weights
    graph.addEdge(0, 1, 10);
    graph.addEdge(0, 2, 15);
    graph.addEdge(1, 2, 20);
    graph.addEdge(2, 3, 5);
    graph.addEdge(0, 4, 25);
    graph.addEdge(1, 5, 30);
    graph.addEdge(3, 6, 35);
    graph.addEdge(4, 7, 40);
    graph.addEdge(4, 8, 45);
    graph.addEdge(5, 9, 50);
    graph.addEdge(6, 7, 55);
    graph.addEdge(7, 8, 60);
    graph.addEdge(8, 9, 65);
    graph.addEdge(1, 3, 70);
    graph.addEdge(2, 6, 75);
    graph.addEdge(5, 8, 80);

    int option;
    cout << "Choose an option:" << endl;
    cout << "1. Calculate Shortest Paths using Dijkstra's Algorithm" << endl;
    cout << "2. Calculate Minimum Spanning Tree using Prim's Algorithm" << endl;
    cout << "3. Calculate Production Inventory" << endl;
    cin >> option;

    switch (option) {
        case 1:
            // Implement Dijkstra's algorithm for shortest path
            {
                int s;
                cout << "Enter source ID:" << endl;
                cin >> s;
                pair<vector<int>, vector<int>> shortestPaths = graph.dijkstraShortestPath(s);
                printShortestPaths(shortestPaths.first, shortestPaths.second, graph.nodes);
            }
            break;
        case 2:
            // Implement Prim's algorithm for minimum spanning tree
            {
                vector<Edge> primMST = graph.primMST();
                printMSTEdges(primMST, graph.nodes);
            }
            break;
        case 3:
            // Example usage of 0/1 knapsack algorithm for production inventory scheduling
            {
                vector<int> weights = {2, 3, 1, 4};
                vector<int> profits = {10, 20, 15, 30};
                int capacity;
                cout << "Enter the capacity: ";
                cin >> capacity;

                cout << "Choose an option for production inventory:" << endl;
                cout << "1. Based on Deadline (Greedy Algorithm)" << endl;
                cout << "2. Based on Capacity (0/1 Knapsack Algorithm)" << endl;
                int prodOption;
                cin >> prodOption;

                switch (prodOption) {
                    case 1:
                        calculateProductionInventoryDeadline(weights, profits, capacity);
                        break;
                    case 2:
                        calculateProductionInventoryCapacity(weights, profits, capacity);
                        break;
                    default:
                        cout << "Invalid production inventory option." << endl;
                        break;
                }
            }
            break;
        default:
            cout << "Invalid option." << endl;
            break;
    }

    return 0;
}

On Mon, 22 Apr 2024 at 00:27, Saumya Sharma <saumya.sharma0915@gmail.com> wrote:
#include <iostream>
#include <vector>
#include <algorithm>
#include <limits>
#include <fstream>
#include <queue>

using namespace std;

// Define structures for nodes, edges, and graph
struct Edge {
    int source, destination, weight;
};

struct Place {
    int id;
    string type;
    string name;
};

struct Node {
    Place place;
    vector<Edge> edges;
};

class Graph {


public:
vector<Node> nodes;
    void addNode(int id, const string& type, const string& name) {
        Place newPlace = {id, type, name};
        Node newNode = {newPlace};
        nodes.push_back(newNode);
    }

    void addEdge(int source, int dest, int weight) {
        Edge newEdge = {source, dest, weight};
        nodes[source].edges.push_back(newEdge);
        nodes[dest].edges.push_back(newEdge); // Assuming undirected graph
    }

    pair<vector<int>, vector<int>> dijkstraShortestPath(int start) {
        vector<int> dist(nodes.size(), numeric_limits<int>::max());
        vector<int> prevNode(nodes.size(), -1);
        vector<bool> visited(nodes.size(), false);
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;

        dist[start] = 0;
        pq.push({0, start});

        while (!pq.empty()) {
            int u = pq.top().second;
            pq.pop();

            if (visited[u]) continue;
            visited[u] = true;

            for (const auto &edge : nodes[u].edges) {
                int v = edge.source == u ? edge.destination : edge.source;
                int weight = edge.weight;

                if (!visited[v] && dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;
                    prevNode[v] = u;
                    pq.push({dist[v], v});
                }
            }
        }

        return {dist, prevNode};
    }

    vector<Edge> primMST() {
        vector<Edge> result;
        vector<bool> visited(nodes.size(), false);
        vector<int> parent(nodes.size(), -1);
        vector<int> minWeight(nodes.size(), numeric_limits<int>::max());
        int src;
        cout<<"Enter source ID: ";
        cin>>src;
        minWeight[src] = 0;
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        pq.push({src, 0});

        while (!pq.empty()) {
            int u = pq.top().second;
            pq.pop();

            if (visited[u]) continue;
            visited[u] = true;

            for (const auto &edge : nodes[u].edges) {
                int v = edge.source == u ? edge.destination : edge.source;
                int weight = edge.weight;

                if (!visited[v] && weight < minWeight[v]) {
                    parent[v] = u;
                    minWeight[v] = weight;
                    pq.push({weight, v});
                }
            }
        }

        for (int i = 1; i < nodes.size(); ++i) {
            result.push_back({parent[i], i, minWeight[i]});
        }

        return result;
    }
};

void printShortestPaths(const vector<int> &shortestPaths, const vector<int> &prevNode, const vector<Node>& nodes) {
    ofstream outputFile("output.txt", ios::app);

    if (!outputFile.is_open()) {
        cout << "Error opening file for writing." << endl;
        return;
    }

    outputFile << "Shortest Paths from src node:" << endl;

    int shortestCost = numeric_limits<int>::max();
    vector<int> shortestPath;

    for (int i = 0; i < shortestPaths.size(); ++i) {
        outputFile << "Node " << nodes[i].place.id << " (" << nodes[i].place.name << "): " << shortestPaths[i] << " | Path: ";
        cout << "Node " << nodes[i].place.id << " (" << nodes[i].place.name << "): " << shortestPaths[i] << " | Path: ";
        vector<int> path;
        int cur = i;
        while (cur != -1) {
            path.push_back(cur);
            cur = prevNode[cur];
        }
        //reverse(path.begin(), path.end());
        for (int j = 0; j < path.size(); ++j) {
            outputFile << nodes[path[j]].place.name;
            cout<< nodes[path[j]].place.name;
            if (j != path.size() - 1) {
                outputFile << " -> ";
                cout << " -> ";
            }
        }
        outputFile << endl;
        cout << endl;

        // Calculate shortest cost path
        if (shortestPaths[i] < shortestCost && shortestPaths[i]!=0) {
            shortestCost = shortestPaths[i];
            shortestPath = path;
        }
    }

    outputFile << "Shortest Cost Path: ";
    for (int i = 0; i < shortestPath.size(); ++i) {
        outputFile << nodes[shortestPath[i]].place.name;
        if (i != shortestPath.size() - 1) {
            outputFile << " -> ";
        }
    }
    cout << "Shortest Cost Path: ";
    for (int i = 0; i < shortestPath.size(); ++i) {
        cout << nodes[shortestPath[i]].place.name;
        if (i != shortestPath.size() - 1) {
           cout<< " -> ";
        }
    }
    cout << " | Cost: " << shortestCost << endl;
    outputFile << " | Cost: " << shortestCost << endl;

    outputFile.close(); // Close the output file
    cout << "Shortest paths and cost information saved to output.txt" << endl;
}

void printMSTEdges(const vector<Edge> &primMST, const vector<Node>& nodes) {
    cout << "\nEdges in Prim's Minimum Spanning Tree:" << endl;
    for (const auto &edge : primMST) {
        cout << nodes[edge.source].place.name << " - " << nodes[edge.destination].place.name << " : " << edge.weight << endl;
    }
}

void calculateProductionInventoryDeadline(const vector<int> &weights, const vector<int> &profits, int capacity) {
    int n = weights.size();
    vector<vector<int>> dp(n + 1, vector<int>(capacity + 1, 0));

    for (int i = 1; i <= n; ++i) {
        for (int w = 1; w <= capacity; ++w) {
            if (weights[i - 1] > w) {
                dp[i][w] = dp[i - 1][w];
            } else {
                dp[i][w] = max(dp[i - 1][w], profits[i - 1] + dp[i - 1][w - weights[i - 1]]);
            }
        }
    }

    vector<int> selectedItems;
    int remainingCapacity = capacity;

    for (int i = n; i > 0 && remainingCapacity > 0; --i) {
        if (dp[i][remainingCapacity] != dp[i - 1][remainingCapacity]) {
            selectedItems.push_back(i - 1);
            remainingCapacity -= weights[i - 1];
        }
    }

    cout << "\nItems selected for production scheduling based on deadline:" << endl;
    for (const auto &item : selectedItems) {
        cout << "Item " << item << " (Weight: " << weights[item] << ", Profit: " << profits[item] << ")" << endl;
    }
}

void calculateProductionInventoryCapacity(const vector<int> &weights, const vector<int> &profits, int capacity) {
    int n = weights.size();
    vector<vector<int>> dp(n + 1, vector<int>(capacity + 1, 0));

    for (int i = 1; i <= n; ++i) {
        for (int w = 1; w <= capacity; ++w) {
            if (weights[i - 1] > w) {
                dp[i][w] = dp[i - 1][w];
            } else {
                dp[i][w] = max(dp[i - 1][w], profits[i - 1] + dp[i - 1][w - weights[i - 1]]);
            }
        }
    }

    vector<int> selectedItems;
    int remainingCapacity = capacity;

    for (int i = n; i > 0 && remainingCapacity > 0; --i) {
        if (dp[i][remainingCapacity] != dp[i - 1][remainingCapacity]) {
            selectedItems.push_back(i - 1);
            remainingCapacity -= weights[i - 1];
        }
    }

    cout << "\nItems selected for production scheduling based on capacity:" << endl;
    for (const auto &item : selectedItems) {
        cout << "Item " << item << " (Weight: " << weights[item] << ", Profit: " << profits[item] << ")" << endl;
    }
}

int main() {
    Graph graph;

    // Add 10 nodes with IDs, types, and names
    graph.addNode(0, "Supplier", "Supplier A");
    graph.addNode(1, "Supplier", "Supplier B");
    graph.addNode(2, "Supplier", "Supplier C");
    graph.addNode(3, "Supplier", "Supplier D");
    graph.addNode(4, "Supplier", "Supplier E");
    graph.addNode(5, "Supplier", "Supplier F");
    graph.addNode(6, "Supplier", "Supplier G");
    graph.addNode(7, "Supplier", "Supplier H");
    graph.addNode(8, "Supplier", "Supplier I");
    graph.addNode(9, "Supplier", "Supplier J");

    // Add 15 edges between the nodes with weights
    graph.addEdge(0, 1, 10);
    graph.addEdge(0, 2, 15);
    graph.addEdge(1, 2, 20);
    graph.addEdge(2, 3, 5);
    graph.addEdge(0, 4, 25);
    graph.addEdge(1, 5, 30);
    graph.addEdge(3, 6, 35);
    graph.addEdge(4, 7, 40);
    graph.addEdge(4, 8, 45);
    graph.addEdge(5, 9, 50);
    graph.addEdge(6, 7, 55);
    graph.addEdge(7, 8, 60);
    graph.addEdge(8, 9, 65);
    graph.addEdge(1, 3, 70);
    graph.addEdge(2, 6, 75);
    graph.addEdge(5, 8, 80);

    int option;
    cout << "Choose an option:" << endl;
    cout << "1. Calculate Shortest Paths using Dijkstra's Algorithm" << endl;
    cout << "2. Calculate Minimum Spanning Tree using Prim's Algorithm" << endl;
    cout << "3. Calculate Production Inventory" << endl;
    cin >> option;

    switch (option) {
        case 1:
            // Implement Dijkstra's algorithm for shortest path
            {
                int s;
                cout << "Enter source ID:" << endl;
                cin >> s;
                pair<vector<int>, vector<int>> shortestPaths = graph.dijkstraShortestPath(s);
                printShortestPaths(shortestPaths.first, shortestPaths.second, graph.nodes);
            }
            break;
        case 2:
            // Implement Prim's algorithm for minimum spanning tree
            {
                vector<Edge> primMST = graph.primMST();
                printMSTEdges(primMST, graph.nodes);
            }
            break;
        case 3:
            // Example usage of 0/1 knapsack algorithm for production inventory scheduling
            {
                vector<int> weights = {2, 3, 1, 4};
                vector<int> profits = {10, 20, 15, 30};
                int capacity;
                cout << "Enter the capacity: ";
                cin >> capacity;

                cout << "Choose an option for production inventory:" << endl;
                cout << "1. Based on Deadline (Greedy Algorithm)" << endl;
                cout << "2. Based on Capacity (0/1 Knapsack Algorithm)" << endl;
                int prodOption;
                cin >> prodOption;

                switch (prodOption) {
                    case 1:
                        calculateProductionInventoryDeadline(weights, profits, capacity);
                        break;
                    case 2:
                        calculateProductionInventoryCapacity(weights, profits, capacity);
                        break;
                    default:
                        cout << "Invalid production inventory option." << endl;
                        break;
                }
            }
            break;
        default:
            cout << "Invalid option." << endl;
            break;
    }

    return 0;
}

On Sun, 21 Apr 2024 at 23:52, Vanshika Gupta <gvanshika528@gmail.com> wrote:
#include <iostream>
#include <vector>
#include <algorithm>
#include <limits>
#include <fstream>
#include <queue>

using namespace std;

// Define structures for nodes, edges, and graph
struct Edge {
    int source, destination, weight;
};

struct Place {
    int id;
    string type;
    string name;
};

struct Node {
    Place place;
    vector<Edge> edges;
};

class Graph {


public:
vector<Node> nodes;
    void addNode(int id, const string& type, const string& name) {
        Place newPlace = {id, type, name};
        Node newNode = {newPlace};
        nodes.push_back(newNode);
    }

    void addEdge(int source, int dest, int weight) {
        Edge newEdge = {source, dest, weight};
        nodes[source].edges.push_back(newEdge);
        nodes[dest].edges.push_back(newEdge); // Assuming undirected graph
    }

    pair<vector<int>, vector<int>> dijkstraShortestPath(int start) {
        vector<int> dist(nodes.size(), numeric_limits<int>::max());
        vector<int> prevNode(nodes.size(), -1);
        vector<bool> visited(nodes.size(), false);
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;

        dist[start] = 0;
        pq.push({0, start});

        while (!pq.empty()) {
            int u = pq.top().second;
            pq.pop();

            if (visited[u]) continue;
            visited[u] = true;

            for (const auto &edge : nodes[u].edges) {
                int v = edge.source == u ? edge.destination : edge.source;
                int weight = edge.weight;

                if (!visited[v] && dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;
                    prevNode[v] = u;
                    pq.push({dist[v], v});
                }
            }
        }

        return {dist, prevNode};
    }

    vector<Edge> primMST() {
        vector<Edge> result;
        vector<bool> visited(nodes.size(), false);
        vector<int> parent(nodes.size(), -1);
        vector<int> minWeight(nodes.size(), numeric_limits<int>::max());
        int src;
        cout<<"Enter source ID: ";
        cin>>src;
        minWeight[src] = 0;
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        pq.push({src, 0});

        while (!pq.empty()) {
            int u = pq.top().second;
            pq.pop();

            if (visited[u]) continue;
            visited[u] = true;

            for (const auto &edge : nodes[u].edges) {
                int v = edge.source == u ? edge.destination : edge.source;
                int weight = edge.weight;

                if (!visited[v] && weight < minWeight[v]) {
                    parent[v] = u;
                    minWeight[v] = weight;
                    pq.push({weight, v});
                }
            }
        }

        for (int i = 1; i < nodes.size(); ++i) {
            result.push_back({parent[i], i, minWeight[i]});
        }

        return result;
    }
};

void printShortestPaths(const vector<int> &shortestPaths, const vector<int> &prevNode, const vector<Node>& nodes) {
    ofstream outputFile("output.txt"); // Open output file

    if (!outputFile.is_open()) {
        cout << "Error opening file for writing." << endl;
        return;
    }

    outputFile << "Shortest Paths from Node 0:" << endl;

    int shortestCost = numeric_limits<int>::max();
    vector<int> shortestPath;

    for (int i = 0; i < shortestPaths.size(); ++i) {
        outputFile << "Node " << nodes[i].place.id << " (" << nodes[i].place.name << "): " << shortestPaths[i] << " | Path: ";
        vector<int> path;
        int cur = i;
        while (cur != -1) {
            path.push_back(cur);
            cur = prevNode[cur];
        }
        reverse(path.begin(), path.end());
        for (int j = 0; j < path.size(); ++j) {
            outputFile << nodes[path[j]].place.name;
            if (j != path.size() - 1) {
                outputFile << " -> ";
            }
        }
        outputFile << endl;

        // Calculate shortest cost path
        if (shortestPaths[i] < shortestCost && shortestPaths[i]!=0) {
            shortestCost = shortestPaths[i];
            shortestPath = path;
        }
    }

    outputFile << "Shortest Cost Path: ";
    for (int i = 0; i < shortestPath.size(); ++i) {
        outputFile << nodes[shortestPath[i]].place.name;
        if (i != shortestPath.size() - 1) {
            outputFile << " -> ";
        }
    }
    outputFile << " | Cost: " << shortestCost << endl;

    outputFile.close(); // Close the output file
    cout << "Shortest paths and cost information saved to output.txt" << endl;
}

void printMSTEdges(const vector<Edge> &primMST, const vector<Node>& nodes) {
    cout << "\nEdges in Prim's Minimum Spanning Tree:" << endl;
    for (const auto &edge : primMST) {
        cout << nodes[edge.source].place.name << " - " << nodes[edge.destination].place.name << " : " << edge.weight << endl;
    }
}

void calculateProductionInventoryDeadline(const vector<int> &weights, const vector<int> &profits, int capacity) {
    int n = weights.size();
    vector<vector<int>> dp(n + 1, vector<int>(capacity + 1, 0));

    for (int i = 1; i <= n; ++i) {
        for (int w = 1; w <= capacity; ++w) {
            if (weights[i - 1] > w) {
                dp[i][w] = dp[i - 1][w];
            } else {
                dp[i][w] = max(dp[i - 1][w], profits[i - 1] + dp[i - 1][w - weights[i - 1]]);
            }
        }
    }

    vector<int> selectedItems;
    int remainingCapacity = capacity;

    for (int i = n; i > 0 && remainingCapacity > 0; --i) {
        if (dp[i][remainingCapacity] != dp[i - 1][remainingCapacity]) {
            selectedItems.push_back(i - 1);
            remainingCapacity -= weights[i - 1];
        }
    }

    cout << "\nItems selected for production scheduling based on deadline:" << endl;
    for (const auto &item : selectedItems) {
        cout << "Item " << item << " (Weight: " << weights[item] << ", Profit: " << profits[item] << ")" << endl;
    }
}

void calculateProductionInventoryCapacity(const vector<int> &weights, const vector<int> &profits, int capacity) {
    int n = weights.size();
    vector<vector<int>> dp(n + 1, vector<int>(capacity + 1, 0));

    for (int i = 1; i <= n; ++i) {
        for (int w = 1; w <= capacity; ++w) {
            if (weights[i - 1] > w) {
                dp[i][w] = dp[i - 1][w];
            } else {
                dp[i][w] = max(dp[i - 1][w], profits[i - 1] + dp[i - 1][w - weights[i - 1]]);
            }
        }
    }

    vector<int> selectedItems;
    int remainingCapacity = capacity;

    for (int i = n; i > 0 && remainingCapacity > 0; --i) {
        if (dp[i][remainingCapacity] != dp[i - 1][remainingCapacity]) {
            selectedItems.push_back(i - 1);
            remainingCapacity -= weights[i - 1];
        }
    }

    cout << "\nItems selected for production scheduling based on capacity:" << endl;
    for (const auto &item : selectedItems) {
        cout << "Item " << item << " (Weight: " << weights[item] << ", Profit: " << profits[item] << ")" << endl;
    }
}

int main() {
    Graph graph;

    // Add 10 nodes with IDs, types, and names
    graph.addNode(0, "Supplier", "Supplier A");
    graph.addNode(1, "Supplier", "Supplier B");
    graph.addNode(2, "Supplier", "Supplier C");
    graph.addNode(3, "Supplier", "Supplier D");
    graph.addNode(4, "Supplier", "Supplier E");
    graph.addNode(5, "Supplier", "Supplier F");
    graph.addNode(6, "Supplier", "Supplier G");
    graph.addNode(7, "Supplier", "Supplier H");
    graph.addNode(8, "Supplier", "Supplier I");
    graph.addNode(9, "Supplier", "Supplier J");

    // Add 15 edges between the nodes with weights
    graph.addEdge(0, 1, 10);
    graph.addEdge(0, 2, 15);
    graph.addEdge(1, 2, 20);
    graph.addEdge(2, 3, 5);
    graph.addEdge(0, 4, 25);
    graph.addEdge(1, 5, 30);
    graph.addEdge(3, 6, 35);
    graph.addEdge(4, 7, 40);
    graph.addEdge(4, 8, 45);
    graph.addEdge(5, 9, 50);
    graph.addEdge(6, 7, 55);
    graph.addEdge(7, 8, 60);
    graph.addEdge(8, 9, 65);
    graph.addEdge(1, 3, 70);
    graph.addEdge(2, 6, 75);
    graph.addEdge(5, 8, 80);

    int option;
    cout << "Choose an option:" << endl;
    cout << "1. Calculate Shortest Paths using Dijkstra's Algorithm" << endl;
    cout << "2. Calculate Minimum Spanning Tree using Prim's Algorithm" << endl;
    cout << "3. Calculate Production Inventory" << endl;
    cin >> option;

    switch (option) {
        case 1:
            // Implement Dijkstra's algorithm for shortest path
            {
                int s;
                cout << "Enter source ID:" << endl;
                cin >> s;
                pair<vector<int>, vector<int>> shortestPaths = graph.dijkstraShortestPath(s);
                printShortestPaths(shortestPaths.first, shortestPaths.second, graph.nodes);
            }
            break;
        case 2:
            // Implement Prim's algorithm for minimum spanning tree
            {
                vector<Edge> primMST = graph.primMST();
                printMSTEdges(primMST, graph.nodes);
            }
            break;
        case 3:
            // Example usage of 0/1 knapsack algorithm for production inventory scheduling
            {
                vector<int> weights = {2, 3, 1, 4};
                vector<int> profits = {10, 20, 15, 30};
                int capacity;
                cout << "Enter the capacity: ";
                cin >> capacity;

                cout << "Choose an option for production inventory:" << endl;
                cout << "1. Based on Deadline (Greedy Algorithm)" << endl;
                cout << "2. Based on Capacity (0/1 Knapsack Algorithm)" << endl;
                int prodOption;
                cin >> prodOption;

                switch (prodOption) {
                    case 1:
                        calculateProductionInventoryDeadline(weights, profits, capacity);
                        break;
                    case 2:
                        calculateProductionInventoryCapacity(weights, profits, capacity);
                        break;
                    default:
                        cout << "Invalid production inventory option." << endl;
                        break;
                }
            }
            break;
        default:
            cout << "Invalid option." << endl;
            break;
    }

    return 0;
}

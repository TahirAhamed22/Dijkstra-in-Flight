# Dijkstra-in-Flight
Flight Path Optimization using Dijkstra’s Algorithm




//  #include <stdio.h>
#include <limits.h>
#include <stdbool.h>

#define V 14  // Number of nodes (countries)
#define MAX_TIME_LENGTH 20
#define MAX_NAME_LENGTH 50

// Function to find the vertex with the minimum distance value
int minDistance(int dist[], bool sptSet[]) {
    int min = INT_MAX, min_index = -1;

    for (int v = 0; v < V; v++) {
        if (sptSet[v] == false && dist[v] <= min) {
            min = dist[v];
            min_index = v;
        }
    }

    return min_index;
}

// Function to print the constructed distance array and paths
void printSolution(int dist[], int parent[], char times[V][2][MAX_TIME_LENGTH], char names[V][MAX_NAME_LENGTH], char airports[V][MAX_NAME_LENGTH]) {
    printf("Country \t Airport \t Distance from India \t Path\t\t\t Arrival Time\t Departure Time\n");
    for (int i = 1; i < V; i++) { // Start from 1 to skip source itself
        printf("%s \t %s \t %d\t\t\t", names[i], airports[i], dist[i]);
        int j = i;
        while (parent[j] != -1) {
            printf("%s <- ", names[j]);
            j = parent[j];
        }
        printf("India\t\t %s\t\t %s\n", times[i][0], times[i][1]);
    }
}

// Function to implement Dijkstra's single source shortest path algorithm
void dijkstra(int graph[V][V], int src, char times[V][2][MAX_TIME_LENGTH], char names[V][MAX_NAME_LENGTH], char airports[V][MAX_NAME_LENGTH]) {
    int dist[V];  // The output array. dist[i] will hold the shortest distance from src to i
    bool sptSet[V];  // sptSet[i] will be true if vertex i is included in shortest path tree
    int parent[V]; // Array to store the shortest path tree

    // Initialize all distances as INFINITE and sptSet[] as false
    for (int i = 0; i < V; i++) {
        dist[i] = INT_MAX;
        sptSet[i] = false;
        parent[i] = -1; // Initialize parent array
    }

    // Distance of source vertex from itself is always 0
    dist[src] = 0;

    // Find shortest path for all vertices
    for (int count = 0; count < V - 1; count++) {
        // Pick the minimum distance vertex from the set of vertices not yet processed
        int u = minDistance(dist, sptSet);

        // Mark the picked vertex as processed
        sptSet[u] = true;

        // Update dist value of the adjacent vertices of the picked vertex
        for (int v = 0; v < V; v++) {
            // Update dist[v] only if is not in sptSet, there is an edge from u to v,
            // and total weight of path from src to v through u is smaller than current value of dist[v]
            if (!sptSet[v] && graph[u][v] && dist[u] != INT_MAX && dist[u] + graph[u][v] < dist[v]) {
                dist[v] = dist[u] + graph[u][v];
                parent[v] = u;
            }
        }
    }

    // Print the constructed distance array and paths
    printSolution(dist, parent, times, names, airports);
}

int main() {
    // Graph distances between 14 Asian countries
    int graph[V][V] = {
        // India  Pakistan Bangladesh Nepal Bhutan China Iraq Iran Sri Lanka Afghanistan Hong Kong North Korea South Korea Maldives
        {0,     500,     700,     600,  300,   2000, 1500, 1200, 2500, 1000, 1300, 1800, 2100,  3000}, // India
        {500,   0,       300,     200,  500,   1500, 1000, 800,  1600, 800,  1200, 1500, 1600,  2500}, // Pakistan
        {700,   300,     0,       400,  700,   1300, 900,  1000, 1400, 700,  900,  1200, 1300,  2000}, // Bangladesh
        {600,   200,     400,     0,    400,   1400, 1000, 1200, 1400, 900,  1000, 1200, 1300,  2000}, // Nepal
        {300,   500,     700,     400,  0,     1400, 1000, 800,  1200, 600,  1200, 1300, 1400,  2000}, // Bhutan
        {2000,  1500,    1300,    1400, 1400,  0,     2500, 2000, 3000, 1700, 2000, 2200, 2500,  3000}, // China
        {1500,  1000,    900,     1000, 1000,  2500, 0,    800,  1500, 1200, 1000, 1200, 1400,  2000}, // Iraq
        {1200,  800,     1000,    1200, 800,   2000, 800,  0,    1200, 800,  900,  1300, 1500,  2500}, // Iran
        {2500,  1600,    1400,    1400, 1200,  3000, 1500, 1200, 0,    1600, 1500, 1700, 2000,  2000}, // Sri Lanka
        {1000,  800,     700,     900,  600,   1700, 1200, 800,  1600, 0,    900,  1200, 1400,  2000}, // Afghanistan
        {1300,  1200,    900,     1000, 1200,  2000, 1000, 900,  1500, 900,  0,    1100, 1200,  1500}, // Hong Kong
        {1800,  1500,    1200,    1200, 1300,  2200, 1200, 1300, 1700, 1200, 1100, 0,    1200,  2000}, // North Korea
        {2100,  1600,    1300,    1300, 1400,  2000, 1400, 1500, 2000, 1400, 1200, 1300, 0,     1500}, // South Korea
        {3000,  2500,    2000,    2000, 2000,  3000, 2000, 2500, 2000, 2000, 1500, 2000, 1500, 0   }  // Maldives
    };

    // Names and airport names for 14 Asian countries
    char names[V][MAX_NAME_LENGTH] = {
        "India", "Pakistan", "Bangladesh", "Nepal", "Bhutan", "China", "Iraq", "Iran", 
        "Sri Lanka", "Afghanistan", "Hong Kong", "North Korea", "South Korea", "Maldives"
    };

    char airports[V][MAX_NAME_LENGTH] = {
        "Indira Gandhi International Airport", "Allama Iqbal International Airport", 
        "Hazrat Shahjalal International Airport", "Tribhuvan International Airport", 
        "Paro International Airport", "Beijing Capital International Airport", 
        "Baghdad International Airport", "Tehran Imam Khomeini International Airport", 
        "Bandaranaike International Airport", "Hamid Karzai International Airport", 
        "Hong Kong International Airport", "Pyongyang Sunan International Airport", 
        "Incheon International Airport", "Velana International Airport"
    };

    char times[V][2][MAX_TIME_LENGTH] = {
        {"00:00", "00:00"},  // India (Starting point)
        {"", ""},  // Placeholder for Pakistan
        {"", ""},  // Placeholder for Bangladesh
        {"", ""},  // Placeholder for Nepal
        {"", ""},  // Placeholder for Bhutan
        {"", ""},  // Placeholder for China
        {"", ""},  // Placeholder for Iraq
        {"", ""},  // Placeholder for Iran
        {"", ""},  // Placeholder for Sri Lanka
        {"", ""},  // Placeholder for Afghanistan
        {"", ""},  // Placeholder for Hong Kong
        {"", ""},  // Placeholder for North Korea
        {"", ""},  // Placeholder for South Korea
        {"", ""}   // Maldives (Ending point)
    };

    // Manually entering arrival and departure times for demonstration
    printf("Enter arrival and departure times for each country in HH:MM format.\n");

    for (int i = 1; i < V; i++) {
        printf("Country %s (%s):\n", names[i], airports[i]);
        printf("  Arrival time: ");
        scanf("%s", times[i][0]);
        printf("  Departure time: ");
        scanf("%s", times[i][1]);
    }

    // Assume node 0 is India and the flight starts from there
    dijkstra(graph, 0, times, names, airports);

    return 0;
}

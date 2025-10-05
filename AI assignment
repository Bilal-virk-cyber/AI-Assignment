#include <iostream>
#include <iomanip>
#include <cmath>
#include <cstdlib>
#include <ctime>
using namespace std;

// =================== Structure Definition ===================
struct Route {
    int from, to;
    double probability;
    Route* next;
    Route* prev;
};

// =================== Doubly Linked List Class ===================
class Network {
private:
    Route* head;
    Route* tail;

public:
    Network() {
        head = tail = nullptr;
    }

    // Add route at end 
    void addRoute(int from, int to, double probability) {
        Route* newRoute = new Route{from, to, probability, nullptr, nullptr};
        if (!head) {
            head = tail = newRoute;
        } else {
            tail->next = newRoute;
            newRoute->prev = tail;
            tail = newRoute;
        }
    }

    // Display all routes
    void displayRoutes() {
        cout << "\nAvailable Routes:\n";
        Route* temp = head;
        while (temp) {
            cout << "Node " << temp->from << " → Node " << temp->to
                 << " | Success Probability: " << fixed << setprecision(2)
                 << temp->probability << endl;
            temp = temp->next;
        }
    }

    // Find most reliable route (up to 2 hops for simplicity)
    double mostReliableRoute(int start, int end, bool show = true) {
        Route* temp = head;
        double maxProb = 0.0;
        string bestPath = "None";

        while (temp) {
            if (temp->from == start) {
                // Direct route
                if (temp->to == end && temp->probability > maxProb) {
                    maxProb = temp->probability;
                    bestPath = "Node " + to_string(start) + " → Node " + to_string(end);
                }
                // Try two-step path
                Route* inner = head;
                while (inner) {
                    if (inner->from == temp->to && inner->to == end) {
                        double totalProb = temp->probability * inner->probability;
                        if (totalProb > maxProb) {
                            maxProb = totalProb;
                            bestPath = "Node " + to_string(start) + " → Node " +
                                       to_string(temp->to) + " → Node " + to_string(end);
                        }
                    }
                    inner = inner->next;
                }
            }
            temp = temp->next;
        }

        if (show) {
            cout << "\nMost Reliable Route from " << start << " to " << end << ":\n";
            if (maxProb > 0)
                cout << bestPath << "\nTotal Probability: "
                     << fixed << setprecision(4) << maxProb << endl;
            else
                cout << "No valid route found.\n";
        }

        return maxProb;
    }

    // Clear the network for next simulation
    void clear() {
        Route* temp = head;
        while (temp) {
            Route* next = temp->next;
            delete temp;
            temp = next;
        }
        head = tail = nullptr;
    }
};

// =================== MAIN ===================
int main() {
    srand(time(0));
    cout << "=== AI Assignment: Maximizing Probability in Route Selection ===\n";

    Network network;
    double maxProb[10];

    // ---------- 10 Random Networks ----------
    for (int i = 0; i < 10; i++) {
        cout << "\n--- Network " << i + 1 << " ---\n";

        network.clear();

        int numRoutes = rand() % 6 + 5; // 5–10 routes
        for (int r = 0; r < numRoutes; r++) {
            int from = rand() % 10 + 1;
            int to = rand() % 10 + 1;
            if (from != to) {
                double prob = (rand() % 50 + 50) / 100.0; // 0.5–1.0
                network.addRoute(from, to, prob);
            }
        }

        network.displayRoutes();

        int start = rand() % 10 + 1;
        int end = rand() % 10 + 1;
        while (end == start) end = rand() % 10 + 1;

        maxProb[i] = network.mostReliableRoute(start, end);
    }

    // ---------- Histogram ----------
    cout << "\n=== Histogram of Maximum Probabilities ===\n";
    for (int i = 0; i < 10; i++) {
        cout << "Network " << setw(2) << i + 1 << ": ";
        int stars = int(maxProb[i] * 20);
        for (int s = 0; s < stars; s++) cout << "*";
        cout << " (" << fixed << setprecision(3) << maxProb[i] << ")\n";
    }

    return 0;
}

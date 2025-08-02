# 🧑‍💻 Metro Route Optimization System

A full-stack web application to find the **shortest path** and **minimum interchanges** in a metro system based on user input. It integrates a C++-based route optimization algorithm with a modern web interface.

---

## 🌟 Overview

The Metro Route Optimization System allows users to input a **source** and **destination** metro station and calculates:
- The **shortest travel time**
- The **minimum number of interchanges**
- A **detailed route** with metro line colors

---

## ✨ Features

### General Features
- 🔍 **Input Form** – Specify source and destination stations
- ⚙️ **Backend Processing** – Generates input files and executes C++ logic for route optimization
- 📈 **Result Display** – Shows shortest time, interchange count, and full station list with line color

### Additional Features
- 💬 **Live Feedback** – Real-time route computation and dynamic display
- 🧭 **Interactive Output** – Station-wise route and metro line visualization

---

## 🛠️ Technology Stack

### 🎨 Frontend
- React.js
- HTML, CSS, JavaScript

### 🧰 Backend
- Node.js
- Express.js
- C++ (for route optimization algorithm)
- Docker (for safe, isolated execution)

### ☁️ Hosting
- AWS / Google Cloud (optional for deployment)

---

## 📊 Database & File Handling

### 📁 File Structure

#### Input Files
- Generated based on user input.
- Passed into the C++ module for processing.

#### Output Files
- Created after C++ execution.
- Used to display results in the frontend.

---

## 🧩 Architecture

### 🧱 Microservices-Based

#### 🔧 Backend Service
- Handles route computation via C++ execution
- Manages file generation and safe runtime environment

#### 🎨 Frontend Service
- React-based UI for taking user input and showing output
- Sends and receives API requests via Express.js

### 🔐 Security
- **Execution Isolation** – Uses Docker containers to sandbox C++ execution
- **Input Validation** – Ensures only valid inputs are processed

---

## 🚀 Installation

### 📋 Prerequisites
- Node.js
- C++ compiler (e.g., `g++`)
- Docker (optional but recommended)

### 🪜 Steps

```bash
# Clone the Repository
git clone https://github.com/Aman-Kesari/MetroGuide

# Install Backend Dependencies
npm install

# Start the Backend Server
npm start

# Run the Frontend
npm run dev
## 🏆 Future Enhancements

- 🔁 **Support for multiple metro networks or cities**
- 💡 **Save favorite or frequent routes**
- 🌐 **Improve UI/UX** for better usability and accessibility
- 📲 **Build a mobile version** using React Native for cross-platform support

---

## 🤝 Contributions

Contributions are welcome! Help us make the Metro Route Optimization System better:

1. 🍴 Fork the repository  
2. 🌿 Create a new feature branch  
3. 🛠️ Make your changes  
4. 📩 Submit a pull request for review

---

## 📬 Contact

📧 Reach out at: **amankesari803@gmail.com**  
For queries, suggestions, or collaboration opportunities.

---

## 📝 Example C++ Code

Below is a simplified snippet of the C++ algorithm used for route optimization:

```cpp
unordered_map<string, unordered_map<string, pair<int, string>>> adjList;

void addEdge(string s1, string s2, int tm, string color) {
    adjList[s1][s2] = {tm, color};
    adjList[s2][s1] = {tm, color};
}

void shortestPathWithMinInterchange(string src, string dest) {
    typedef tuple<int, int, string, string> Node;
    priority_queue<Node, vector<Node>, greater<Node>> pq;
    pq.push({0, 0, src, ""});

    unordered_map<string, pair<int, int>> dist;
    unordered_map<string, pair<string, string>> parent;

    for (auto& node : adjList) {
        dist[node.first] = {INT_MAX, INT_MAX};
        parent[node.first] = {"", ""};
    }

    dist[src] = {0, 0};

    while (!pq.empty()) {
        auto [curInterchanges, curTime, curStation, prevColor] = pq.top();
        pq.pop();

        if (curStation == dest) {
            cout << "Shortest time: " << curTime << endl;
            cout << "Minimum interchanges: " << curInterchanges << endl;
            // Print path logic here
            return;
        }

        for (auto& [nextStation, info] : adjList[curStation]) {
            auto [travelTime, color] = info;
            int newInterchanges = curInterchanges + (prevColor != "" && prevColor != color);
            int newTime = curTime + travelTime;

            if (newInterchanges < dist[nextStation].second ||
               (newInterchanges == dist[nextStation].second && newTime < dist[nextStation].first)) {
                dist[nextStation] = {newTime, newInterchanges};
                parent[nextStation] = {curStation, color};
                pq.push({newInterchanges, newTime, nextStation, color});
            }
        }
    }

    cout << "Destination not reachable from source" << endl;
}

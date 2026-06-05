# Group-Project-2_Smart-City-Traffic-Simulation

## 1. Project Overview

This project is a C++ console-based Smart City Traffic Simulation System.  
It simulates different types of vehicles moving through a city map made of intersections, two-sided road segments, traffic lights, and accident events.

The system supports:

- Different vehicle types: Car, Bus, Truck, Emergency Vehicle
- Vehicle speed and priority
- Two-sided road segments, where each direction has its own road ID and properties
- Traffic lights placed at intersections
- Accident data that can reduce road speed or block a road
- Route planning using Dijkstra's Algorithm
- File input/output for data loading and result saving

---

## 2. Project Structure

Group-Project-2-main/
├── main.cpp
├── include/
│   ├── AccidentManager.hpp
│   ├── CityMap.hpp
│   ├── FileManager.hpp
│   ├── Intersection.hpp
│   ├── Road.hpp
│   ├── RoutePlanner.hpp
│   ├── Simulation.hpp
│   ├── TrafficAccident.hpp
│   ├── TrafficLight.hpp
│   └── Vehicle.hpp
├── src/
│   ├── AccidentManager.cpp
│   ├── CityMap.cpp
│   ├── FileManager.cpp
│   ├── Intersection.cpp
│   ├── Road.cpp
│   ├── RoutePlanner.cpp
│   ├── Simulation.cpp
│   ├── TrafficAccident.cpp
│   ├── TrafficLight.cpp
│   └── Vehicle.cpp
├── data/
│   ├── roads.txt
│   ├── vehicles.txt
│   └── accidents.txt
├── Makefile
└── README.md

## 3. How To Compile
Using g++, type in console: 
g++ -std=c++17 -Iinclude main.cpp src/*.cpp -o traffic_sim.exe
Then type in console: 
.\traffic_sim.exe
Then enter number of simulation step,
output will be stored in simulation_result.txt

## 4. Input File Formats

roads.txt
Each line represent one directional road segment:
RoadID FromIntersection ToIntersection Length(m) SpeedLimit(km/h) Blocked(0/1)
Example Input:
R1_AB I1 I2 100 60 0
R1_BA I2 I1 100 60 0
Note: Roads are stored as two-sided, so segment from I1 to I2 are different from I2 to I1. This allows different speed limits, block status,...

vehicles.txt
Each line represent one vehicle:
VehicleID VehicleType StartIntersection DestinationIntersection
Example Input:
C01 Car I1 I3
E01 Emergency I1 I4
Explanation for each type of vehicle:
| Vehicle Type |   Speed | Priority |
| ------------ | ------: | -------: |
| Car          | 60 km/h |        1 |
| Bus          | 40 km/h |        1 |
| Truck        | 30 km/h |        1 |
| Emergency    | 80 km/h |        3 |

accidents.txt
Each line represents one accident event:
AccidentID RoadID Severity StartStep DurationSteps BlockRoad(0/1)
Example Input:
A01 R5_BD Medium 3 10 0
A02 R3_CD High 7 8 1
A03 R2_BC Low 5 15 0
Explanation:
-Severity can be Low, Medium, or High.
-StartStep is the simulation step where the accident begins.
-DurationSteps is how many steps the accident lasts.
-BlockRoad = 1 means the road is fully blocked.
-BlockRoad = 0 means the road is not fully blocked, but speed is reduced.

## 5. Simulation Algorithms

Time-Step Simulation
At each simulation step:
Update all traffic lights.
Update accident status.
Apply road blockage or speed reduction.
Update each vehicle.
Move vehicles based on speed and road length.
Stop vehicles at red lights.
Allow higher-priority vehicles to pass first.
Reroute vehicles if the next road is blocked.
Print and save the simulation status.

Dijkstra's Algorithm
The city map is represented as a graph:
Intersections are nodes.
Road segments are directed edges.
Road cost is based on travel time.
Travel time = road length / effective speed
Blocked roads are ignored by the route planner.

## 6. Output

Program print output to console, also save to simulation_result.txt
Each output include: 
Traffic light states
Active accidents
Vehicle status
Current road
Vehicle position
Completed vehicle count
Average waiting time
Blocked roads
Congested roads

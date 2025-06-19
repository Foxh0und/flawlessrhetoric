---
layout: post
title: "Railway Route Finder"
description: "An implementation of Dijkstra"
tag: Algorithm
---

### Problem Description
The local commuter railroad services a number of towns in Kiwiland. Because of monetary concerns, all of the tracks are 'one­way.' That is, a route from Kaitaia to Invercargill does not imply the existence of a route from Invercargill to Kaitaia. In fact, even if both of these routes do happen to exist, they are distinct and are not necessarily the same distance!

The purpose of this problem is to help the railroad provide its customers with information about the routes. In particular, you will compute the distance along a certain route, the number of different routes between two towns, and the shortest route between two towns.

#### Classes

- Track  
  Each track represents both a destination, and a distance. To avoid coupling issues, it does not contain a reference to a Station object, only a char to represent the destination's station.

- Station  
  Each station contains a character label, and maintains a list of tracks which represent possible destinations to which it links.

- Railway  
  A railway is the data structure representing the graph. It maintains a list of stations, has the ability to create more stations, and create more tracks.

- Route  
  A route contains a list of tracks, an origin (from which the track originated), and an overall distance (cost) of all the tracks combined. It contains a formatted ToString method to correctly show the route. It maintains the number of stations visited as well.

- Conductor  
  This is the controller class, it contains the algorithms, maintains a railway, and informs the user of both questions and their answers. It takes a blueprint file to both build it's railway, and maintain it's question list.

- BluePrint  
  A BluePrint handles the parsing of the input file, it assembles a list of stations and parses the questions into a list of required variables for the conductor, and their question type, as specified by the QuestionType enumerator. It uses many regular expressions to both extract the questions, and determine what variables are required.

### Algorithm
Upon analysis of the problem description and a visualisation of the map, it is clear to see that this requires an implementation of Dijkstra's Shortest Path Algorithm [1]. The maximum distance questions build upon this, and limit routes where the limit reaches and exceeds the maximum, sorting the open list (list of routes to search) with the distance ascending. Questions that involve a maximum or exact number of stations are treated similarly, however their open list is sorted based upon the number of stations visited descending.

![Dijkstra](https://upload.wikimedia.org/wikipedia/commons/5/57/Dijkstra_Animation.gif)
[2] Dijkstra Visualisation

#### Assumptions:

1. As per the document
	"For the test input, the towns are named using the first few letters of the alphabet from A to D", however, the following line lists the letter "E", so it assumed that E is included. Regardless, the program has been built to handle 26 stations, represented by each alphabetical letter.

#### Input
The input file is relatively specific.

Each time you wish to input a track (which will in turn create both the stations), it must be prefixed with "Graph: ". Each entry must have two capital characters, for the origin and destination station, followed by a number for the distance. Each entry must be seperated by a comma. There may be multiple graph entries.

EG: "Graph: AB5, CD3, GH12".

Each question must begin with a number followed by a period ("1."), followed by the question and finalised with a period. Anything after the second period will be ignored.

There are five question types.
	1. Direct Path: The distance for a direct path for n number of stations.
	2. Shortest Path: The shortest path between two stations.
	3. Maximum Stops: The number of paths between two points with a maximum number of stops.
	4. Exact stops. The number of paths between two points with a exact number of stops specified.
	5. Maximum distance. The number of paths between two points with a maximum distance.

EG:
	1. The distance of the route A-B-C.
	2. The length of the shortest route (in terms of distance to travel) from A to C.
	3. The number of trips starting at C and ending at C with a maximum of 3 stops.
	4. The number of trips starting at A and ending at C with exactly 4 stops
	5. The number of different routes from C to C with a distance of less than 30.  

#### Unit Testing
All unit testing was completed using Visual Studio's unit testing abilities with the NUnit Unit Testing Framework.

### Operating Instructions
You may wish to either compile the code into an executable, or run it in Visual Studio.

If an executable is used, then the first (and only) command line argument after the executable is the filename for the input.

If Visual Studio is desired, then the solution must be opened, and the file placed into ..../RailwayRouteFinder/bin/Debug. The same goes for the unit testing file ("testinput.txt").

Two input files can be found inside the root directory under the folder "Input Files", "ThoughtWorksInput.txt" which are the ThoughtWorks railway specification and questions, as well as "testinput.txt", containing the input required for the unit tests.

### Future Work:
1.Investigate C# 7's new pattern matching features. The function questionParser in BluePrint uses a relatively long if else chain, however, the new C# features may be able to mitigate this and provide the ability to better write it with a case statement.

### References
- [1] T. H. Cormen, C. E. Leiserson, R. L. Rivest, and C. Stein, Introduction to algorithms. Cambridge, MA: The MIT Press, 2014.
- [2] “Dijkstra's algorithm,” Wikipedia, 26-May-2017. [Online]. Available: https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm. [Accessed: 02-Jun-2017].

The source code can be found [here](https://github.com/Foxh0und/railwayroutefinder).

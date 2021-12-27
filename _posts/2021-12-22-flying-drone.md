---
layout: single
title: Drone Flight Path Simulation Project - TSP
excerpt: "This project emulates a food delivery service at the Edinburgh University Campus. It addresses the Travel Salesman Problem. The drone used to make the deliveries have a limited battery, and the system must optimise the orders to achieve the highest value delivered. There are no-fly zone areas to increase the difficulty. They are spaces around the campus through which the drone must not fly. In this way, the drone must also improvise flight paths when required."
date: 2021-12-22
classes: wide
header:
  teaser: /assets/images/flying-drone/university_of_edinburgh.png
  teaser_home_page: true
categories:
  - Simulation
  - Travel Salesman Problem
tags:
  - Java
  - Simulation
  - Travel Salesman Problem
  - GeoJSON
  - University of Edinburgh
---

This project emulates a food delivery service at the Edinburgh University Campus. It addresses the Travel Salesman Problem. The drone used to make the deliveries have a limited battery, and the system must optimise the orders to achieve the highest value delivered. Where are no-fly zone areas to increase the difficulty. They are spaces around the campus through which the drone must not fly. In this way, the drone must also improvise flight paths when required.

![](/assets/images/flying-drone/drone.png){: class="drone_image"}

## Report PDF ##
The following document includes the complete description of the project:
  Software Architecture Description
      General Overview
  Classes Documentation
  Drone Control Algorithm
      Main Aim, Movement Restrictions, Confined Area
      Sorting Orders
      Order Selection
      Flight Simulation Heuristic
      Restaurants Heuristic
      Finding Landmarks Heuristic 
      Avoiding No-Fly Zones, Out of Confined Area & Loops
      Returning to Base
      Writing GeoJSON Files
      Drone Flight Figures
      Results and Statistics
  References

# :point_right: [**VIEW PDF DOCUMENT**](/guides-and-articles/Flying-Drone-Report/Flying_Drone_Report.pdf)     :page_facing_up:

## Software Architecture Description

The application is formed by 11 classes which are basic to perform the operations of this project. These are App, Basic Utils, Database Handler, Drone, GeoJSON Handler, HTTP Client, Line_, LongLat, Menus, Restaurants and Words Handler. Adding on, an interface, Request Utils has also been used. 
The user must input the following values to initialise the program correctly:
  - date of the year 2022 or 2023 (the deliveries simulation will run for this specific date).
  - a port number to a web server (which will contain the information of no-fly zones, landmarks, the menus of all the restaurants, and What3Words details to get coordinates).
  - a port to a database (which contains a list of orders, their details, and where the flight path information and completed deliveries must be written).

### General Overview ###

The main structure can be found in **App**, where the general pipeline of the project is developed. It is where the user inputs are read and processed into the program. This class creates 5 key Objects which are needed to make the deliveries. These are a Drone, Database Handler, a Words Handler, a GeoJSON Handler, and a Menus Object. They are basic and will retrieve the required information to do the deliveries, the no-fly zones, landmark coordinates, all the menus and their items; and the drone which will effectuate the deliveries. Having all this information, the program (specifically the Drone), is ready to start taking decisions of which, through where and how, the deliveries should be made.

The **Database Handler** is the class through which any type of interaction with the database must be done. When an instance of Database Handler is created, it connects to a derby database, sets up two tables: “DELIVERIES” and “FLIGHTPATH”; and retrieves the orders information of the date input by the user. It generates two important HashMaps “hashmap_orderNoToW3w” and “hashmap_orderNumbToItems” key to get pieces of information from the orders in a fast way. It also generates s list of all the orders numbers of a day. The class has methods implemented to create and drop tables, get the orders of a specific date, insert values, retrieve, and print all the information.

The **Drone** contains all the functions which involve taking decision of which deliveries must be done, the path it should take, and the logic used to avoid no-fly zones or getting out of the confinement region. The class contains two subclasses, Drone Movement (which will contain all the methods that are related to the displacement of the drone); and Drone Control Algorithm (dedicated to the decision making of the drone to effectuate deliveries, simulate paths and checking that the flying restrictions are being obeyed).

The abstract **Basic Utils** class contains the variables and constants used across multiple classes. This would be the input data given by the user, and a constant with the hovering angle value. Adding on, it has the method to print and save the final statistics. The GeoJSON Handler has the main task of generating an output file which will contain the flight path of the drone of that date. Adding on, it is used to read GeoJSON files from the web server which contains the information of the no-fly zones, and the coordinates of the landmarks. These are all parsed into Objects the Drone can work with to take decisions. All the requests to the web server are done through the class HTTP Client. It contains the static CLIENT attribute and the method used in all operations “make request()”. These are all called from the Request Utils Interface used by all the classes which connect to the web server (GeoJSON Handler, Menus & Words Handler).

The **Line_** class is used to define 2D line Objects by knowing two LongLat points that are contained in the line. This Object is used in multiple classes (Drone & GeoJSON Handler) to describe limits of no-fly zones, the path the drone is doing, or the direction the drone wants to move at.

All the coordinates of the project are in form **LongLat** Objects. This class contain basic functionalities used across the classes Drone, GeoJSON Handler & Database Handler. From checking distances to a new coordinate having given the angle to make the move. When an instance of LongLat is created, the given values for the geographical longitude (lng) and latitude (lat) are checked to ensure the point is valid.

The **Menus** class is used to request to the web server and parse all the information of all the restaurants that can be part of an order, the items each one sells and their prices. The web server returns this information in a JSON format. When an instance of Menus is created, it will automatically populate the two HashMaps, “hashMap_Item_Price” and “hasMap_Item_w3w”, for each restaurant with the information acquired. Another key method of the class is “getDeliveryCost()” used to sort the deliveries in App. The Restaurants class is used to parse the information retrieved in Menus class. A Restaurants Object will contain all the information of a restaurant. The key properties are stored in two HashMaps, “hashMap_Item_Price” and “hashMap_Item_w3w”, which are filled when called in the Menus constructor.

The **Words Handler** class contains all the tools required to request to the web server the coordinates used through the program in What3Words format. When an instance of Words Handler is constructed, the request to a list of locations is performed using the method “requestCoordinate()”. In this case, a list with the w3ws of the delivery locations is passed into the constructor. Each coordinate is added into an important HashMap used across the project named “hashMap_w3w_coordinates”. When ever acoordinate is needed, the HashMap is checked and if it is not found, a new request to the web server is done and the new information is added into the HashMap. With this approach, the program will only run with the information really required to process the orders of that specific date, optimising the runtimes and the program.

The **Request Utils** Interface is used to define the constants used across all the classes that make requests into the web server (GeoJSON Handler, Menus & Words Handler). Adding on, it includes the abstract method to make a JSON request of any type. This also includes GeoJSON files.

Through all these classes, the structure of the program is fully defined, and all the functionalities required in the specifications are completely covered. This distribution ensures the code to be readable and easy to maintain. The App class will contain the general process and the Drone will contain the algorithm and the methods to move through the map. Moreover, there classes which will have individual specific aims (Database Handling, Words Handling, Menus, HTTP Client & Request Utils), or a more abstract usage (LongLat, Line_, Restaurants & Basic Utils). Adding on, the methods inside the classes form atomic pieces of logic which have high relevance in their class and are used to build up to perform complex decision and calculation processes, leading to high cohesion. Another important aspect to consider is encapsulation. All the possible attributes and methods used across the program are private always, except special cases where they had to be turned into package protected. The Drone will correctly encapsulate all the possible methods, letting App just access the sorting restaurants method, requesting making all the deliveries and a returning to appleton. With all these characteristics, the project ensures a correct behaviour, readability, maintenance, and independence/encapsulation of information.


## Drone Control Algorithm

### Main Aim, Movement Restrictions and Confined Area ###

The program must optimise the path to complete as many deliveries as possible trying to achieve the highest monetary value delivered. This is a clear example/application of the Travel Salesman Problem as the order of deliveries and the way they are processed must be done trying to minimise the flight path of the drone. All these will take place in a limited space (Square) delimited by 4 restaurants coordinates of the city of Edinburgh. The drone must remain at all time in this area. There will be areas/building the drone must not fly through called no-fly zones. These will be restricted areas or obstacles the drone must avoid while completing the deliveries. The system to avoid getting out of the confined area or into no-fly zone areas is explained at the entry **Avoid No-Fly Zone Areas, Out of Confined Area & Loops**. The drone has a battery which allows a total of 1500 fly movements and algorithm must ensure that there will be enough battery to return the base at the end of the day. This restriction must be considered as some deliveries might not take place as there is not enough battery to accomplish them or to return afterwards to Appleton Tower. Furthermore, the movements are restricted to have a distance of 0.00015 distance units and the angles to get new positions will be in the range [0, 350]. Only those angles divisible by 10 are valid. In conclusion, the program must ensure the correct movement of the drone through the space, avoiding the no-fly zones or getting out of the confined area, and try to achieve the highest monetary value delivered of that date.

### Sorting Orders ###

The method of evaluating the drone’s performance is through the percentage monetary value delivered of the date. A greedy algorithm was used to try optimising the selection of the orders to deliver. The program retrieves the information necessary to calculate the cost of each delivery and sorts them in decreasing order by the money to be gained in the delivery. This process does not ensure the best combination of orders, but in most cases, it will perform correctly considering the small needed computational effort to do this task.

### Order Selection ###

When an order is taking place, the drone will simulate the path that it will take. This process allows calculating the cost of battery movements that the delivery will have if the flight is completed. These allows the drone to know if the delivery can be safely completed or not. The drone will run two simulations in this process:
 - Flight path to the destination and Returning from the destination to Appleton. In this way it can be checked if the remaining battery is enough to make the full delivery and return to the base. 
 - In the case it cannot, the drone will simulate its way back to Appleton and then complete the flight.

### Flight Simulation Heuristic ###

Whenever a flight is being simulated the following algorithm will take place to go through the landmarks if needed. If no landmarks are needed the method go to() will be used (Also described in the following pseudo code) to reach the final position:

```
  get path through landmarks(to place, path) =>
      good direction(drone position, location) => check if no instersections with no-fly zones are found between the drone position and the location
          landmark = get landmark(drone position, location) => In case there is, check all the landmarks reachable from the drone's position and that have a clear path to the location. Return the one which implies the least number of steps.
          go to(landmark, path) => while the drone is not close to landmark, make a move and store the information in path.
      
  go to(to place, path) =>
      while drone position is not close to place
          drone position = make a move (drone position, to place) => gives a new coordinate by calculating the optimum angle
```

This process is used to reach a destination point, going through landmarks if needed. It is also used to get back to Appleton. The key aspect of this implementation is that the path is saved. Then, the flight is checked to ensured that it can be completed. Finally, the ending process would just be including each of the entries of the path into the database and the GeoJSON file. The algorithm is simulations all the possible paths that can be created by going through each landmark, despite adding computational cost, this gives the benefit of choosing the shortest flight, avoiding wasting movements in unnecessary longer paths. 

### Restaurants Heuristic ###

When a simulation of a delivery flight is taking place, two different cases can occur. The delivery implies going through one, or two restaurants. In the first case, there path is immediately traced, but in the case of two restaurants, the drone could take two different paths. From Origin to Restaurant 1, to Restaurant 2, to Destination; or from Origin to Restaurant 2, to Restaurant 1, to Destination.

The key aspect of the method is that, when having multiple restaurants to fly through, the drone can compare and decide in which order it should fly to the restaurants; and complete the delivery in the most optimum number of steps. 

### Finding Landmarks Heuristic ###

In order to get correctly to whichever location, the system will try performing a simulated path to the destination directly. Not always this path is possible and intersection with a no-fly zones will take place. To solve this issue, the system will try searching for a landmark to which the drone will have a clear flight and, at the same time, from which the destination can be reached. The method get landmark() will go each landmark and compare which of the valid landmarks generates the shortest distance. In the case no landmarks are found to reach the destination, the system will just try going through the optimum path and if it crashes with a nofly zone, it will try adapting the movements angles to avoid it using the methods described in the Avoiding No-Fly Zones entry.

### Avoiding No-Fly Zones, Out of Confined Area & Loops ###
The method followed to avoid no-fly zones has been done by checking in each movement that the drone does. If there is an intersection with a no-fly zone perimeter line, the angle of the movement will immediately be corrected. The angle will be generated using the current position of the drone and the final coordinate to be reached. In this way, the final location will be reference and the angle 𝛼 will try pointing always to move into that direction. Knowing this, it has been decided that going backwards would be counter efficient and will create a loop as the following movement would be returning to the conflict position. Therefore, the approach chosen to correct angles is to check, the angles in the range +/- 120º from the suggested initial one 𝛼. The process will check first all the angles to the left till 𝛼 + 120º , increasing the angle 10 by 10; and if no valid option is found, the angles to the right will be checked, 𝛼 − 120º, reducing the angle -10 by -10. 

Finally, it should be added that if the drone does not find any direction to
go to, the system will operate the initial 𝛼 move and searches an option to get out. As all these steps are simulated, in the worst-case scenario, the drone will go into the no-fly zone and try getting out. This will cause the drone to intersect continuously with the inner lines of the no-fly zone, generate a loop.

To avoid loops, the simulation stops immediately when the method go to() has operated the maximum number of available steps plus 1 (15001). This breaks possible loops generated if the drone gets stuck in an area with no way out. The path is later one compared with the number of available moves (maximum 1500) and immediately the order will be skipped. This method avoids those orders that could provoke runtime exceptions. 

It should be remarked that when a movement is going to be done and the angles are being checked, the new position is also checked to ensure that is confined. Therefore, all the implementation to avoid the no-fly zones and the logic used to break loops is applied directly over this restriction and when this issue arises, the drone directly does not effectuate the movement.

### Returning to the Base ###
As mentioned in the entry Main Aim, Movement Restrictions, & Confined Area the drone must ensure it can return to Appleton without running out of battery (before it reaches 1500 movements effectuated). The drone will simulate the path to the base, similarly to a delivery, but not going through restaurants. The same methods get path to landmarks(), get landmarks() and go to() will be used in this process. Finally, the path simulated is committed into the flightpath database table and into the GeoJSON file.

### Writing a GeoJSON File ###
When the drone returns to the base, the system must generate a GeoJSON file which will contain a FeatureCollection with a LineString with all the coordinates through which the drone has flown. This file will be named “drone-DD-MM-YYYY.geojson”, where DD is the date, MM is the month, and YYYY the year input by the user. The program will use the method write GeoJSON() of the GeoJSON Handler class.

### Drone Flight Figures ###

 - Figure 1. Simulation of deliveries completed on 11/06/2022. 
![Simulation of deliveries completed on 11/06/2022](/assets/images/flying-drone/drone_flight.png "Simulation of deliveries completed on 11/06/2022")
  * Deliveries 21/21
  * Movements 1446
  * Money Made 19785

 - Figure 2. Simulation of deliveries completed on 11/12/2023.
![Simulation of deliveries completed on 11/12/2023](/assets/images/flying-drone/drone_flight2.png)
  * Delivereis 25/27
  * Movements 1500
  * Money Made 19950


### Generating Submission Files ###

To improve the methodology to generate the files from which the results and statistics will be done & easily generate the 12 submission files, a python script files and statistics generator.py has been written to run the jar file of the project. This script will contain multiple methods that allow running the program in a loop and generate files where DD = MM of a year, files of all months for a given DD, files of all a month. The ending results of each date (completed deliveries out of total, total number of steps, money made out of total possible, and percentage of money achieved) is also stored into a “drone-DD-MM-YYYY-results.txt”. Important notice, the ports used in the file are the default ones. Consider changing them if necessary. The main function is set to generate the submission files requested for the years 2022 and 2023.

### Results and Statistics ###
To get an accurate idea of the performance of the algorithm, the student took three sampled average percentage monetary values delivered by the service:
  * Getting the sampled average percentage monetary values of all the 11ths of the database. (all months & all years):
    - Total number of dates: 24 (all 11-MM-YYYY)
    - Total number of deliveries completed: 358 / 372 – 96%
    - Total value of the deliveries: 299641 / 303736 (pence)
    - ***sampled average percentage: 99%***
  * Getting the sampled average percentage monetary values of all the dates in December 2023 (the month with more deliveries):
    - Total number of dates: 31 (all DD-12-2023)
    - Total number of deliveries completed: 713/ 837 – 85%
    - Total value of the deliveries: 617019 / 660381 (pence)
    - ***sampled average percentage: 93%***
  * Getting the sampled average percentage monetary values of all the dates from the last 2nd half of 2023 (1stJune – 31st December):
    - Total number of dates: 214 (all DD-MM-2023, MM from 6 to 12)
    - Total number of deliveries completed: 4705/ 5137– 92%
    - Total value of the deliveries: 3956095 / 4100616 (pence)
    - ***sampled average percentage: 97%***
    - *Movements Done: 309355*

*Interesting fact: the runtime of the process was of exactly 8 mins (start 16:25:00 – end 16:33:00):*  
  *June 67s, July 67s, Aug 70s, Sep 67s, Oct 72s, Nov 68s, Dec 69s (with slight possible fluctuation beween them).*

**In any of these processes, the drone algorithm got stuck in a loop generating runtime exceptions.**

## References ##
* Informatics Large Practical, Stephen Gilmore and Paul Jackson – School of Informatics, University of Edinburgh, 2021 – 2022.
* GeeksforGeeks. (2013). How to check if two given line segments intersect? [online] Available at:
[https://www.geeksforgeeks.org/check-if-two-given-line-segments-intersect/](https://www.geeksforgeeks.org/check-if-two-given-line-segments-intersect/) [Accessed 22 Nov. 2021].
* docs.mapbox.com. (n.d.). services-geojson API. [online] Available at: [https://docs.mapbox.com/android/java/api/libjavageojson/5.8.0/](https://docs.mapbox.com/android/java/api/libjavageojson/5.8.0/) [Accessed 19 Dec. 2021].


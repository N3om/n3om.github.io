---
layout: single
title: Drone flightpath simulation
excerpt: "This project emulates a food delivery service at the Edinburgh University Campus. It addresses the Travel Salesman Problem. The drone used to make the deliveries have a limited battery, and the system must optimise the orders to achieve the highest value delivered. There are no-fly zone areas to increase the difficulty. They are spaces around the campus through which the drone must not fly. In this way, the drone must also improvise flight paths when required."
date: 2021-12-22
classes: wide
header:
  teaser: /assets/images/flying-drone/drone.png
  teaser_home_page: true
  icon: /assets/images/university_of_edinburgh.jpg
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

![](/assets/images/flying-drone/drone.png){: width="150" }

![Simulation of deliveries completed on 11/06/2022](/assets/images/flying-drone/drone_flight.png)

![Simulation of deliveries completed on 11/12/2023](/assets/images/flying-drone/drone_flight2.png)

This project emulates a food delivery service at the Edinburgh University Campus. It addresses the Travel Salesman Problem. The drone used to make the deliveries have a limited battery, and the system must optimise the orders to achieve the highest value delivered. There are no-fly zone areas to increase the difficulty. They are spaces around the campus through which the drone must not fly. In this way, the drone must also improvise flight paths when required.

## Report PDF
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

  * [PDF DOCUMENT HERE](https://github.com/N3om/guides-and-articles/blob/main/Flying-Drone-Report/Flying_Drone_Report.pdf)

## Software Architecture Description

The application is formed by 11 classes which are basic to perform the operations of this project. These are App, Basic Utils,
Database Handler, Drone, GeoJSON Handler, HTTP Client, Line_, LongLat, Menus, Restaurants and Words Handler. Adding on,
an interface, Request Utils has also been used.
The user must input the following values to initialise the program correctly:
  - date of the year 2022 or 2023 (the deliveries simulation will run for this specific date).
  - a port number to a web server (which will contain the information of no-fly zones, landmarks, the menus of all the
    restaurants, and What3Words details to get coordinates).
  - a port to a database (which contains a list of orders, their details, and where the flight path information and completed
    deliveries must be written).



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

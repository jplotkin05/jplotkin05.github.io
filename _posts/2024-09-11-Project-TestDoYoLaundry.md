---
layout: post
title:  "Project: TestDoYoLaundry"
---
[Link to site](https://www.testdoyolaundry.com)

## Background
During the fall semester of my freshman year, I noticed that the washer and driers in my dorm had some "smart features." 

The machines are connected to a companion app that allows users to view the status of machines and even reserve them for use.

I was fascinated by this! However, I was not so fascinated by the interface of the app. At the same time, I began to feel a rush of ideas of other things that could be with the data.

I managed to gain access to the laundry system API and from there, I developed this web interface with my roommate to help students track their laundry and make the process more efficient!

## What does it do?
The application allows for users to view current status of laundry machines at any particular laundry room at the University of Maryland.

Users can also view helpful insights on what day most people do their laundry and what time. They can see which dorms do the most laundry and how long people leave their laundry in the machines.

A basic feature that adds sort of a competitive edge to laundry is on the home page where a daily leaderboard ranks laundry rooms by most loads per machine.

## Technical Details
This application pulls out my knowledge from my project first project. This site is written with HTML/CSS/JS and PHP. The entire site is powered by a Linux, [Apache, MySQL, PHP (LAMP) software bundle](https://en.wikipedia.org/wiki/LAMP_(software_bundle)) on a Google Cloud Server running on Ubuntu. All the data comes from a data stream by the laundry company and is stored into a MYSQL relational database. A script created in Rust formats the data from the stream accordingly and inserts it into the database. For additional details, please contact me.

## Future Plans
There are several plans in mind going forward:
1. Allow users to subscribe to receive notification when a particular machine is done or available.
2. Implement an insights page for individual laundry rooms.
3. Generate insights on the environmental impact of the laundry system at the University of Maryland.
Stay tuned...
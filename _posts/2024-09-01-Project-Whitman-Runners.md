---
layout: post
title:  "Project: Whitman Runners"
---
[Click Here for Repo](https://github.com/jplotkin05/vikingxctf)

## Purpose
This was one of my very first projects that I developed independently back in 2021 (Looks very messy under the hood). I developed this site to allow athletes from my high school to track their cross country and track performances and progress. 
## What does it do?
Users can search for individual athlete profiles, rank athletes based on event, class, and or season. They can also search for race results from different competitions and view custom articles. Additionally, there is a page for users to view current and future weather conditions. What makes this different from traditional weather forecasts is that I developed a "Run Index" which is a score out of 5 on how optimal it is to run. I tried to be as objective as possible with setting this metric up based on extensive research each athlete will always have their own preferences. I am thinking of implemented a large language model to gauge user preferences and generate a dynamic score based on user desires. 

## Technical Details
This site is written with HTML/CSS/JS and PHP. The entire site is powered by a Linux, [Apache, MySQL, PHP (LAMP) software bundle](https://en.wikipedia.org/wiki/LAMP_(software_bundle)) on a Google Cloud Server running on Ubuntu. All athlete data and race performances are stored in a relational database that I configured using the phpMyAdmin interface. I scraped all the race result data with a custom Python script I wrote to automatically collect the data and push it to the database.

## Future Plans
I am in the works of developing a full scale database platform to serve athlete data to high school and collegiate runners across the country. I want to leverage [large language models (LLMs)](https://en.wikipedia.org/wiki/Large_language_model) to help provide guidance and prediction to athletes. This current site has not been attended to in a while and so it is not up to date and the code style and format deserves some much needed attention! 😅 Stay tuned....
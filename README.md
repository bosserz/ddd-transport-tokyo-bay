# Data-Driven Design for Autonomous Bus in Tokyo Bay
This is a section of the whole study of Data Driven Design for Future Transportation System in Tokyo Bay project.
## Introduction
Japan's urban landscape is evolving rapidly, and the Tokyo Bay Area—particularly districts such as Toyosu, Shinonome, and Ariake in Koto Ward—illustrates both the challenges and opportunities of modern transit demands. While Japan's aging population (with nearly 30% of residents aged 65+ as of 2025[1]) underscores the need for accessible mobility solutions, urban residents of all ages require a transit system that is efficient, inclusive, and flexible.

Existing transit options, such as the Yurikamome line, provide valuable connectivity but often fall short in offering fine-grained, intra-area mobility that supports short trips—whether it be heading to hospitals, markets, or workplace clusters. In response, this project proposes a resilient, sustainable transportation system designed to serve the diverse needs of the community. By leveraging autonomous electric buses, the system aims to enhance accessibility, decrease car dependency, and build in the flexibility necessary to support elderly passengers without compromising service for younger residents.

## Objective
The objective of this module was to design an autonomous electric bus system serving the neighborhoods of Toyosu, Shinonome, and Ariake, with a focus on enhancing intra-area mobility for all residents—particularly in accommodating the needs of an aging population. The design process aimed to:
  1. identify high-demand bus stop locations using GPS-based origin-destination (O-D) data
  2. optimize a route that efficiently connects these stops
  3. develop a demand-responsive timetable aligned with observed travel patterns
  4. propose a custom electric bus solution to support both environmental sustainability and practical operability
The system is designed with flexibility for future enhancements to ensure long-term resilience.

## Data & Processing
The dataset used in this study consisted of GPS-based mobility data capturing the movement of individuals across the Tokyo metropolitan area over a 7-day period from May 21 to May 27, 2023. Each record included geolocation points (latitude and longitude), timestamps, and inferred modes of transportation such as “in_vehicle,” “on_foot,” and “on_bicycle.”
For this module, the data was filtered to include only trips that both originated and ended within the Toyosu area (35.62°–35.67°N, 139.77°–139.83°E), isolating intra-area motorized travel. Although the full dataset opens opportunities for future analysis of trips entering or exiting the area, this module focuses exclusively on internal mobility.

The preprocessing steps included:
  - Filtering transportation mode to retain only "in_vehicle" trips.
  - Identifying trip endpoints using the first and last recorded coordinates of each trip.
  - Transforming timestamps into two temporal features: day of the week and hour of the day (0–23), enabling spatiotemporal aggregation of passenger flows.
  - This cleaned and structured dataset served as the foundation for identifying high-demand bus stop locations and developing a time-sensitive routing and scheduling model.

## Methodology
The methodology followed a data-driven approach:
1. Stop Identification: Origin-destination points derived from GPS data were clustered using K-means to identify high-density candidate stops within Toyosu, Shinonome, and Ariake. To ensure inclusivity, additional stops were added near elderly-relevant points of interest (e.g., Showa University Hospital).

2. Route Optimization: The street network was extracted using OSMnx, and the optimal route connecting the selected stops was computed using NetworkX’s approximate Travelling Salesman Problem (TSP) solver. The expected result was a closed-loop route, designed to minimize travel time while covering major demand clusters, with an estimated loop duration of ~40 minutes. The result from TSP will be refined if needed.

3. Scheduling: Passenger demand patterns—extracted from the processed heatmap data—were used to define service frequency. The schedule adapts based on hourly passenger volumes:
  - 15-minute frequency for peak demand (>1,000 passengers/hour),
  - 20–30 minutes for moderate demand (500–1,000),
  - 60 minutes for low demand (<250).
This approach ensures responsiveness to temporal ridership fluctuations.

4. Bus Specification: A custom electric autonomous bus was specified to meet sustainability and operational needs based on the results from previous steps.
The entire process was carried out in Python using several packages, including Scikit-learn for KMeans clustering, Python-TSP for solving the Traveling Salesman Problem, and OSMnx and NetworkX for network analysis. The Jupyter Notebooks developed for this analysis are available in this GitHub repository: https://github.com/bosserz/ddd-transport-tokyo-bay.git.

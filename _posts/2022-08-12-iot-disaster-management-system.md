---
title: Disaster Management System for Floods using IoT and ML
date: 2022-08-12 00:00:00 +0800
categories: [Projects, ML]
tags: [ml, iot]     # TAG names should always be lowercase
subsection: "Personal Projects"
author: <author_id>
mermaid: true
pin: false
image: /assets/images/Thumbnail/iot_rsz.jpg
---

[![Bhaswanth-A/IoT-Disaster-Management-System-for-Floods - GitHub](https://gh-card.dev/repos/Bhaswanth-A/IoT-Disaster-Management-System-for-Floods.svg)](https://github.com/Bhaswanth-A/IoT-Disaster-Management-System-for-Floods)

# Introduction

Floods are the most catastrophic and cataclysmic events of all-natural disasters. The World Meteorological Organization has stated that out of all the disasters in the world, floods are the most severe. Specifically, in India, about 12% of the land is vulnerable to flood conditions. Heavy unprecedented rainfall results in floods bringing everyday life to a standstill. Most floods occur during monsoons; however, floods can also occur due to dams & levees breaking, which can be triggered by thunderstorms, cyclones, and low-pressure regions.

With substantial technological innovations in sensing systems, communication networks, cloud computing, machine learning, and data analytics, it is readily possible to develop an integrated flood disaster management system that can effectively alert the flood affecting regions. Internet of Things (IoT) is one such technology that can not only help predict the occurrence of a flood but can also help provide an emergency floor plan using an AI approach.

This project uses an IoT framework with AI to develop flood monitoring, prediction, and post-flood management systems.


# Basic Outline

## Occurrences

### Prediction

A model will be designed to monitor the environmental parameters used for flood disaster prediction. The environmental parameters like temperature, relative humidity, atmospheric pressure, rainfall, etc., are sensed by an array of sensors, and the measured data is sent to the microcontroller via a suitable communication standard. Further, the relationships between the input data received and the output rainfall are modeled using ANN (Artificial Neural Network) techniques. Continuous monitoring of changes in the environment will be done by updating the old values with new ones after a specified time interval. A flood event is thus predicted using the ANN model, which will alert people to upcoming disasters according to the increase of rainfall and corresponding rising water levels in low-lying areas near river flow.

### Emergency Floor Plan

People routing in an emergency is an exciting and complex problem due to the several challenges that affect the result of the provided solution. We would have to consider minute-by-minute changes in the emergency environment, which would require dynamic generation and management of the evacuation routes. Some possible hazards might include structural damages and collapses that block the route or limit route capacity while evacuating, low visibility, too much water logging, etc. Taking into consideration all the possible challenges, clear objectives have to be decided for the same.

## Communication

### Use of Intermediate Nodes

ESP32 nodes can be used for effective communication of the sensor information and traversing floor plan information in times of an emergency. The range of a typical ESP32 board is limited to around 240 meters but can be extended up to 10km using a directional antenna, with a 4-kilometer range of efficient communication. When implemented over an extensive area, intermediate nodes can be utilized to deliver the information to the destination nodes and ensure data syncing. 

### Deployment

The smart buoy being built can be deployed in multiple ways depending on the city and the area where it is being placed. The purpose is flood detection. In coastal areas where floods can be caused by the water in the sea i.e. tsunamis and cyclones , the smart buoy can be placed near the sea region , where it can predict the chances of a flood judging the sea levels. Smart buoys can be deployed using drones , in case the water enters the cities.

These buoys will communicate the info from various areas and can be used to analyze the situation in the entire city at once.

## End goal : To achieve efficient flood management

### Rescue Teams/Police Station

Rescue teams and police stations can act faster in case floods are predicted and an overall information is presented to them. The smart buoy can aid in better application of emergency procedures and can decrease the loss caused due to these disasters by a large scale.

### News Media

The media can alert the citizens in case a flood is approaching and warn the areas which are at a high risk. The timely updates can help in efficient evacuation.

### Internet and SMS

The Internet and SMS play a major role in any disaster management situation. Notifications regarding the routes to be taken during evacuation, roads blocked, structural damage in various areas can be made available to the public.

Requests for help so that the rescue teams arrive can also be made , by providing an emergency number to which sms sent will alert the authorities about the problem there.

# Protocols and Architecture

## IEEE 802.11

IEEE 802.11 is a set of LAN standards, and specifies a set of MAC and PHY protocols for implementing Wireless Local Area Networks (WLAN) communication. It supports wireless connectivity for portable, moving devices (Wi-Fi devices) within a local area. Compared to the

IEEE 802.16 standard, IEEE 802.11 standard supports shorter distance and higher data rate for the Wi-Fi devices. The data rate supported by IEEE 802.11 ranges from 11 Mbps to more than 1 Gbps.

802.11b, 802.11g, and 802.11n-2.4 utilize the 2.400–2.500 GHz spectrum, one of the [ISM bands](https://en.wikipedia.org/wiki/ISM_band). 802.11a, 802.11n, and 802.11ac use the more heavily regulated 4.915–5.825 GHz band. Each spectrum is subdivided into channels with a center frequency and bandwidth.

The 2.4 GHz band is divided into 14 channels spaced 5 MHz apart, beginning with channel 1, which is centered on 2.412 GHz. The latter channels have additional restrictions or are unavailable for use in some regulatory domains.

## IEEE 802.15.4

Unlike the 802.11 based networks, the IEEE 802.15 family of standards support short-range, low-power, low-rate communications.

The IEEE 802.15.4 standard is designed to support low-data rate wireless connectivity with

fixed, portable, and mobile devices with very limited battery consumption and relaxed throughput requirements. The standard through its energy-efficient link-layer technologies sup-

ports networking of the constrained IoT devices. It supports longer network lifetime with periodic sleep cycles, low-rate, and low-power communications. Hence, it is one of the most widely adopted link-layer technologies for building IP-based IoT networks.

Standards like ZigBee, WirelessHart and ISA100.11a define higher-layers on top of the IEEE 802.15.4 standard.

IEEE 802.15.4 specifies three frequency bands of operation: 868 MHz, 915 MHz and the 2.4GHz unlicensed industrial, scientific and medical (ISM) band.

## IEEE 802.11ah

IEEE 802.11ah is a wireless networking standard that uses 900 MHz license-exempt bands to provide extended-range [Wi-Fi](https://en.wikipedia.org/wiki/Wi-Fi) networks, compared to conventional Wi-Fi networks operating in the 2.4 GHz and 5 GHz bands. It also benefits from lower energy consumption, allowing the creation of large groups of stations or sensors that cooperate to share signals, supporting the concept of the [Internet of things](https://en.wikipedia.org/wiki/Internet_of_things) (IoT). The protocol's low power consumption competes with [Bluetooth](https://en.wikipedia.org/wiki/Bluetooth) and has the added benefit of higher data rates and wider coverage range.

The sensor network standards (such as ZigBee, RFID, or Bluetooth) work over relatively short distances (i.e., tens of meters), with low data rates and [low energy consumptions](https://www.sciencedirect.com/topics/computer-science/lower-energy-consumption). On the other hand, standards like GPRS, LTE, WiMAX, etc., work over long distances and provide [high throughput](https://www.sciencedirect.com/topics/computer-science/high-throughput); however, they consume more energy, and demand an expensive and fixed infrastructure of [base stations](https://www.sciencedirect.com/topics/engineering/basestation) with proper line of sight. Owing to its [low power consumption](https://www.sciencedirect.com/topics/engineering/low-power-consumption), the IEEE 802.15.4 is a suitable standard for many IoT applications. However, it is not suited for facilitating communication among a large number of IoT devices or for covering large areas.

The 802.11ah standard enables single-hop communication over distances up to 1000 m. Relay Access Points can be used to extend the connectivity as well. The 802.15.4 standard, with a maximum range of 100m, alone cannot provide a communication framework for a larger coverage range.

The 802.15.4 standard usually operates in the unlicensed 2.4 GHz band which can accommodate data rates up to 250 Kbps. On the other hand, the 802.11ah utilizes the sub-1 GHz license-exempt bands to provide an extended range to Wi-Fi networks.

You can refer to sections 2.3, 2.5, 3.2, 3.3, 3.4 from [here](https://www.sciencedirect.com/science/article/pii/S2405959516300650) for more information.

# Devices

## Utility

### Sensors

Reference - [https://flood.network/](https://flood.network/)

1. Smart Cameras - Intelligent image processing and pattern recognition algorithms for coastal zone management, forecasting tidal waves and detecting overtopping waves, detecting smoke and fire caused by a flood, etc.
2. Smart Buoys - Can be used to detect water level, flow velocity, tidal waves, etc. The floating object can carry a GPS and an acceleration sensor. Any sudden rise or gradual change in water level can be instantaneously used to alert nearby people through the web.
3. Water Level Sensors - Used to measure water level in real-time near dams, rivers, reservoirs, etc.
4. Weather Sensors - They measure humidity, wind speed, amount of rainfall, air pressure and temperature, the data from which can be used to predict the occurrence of a flood.
5. Navigation Platforms - Real-time navigation platforms used by drivers/common people can provide hazard reports, making it possible to categorize the hazard into subcategories, such as a small flood, etc.

# Creating the Mesh Network and Testing

The initial setup consists of 9 ESP8266 boards, each loaded with the code and path-planning algorithm.

*Code can be found on Github*

The operating voltage range on an ESP8266 board is 3V-3.6V. The board comes with a low-dropout (LDO) voltage regulator to keep the voltage steady at 3.3V. It can reliably supply up to 600mA of current and has an operating current of 80mA. Power to the NodeMCU can be supplied via its USB connector, which is also used to load code into the board. Alternatively, we can use a 5V power supply by connecting it to the Vin pin in order to power up the board and its peripherals.

The NodeMCU will not be connected to any other external electronic components but will just act as nodes in the mesh.

Once all the boards are powered up, they will connect to a mesh of the following specifications (as directed by the code):

- MESH_SSID "disasterManagement”
- MESH_PASSWORD "password”
- MESH_PORT 5555

## Power connections and Hardware Setup


## Free Space Path Loss

Free Space Path Loss (FSPL) is the loss in signal strength of an electromagnetic wave (WiFi Signal in the present case). Free-space path loss is proportional to the square of the distance between the transmitter and receiver, and also proportional to the square of the frequency of the signal.

$$
\begin{align*}FSPL(dB)=10\log_{10}((\frac{4{\pi}df}{c})^2)\\=20\log_{10}(\frac{4{\pi}df}{c})\end{align*}\\\begin{align*}=20\log_{10}(d)+20\log_{10}(f)+20\log_{10}(\frac{4{\pi}}{c})\\=20\log_{10}(d)+20\log_{10}(f)-147.55\end{align*}
$$

d- distance between emitter and receiver
f- frequency of WiFi Signal (lies between 2400-2500 Hz)
c- speed of light
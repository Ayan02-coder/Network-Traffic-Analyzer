 # Network Traffic Analyzer

## Description

The Network Traffic Analyzer project aims to analyze captured network packets to identify users downloading software from blacklisted/illegal websites. It uses source and destination IP addresses to determine the geographical location of users and continuously monitors network activities to detect illegal downloads. Relevant data is stored in an encrypted database for future analysis and security enhancements.

## Features

- Monitors and analyzes network traffic for illegal downloads.
- Identifies users accessing blacklisted websites.
- Retrieves geographical location information based on IP addresses.
- Stores packet details and geographical data in an encrypted format.
- Visualizes geographical data on Google Maps.

## Technologies and Tools

- **Python 3**: Main programming language for the project.
- **MySQL**: Database for storing encrypted information.
- **GeoLiteCity Database**: Provides geographical location data.
- **PyGeoIP**: Library for correlating IP addresses to physical locations.
- **Wireshark**: Tool for capturing network packets (pcap files).
- **Ettercap**: Network security tool for intercepting and analyzing traffic.
- **Google Earth API**: Displays geographical locations of packet destinations.
- **dpkt**: Python module for parsing network packets.
- **Webbrowser**: Python module for opening URLs in a web browser.

## Installation

### Prerequisites

- Python 3.x
- Pip (Python package installer)

### Steps

1. **Clone the repository:**

   
   git clone https://github.com/Ayan02-coder/network-traffic-analyzer.git
   
   cd network-traffic-analyzer


2. **Install required Python packages:**

    pip install pygeoip dpkt

3. **Download GeoLiteCity Database:**

    Download the GeoLiteCity database from MaxMind's website and place it in the appropriate directory. Update the path       in the script accordingly.

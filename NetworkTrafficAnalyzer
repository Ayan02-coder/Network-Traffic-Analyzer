 import sys
import pygeoip
import dpkt
import socket
import webbrowser

# Dictionaries for storing IP address and lat-long positions
abc = {}
dest_abc = {}

# List of blacklisted IP addresses
black_listed_ip = ['217.168.1.2', '192.37.115.0', '212.242.33.35', '147.137.21.94']

# Authorized users
auth_users = {"root": "root", "soumil": "soumil"}

def geoip_city(ip):
    if ip in black_listed_ip:
        gic = pygeoip.GeoIP('/path/to/GeoLiteCity.dat')
        try:
            record = gic.record_by_addr(ip)
            print(f"\n[*] Target: {ip} Geo Located.")
            print(f"[+] City: {record['city']}, Region: {record['region_code']}, Country: {record['country_name']}")
            print(f"[+] Latitude: {record['latitude']}, Longitude: {record['longitude']}\n")
        except:
            print("\n********** IP Unregistered **********")

def kml_geoip_city(ip):
    if ip in black_listed_ip:
        gic = pygeoip.GeoIP('/path/to/GeoLiteCity.dat')
        try:
            record = gic.record_by_addr(ip)
            abc[ip] = f"{record['latitude']},{record['longitude']}"
        except:
            pass

def kml_dest_geoip_city(ip):
    if ip in black_listed_ip:
        gic = pygeoip.GeoIP('/path/to/GeoLiteCity.dat')
        try:
            record = gic.record_by_addr(ip)
            dest_abc[ip] = f"{record['latitude']},{record['longitude']}"
        except:
            pass

def printpcap(pcap):
    for ts, buf in pcap:
        try:
            eth = dpkt.ethernet.Ethernet(buf)
            ip = eth.data
            src = socket.inet_ntoa(ip.src)
            dst = socket.inet_ntoa(ip.dst)
            if src in black_listed_ip:
                print(f"-------------------------------------------------------------------------------------------------")
                print(f'[+] Source IP: {src} -------> Destination IP: {dst}')
                print(f"Source IP Information:")
                geoip_city(src)
            elif dst in black_listed_ip:
                print(f"=-=-=-=-=-=-=-=-=-=-=-=-=-=--=-")
                print(f"Destination IP Information:")
                geoip_city(dst)
                print(f"--------------------------------------------------------------------------------------------------")
        except:
            pass

def view_google(pcap):
    for ts, buf in pcap:
        try:
            eth = dpkt.ethernet.Ethernet(buf)
            ip = eth.data
            src = socket.inet_ntoa(ip.src)
            dst = socket.inet_ntoa(ip.dst)
            kml_geoip_city(src)
            kml_dest_geoip_city(dst)
        except:
            pass

def print_placemarks_in_kml(abc):
    for ip, coords in abc.items():
        print(f"""
  <Placemark>
    <name> SOURCE IP Address: {ip}</name>
    <styleUrl>#exampleStyleDocument</styleUrl>
    <Point>
      <coordinates>{coords}</coordinates>
    </Point>
  </Placemark>
""")

def print_dest_placemarks_in_kml(dest_abc):
    for ip, coords in dest_abc.items():
        print(f"""
  <Placemark>
    <name> DESTINATION IP Address: {ip}</name>
    <styleUrl>#exampleStyleDocument</styleUrl>
    <Point>
      <coordinates>{coords}</coordinates>
    </Point>
  </Placemark>
""")

if len(sys.argv) < 4:
    print("\nPlease enter the required arguments")
    print("\nCorrect syntax is : <FileName>.py username password cli/kml\n")
else:
    username, password, output_type = sys.argv[1:4]

    if username in auth_users and auth_users[username] == password:
        if output_type == "cli":
            with open('/path/to/pcap_file.pcap', 'rb') as f:
                pcap = dpkt.pcap.Reader(f)
                printpcap(pcap)
        elif output_type == "kml":
            print("""<?xml version="1.0" encoding="UTF-8"?>
<kml xmlns="http://www.opengis.net/kml/2.2">
<Document>
  <name>sourceip.kml</name>
  <open>1</open>
  <Style id="exampleStyleDocument">
    <LabelStyle>
      <color>ff0000cc</color>
    </LabelStyle>
  </Style>""")
            with open('/path/to/pcap_file.pcap', 'rb') as f:
                pcap = dpkt.pcap.Reader(f)
                view_google(pcap)
            print_dest_placemarks_in_kml(dest_abc)
            print_placemarks_in_kml(abc)
            print("""</Document>
</kml>""")
            webbrowser.open("https://www.google.com/maps/d/splash?app=mp", new=1)
    else:
        print("\nIncorrect username or password")
        print("\nCorrect syntax is : <FileName>.py username password cli/kml\n")

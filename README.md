import network
import socket
from machine import Pin
import time

led1 = Pin(15,Pin.OUT)
led2 = Pin(0,Pin.OUT)
led3 = Pin(4,Pin.OUT)
led4 = Pin(5,Pin.OUT)



# Connect to Wi Fi
ssid = 'Deepu'
password = 'deepu1203'

wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect(ssid, password)

print("Connecting to WiFi...", end="")
while not wlan.isconnected():
    print(".", end="")
    time.sleep(1)
print("\nConnected!")

ip = wlan.ifconfig()[0]
print("IP address:", ip)

# Create socket server
addr = socket.getaddrinfo('0.0.0.0', 80)[0][-1]
s = socket.socket()
s.bind(addr)
s.listen(1)

print("Listening on", addr)

def web_page():
    return "LED Control"

while True:
    conn, addr = s.accept()
    print("Got a connection from", addr)
    request = conn.recv(1024)
    request = str(request)

    if '/led1/on' in request:
        led1.value(1)
 
    elif '/led1/off' in request:
        led1.value(0)
    elif '/led2/on' in request:
        led2.value(1)
 
    elif '/led2/off' in request:
        led2.value(0)
    elif '/led3/on' in request:
        led3.value(1)
 
    elif '/led3/off' in request:
        led3.value(0)
    elif '/led4/on' in request:
        led4.value(1)
 
    elif '/led4/off' in request:
        led4.value(0)
      
      

    response = web_page()
    conn.send('HTTP/1.1 200 OK\n')
    conn.send('Content-Type: text/html\n')
    conn.send('Connection: close\n\n')
    conn.sendall(response)
    conn.close()

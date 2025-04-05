# NodeMCU/ ESP28266

NodeMCU is a firmware that allows you to program the ESP8266 chip with a LUA script, or C/C++ via the Arduino IDE.

The NodeMCU programming model is similar to that of Node.js, only in Lua. It is asynchronous and event-driven. Many functions, therefore, have parameters for callback functions.

While the ESP8266 is often used as a ‘dumb’ Serial-to-WiFi bridge, it’s a very powerful microcontroller on its own, as it's basically an Arduino.

# Power

The chip itself uses 3.3v, but if the board has a usb port, then it knows that usb comes with a 5v supply, so it probably has a regulator built-in to convert the 5v to the 3.3v the chip needs.

A phone charger or power bank should work just fine.

https://tttapa.github.io/ESP8266/Chap04%20-%20Microcontroller.html

# Setup

1. Start Arduino IDE and open Preferences window.

2. Enter http://arduino.esp8266.com/stable/package_esp8266com_index.json into Additional Board Manager URLs field. You can add multiple URLs, separating them with commas.

3. Open Boards Manager from Tools > Board menu and install esp8266 platform (and don't forget to select your ESP8266 board from Tools > Board menu after installation).

# GPIO

NodeMCU Development kit provides access to the GPIOs of ESP8266. **The NodeMCU Dev kit pins are numbered differently than internal GPIO notations of ESP8266**. For example, the D0 pin on the NodeMCU Dev kit is mapped to the internal GPIO pin 16 of ESP8266.

```c
// Pins can be assigned like this
int PIN_A = D7 // GPIO13
int PIN_A = 13 // D7

// This can also be used. Takes less RAM.
#define PIN_A 13
```

# LED example

```c
int LED = D7;

void setup() {
  pinMode(LED, OUTPUT);
}

void loop() {
  digitalWrite(LED, LOW);
  delay(1000);
  digitalWrite(LED, HIGH);
  delay(2000);
}
```

# Remote LED light

Upload the code into NodeMCU, and go to the local address specified in the serial monitor in order to toggle the LED light, connected in a classic GPIO way.

```c
#include <ESP8266WiFi.h>

const char* ssid = "WIFI_NAME";
const char* password = "WIFI_PASSWORD";

int ledPin = 13; // GPIO13---D7 of NodeMCU
WiFiServer server(80);

void setup() {
  Serial.begin(115200);
  delay(10);

  pinMode(ledPin, OUTPUT);
  digitalWrite(ledPin, LOW);

  // Connect to WiFi network
  Serial.println();
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("WiFi connected");

  // Start the server
  server.begin();
  Serial.println("Server started");

  // Print the IP address
  Serial.print("Use this URL to connect: ");
  Serial.print("http://");
  Serial.print(WiFi.localIP());
  Serial.println("/");

}

void loop() {
  // Check if a client has connected
  WiFiClient client = server.available();
  if (!client) {
    return;
  }

  // Wait until the client sends some data
  Serial.println("new client");
  while(!client.available()){
    delay(1);
  }

  // Read the first line of the request
  String request = client.readStringUntil('\r');
  Serial.println(request);
  client.flush();

  // Match the request

  int value = LOW;
  if (request.indexOf("/LED=ON") != -1)  {
    digitalWrite(ledPin, HIGH);
    value = HIGH;
  }
  if (request.indexOf("/LED=OFF") != -1)  {
    digitalWrite(ledPin, LOW);
    value = LOW;
  }

// Set ledPin according to the request
//digitalWrite(ledPin, value);

  // Return the response
  client.println("HTTP/1.1 200 OK");
  client.println("Content-Type: text/html");
  client.println(""); //  do not forget this one
  client.println("<!DOCTYPE HTML>");
  client.println("<html>");

  client.print("Led is now: ");

  if(value == HIGH) {
    client.print("On");
  } else {
    client.print("Off");
  }
  client.println("<br><br>");
  client.println("<a href=\"/LED=ON\"\"><button>On </button></a>");
  client.println("<a href=\"/LED=OFF\"\"><button>Off </button></a><br />");
  client.println("</html>");

  delay(1);
  Serial.println("Client disonnected");
  Serial.println("");
}
```

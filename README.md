# Hackfest
Autonomous waiter/clerk
#include <ESP8266WiFi.h>
 int h=0;
const char* ssid = "Mr.A";
const char* password = "ayan1999";
int ir1=15,ir2=2,ir3=3;

WiFiServer server(80);
 
void setup() {
  pinMode(ir1,OUTPUT);
  pinMode(ir2,OUTPUT);
  pinMode(ir3,OUTPUT);
  digitalWrite(ir1,LOW);
  digitalWrite(ir2,LOW);
  digitalWrite(ir3,LOW);
  Serial.begin(115200);
  delay(10);
 

 
  
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
 
void loop() 
{
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
  if (request.indexOf("/j1") != -1){digitalWrite(ir1,HIGH);}
    
 if (request.indexOf("/j2") != -1){digitalWrite(ir2,HIGH);}
   
   if (request.indexOf("/j3") != -1){
    digitalWrite(ir3,HIGH);}
    
    if (request.indexOf("/return") != -1)  { digitalWrite(ir1,LOW);
    digitalWrite(ir2,LOW);digitalWrite(ir2,LOW);}
    
  

  // Return the response
  client.println("HTTP://192.168.43.171");
  client.println("Content-Type: text/html");
  client.println(""); //  do not forget this one
  client.println("<!DOCTYPE HTML>");
  client.println("<html>");
  client.println("<br><br>");
  client.println("<a href=\"/j1\"\"><button>Junction 1 </button></a>");
  client.println("<a href=\"/j2\"\"><button>Junction 2 </button></a>");
  client.println("<a href=\"/j3\"\"><button>Junction 3 </button></a>");
  client.println("<a href=\"/return\"\"><button>Return </button></a><br />");    
  client.println("</html>");
 
  delay(1);
  Serial.println("Client disonnected");
  Serial.println("");
 
}

 

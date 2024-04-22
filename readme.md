#include <WiFi.h>
#include <WebServer.h>

const char* ssid = "secret";
const char* password = "secret";

WebServer server(80);

void handleRoot() {
  // Get client IP address
  String clientIP = server.client().remoteIP().toString();

  // Log visit with IP address
  Serial.print("Visit from: ");
  Serial.println(clientIP);

  // HTML content to be served
  String htmlContent = "<!DOCTYPE html><html><head><title>ESP32 Web Server</title></head><body>";
  htmlContent += "<h1>Hello from ESP32!</h1>";
  htmlContent += "</body></html>";

  // Send HTML response
  server.send(200, "text/html", htmlContent);
}

void setup() {
  Serial.begin(115200);
  
  // Connect to Wi-Fi
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi...");
  }
  Serial.println("Connected to WiFi");
  
  // Print local IP address
  Serial.println(WiFi.localIP());

  // Route for root
  server.on("/", handleRoot);

  // Start server
  server.begin();
  Serial.println("HTTP server started");
}

void loop() {
  server.handleClient();
}

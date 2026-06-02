#include <ESP8266WiFi.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <DHT.h>
#include <NTPClient.h>
#include <WiFiUdp.h>

#define DHTPIN D4
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE);

LiquidCrystal_I2C lcd(0x27, 20, 4);

const char* ssid = "YOUR_WIFI_NAME";
const char* password = "YOUR_WIFI_PASSWORD";

WiFiUDP ntpUDP;
NTPClient timeClient(ntpUDP, "pool.ntp.org", 19800);

#define MQ135 A0

void setup() {
  Serial.begin(115200);

  lcd.init();
  lcd.backlight();

  dht.begin();

  WiFi.begin(ssid, password);

  lcd.setCursor(0,0);
  lcd.print("Connecting WiFi");

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    lcd.print(".");
  }

  lcd.clear();
  lcd.print("WiFi Connected");

  timeClient.begin();

  delay(2000);
}

void loop() {

  timeClient.update();

  float temp = dht.readTemperature();
  float hum = dht.readHumidity();

  int airQuality = analogRead(MQ135);

  lcd.clear();

  lcd.setCursor(0,0);
  lcd.print("Temp:");
  lcd.print(temp);
  lcd.print((char)223);
  lcd.print("C");

  lcd.setCursor(0,1);
  lcd.print("Hum:");
  lcd.print(hum);
  lcd.print("%");

  lcd.setCursor(0,2);
  lcd.print("Air:");
  lcd.print(airQuality);

  lcd.setCursor(0,3);
  lcd.print(timeClient.getFormattedTime());

  Serial.print("Temperature: ");
  Serial.println(temp);

  Serial.print("Humidity: ");
  Serial.println(hum);

  Serial.print("Air Quality: ");
  Serial.println(airQuality);

  delay(3000);
}# Smart-Weather-Monitoring-System-IoT
An IoT-based weather monitoring system using ESP8266 and multiple sensors to monitor temperature, humidity, air quality, and harmful gases in real time. Features a 20x4 LCD display with date, time, and environmental data visualization.

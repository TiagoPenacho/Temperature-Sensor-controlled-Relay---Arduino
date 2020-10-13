#include <LiquidCrystal.h>
#include <OneWire.h>
#include <DallasTemperature.h>

float max_temp;
float min_temp;
float temp;
int lcd_key = 0;
int adc_key_in = 0;
int oneWireBus = 2;
int relay = A5;
int display_options;
int multi;

LiquidCrystal lcd(8, 9, 4, 5, 6, 7);
OneWire oneWire(oneWireBus);
DallasTemperature sensors(&oneWire);

#define btnRIGHT  0
#define btnUP     1
#define btnSELECT 2
#define btnDOWN   3
#define btnLEFT   4
#define btnNONE   5

int read_LCD_buttons(){             
  adc_key_in = analogRead(0);     
    if (adc_key_in < 60)   return btnRIGHT; 
    if (adc_key_in < 200)  return btnUP;
    if (adc_key_in < 400)  return btnDOWN;
    if (adc_key_in < 600)  return btnLEFT; 
    if (adc_key_in < 800)  return btnSELECT;
    if (adc_key_in > 1000) return btnNONE;       
}

void setup(){
  min_temp = 6;
  max_temp = 8;
  multi = 1;
  lcd.begin(16, 2);
  lcd.setCursor(0,0);
  lcd.print ("Min:");
  lcd.print (min_temp);
  lcd.setCursor(0,1);
  lcd.print ("Max:");
  lcd.print (max_temp);
  lcd.setCursor(8,0);
  lcd.print (" T:");
  lcd.print (temp);
  lcd.setCursor(8,1);
  lcd.print (" Wait");
  lcd.setCursor(13,1);
  lcd.print("*.1");
  pinMode(relay, OUTPUT);
  digitalWrite(relay, HIGH);
  digitalWrite(relay, LOW);
  Serial.begin (9600);
  Serial.println (adc_key_in);
  delay(100);    
}

void loop(){ 
  sensors.requestTemperatures();          
  temp = sensors.getTempCByIndex(0); 
  lcd_key = read_LCD_buttons(); 
  float temp = sensors.getTempCByIndex(0); 
  Serial.println (adc_key_in);
  Serial.println(temp);
  delay(10);
  if(temp >= max_temp){
    powerOnRelay();
  }
  else if (temp <= min_temp) {
    powerOffRelay();
  }

  switch (lcd_key){  
    case btnUP:{ 
      lcd.setCursor(0,0);
      min_temp = min_temp + (multi * 0.1);
      lcd.print("Min:");
      lcd.print(min_temp);
      lcd.setCursor(0,1);
      lcd.print("Max:");
      lcd.print(max_temp);
      lcd.setCursor(8,0);
      lcd.print (" T:");
      lcd.print (temp);
      lcd.setCursor(8,1);
      lcd.print (" ");
      delay(10);
      break;
    }
    case btnDOWN:{ 
      lcd.setCursor(0,0);
      min_temp = min_temp - (multi * 0.1);
      lcd.print("Min:");
      lcd.print(min_temp);
      lcd.setCursor(0,1);
      lcd.print("Max:");
      lcd.print(max_temp);
      lcd.setCursor(8,0);
      lcd.print (" T:");
      lcd.print (temp);
      lcd.setCursor(8,1);
      lcd.print (" ");
      delay(10);
      break;
    }
    case btnLEFT:{ 
      lcd.setCursor(0,0);
      lcd.print("Min:");
      lcd.print(min_temp);
      lcd.setCursor(0,1);
      max_temp = max_temp - (multi * 0.1);
      lcd.print("Max:");
      lcd.print(max_temp);
      lcd.setCursor(8,0);
      lcd.print (" T:");
      lcd.print (temp);
      lcd.setCursor(8,1);
      lcd.print (" ");
      delay(10);
      break;
    }
    case btnRIGHT:{
      lcd.setCursor(0,0);
      lcd.print("Min:");
      lcd.print(min_temp);
      lcd.setCursor(0,1);
      max_temp = max_temp + (multi * 0.1);
      lcd.print("Max:");
      lcd.print(max_temp);
      lcd.setCursor(8,0);
      lcd.print (" T:");
      lcd.print (temp);
      lcd.setCursor(8,1);
      lcd.print (" ");
      delay(10);
      break;
    }
    case btnSELECT:{
      if (multi == 1) {
        multi = 10; 
        lcd.setCursor(13,1);
        lcd.print(" *1");
      }
      else if (multi == 10) {
        multi = 50;
        lcd.setCursor(13,1);
        lcd.print(" *5");
      }
      else if (multi == 50) {
        multi = 1;
        lcd.setCursor(13,1);
        lcd.print("*.1");
      }
      lcd.setCursor(0,0);
      lcd.print("Min:");
      lcd.print(min_temp);
      lcd.setCursor(0,1);
      lcd.print("Max:");
      lcd.print(max_temp);
      lcd.setCursor(8,0);
      lcd.print (" T:");
      lcd.print (temp);
      lcd.setCursor(8,1);
      lcd.print (" ");
      delay(10);
      break;
    }
    case btnNONE:{
      lcd.setCursor(0,0);
      lcd.print("Min:");
      lcd.print(min_temp);
      lcd.setCursor(0,1);
      lcd.print("Max:");
      lcd.print(max_temp);
      lcd.setCursor(8,0);
      lcd.print (" T:");
      lcd.print (temp);
      lcd.setCursor(8,1);
      lcd.print (" ");
      delay(10);
      break;
    }    
  }
}

void powerOnRelay() {
  Serial.println("Relay On");
  lcd.setCursor(8,1);
  lcd.print (" ON  ");
  digitalWrite(relay, HIGH);
  delay(10);
}
void powerOffRelay() {
  Serial.println("Relay Off");
  lcd.setCursor(8,1);
  lcd.print (" OFF ");
  digitalWrite(relay, LOW);
  delay(10);
}

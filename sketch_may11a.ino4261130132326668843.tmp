//#include <TEA5767Radio.h>

#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#define REC 2 
#define PLAY_E 3
#define PLAY_L 4  
#define FT 5 
#define playTime 5000 
#define recordTime 3000
#define playLTime 900
unsigned char frekansH = 0;
unsigned char frekansL = 0;
unsigned int frekansB;
double frekans = 0;
static char frekans_ekran[15];
LiquidCrystal_I2C lcd(0x3F,2,1,0,4,5,6,7,3,POSITIVE);

void setup()
{ 
 Wire.begin();
 lcd.begin(16,2);
 lcd.setBacklightPin(3,POSITIVE);
 lcd.setBacklight(HIGH);
  frekans = 93.0;
  frekansAyarla();
 lcd.setCursor(3, 0);
lcd.write("FM Radyo");
// ISD1820 
  pinMode(REC,OUTPUT);
  pinMode(PLAY_L,OUTPUT);
  pinMode(PLAY_E,OUTPUT);
  pinMode(FT,OUTPUT);
  Serial.begin(9600);
}
void loop()
{
  int reading = analogRead(A0);
  frekans = ((double)reading * (108.0 - 87.5)) / 1024.0 + 87.5;
  frekans = ((int)(frekans * 10)) / 10.0;
 frekansAyarla();
dtostrf(frekans, 6, 2, frekans_ekran);
  lcd.setCursor(4, 1);
lcd.print(frekans_ekran);
seskayit();
}
void seskayit(){
    while (Serial.available() > 0) {
          char inChar = (char)Serial.read();
            if(inChar =='p' || inChar =='P'){
            digitalWrite(PLAY_E, HIGH);Serial.println("oynatma basladi");  
            delay(50);
            digitalWrite(PLAY_E, LOW);  
              
            delay(playTime);
              Serial.println("oynatma bitti");
            break; 
            }         
            else if(inChar =='r' || inChar =='R'){
              digitalWrite(REC, HIGH);
              Serial.println("Kayıt BaŞladı");
              delay(recordTime);
              digitalWrite(REC, LOW);
              Serial.println("Kayıt Sonlandı ");              
            } 
            else if(inChar =='l' || inChar =='L'){
            digitalWrite(PLAY_L, HIGH); 
              Serial.println("Oynatma L Başladı");  
            delay(playLTime);
            digitalWrite(PLAY_L, LOW);
              Serial.println("Oynatma L Sonlandı");            
            }     
      Serial.println("**** Seri Monitörden Çıkıldı");      
    }
Serial.println("****Kayıt için R'ye, Oynatma için P'ye basınız ve ardından ENTER'a basınız");

  delay(500);}


void frekansAyarla()
{
  frekansB = 4 * (frekans * 1000000 + 225000) / 32768;
  frekansH = frekansB >> 8;
  frekansL = frekansB & 0XFF;
  delay(100);
  Wire.beginTransmission(0x60);
  Wire.write(frekansH);
  Wire.write(frekansL);
  Wire.write(0xB0);
  Wire.write(0x10);
  Wire.write((byte)0x00);
  Wire.endTransmission();
  delay(100);
}

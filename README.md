#include <LiquidCrystal.h>

const int PINO_SENSOR_TEMPERATURA = A2; 
const int PINO_SENSOR_UMIDADE = A4; 
const int PINO_SENSOR_LAMPADA_OPERANDO = A0;

const int PINO_LED_ERRO = 13;

const int PINO_LCD_RS = 2; 
const int PINO_LCD_ENABLE = 3; 
const int PINO_LCD_DB4 = 4;
const int PINO_LCD_DB5 = 5;
const int PINO_LCD_DB6 = 6; 
const int PINO_LCD_DB7 = 7;

const int LCD_NUMERO_LINHAS = 2; 
const int LCD_NUMERO_COLUNAS = 16;

LiquidCrystal _lcd(PINO_LCD_RS, 
PINO_LCD_ENABLE,
PINO_LCD_DB4, 
PINO_LCD_DB5,
PINO_LCD_DB6, 
PINO_LCD_DB7);

String _ssidName = "Simulator Wifi"; 
String _ssidPassword = "";

int _tcpHttpPort = 80;

String _siteHost = "api.thingspeak.com";
String _siteAPPID = "WJPE65B0OQBNRXXV"; 
String _siteURIbase = "/update?api_key=" + _siteAPPID; 
String _siteField1 = "&field1="; 
String _siteField2 = "&field2="; 
String _siteField3 = "&field3=";

const int TEMPERATURA_CELSIUS_MINIMA = -30; 
const int TEMPERATURA_CELSIUS_MAXIMA = 125;

int _leituraMinima; 
int _leituraMaxima;

void setup() {

pinMode(PINO_LED_ERRO, OUTPUT); d
igitalWrite(PINO_LED_ERRO, LOW);

pinMode(PINO_SENSOR_TEMPERATURA, INPUT); 
pinMode(PINO_SENSOR_UMIDADE, INPUT);
pinMode(PINO_SENSOR_LAMPADA_OPERANDO, INPUT);

_lcd.begin(LCD_NUMERO_COLUNAS,LCD_NUMERO_LINHAS); 
_lcd.print(" Final Project"); 
_lcd.setCursor(0,1); _lcd.print("Out oF Home");

_leituraMinima = 20; 
_leituraMaxima = 358;

delay(1000);

_leituraMinima = 20;
 _leituraMaxima = 358;

}



bool enviarSensores(String temperatura, String nivel, String lampada) 

{ bool sucesso = true;


String httpPacket = "GET " + uriCompleta + 
" HTTP/1.1\r\nHost: " + _siteHost +
"\r\n\r\n"; int length = httpPacket.length();


String tamanhoPacote = "AT+CIPSEND=" + numberToString(length);

sucesso &= sendCommandTo8266(tamanhoPacote, ">");

sucesso &= sendCommandTo8266(httpPacket, "SEND OK");

return sucesso; }

void mostraDisplayLCD(String temperatura, String nivel, bool lampada) {

_lcd.setCursor(0,0); _lcd.print("Temp: "); 
_lcd.print(temperatura); delay (125); 
_lcd.print('\xB2'); _lcd.print("C "); 


if (lampada) _lcd.print("OFF "); 

else _lcd.print(" ");


_lcd.setCursor(0,1); 
_lcd.print("Umid: "); 
_lcd.print(nivel);
_lcd.print("% "); 

if (!lampada) _lcd.print("ON "); 

else _lcd.print(" "); }

bool sendCommandTo8266(String comando, char * aguardar) { bool sucesso = false;

Serial.println(comando); 
Serial.flush(); 
delay(50);

if (0 == aguardar[0]) sucesso = true; 
else if (Serial.find(aguardar)) sucesso = true; 
else digitalWrite(PINO_LED_ERRO, HIGH);

return sucesso; }

String numberToString(int valor) { char numero[6]; sprintf(numero, "%i", valor); 

return String(numero); }

# sistema-automatico-de-enfriamiento
#include <OneWire.h>
#include <DallasTemperature.h>

// Pin donde se conecta el bus 1-Wire
const int pinDatosDQ = 9;
const int LED = 8;
const float TempAlt = 30.0;
const float TempBaj = 27.0;
bool state = 0;


// Instancia a las clases OneWire y DallasTemperature
OneWire oneWireObjeto(pinDatosDQ);
DallasTemperature sensorDS18B20(&oneWireObjeto);
 
void setup() {
  pinMode(8, OUTPUT);
    // Iniciamos la comunicación serie
    Serial.begin(9600);
    // Iniciamos el bus 1-Wire
    sensorDS18B20.begin();
    
}
 
void loop() {

  float Temp = sensorDS18B20.getTempCByIndex(0);
    // Mandamos comandos para toma de temperatura a los sensores
    Serial.println("Mandando comandos a los sensores");
    sensorDS18B20.requestTemperatures();
 
    // Leemos y mostramos los datos de los sensores DS18B20
    Serial.print("Temperatura sensor 0: ");
    Serial.print(sensorDS18B20.getTempCByIndex(0));
    Serial.println(" C");
    delay(1000); 
    
  if(Temp >= TempAlt)
  {
      digitalWrite(LED, HIGH);   // encender la placa Peltier
  }
  else if(Temp <= TempBaj)
  {
      digitalWrite(LED, LOW);   // apagar la placa Peltier
  }
    
}

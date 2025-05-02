# Proyecto_carrusel
Nuestro proyecto está basado en el mecanismo y configuración  de un carrusel,así mismo  mencionaremos  sus pasos.
Integrantes:
Rodriguez Almaraz Ximena Zuleyka 
Montiel  Hernandez Emilio Alberto 
Cisneros Islas Oswualdo Emiliano
Salinas Marquez Jose Ramón 

DIAGRAMA DE FLUJO:

Inicio _>verificaciones de seguridad
_>carga de pasajeros->Inicio del movimiento->Ciclo de rotacion->(sube/baja)->Monitoreo de operacion->Fin del ciclo->>Detención _>(SI)->BAJAN PASAJEROS->(NO)->FIN

 SISTEMA
Sistema del carrusel (componentes y funciones)

Entradas (sensores):

Sensor de puerta abierta/cerrada

Sensor de presencia de pasajeros

Botón de inicio/parada de emergencia


Procesamiento (controlador PLC o microcontrolador):

Verifica condiciones de seguridad

Controla el encendido del motor

Temporiza la duración del giro


Salidas (actuadores):

Motor de rotación

Alarma sonora/visual

Luces de operación

PSEUDOCODIGOS
sensorPuerta = Pin 2 (sensor de puerta cerrada)

sensorPasajero = Pin 3 (sensor de presencia)

botonEmergencia = Pin 4 (botón de parada de emergencia)

motorPin = Pin 8 (rele o controlador del motor)

lucesOperacion = Pin 9

alarmaPin = Pin 10
const int sensorPuerta = 2;
const int sensorPasajero = 3;
const int botonEmergencia = 4;
const int motorPin = 8;
const int lucesOperacion = 9;
const int alarmaPin = 10;

void setup() {
  pinMode(sensorPuerta, INPUT);
  pinMode(sensorPasajero, INPUT);
  pinMode(botonEmergencia, INPUT);
  pinMode(motorPin, OUTPUT);
  pinMode(lucesOperacion, OUTPUT);
  pinMode(alarmaPin, OUTPUT);

  Serial.begin(9600);
}

void loop() {
  bool puertaCerrada = digitalRead(sensorPuerta);
  bool pasajeroPresente = digitalRead(sensorPasajero);
  bool emergencia = digitalRead(botonEmergencia);

  if (puertaCerrada && pasajeroPresente) {
    digitalWrite(lucesOperacion, HIGH);
    delay(3000); // Esperar 3 segundos antes de arrancar

    digitalWrite(motorPin, HIGH); // Encender motor
    unsigned long tiempoInicio = millis();

    while (millis() - tiempoInicio < 10000) { // 10 segundos de operación
      if (digitalRead(botonEmergencia)) {
        digitalWrite(motorPin, LOW);
        digitalWrite(alarmaPin, HIGH);
        Serial.println("Emergencia: Motor detenido.");
        break;
      }
    }

    digitalWrite(motorPin, LOW); // Apagar motor
    digitalWrite(lucesOperacion, LOW);
    delay(2000); // Tiempo para descargar pasajeros
  } else {
    Serial.println("Condiciones de seguridad no cumplidas.");
  }

  delay(500); // Pequeña pausa antes de la siguiente verificación
}
  #include <Servo.h>

// Definición de pines para motores
const int motor1Pin1 = 2;
const int motor1Pin2 = 3;
const int motor2Pin1 = 4;
const int motor2Pin2 = 5;
const int enablePinA = 9; // PWM

const int motor3Pin1 = 6;
const int motor3Pin2 = 7;
const int motor4Pin1 = 8;
const int motor4Pin2 = 10;
const int enablePinB = 11; // PWM

// Definición de pines para sensor ultrasónico
const int trigPin = 12;
const int echoPin = 13;
Servo myservo;

// Definición de pines para sensor de color
const int S0 = A0;
const int S1 = A1;
const int S2 = A2;
const int S3 = A3;
const int sensorOut = A4;

// Definición de pin para sensor infrarrojo
const int IR = 14; // Puerto digital

// Contadores
int lineCounter = 0;
int lapCounter = 0;

// Umbral para la detección de colores
const int threshold = 500; // Este valor puede ser ajustado

void setup() {
  // Configuración de pines de motores
  pinMode(motor1Pin1, OUTPUT);
  pinMode(motor1Pin2, OUTPUT);
  pinMode(motor2Pin1, OUTPUT);
  pinMode(motor2Pin2, OUTPUT);
  pinMode(enablePinA, OUTPUT);
 
  pinMode(motor3Pin1, OUTPUT);
  pinMode(motor3Pin2, OUTPUT);
  pinMode(motor4Pin1, OUTPUT);
  pinMode(motor4Pin2, OUTPUT);
  pinMode(enablePinB, OUTPUT);

  // Configuración de pines de sensor ultrasónico
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  myservo.attach(9); // Pin del servomotor

  // Configuración de pines de sensor de color
  pinMode(S0, OUTPUT);
  pinMode(S1, OUTPUT);
  pinMode(S2, OUTPUT);
  pinMode(S3, OUTPUT);
  pinMode(sensorOut, INPUT);

  digitalWrite(S0, HIGH);
  digitalWrite(S1, LOW);

  // Configuración de pin de sensor infrarrojo
  pinMode(IR, INPUT);

  Serial.begin(9600);
}

void loop() {
  // Código para manejar motores
  digitalWrite(motor1Pin1, HIGH);
  digitalWrite(motor1Pin2, LOW);
  digitalWrite(motor2Pin1, HIGH);
  digitalWrite(motor2Pin2, LOW);
  analogWrite(enablePinA, 255);
 
  digitalWrite(motor3Pin1, HIGH);
  digitalWrite(motor3Pin2, LOW);
  digitalWrite(motor4Pin1, HIGH);
  digitalWrite(motor4Pin2, LOW);
  analogWrite(enablePinB, 255);

  // Código para sensor ultrasónico y servomotor
  myservo.write(90); // Centra el sensor ultrasónico
  delay(500);
  long duration, distance;
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distance = (duration / 2) / 29.1;

  if (distance < 20) { // Si hay un obstáculo cerca
    // Código para evitar obstáculos usando el servomotor y el sensor ultrasónico
  }

  // Código para sensor de color
  int redFrequency = readColor('R');
  int greenFrequency = readColor('G');
  int blueFrequency = readColor('B');

  if (greenFrequency < threshold) {
    turnLeft();
  } else if (redFrequency < threshold) {
    turnRight();
  }

  // Código para sensor infrarrojo y contadores
  int irState = digitalRead(IR);

  if (irState == LOW) {
    lineCounter++;
    Serial.print("Contador de líneas: ");
    Serial.println(lineCounter);
    delay(500); // Debounce

    if (lineCounter % 8 == 0) {
      lapCounter++;
      Serial.print("Contador de vueltas: ");
      Serial.println(lapCounter);

      if (lapCounter >= 3) {
        // Detener el vehículo
        stopVehicle();
      }
    }
  }

  delay(100);
}

int readColor(char color) {
  if (color == 'R') {
    digitalWrite(S2, LOW);
    digitalWrite(S3, LOW);
  } else if (color == 'G') {
    digitalWrite(S2, HIGH);
    digitalWrite(S3, HIGH);
  } else if (color == 'B') {
    digitalWrite(S2, LOW);
    digitalWrite(S3, HIGH);
  }

  return pulseIn(sensorOut, LOW);
}

void turnLeft() {
  digitalWrite(motor1Pin1, LOW);
  digitalWrite(motor1Pin2, HIGH);
  digitalWrite(motor2Pin1, LOW);
  digitalWrite(motor2Pin2, HIGH);
  analogWrite(enablePinA, 255);
 
  digitalWrite(motor3Pin1, HIGH);
  digitalWrite(motor3Pin2, LOW);
  digitalWrite(motor4Pin1, HIGH);
  digitalWrite(motor4Pin2, LOW);
  analogWrite(enablePinB, 255);

  delay(500); // Ajustar el tiempo según sea necesario

  // Restaurar movimiento recto
  digitalWrite(motor1Pin1, HIGH);
  digitalWrite(motor1Pin2, LOW);
  digitalWrite(motor2Pin1, HIGH);
  digitalWrite(motor2Pin2, LOW);
  analogWrite(enablePinA, 255);
 
  digitalWrite(motor3Pin1, HIGH);
  digitalWrite(motor3Pin2, LOW);
  digitalWrite(motor4Pin1, HIGH);
  digitalWrite(motor4Pin2, LOW);
  analogWrite(enablePinB, 255);
}

void turnRight() {
  digitalWrite(motor1Pin1, HIGH);
  digitalWrite(motor1Pin2, LOW);
  digitalWrite(motor2Pin1, HIGH);
  digitalWrite(motor2Pin2, LOW);
  analogWrite(enablePinA, 255);
 
  digitalWrite(motor3Pin1, LOW);
  digitalWrite(motor3Pin2, HIGH);
  digitalWrite(motor4Pin1, LOW);
  digitalWrite(motor4Pin2, HIGH);
  analogWrite(enablePinB, 255);

  delay(500); // Ajustar el tiempo según sea necesario

  // Restaurar movimiento recto
  digitalWrite(motor1Pin1, HIGH);
  digitalWrite(motor1Pin2, LOW);
  digitalWrite(motor2Pin1, HIGH);
  digitalWrite(motor2Pin2, LOW);
  analogWrite(enablePinA, 255);
 
  digitalWrite(motor3Pin1, HIGH);
  digitalWrite(motor3Pin2, LOW);
  digitalWrite(motor4Pin1, HIGH);
  digitalWrite(motor4Pin2, LOW);
  analogWrite(enablePinB, 255);
}

void stopVehicle() {
  digitalWrite(motor1Pin1, LOW);
  digitalWrite(motor1Pin2, LOW);
  digitalWrite(motor2Pin1, LOW);
  digitalWrite(motor2Pin2, LOW);
  analogWrite(enablePinA, 0);
 
  digitalWrite(motor3Pin1, LOW);
  digitalWrite(motor3Pin2, LOW);
  digitalWrite(motor4Pin1, LOW);
  digitalWrite(motor4Pin2, LOW);
  analogWrite(enablePinB, 0);
}
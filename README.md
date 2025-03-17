# polevod
// Подключение библиотек
#include <Servo.h>

// Пины для управления моторами
const int motorLeftPin = 9;
const int motorRightPin = 10;
const int depthServoPin = 6;

// Пины для сенсоров (например, для контроля скорости или глубины посадки)
const int depthSensorPin = A0; // Пример пин для аналогового сенсора глубины

// Переменные для управления
int motorSpeed = 255;  // Максимальная скорость для моторов (0 - 255)
int depthPosition = 90;  // Начальная позиция сервомотора (глубина посадки)

// Инициализация объектов
Servo depthServo;

void setup() {
  // Настройка пинов моторов
  pinMode(motorLeftPin, OUTPUT);
  pinMode(motorRightPin, OUTPUT);

  // Инициализация серво мотора
  depthServo.attach(depthServoPin);
  depthServo.write(depthPosition); // Установка начальной глубины

  // Настройка последовательного монитора для отладки
  Serial.begin(9600);
}

void loop() {
  // Пример движения машины вперед
  moveForward(motorSpeed);

  // Пример настройки глубины посадки в зависимости от показаний датчика
  int sensorValue = analogRead(depthSensorPin);
  int newDepth = map(sensorValue, 0, 1023, 30, 150);  // Маппируем значение на диапазон от 100 до 150 (глубина в мм)
  if (newDepth != depthPosition) {
    depthPosition = newDepth;
    depthServo.write(depthPosition); // Корректировка глубины
    Serial.print("New Depth: ");
    Serial.println(depthPosition);
  }

  delay(1000);  // Задержка для стабилизации процесса
}

// Функция для движения вперед
void moveForward(int speed) {
  analogWrite(motorLeftPin, speed);
  analogWrite(motorRightPin, speed);
}

// Функция для остановки машины
void stopMovement() {
  analogWrite(motorLeftPin, 0);
  analogWrite(motorRightPin, 0);
}

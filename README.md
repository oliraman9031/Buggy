write readme for this code const int trigPin = 13;
const int echoPin = 12;
const int pin = 4;
long duration;
int distanceCm, distanceInch;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  for (int i = 5; i < 9; i++) {
    pinMode(i, OUTPUT);
  }
  pinMode(A0, INPUT);
  pinMode(A1, INPUT);
  Serial.begin(9600);
}

long st = millis();
int count = 0;
int flag = 0;

void stopp() {
  digitalWrite(5, LOW);
  digitalWrite(8, LOW);
  digitalWrite(6, LOW);
  digitalWrite(7, LOW);
}

void forward() {
  digitalWrite(5, HIGH);
  digitalWrite(8, HIGH);
  digitalWrite(6, LOW);
  digitalWrite(7, LOW);
}

void backward() {
  digitalWrite(6, HIGH);
  digitalWrite(7, HIGH);
  digitalWrite(5, LOW);
  digitalWrite(8, LOW);
}

void left() {
  digitalWrite(6, LOW);
  digitalWrite(7, LOW);
  digitalWrite(5, HIGH);
  digitalWrite(8, LOW);
}

void right() {
  digitalWrite(6, LOW);
  digitalWrite(7, LOW);
  digitalWrite(5, LOW);
  digitalWrite(8, HIGH);
}

void clockwise() {
  digitalWrite(6, HIGH);
  digitalWrite(7, LOW);
  digitalWrite(5, LOW);
  digitalWrite(8, HIGH);
}

void counterclockwise() {
  digitalWrite(6, LOW);
  digitalWrite(7, HIGH);
  digitalWrite(5, HIGH);
  digitalWrite(8, LOW);
}

void loop() {
  if (digitalRead(pin) > 0) {
    int value = pulseIn(pin, HIGH);
    Serial.print("Value =");
    Serial.println(value);

    if (value > 3200 && value < 3700) {}
    Serial.println("Gantry 2 Crossed");
    stopp();
    delay(1000);

    if (value > 2500 && value < 3000)
      Serial.println("Gantry 3 Crossed");
    stopp();
    delay(1000);

    if (value > 500 && value < 1000) {
      Serial.println("Gantry 1 Crossed");
      stopp();
      delay(1000);
    }
  }

  if (Serial.read() == 'C' || flag == 1) {
    flag = 1;
    int s = 0;

    digitalWrite(trigPin, LOW);
    delayMicroseconds(2);
    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPin, LOW);

    duration = pulseIn(echoPin, HIGH);
    distanceCm = (duration * 0.034) / 2;
    distanceInch = (duration * 0.0133) / 2;

    if (distanceCm > 10) {
      int r = digitalRead(A0);
      int l = digitalRead(A1);

      if (l == 1 && r == 1)
        forward();
      else if (l == 1 && r == 0)
        left();
      else if (l == 0 && r == 1)
        right();
      else {
        long endt = millis();
        if (endt - st > 1000) {
          count++;
          Serial.print("Count =");
          Serial.println(count);
          st = millis();
        }

        if (count == 1) {
          forward();
        }

        if (count == 2) {
          right();
          delay(150);
        }

        if (count == 3) {
          forward();
        }

        if (count == 4) {
          forward();
        }

        if (count == 5) {
          right();
        }

        if (count == 6) {
          forward();
        }

        if (count > 6) {
          stopp();
          flag = 0;
        }
      }
    } else {
      stopp();
    }
  }
}

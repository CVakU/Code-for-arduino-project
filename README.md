// Code for arduino project
// Define pins for ultrasonic sensor
#define trigPin 13
#define echoPin 12
// Define pins for 7-segment display
byte pin[] = {3, 4, 5, 6, 7, 8, 9, 10}; // DP, C, D, E, B, A, F, G
// Define delay for display refresh
#define d 750
// Define array for number patterns on 7-segment display (assuming common anode)
int number[10][8] = {
//DP,C,D,E,B,A,F,G
  {0,0,0,0,0,0,0,1}, //0
  {1,0,1,1,0,1,1,1}, //1
  {1,1,0,0,0,0,1,0}, //2
  {1,0,0,1,0,0,1,0}, //3
  {1,0,1,1,0,1,0,0}, //4
  {1,0,0,1,1,0,0,0}, //5
  {0,0,0,0,1,0,0,0}, //6
  {1,0,1,1,0,0,1,1}, //7
  {0,0,0,0,0,0,0,0}, //8
  {0,0,0,1,0,0,0,0}  //9
    
};

long duration;
int distance;

void setup() {
  // Initialize all pins as outputs for 7-segment display
  for (byte a = 0; a < 8; a++) {
    pinMode(pin[a], OUTPUT);
  }
  // Initialize ultrasonic sensor pins
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  Serial.begin(9600);
}

void loop() {
  // Measure distance
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2; // Calculate distance
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // Display distance on 7-segment display
  int displayNumber = distance % 10; // Get the last digit of the distance
  for (int b = 0; b < 8; b++) {
    digitalWrite(pin[b], number[displayNumber][b]);
  }
  delay(d); // Refresh display
}

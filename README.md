# reflecctions
# in1 11   //left rear motor
#define in2 12
#define in3 9    //right rear motor
#define in4 8
#define ENA 5
#define ENB 6
float duration;// establish variables for duration
float distanceCm;// establish variables for distance
const int triggerpin = 10; //connect trigger pin of  ultrasonic sensor to 10th pin of arduino uno
const int echopin = 13; //connect echo pin of  ultrasonic sensor to 13th pin of arduino uno
char str = '0';
void forward()
{
  analogWrite(in1, 255);
  analogWrite(in2, 0);
  analogWrite(in3, 100);
  analogWrite(in4, 0);
  digitalWrite(ENA, HIGH);
  digitalWrite(ENB, HIGH);
}
void backward()
{
  analogWrite(in1, 0);
  analogWrite(in2, 255);
  analogWrite(in3, 0);
  analogWrite(in4, 100);
  digitalWrite(ENA, HIGH);
  digitalWrite(ENB, HIGH);
}
void left()
{
  analogWrite(in1, 0);
  analogWrite(in2, 0);
  analogWrite(in3, 100);
  analogWrite(in4, 0);
  digitalWrite(ENA, LOW);
  digitalWrite(ENB, HIGH);
}
void right()
{
  analogWrite(in1, 255);
  analogWrite(in2, 0);
  analogWrite(in3, 0);
  analogWrite(in4, 0);
  digitalWrite(ENA, HIGH);
  digitalWrite(ENB, LOW);
}
void Stop()
{
  analogWrite(in1, 0);
  analogWrite(in2, 0);
  analogWrite(in3, 0);
  analogWrite(in4, 0);
  digitalWrite(ENA, LOW);
  digitalWrite(ENB, LOW);
}
float microsecondsToCentimeters(float microseconds)
{
  // The speed of sound is 340 m/s or 29 microseconds per centimeter.
  // The ping travels out and back, so to find the distance of the
  // object we take half of the distance travelled.
  return microseconds / 29.0 / 2.0;
}
void setup()
{
  Serial.begin(38400);  // start serial communicatiion
  pinMode(in1, OUTPUT);     //make m12 as output
  pinMode(in2, OUTPUT);    //make m11 as output
  pinMode(in3, OUTPUT);    //make m9 as output
  pinMode(in4, OUTPUT);    //make m8 as output
  pinMode(ENA, OUTPUT);
  pinMode(ENB, OUTPUT);
  pinMode(triggerpin, OUTPUT); //set trigger pin as output pin
  pinMode(echopin, INPUT); //sets echo pin as input pin
}

void loop() {
  while (Serial.available())       //enters the loop only if any letter pressed in serial monitor
  {
    char ch = Serial.read();           //read the value in typed in serial monitor and stores it in 'ch'
    str = ch;
  }
  digitalWrite(triggerpin, LOW); //sets trigger pin off for 2 microseconds
  delayMicroseconds(2);
  digitalWrite(triggerpin, HIGH);//sets trigger pin on for 10 microseconds
  delayMicroseconds(10);
  digitalWrite(triggerpin, LOW); //sets trigger pin off for 2 microseconds
  delayMicroseconds(2);
  duration = pulseIn(echopin, HIGH); //how much time the echo pin is high
  distanceCm = microsecondsToCentimeters(duration);// microsecondsToCentimeters function is called to convert the time into a distance
  Serial.println(distanceCm);
  if (str == '1' && distanceCm > 8.0 )
  {
    forward();
    Serial.println("Swift moving forward");
  }
  if (str == '1' && distanceCm < 8.0 )
  {
    Stop();
    Serial.println("Swift moving forward");
  }
  if (str == '2')
  {
    right();
    Serial.println("Swift moving right");
  }
  if (str == '3')
  {
    left();
    Serial.println("Swift moving left");
  }
  if (str == '4')
  {
    backward();
    Serial.println("Swift moving backward");
  }
  if (str == '0')
  {
    Stop();
    Serial.println("Swift stopped");
  }
  delay(100);
  Serial.println(str);
}

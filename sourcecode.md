###
int pos = 0; 
int echoPin = 3;
int trigPin = 2;
float duration; 
float distance = 100;

void setup(){
  s.attach(9);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  Serial.begin(9600);
}

void loop(){
  int val = analogRead(sensor);
  val = constrain(val,50,800);
  val = map(val, 50, 800, 0, 180);
  s.write(val);
  delay(50);
  
  digitalWrite(trigPin, HIGH);
  delay(10);
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distance = ((float)(340 * duration) / 10000) / 2;  
  if(distance < 4) { 
    for(pos = 10; pos < 180; pos += 2)  
    {                                  
      s.write(pos);              
      delay(10); 
      //가는 코드                      
    } 
    delay(3000); //한번 실행되고 멈췄다가 돌아가는 코드 딜레이 속도조절
    for(pos = 180; pos>=10; pos-=2)    
    {                                
      s.write(pos);        
      delay(10);
      //천천히 오는코드        
    } 
  }
  delay(10);
  Serial.println(distance);
}

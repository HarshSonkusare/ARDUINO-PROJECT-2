# ARDUINO-PROJECT-2
#include <Servo.h> 
#include <IRremote.h>
Servo servo_1; //creating object as servo_1
Servo servo_2; //creating object as servo_2
int trigpin_1=7;
int trigpin_2=5;
int echopin_1=6;
int echopin_2=4;
int ir_1=12;
int ir_2=10;
int ir_3=9;
int ir_4=8;
int led_r=14;
int led_b=15;
int led_g=16;
int led_y=17;
int distance_1,distance_2,time_1,time_2;
IRrecv irrecv_1(ir_1); //pin definition
IRrecv irrecv_2(ir_2); //pin definition
IRrecv irrecv_3(ir_3); //pin definition
IRrecv irrecv_4(ir_4); //pin 
decode_results results; //Value sent by remote
int pos=0;//initial position of servo motor

void setup()
{
  servo_1.attach(11);
  servo_2.attach(3);
  Serial.begin(9600);
  pinMode(led_r,OUTPUT);
  pinMode(led_b,OUTPUT);
  pinMode(led_g,OUTPUT);
  pinMode(led_y,OUTPUT);
  pinMode(ir_1,INPUT);//input from IR 1
  pinMode(ir_2,INPUT);//input from IR 2
  pinMode(ir_3,INPUT);//input from IR 3
  pinMode(ir_4,INPUT);//input from IR 4
  pinMode(trigpin_1,OUTPUT);
  pinMode(trigpin_2,OUTPUT);
  pinMode(echopin_1,INPUT);//input time from US1
  pinMode(echopin_2,INPUT);//input time from US2
  irrecv_1.enableIRIn(); //Initialize
  irrecv_2.enableIRIn(); //Initialize
  irrecv_3.enableIRIn(); //Initialize
  irrecv_4.enableIRIn(); //Initialize
}

void loop()
{
  servo_1.write(pos);//initial position for servo motor 
  servo_2.write(pos);
   
  for(int i=1; i>=0; i++)
  {
    servo_1.write(0);//initial position for servo motor is 0
    servo_2.write(0);
  
  digitalWrite(trigpin_1,LOW);//creating a pulse through trig pin
  delayMicroseconds(100);
  digitalWrite(trigpin_1,HIGH);
  delayMicroseconds(100);
  digitalWrite(trigpin_1,LOW);
  time_1=pulseIn(echopin_1,HIGH);//input time through echo pin
  distance_1= (time_1*(0.017));//calculated distance from time
  
  if(distance_1 <= 15)//if vehicle is at distance less than 15cm then the barricade will ope
  {
    for(int pos=0; pos<=90 ;pos++)//opening barricade
  	{
    servo_1.write(pos);
    delay(20);
  	}
    delay(2000);//delay of 2000ms so that vehicle can pass barricade
  	for(int pos=90; pos>=0; pos--)//closing barricade
  	{
    servo_1.write(pos);
    delay(20);
  	}
    break;
  }
  }
  
  //Menu
    Serial.println("For Menu 1 - enter 1 in remote");
    Serial.println("For Menu 2 - enter 3 in remote");
    Serial.println("For Menu 3 - enter 5 in remote");
    Serial.println("For Menu 4 - enter 7 in remote");
  
  delay(5000);//time for selecting order from menu 
  
  for(int i=1; i>=0; i++)
  {
    if(irrecv_1.decode(&results))//when signal is sent by remote
    {       
      Serial.println(results.value, HEX); //print value
      switch(results.value){

  //'0' on remote
    case 0xFD30CF:
      digitalWrite(led_r, LOW);
        
  //'1' on remote
    case 0xFD08F7: 
      delay(2000);//order preparation time
      digitalWrite(led_r, HIGH);
      delay(3000);
      digitalWrite(led_r, LOW); 
      break; 	
     }
   }
  
    if(irrecv_2.decode(&results))//when signal is sent by remote
    {       
      Serial.println(results.value, HEX); //print value
      switch(results.value){
      
    //'2' on remote
    case 0xFD8877:
      digitalWrite(led_b, LOW );

     //'3' on remote
    case 0xFD48B7:
      delay(2000);//order preparation time
      digitalWrite(led_b, HIGH);//order ready
      delay(3000);
      digitalWrite(led_b, LOW );
      break;
     }
    }
    
   if(irrecv_3.decode(&results))//when signal is sent by remote
   {       
     Serial.println(results.value, HEX); //print value
     switch(results.value){
    
      
       //'4' on remote
    case 0xFD28D7:
      digitalWrite(led_g, LOW );

     //'5' on remote
    case 0xFDA857:
      {
      delay(2000);//order preparation time  
      digitalWrite(led_g, HIGH);//order ready
      delay(3000);
      digitalWrite(led_g, LOW );
       break;}
    }
   }
        
   if(irrecv_4.decode(&results))//when signal is sent by remote
   {       
     Serial.println(results.value, HEX); //print value
     switch(results.value){
      
       //'6' on remote
      case 0xFD6897:
      digitalWrite(led_y, LOW );

     //'7' on remote
    case 0xFD18E7:
      delay(2000);//order preparation time
      digitalWrite(led_y, HIGH);//order ready
      delay(3000);
      digitalWrite(led_y, LOW );
    }
   }
    delay(1000);
    break;
  }
  
  for(int i=1; i>=0; i++)
  {
   digitalWrite(trigpin_2,LOW);//creating a pulse through trig pin
   delayMicroseconds(100);
   digitalWrite(trigpin_2,HIGH);
   delayMicroseconds(100);
   digitalWrite(trigpin_2,LOW);
   time_2=pulseIn(echopin_2,HIGH);//input time through echo pin
   distance_2=(time_2*(0.017));//calculated distance from time
  
  
  if(distance_2<=15)//if vehicle is at distance less than 15cm then the barricade will open
  {
    for(int pos=0; pos<=90 ;pos++)//opening barricade
  	{
    servo_2.write(pos);
    delay(20);
  	}
    delay(2000);//delay of 2000ms so that vehicle can pass barricade
  	for(int pos=90; pos>=0; pos--)//closing barricade
  	{
    servo_2.write(pos);
    delay(20);
  	}
    break;
  }
  }
}

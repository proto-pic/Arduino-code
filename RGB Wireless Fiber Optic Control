
int otherRed;
int otherGreen;
int otherBlue;
int myRed = 0;         // Red value
int myGreen = 0;         // Green value
int myBlue = 0;         // Blue value

int oldmyRed = 0;         // Red value
int oldmyGreen = 0;         // Green value
int oldmyBlue = 0;         // Blue value

int theirRed=0;
int theirGreen=0;
int theirBlue=0;

int oldtheirRed=0;
int oldtheirGreen=0;
int oldtheirBlue=0;
#define DULL 5
 byte a;
byte b;
 byte c;
boolean fMyColourChange;
boolean fTheirColourChange;
boolean fNoMaster;// initially both sets are no master 
//boolean fButtonPressed=false;
int pinRED = 9;
int pinGREEN = 10;
int  pinBLUE = 11;
int masterSwitch = 8;
char inChar;
boolean fMineIsMaster;
int x;
String OutputBuffer;
//boolean fShowMyColours;
//declare void function SendColours(int r, int g, int b);

//void function SendColours(int r, int g, int b)
//{
//OutputBuffer = "";
//OutputBuffer += (r);
//OutputBuffer += (",");
//OutputBuffer += (g);
//OutputBuffer += (",");
//OutputBuffer += (b);
//OutputBuffer += ("\n");
//Serial.print(OutputBuffer);
//}

void setup()
{
  fMineIsMaster = false;
  fNoMaster=true;
  //fShowMyColours = true;
  pinMode(pinRED, OUTPUT);   // sets the pin as output
  pinMode(pinGREEN, OUTPUT);   // sets the pin as output
  pinMode(pinBLUE, OUTPUT);   // sets the pin as output
  pinMode(masterSwitch, INPUT);
  digitalWrite(masterSwitch, HIGH);
  Serial.begin(57600); 
  Serial.setTimeout(50);
//  Show startup sequence

for( x = 0; x <= 255; x++)
{
 analogWrite(pinRED, x); 
 delay(5);
}

for( x = 0; x <= 255; x = x++)
{
 analogWrite(pinGREEN, x); 
 delay(5);
}

for( x = 0; x <= 255; x = x++)
{
 analogWrite(pinBLUE, x); 
 delay(5);
}
digitalWrite(pinRED, LOW);
digitalWrite(pinGREEN, LOW);
digitalWrite(pinBLUE, LOW);
fTheirColourChange=false;//needed for initial conditions
delay(1000);

}//end of setup
//*********************************** MAIN LOOP
void loop()
{
 fMyColourChange=false; //set it further down loop if needed
//Get analog values from this module
delay(200); // don't take too many readings
Serial.flush();
//************************************* 
// initially no master so last flag is only true at beginning - no point in reading if not a master or no one has pressed a button yet
        if (fMineIsMaster || fNoMaster)
        {
          myRed = analogRead (A0);
          myGreen = analogRead (A1);
          myBlue = analogRead (A2);
          myRed = constrain(myRed, 0, 255);
          myGreen = constrain(myGreen, 0, 255);
          myBlue = constrain(myBlue, 0, 255);
           myRed = (myRed/DULL)*DULL;
          myGreen = (myGreen/DULL)*DULL;
          myBlue = (myBlue/DULL)*DULL;
         
          fMyColourChange = (myRed!=oldmyRed || myGreen!=oldmyGreen || myBlue!=oldmyBlue);
            if (fMyColourChange)
            {
             oldmyRed = myRed;
             oldmyGreen = myGreen;
             oldmyBlue = myBlue; 
            }
        }


//********************* SERIAL INPUT ROUTINE
  // if there's any serial available, read it:
      while (Serial.available() > 0)
      {
        // look for the next valid integer in the incoming serial stream:
        //inChar=Serial.parseChar();
        char ch=(char)Serial.read();
        if (ch!='&')
        {
          Serial.flush();
          goto outRead;
        }
        
//        theirRed = Serial.parseInt();
//        
//        theirGreen = Serial.parseInt();      
//        theirBlue = Serial.parseInt();   
        a=(unsigned byte)Serial.read();
        b=(unsigned byte)Serial.read(); 
         c=(unsigned byte)Serial.read(); 
       theirRed=(int)a;
       theirGreen=(int)b;
       theirBlue=(int) c;
        theirRed=(theirRed/DULL)*DULL;
        theirGreen=(theirGreen/DULL)*DULL;
        theirBlue=(theirBlue/DULL)*DULL;
        
        // look for the newline. That's the end of your
        // sentence:
        if (Serial.read() == '\n')
        {
         // Serial.flush();
          fMineIsMaster = false;
          fNoMaster = false; // other pressed their button
          fTheirColourChange = (theirRed!=oldtheirRed || theirGreen!= oldtheirGreen || theirBlue!= theirBlue);
        if (fTheirColourChange)
          {
           oldtheirRed = theirRed;
           oldtheirGreen = theirGreen;
           oldtheirBlue = theirBlue; 
          }     
          
        }// \n
        else
        {
          //failed read
          Serial.flush();
          goto outRead;
        
        }
         
    }// end while
//******************************** END SERIAL INPUT ROUTINE



outRead:

//********************* SET LED COLOURS ROUTINE
//  if ((fMineIsMaster && fMyColourChange) || fNoMaster)
if ((fMineIsMaster ) || fNoMaster)
  {
      analogWrite(pinRED, myRed);
      analogWrite(pinGREEN, myGreen);
      analogWrite(pinBLUE, myBlue);  
  }
  if (!fMineIsMaster)// && fTheirColourChange)
  {
      analogWrite(pinRED, theirRed);
      analogWrite(pinGREEN, theirGreen);
      analogWrite(pinBLUE, theirBlue);
            
  }
  //*********************** END SET LED COLOURS
  //********************** READ BUTTON ROUTINE
  if (digitalRead(masterSwitch) == 0)//button pressed
  {
   if (fMineIsMaster)
   {
     //do nothing - alreeady a master so no sense in blatting again
     //goto skip;
   }
    delay(20);
   //can't be a master so make me one
   fMineIsMaster=true;
   fNoMaster=false; //after initialise
       analogWrite(pinRED, myRed);
      analogWrite(pinGREEN, myGreen);
      analogWrite(pinBLUE, myBlue);  
Serial.flush();
//  OutputBuffer = "&";
//  OutputBuffer += (myRed);
//  OutputBuffer += (",");
//  OutputBuffer += (myGreen);
//  OutputBuffer += (",");
//  OutputBuffer += (myBlue);
//  OutputBuffer += ("\n");
//  Serial.print(OutputBuffer);
Serial.write('&');
Serial.write((unsigned byte)myRed);
Serial.write((unsigned byte)myGreen);
Serial.write((unsigned byte)myBlue);
Serial.write((unsigned byte)'\n');
    //SendColours(myRed,myGreen,myBlue);
    goto skip;
  
  } //end button press 
//************************ END BUTTON READ ROUTINE
  if (fMineIsMaster && fMyColourChange)
  {
    Serial.flush();
//    OutputBuffer = "&";
// 
//    OutputBuffer += (myRed);
//    OutputBuffer += (",");
//    OutputBuffer += (myGreen);
//    OutputBuffer += (",");
//    OutputBuffer += (myBlue);
//    OutputBuffer += ("\n");
//    Serial.print(OutputBuffer);
Serial.write('&');
Serial.write((unsigned byte)myRed);
Serial.write((unsigned byte)myGreen);
Serial.write((unsigned byte)myBlue);
Serial.write((unsigned byte)'\n');    
    
  }

//}
  skip:
myGreen=myGreen; 


}//end main loop



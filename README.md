# Track-Follower-Bot
An autonomous track-following robot that uses sensors and control algorithms to detect and follow a predefined path.

const int IR_left = 14;
const int IR_mid_1 = 15;
const int IR_mid_2 = 16;
const int IR_right = 17;
const int speedControl1 = 10;
const int speedControl2 = 11;
const int Motor1_1 = 5;
const int Motor1_2 = 6;
const int Motor2_1 = 7;
const int Motor2_2 = 8;

void setup() {
  Serial.begin(9600);
  pinMode(IR_left, INPUT);
  pinMode(IR_mid_1, INPUT);
  pinMode(IR_mid_2, INPUT);
  pinMode(IR_right, INPUT);
  
  pinMode(speedControl1, OUTPUT);
  pinMode(speedControl2, OUTPUT);
  pinMode(Motor1_1, OUTPUT);
  pinMode(Motor1_2, OUTPUT);
  pinMode(Motor2_1, OUTPUT);
  pinMode(Motor2_2, OUTPUT);
  randomSeed(analogRead(9));
}


int TurnCounter = 0; //1 -> TurnRight(), 2 -> SlightlyTurnRight, -1 -> TurnLeft(), -2 -> SlightlyTurnLeft
int Count1 = 0; // 1100
int Count2 = 0; // 0011


void loop() {
int v1 = !(digitalRead(IR_left));
int v2 = !(digitalRead(IR_mid_1));
int v3 = !(digitalRead(IR_mid_2));
int v4 = !(digitalRead(IR_right));

    if (v1 == 0 && v2 == 0 && v3 == 0 && v4 == 0) {
        //Reverse Previous Turn and MoveForward();
        ReversePreviousTurn();
        MoveForward();
    } else if (v1 == 0 && v2 == 0 && v3 == 0 && v4 == 1) {
        SlightlyTurnRight();
        TurnCounter = 2;
    } else if (v1 == 0 && v2 == 0 && v3 == 1 && v4 == 0) {
        //Continue Previous Turn
        ContinuePreviousTurn();
        MoveForward();
    } else if (v1 == 0 && v2 == 0 && v3 == 1 && v4 == 1) {
        SlightlyTurnRight();
        TurnCounter = 2;
        Count2 = 1;
    } else if (v1 == 0 && v2 == 1 && v3 == 0 && v4 == 0) {
        //Continue Previous Turn
        ContinuePreviousTurn();
        MoveForward();
    } else if (v1 == 0 && v2 == 1 && v3 == 0 && v4 == 1) {
        //Continue Previous Turn
        ContinuePreviousTurn();
        MoveForward();
    } else if (v1 == 0 && v2 == 1 && v3 == 1 && v4 == 0) {
        MoveForward();
        //TurnCounter = 0;
    } else if (v1 == 0 && v2 == 1 && v3 == 1 && v4 == 1) {
        if(Count2 == 1)
        {
          Count2 = 0;
          ReversePreviousTurn();
          MoveForward();
        }
        else{
          TurnRight();
          TurnCounter = 1;
        }
    } else if (v1 == 1 && v2 == 0 && v3 == 0 && v4 == 0) {
        SlightlyTurnLeft();
        TurnCounter = -2;
    } else if (v1 == 1 && v2 == 0 && v3 == 0 && v4 == 1) {
        MoveForward();
        //TurnCounter == 0;
    } else if (v1 == 1 && v2 == 0 && v3 == 1 && v4 == 0) {
        //Continue Previous Turn
        ContinuePreviousTurn();
        MoveForward();
    } else if (v1 == 1 && v2 == 0 && v3 == 1 && v4 == 1) {
        //Continue Previous Turn
        ContinuePreviousTurn();
        MoveForward();
    } else if (v1 == 1 && v2 == 1 && v3 == 0 && v4 == 0) {
        SlightlyTurnLeft();
        TurnCounter = -2;
        Count1 = 1;
    } else if (v1 == 1 && v2 == 1 && v3 == 0 && v4 == 1) {
        //Continue Previous Turn
        ContinuePreviousTurn();
        MoveForward();
    } else if (v1 == 1 && v2 == 1 && v3 == 1 && v4 == 0) {
      if(Count1 == 1)
      {
        Count1 = 0;
        ReversePreviousTurn();
        MoveForward();
      }
      else{
        TurnLeft();
        TurnCounter = -1;
      }
        
    } else if (v1 == 1 && v2 == 1 && v3 == 1 && v4 == 1) {
        int randomValue;
        do {
            randomValue = random(-2, 3); // Generates values from -2 to 2
          } while (randomValue == 0);
        TurnCounter = randomValue;
        ContinuePreviousTurn();
        MoveForward();
        //TurnCounter = 0;
    } else {
        Serial.println("Invalid input");
    }

    //delay(200);
}

void ReversePreviousTurn() {
    if (TurnCounter == 0)
    {
      MoveForward();
    }
    if (TurnCounter == 1) {
        TurnLeft();
    } else if (TurnCounter == 2) {
        SlightlyTurnLeft();
    }
    if (TurnCounter == -1) {
        TurnRight();
    } else if (TurnCounter == -2) {
        SlightlyTurnRight();
    }
    TurnCounter = 0;
}

void ContinuePreviousTurn() {
    if (TurnCounter == 0)
    {
      MoveForward();
    }
    if (TurnCounter == -1) {
        TurnLeft();
    } else if (TurnCounter == -2) {
        SlightlyTurnLeft();
    }
    if (TurnCounter == 1) {
        TurnRight();
    } else if (TurnCounter == 2) {
        SlightlyTurnRight();
    }
}


void MoveForward() {
  digitalWrite(Motor1_1, HIGH);
  digitalWrite(Motor1_2, LOW);
  digitalWrite(Motor2_1, HIGH);
  digitalWrite(Motor2_2, LOW);
  
  analogWrite(speedControl1, 80);
  analogWrite(speedControl2, 80);
}

/**void MoveBackward() {
  unsigned long TimeStart = millis();
  while(millis() - TimeStart < 400)
  {
    digitalWrite(Motor1_1, LOW);
    digitalWrite(Motor1_2, HIGH);
    digitalWrite(Motor2_1, LOW);
    digitalWrite(Motor2_2, HIGH);
  
    analogWrite(speedControl1, 80);
    analogWrite(speedControl2, 80);
  }
  
}
**/

void SlightlyTurnLeft() {
  digitalWrite(Motor1_1, HIGH);
  digitalWrite(Motor1_2, LOW);
  digitalWrite(Motor2_1, LOW);
  digitalWrite(Motor2_2, HIGH);
  
  analogWrite(speedControl1, 65);
  analogWrite(speedControl2, 65);
}

void TurnLeft() {
  digitalWrite(Motor1_1, HIGH);
  digitalWrite(Motor1_2, LOW);
  digitalWrite(Motor2_1, LOW);
  digitalWrite(Motor2_2, HIGH);
  
  analogWrite(speedControl1, 65);
  analogWrite(speedControl2, 85);
}

void SlightlyTurnRight() {
  digitalWrite(Motor1_1, LOW);
  digitalWrite(Motor1_2, HIGH);
  digitalWrite(Motor2_1, HIGH);
  digitalWrite(Motor2_2, LOW);
  
  analogWrite(speedControl1, 65);
  analogWrite(speedControl2, 65);
}

void TurnRight() {
  digitalWrite(Motor1_1, LOW);
  digitalWrite(Motor1_2, HIGH);
  digitalWrite(Motor2_1, HIGH);
  digitalWrite(Motor2_2, LOW);
  
  analogWrite(speedControl1, 85);
  analogWrite(speedControl2, 65);
}

void StopMotors() {
  digitalWrite(Motor1_1, LOW);
  digitalWrite(Motor1_2, LOW);
  digitalWrite(Motor2_1, LOW);
  digitalWrite(Motor2_2, LOW);
}

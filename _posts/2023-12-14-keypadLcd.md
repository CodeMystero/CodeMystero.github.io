---
layout: single

title: "[Linux] Making a calculator using RPi"
excerpt: "raspberryPi, LCD monitor and 4X4 buttons"

categories:
  - Linux server

tag: [linux, raspberryPi, lcd] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)


# Let's connect 4X4 buttons on raspberry Pi

```cpp
 #include <stdio.h>
 #include <wiringPi.h>
 
 const int _R[4] = {12, 16, 20, 21};
 const int _C[4] = {26, 19, 13, 6};

void pin_mode();
void pin_pupd();
void read_keypad(const int, int);
// '{' replaced to '(' due to markdown issue
char key_pad[4][4] = {('1','2','3','+'),
                      ('4','5','6','-'),
                      ('7','8','9','*'),
                      ('=','0','#','/')};

char value = 'a';

int main(){

   wiringPiSetupGpio();
   pin_mode();
   pin_pupd();
   while(1){

      for(int i = 0;i <4;i++)read_keypad(_C[i],i);
     delay(200);
      if(value != 'a')printf("%c\n",value);
       value = 'a';

   }
 }

void read_keypad(const int c, const int col){
   digitalWrite(c, HIGH);
    for(int i = 0;i < 4;i++){
      if(digitalRead(_R[i]) == HIGH)value = key_pad[i][col];
    }
    digitalWrite(c, LOW);
 }

 void pin_mode(){
   for(int i = 0; i<4;i++){
      pinMode(_C[i],OUTPUT);
      pinMode(_R[i],INPUT);
    }
}

void pin_pupd(){
   for(int i = 0; i <4; i++)pullUpDnControl(_R[i], PUD_DOWN);
}
```

## Key_pad 

The variable key_pad represents a mapping of values from the matrix above to a keypad in a one-to-one correspondence. The reason for configuring the keypad in this way is to implement a calculator.

<p align="center"><img src="assets/images/2023-12-14-keypadLcd/buttonlayout.png"></p>

## pin_mode()

The arrays _C and _R indicate the GPIO (General Purpose Input/Output) pins on the Raspberry Pi to which the wires coming from each button are connected. The GPIO pins in the _C array have been configured as outputs, while those in the _R array have been set as inputs.

## read_keypad 

This function allows for the detection of a specific button press by observing which R-input responds with a HIGH signal when the corresponding C-output is set to HIGH. For example, when C1 is set to HIGH, it can be observed that the button containing the number 1 responds by causing R1 to transition from LOW to HIGH. It's crucial to note that when C1 is set to HIGH, the remaining C2, C3, and C4 should be LOW to accurately identify which button has been pressed. This approach follows the CPU polling method.

# Adding calculation function on keypad 

```cpp

#include <stdlib.h>

int chaIntId(char);
void intIsr();
void int_container(double);
void interInit(void);

char operater_1;
double num_cnt = 0;
int flag_cnt = 0;
double snum[2] = {0,};

int main(){
   interInit();
   while(1){
      double src_temp;
      if(chaIntId(value) == 0){
         src_temp = atoi(&value);
         if(flag_cnt == 1){
            int_container(src_temp);
            flag_cnt = 0;
            printf("%.1f\r",num_cnt);
            fflush(NULL);
         }
      }else if(chaIntId(value) == 1){
         printf("\n%c\n",value);
         fflush(NULL);
         snum[0] = num_cnt;
         num_cnt = 0;
         operater_1 = value;
      }else if(chaIntId(value) == 2){
         snum[1] = num_cnt;
         num_cnt = 0;

         switch(operater_1){
            case '+':
               printf("\n>> %.1f\n",snum[0]+snum[1]);
               break;
            case '/':
               printf("\n>> %.1f\n",snum[0]/snum[1]);
               break;
            case '*':
               printf("\n>> %.1f\n",snum[0]*snum[1]);
               break;
            case '-':
               printf("\n>> %.1f\n",snum[0]-snum[1]);
               break;
         }
      }
      value = 'a';
   }
}

void interInit(void){
   for(int i = 0; i<4;i++){
      wiringPiISR(_R[i],INT_EDGE_FALLING,&intIsr);
   }
}
void intIsr(void){
   flag_cnt = 1;
}
void int_container(double src){
   num_cnt = num_cnt*10 + src;
}
int chaIntId(char src){ // 0 if int, 1 if char

   if(src == '1'||src == '2'||src == '3'||src =='4'||src == '5'\
      ||src == '6'||src == '7'||src == '8'||src == '9'||src =='0'){
      return 0;
   }else if (src == '#'||src == '/'||src == '*'||src == '-'||src =='+'){
      return 1;
   }else if (src == '='){
      return 2;
   }
}
```
## interInit()

To ensure that only one input value is stored in the operand when a button is pressed, an interrupt has been triggered. This function takes as parameters the GPIO for the interrupt (_R), the interrupt mode (INT_EDGE_FALLING), and arguments for the interrupt service routine (ISR).

## intIsr()

This function defines the interrupt service routine. When a button is pressed, an interrupt is triggered, setting the flag_cnt variable to 1. Subsequently, the int_container variable stores the input value only once before resetting the flag_cnt variable to 0, preventing duplicate inputs

## int_container()

This function takes an operand as input. To input a number as the higher digit of the operand, the function multiplies the previously entered value by 10 when a new value is received and performs addition with the new value.

## chaIntId()

This function determines whether the input from the button is an operand, an operator, or the '=' sign, and returns the corresponding value. If an operand is input, the function returns 0. If an operator is input, it returns 1. If the '=' sign is input, it returns 2.

# Displaying computing on LCD display

To refer to details, please visit to the GitHub repository for detailed code related to the LCD, including the header files and implemented functions. Note that the LCD in question is implemented using I2C communication.

## keypad.c 

```cpp
#include <unistd.h>
#include "lcd_config.h"


char equal[] = "     >> ";
char space[] = " ";

int main(){

  xio = wiringPiI2CSetup(I2C_ADDR);
  lcd_init();

  while(1){
    double src_temp;
    if(chaIntId(value) == 0){
        src_temp = atoi(&value);
        if(flag_cnt == 1){
          int_container(src_temp);
          flag_cnt = 0;

          if(snum[0] == 0){
              lcd_byte(0x01, LCD_CMD);
              lcdLoc(LINE1);
              typeInt(num_cnt);
          }else if(snum[0] != 0){
              typeInt(src_temp);
          }
        }
    }else if(chaIntId(value) == 1){
        typeln(space);
        typeChar(value);
        typeln(space);
        snum[0] = num_cnt;
        num_cnt = 0;
        operater_1 = value;
    }else if(chaIntId(value) == 2){
        snum[1] = num_cnt;
        num_cnt = 0;

        switch(operater_1){
          case '+':
              lcdLoc(LINE2);
              typeln(equal);
              typeDouble(snum[0]+snum[1]);
              snum[0] = 0;
              break;
          case '/':
              lcdLoc(LINE2);
              typeln(equal);
              typeDouble(snum[0]/snum[1]);
              snum[0] = 0;
              break;
          case '*':
              lcdLoc(LINE2);
              typeln(equal);
              typeDouble(snum[0]*snum[1]);
              snum[0] = 0;
              break;
          case '-':
              lcdLoc(LINE2);
              typeln(equal);
              typeDouble(snum[0]-snum[1]);
              snum[0] = 0;
              break;

        }
    }
    value = 'a';
  }
}
```

### lcdLoc()

This function determines the line of the LCD panel. It takes the addresses of LINE1 and LINE2 as arguments. Depending on the address provided, the subsequent input characters will be written to the corresponding line on the LCD.



# Result



<p align="center"><img src="images\keppad_lcd.gif"></p>

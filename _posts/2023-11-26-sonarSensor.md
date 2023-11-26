---
layout: single

title: "[STM32] Driving ultrasonic sencor(HC-SR04)"
excerpt: "using systick for Rear Vehicle Detection"

categories:
  - Firmware enginnering

tag: [STM32, systick, RVD, Keil, nucleo, HCSR04, C, piezo_speaker] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)

# HC-SR04 (ultrasonic sensor)

## operating principle
>HC-SR04 typically operates using four pins</br>
### pin description

|pin|description|
|---|---|
|Vcc|high voltage|
|GRD| ground voltage|
|Trig|Trigger input to sensor module|
|Echo|Echo purse output to user module|


### pulse signal
>To operate the sensor, requies to send the pulse signal (10us TTL) specified in the datasheet. There are two methods for this. The first method involves sending a PWM signal and using the input capture method to read the echo. The second method is to use SysTick to create a delay function and adjust the GPIO pin output according to the specifications in the datasheet. please refer to the table below



<img src="/assets/images/2023-11-26-RVD_sonarSensor/sornal_sensor.png">


# systick configuration 
> To operate the sensor, microsecond (us) delays are required. However, the default HAL_Delay provided by STM32 offers delays only in milliseconds (ms). Therefore, we create a microsecond delay function using SysTick configuration.


## define systick register address
>define required register address 

```c
#define STK_CRTL 		*(volatile unsigned int*)0xE000E010
#define STK_LOAD 		*(volatile unsigned int*)0xE000E014
#define STK_VAL  		*(volatile unsigned int*)0xE000E018
#define STK_CALIB 	        *(volatile unsigned int*)0xE000E01C


// the system clock period to 1us as follows:
void SysTick_Init(){
	STK_LOAD = 100 - 1;		//1us
	STK_VAL = 0;
	STK_CRTL = 6;
	uwTick = 0;
}

/*The following function completes a sequence of generating
a pulse, reading the echo, and then restoring the clock to
its original state. so that Hal_Delay() can be used again*/
void HAL_Delay_Porting(){
	STK_LOAD = 100000 - 1;
	STK_CRTL |= 7;
}

//The following code is written inside a while loop.
SysTick_Init();
/*Trigger input to Module*/
HAL_GPIO_WritePin(Trig_GPIO_Port,Trig_Pin,1);
usDelay(15);
HAL_GPIO_WritePin(Trig_GPIO_Port,Trig_Pin,0);
		
while(HAL_GPIO_ReadPin(Echo_GPIO_Port, Echo_Pin)==0);
STK_CRTL |= (1<<ENABLE); // system timer start
while(HAL_GPIO_ReadPin(Echo_GPIO_Port, Echo_Pin) ==1);
echoTime = HAL_GetTick();
STK_CRTL &= ~(1<<ENABLE);
// 340 m/s ->340 * (100/1000000) cm/us -> 0.034 cm/us
distance = ((echoTime/2.0) *0.034);
printf("distance = %.1lf cm \r\n",((echoTime/2.0) *0.034));
HAL_Delay_Porting();
```



# rear vehicle detection 
>To implement a rear sensor for vehicles, it is necessary to convert the echo signal (in microseconds) coming from the sensor into distance. To implement sound, the pin allocated for the Timer is connected to the piezoelectric speaker.
>>speed of sound = 340ms = **0.034us**
>>>distance = ((echoTime/2.0) *0.034);


```c
HAL_TIM_Base_Start(&htim3); //base timer start
TIM3->PSC = 100-1;
	
HAL_TIM_PWM_Start(&htim4, TIM_CHANNEL_1);
TIM4->PSC |=100-1 ; // 1000000Hz
TIM4->ARR = ((1000000/1700) - 1);
TIM4->CCR1 &= 0x00;

while(1){
if( distance <10){
  TIM4->CCR1 = (1000000/1700)/2 - 1;
  if(distance <10 && distance >=2) HAL_Delay(distance *30);
  if (distance < 2)HAL_Delay(1000);
  TIM4->CCR1 &= 0x00;
}
HAL_Delay(50);
```


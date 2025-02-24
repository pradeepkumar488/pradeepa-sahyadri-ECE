#include <ch32v00x.h> 
#include <debug.h> 

/* PWM Output Mode Definition */ 
#define PWM_MODE1 0 
#define PWM_MODE2 1 
/* PWM Output Mode Selection */ 
//#define PWM_MODE PWM_MODE1 
#define PWM_MODE PWM_MODE2 

// Configuration for Timer 1 void TIM1_PWMOut_Init(u16 arr, u16 psc, u16 ccp) 

{ 

GPIO_InitTypeDef GPIO_InitStructure={0}; 
TIM_OCInitTypeDef TIM_OCInitStructure={0}; 
TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure={0}; 

RCC_APB2PeriphClockCmd( RCC_APB2Periph_GPIOD, ENABLE );
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_2; 
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP; //alternate mode
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_10MHz; 
GPIO_Init( GPIOD, &GPIO_InitStructure ); 

RCC_APB2PeriphClockCmd( RCC_APB2Periph_TIM1, ENABLE );
TIM_TimeBaseInitStructure.TIM_Period = arr;
TIM_TimeBaseInitStructure.TIM_Prescaler = psc;
TIM_TimeBaseInitStructure.TIM_ClockDivision = TIM_CKD_DIV1;
TIM_TimeBaseInitStructure.TIM_CounterMode = TIM_CounterMode_Up;
TIM_TimeBaseInit( TIM1, &TIM_TimeBaseInitStructure); 

#if (PWM_MODE == PWM_MODE1) TIM_OCInitStructure.TIM_OCMode = TIM_OCMode_PWM1; 

#elif (PWM_MODE == PWM_MODE2) TIM_OCInitStructure.TIM_OCMode = TIM_OCMode_PWM2; 

#endif TIM_OCInitStructure.TIM_OutputState = TIM_OutputState_Enable; 

TIM_OCInitStructure.TIM_Pulse = ccp; 
TIM_OCInitStructure.TIM_OCPolarity = TIM_OCPolarity_High; 
TIM_OC1Init( TIM1, &TIM_OCInitStructure );
TIM_CtrlPWMOutputs(TIM1, ENABLE ); 
TIM_OC1PreloadConfig( TIM1, TIM_OCPreload_Disable );
TIM_ARRPreloadConfig( TIM1, ENABLE ); 
TIM_Cmd( TIM1, ENABLE ); 

} 

void GPIO_Config(void) 
{ 

GPIO_InitTypeDef GPIO_InitStructure = {0}; 
//structure variable used for the GPIO configuration
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE); // to Enable the clock for Port D 

GPIO_InitStructure.GPIO_Pin = GPIO_Pin_3; // Defines which Pin to configure 

GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU; // Defines Output Type 
GPIO_Init(GPIOD, &GPIO_InitStructure); 
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_6; // Defines which Pin to configure 

GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP; // Defines Output Type 

GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz; // Defines speed
GPIO_Init(GPIOD, &GPIO_InitStructure); 

} 

int main(void) 
{ 

uint8_t GPIOInputStatus = 0;
NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2); 
SystemCoreClockUpdate(); 
Delay_Init(); 
GPIO_Config(); 

 while(1) 
 { 
  GPIOInputStatus = GPIO_ReadInputDataBit(GPIOD,GPIO_Pin_3);
  if(GPIOInputStatus == 0) 

 { 
  GPIO_WriteBit(GPIOD, GPIO_Pin_6, SET); 
  // blink led TIM1_PWMOut_Init( 100, 480-1, 10); 
 //10% dutycycle pwm out 
 } 
 else 
 { 
 GPIO_WriteBit(GPIOD, GPIO_Pin_6, RESET); 
 TIM1_PWMOut_Init( 100, 480-1, 95);  //95% dutyclycle pwm out 
 } 

 Delay_Ms(100); 
 } 
 } 

void NMI_Handler(void) 
{
} 

void HardFault_Handler(void) 
 { 
   while (1) 
   {

   } 
 }

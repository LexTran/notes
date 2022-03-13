# 自定义头文件

```c
//helloRobot.h
//定义延时函数
void delay_nus(unsigned long n){//延时n us，n>=6,最小延时单位6us
  unsigned long j;
  while(n--){//外部晶振：8M；PLL(Phase Lock Loop)：9；8M*9=72MHz
    j = 8;//微调参数，保证延时精度
    while(j--);
  }
}

void delay_nms(unsigned long n){//延时n ms
  while(n--)//外部晶振：8M；PLL：9；8M*9=72MHz
    delay_nus(1100);//1ms延时补偿
}

GPIO_InitTypeDef GPIO_InitStructure;

//定义GPIO配置
void GPIO_Configuration(){
  //configuring USART1 Tx(PA.09) as AF_PP
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_9;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOA, &GPIO_InitStructure);
  
  //configuring USART1 Rx(PA.09) as IN_FLOATING
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_10;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
  GPIO_Init(GPIOA, &GPIO_InitStructure);
  
  //configuring LEDs IO
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_8|GPIO_Pin_9;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOB, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_12|GPIO_Pin_13;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOC, &GPIO_InitStructure);
  
  //configuring Motors IO
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_7;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOD, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_8;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOD, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_9;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOD, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_10;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOD, &GPIO_InitStructure);
  
  //congifure infrared IO
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOE, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_1;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOE, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_2;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOE, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_3;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOE, &GPIO_InitStructure);
  
  //configure ADC IO
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AIN;
  GPIO_Init(GPIOB, &GPIO_InitStructure);
  
  //configure KEY IO PC8 to PC11
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_8;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOC, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_9;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOC, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_10;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOC, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_11;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOC, &GPIO_InitStructure);
  
  //configure INT IO PE4
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_4;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOE, &GPIO_InitStructure);
  
  //configure INT IO PE5
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_5;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOE, &GPIO_InitStructure);
  
  //configure LCD1602 IO
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0|GPIO_Pin_1|GPIO_Pin_2|GPIO_Pin_3
    													|GPIO_Pin_4|GPIO_Pin_5|GPIO_Pin_6|GPIO_Pin_7;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOC, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_5|GPIO_Pin_6|GPIO_Pin_4;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_Init(GPIOD, &GPIO_InitStructure);
}
```

# stm32I/O端口与伺服电机控制

## 输入输出端口

控制机器人伺服电机以不同速度运动是通过让单片机的I/O端口输出不同脉冲序列实现。stm32-M3有5个16位并行I/O口：PA、PB、PC、PD、PE，这五个口即可以做输入也可以做输出；可以按照16位处理，也可以按位方式(1位)使用。

为了验证端口输出电平是不是程序输出的电平，可以在想验证的端口处接一个发光二极管，输出低电平时二极管亮，如下程序使用PC13控制发光二极管以1Hz频率闪烁

```c
//ledBlink.c
#include "stm32f10x_heads.h"//stm32官方库
#incldue "helloRobot.h"//此为自定义库

int main(void){
  BSP_Init();//开发板初始化
  /*
  * RCC_Configuration();   //系统时钟设置
  * GPIO_Configuration();  //设置I/O口
  * NVIC_Configuration();  //设置中断
  */
  USART_Configuration();//配置USART
  /*
  * USART_InitStructure.USART_BaudRate = 115200;
  * USART_InitStructure.USART_WordLength = USART_WordLength_8b;
  * USART_InitStructure.USART_StopBits = USART_StopBits_1;
  * USART_InitStructure.USART_Parity = USART_Parity_No;
  * USART_InitStructure.USART_HardwareFlowControl = USART_HardwareFlowControl_None;
  * USART_InitStructure.USART_Mode = USART_Mode_Rx | USART_Mode_Tx;
  * USART_Init(USART1, &USART_InitStructure);
  * USART_Cmd(USART1, ENABLE);
  */
  printf("Program Running!\n");
  
  while(1){
    GPIO_SetBits(GPIOC, GPIO_Pin_13);//PC13输出高电平
    delay_nms(500);//延时500ms
    GPIO_ResetBits(GPIOC, GPIO_Pin_13);//PC13输出低电平
    delay_nms(500);
  }
}
```

## 时钟配置

五个时钟源：HSI(High Speed Internal)、HSE(High Speed External)、LSI(Low Speed Internal)、LSE(Low Speed Internal)、PLL(Phase Lock Loop)。

- HSI：RC振荡器，8MHz
- HSE：接石英/陶瓷谐振器或外部时钟源，4～16MHz
- LSI：RC振荡器，30～60kHz，40kHz(典型值)的供独立看门狗IWDG使用(还可被选择为实时时钟RTC时钟源)
- LSE：接频率为32.768kHz的石英晶体，供实时时钟RTC使用
- PLL：锁相环倍频输出，时钟输入源可选HSI/2、HSE或HSE/2，倍频可选2～16倍，但输出不得超过72MHz

stm32中有一个全速功能的USB模块，其串行接口引擎需要一个频率为48MHz的时钟源，只能从PLL获取；系统时钟SYSCLK，提供stm32中绝大部分器件工作的时钟源。系统时钟可选择为PLL输出、HSI或HSE，系统时钟最大频率为72MHz，通过AHB分频器分频后送给各模块使用。AHB分频器可选择1、2、4、8、16、64、128、256、512分频。AHB分频器输出的时钟送给8大模块使用：

1. 送给SDIO使用的SDIOCLK时钟
2. 送给FSMC使用的FSMCCLK时钟
3. 送给AHB总线、内核、内存和DMA使用的HCLK时钟
4. 通过8分频后送给Cortex的系统定时器时钟(SysTick)
5. 直接送给Cortex的空闲运行时钟FCLK
6. 送给APB1分频器，APB1分频器可选择1、2、4、8、16分频，其输出一路供APB1外设使用(PCLK1，最大频率为36MHz)另一路送给定时器2、3、4倍频器使用
7. 送给APB2分频器，APB2分频器可选择1、2、4、8、16分频，其输出一路供APB2外设使用(PCLK2，最大频率为36MHz)另一路送给定时器1倍频器使用，另外还有一路输出供ADC分频器使用，分频后得到ADCCLK时钟，送给ADC模块使用
8. 2分频后送给SDIO AHB接口使用(HCLK/2)

随着芯片工艺发展，处理器频率越来越快，但是外设接口(如串口)的时钟没有处理器频率快，所以需要分离时钟，最大限度发挥器件功效。

```c
//stm32f10x_map.h
//复位和时钟配置函数RCC_Configuration
typedef struct{
  vu32 CR;				//时钟控制寄存器
  vu32 CFGR;			//时钟配置寄存器
  vu32 CIR;				//时钟中断寄存器
  vu32 APB2RSTR;  //APB2外设复位寄存器
  vu32 APB1RSTR;  //APB1外设复位寄存器
  vu32 AHBENR;		//AHB外设时钟使能寄存器
  vu32 APB2ENR;		//APB2外设时钟使能寄存器
  vu32 APB1ENR;		//APB1外设时钟使能寄存器
  vu32 BDCR;			//备份域控制寄存器
  vu32 CSR;				//控制/状态寄存器
}RCC_TypeDef;			

#define PERIPH_BASE ((u32)0x40000000)
#define AHBPERIPH_BASE (PERIPH_BASE + 0x20000)
#define RCC_BASE (AHBPERIPH_BASE + 0x1000)
#ifndef _RCC
#define RCC ((RCC_TypeDef *)RCC_BASE)
#endif
```

其中vu32是一个在stm32f10x_type.h文件中定义的，代表32位无符号长整型数

## I/O端口配置

stm32系列单片机的输入/输出引脚可配置成以下8种(4输入+2输出+2复用输出)形式：

1. 浮空输入：In_Floating
2. 带上拉输入：IPU(In Push-Up)
3. 带下拉输入：IPD(In Push-Down)
4. 模拟输入：AIN(Analog In)
5. 开漏输出：OUT_OD，OD代表开漏(Open Drain)
6. 推挽输出：OUT_PP，PP代表推挽式(Push-Pull)
7. 复用功能的推挽输出：AF_PP，AF代表复用(Alternate-Function)
8. 复用功能的开漏输出：AF_OD

开漏输出指的是MOS管漏极开路，要得到高电平状态需要上拉电阻才行，一般用于线或、线与，适合做电流型驱动。

推挽输出指的是若两个参数相同的MOS管受两个互补信号的控制，始终处于一个导通、一个截止的状态，就是推挽相连，成为推拉式电路。

```c
//stm32f10x_gpio.h
//GPIO_Configuration
typedef enum{
  GPIO_Speed_10Mhz = 1,
  GPIO_Speed_2Mhz,
  GPIO_Speed_50Mhz
}GPIOSpeed_TypeDef;

typedef enum{
  GPIO_Mode_AIN = 0x0,
  GPIO_Mode_IN_FLOATING = 0x04,
  GPIO_Mode_IPD = 0x28,
  GPIO_Mode_IPU = 0x48,
  GPIO_Mode_Out_OD = 0x14,
  GPIO_Mode_Out_PP = 0x10,
  GPIO_Mode_AF_OD = 0x1c,
  GPIO_Mode_AF_PP = 0x18
}GPIOMode_TypeDef;

typedef struct{
  u16 GPIO_Pin;
  GPIOSpeed_TypeDef GPIO_Speed;
  GPIOMode_TypeDef GPIO_Mode;
}GPIO_InitTypeDef;
```

对应的外设的I/O功能有以下三种情况：

1. 外设对应的引脚为输入：则根据外围电路的配置可以选择浮空输入、带上拉输入或带下拉输入
2. ADC对应的引脚：配置引脚为模拟输入
3. 外设对应的引脚为输出：需要根据外围电路的配置选择对应的引脚为复用功能的推挽输出或复用功能的开漏输出。若将端口配置成复用输出，引脚和输出寄存器断开，并和片上外设的输出信号连接；将引脚配置成复用输出后，若外设未激活，则输出不确定。当GPIO口设为输入模式时，输出驱动电路与端口是断开，此时输出速度配置无意义。当复位期间和刚复位后，复用功能为开启，I/O端口配置成浮空输入。当GPIO口配置成输出，有3种速度可选，指的是I/O口驱动电路的响应速度不是信号输出的速度。

stm32系列的单片机每个GPIO端口有两个32位配置寄存器，两个32位数据寄存器，一个32位置位/复位寄存器，一个16位复位寄存器和一个32位锁定寄存器，IO端口寄存器必须按32位字访问，不允许半字访问或字节访问。

```c
//stm32f10x_map.h
#define PERIPH_BASE
#define APB2PERIPH_BASE

typedef struct{
  vu32 CRL;//配置寄存器低32
  vu32 CRH;//配置寄存器高32
  vu32 IDR;//输入数据寄存器
  vu32 ODR;//输出数据寄存器
  vu32 BSRR;//位置位/复位寄存器
  vu32 BSR;//位复位寄存器
  vu32 LCKR;//位锁定寄存器
}GPIO_TypeDef;

#define AFIO_BASE (APB2PERIPH_BASE + 0x0000)
#define EXTI_BASE (APB2PERIPH_BASE + 0x0400)
#define GPIOA_BASE (APB2PERIPH_BASE + 0x0800)
#define GPIOB_BASE (APB2PERIPH_BASE + 0x0C00)
#define GPIOC_BASE (APB2PERIPH_BASE + 0x1000)
#define GPIOD_BASE (APB2PERIPH_BASE + 0x1400)
#define GPIOE_BASE (APB2PERIPH_BASE + 0x1800)

#ifndef _GPIOA
#define GPIOA ((GPIO_TypeDef *)GPIOA_BASE) 
#endif
#ifndef _GPIOB
#define GPIOB ((GPIO_TypeDef *)GPIOB_BASE) 
#endif
#ifndef _GPIOC
#define GPIOC ((GPIO_TypeDef *)GPIOC_BASE) 
#endif
#ifndef _GPIOD
#define GPIOD ((GPIO_TypeDef *)GPIOD_BASE) 
#endif
#ifndef _GPIOE
#define GPIOE ((GPIO_TypeDef *)GPIOE_BASE) 
#endif
```

GPIO_Init的第一个参数是GPIOx(x=A，B，C，D，E)寄存器的存储映射首地址，第二个参数是用户对GPIO端口设置的参数所在的首地址。

```c
//stm32f10x_gpio.c
void GPIO_SetBits(GPIO_TypeDef *GPIOx, u16 GPIO_Pin){
  assert(IS_GPIO_PIN(GPIO_Pin));//断言检查是否定义了GPIO_Pin
  GPIOx->BSSR = GPIO_Pin;
}

void GPIO_ResetBits(GPIO_TypeDef *GPIOx, u16 GPIO_Pin){
  assert(IS_GPIO_PIN(GPIO_Pin));
  GPIOx->BRR = GPIO_Pin;
}

void GPIO_write(GPIO_TypeDef *GPIOx, u16 PortVal){//写入数据
  GPIOx->ODR = PortVal;
}

void GPIO_WriteBit(GPIO_TypeDef *GPIOx, u16 GPIO_Pin, BitAction BitVal){//写入位数据
  assert(IS_GET_GPIO_PIN(GPIO_Pin));
  assert(IS_GPIO_BIT_ACTION(BitVal));
  
  if(BitVal != Bit_RESET)
  {
    GPIOx->BSRR = GPIO_Pin;
  }else{
    GPIOx->BRR = GPIO_Pin;
  }
}

u16 GPIO_ReadOutputData(GPIO_TypeDef *GPIOx){//读出输出数据
  return ((u16)GPIOx->ODR);
}

u8 GPIO_ReadOutputDataBit(GPIO_TypeDef *GPIOx, u16 GPIO_Pin){//读出输出数据位
  u8 bitstatus = 0x00;
  assert(IS_GET_GPIO_PIN(GPIO_Pin));
  if((GPIOx->ODR & GPIO_Pin) != (u32)Bit_RESET){
    bitstatus = (u8)Bit_SET;
  }else{
    bitstatus = (u8)Bit_RESET;
  }
  return bitstatus;
}
```

stm32配置片内外设使用的通用IO端口需经过以下几步设置：

- 配置输入的时钟
- 初始化后激活
- 若使用该外设的IO引脚，则需要配置相应的GPIO端口，否则该外设对应的IO引脚可以做普通GPIO引脚使用
- 配置各个PIN端口的模式和速度
- GPIO初始化

### 流水灯

```c
#include "stm32f10x_heads.h"
#include "Led_Blink.h"
int main(void){
  BSP_Init();
  USART_Configuration();
  printf("Program Running!\n");
  
  while(1){
    GPIO_SetBits(GPIOB, GPIO_Pin_8);
    delay_nms(500);
    GPIO_ResetBits(GPIOB, GPIO_Pin_8);
    delay_nms(500);
    
    GPIO_SetBits(GPIOB, GPIO_Pin_9);
    delay_nms(500);
    GPIO_ResetBits(GPIOB, GPIO_Pin_9);
    delay_nms(500);
    
    GPIO_SetBits(GPIOC, GPIO_Pin_12);
    delay_nms(500);
    GPIO_ResetBits(GPIOC, GPIO_Pin_12);
    delay_nms(500);
    
    GPIO_SetBits(GPIOC, GPIO_Pin_13);
    delay_nms(500);
    GPIO_ResetBits(GPIOC, GPIO_Pin_13);
    delay_nms(500);
  }
}
```

## IO端口应用

### 机器人伺服电机控制信号

输出高电平持续1.5ms，低电平持续20ms然后不断重复地控制脉冲序列发给伺服电机时，伺服电机不会转动，若此时转动，说明伺服电机需要标定。当高电平持续时间为1.3ms时，伺服电机顺时针全速旋转；当高电平持续时间为1.7ms时，伺服电机逆时针全速旋转。

```c
while(1){
  GPIO_SetBits(GPIOD, GPIO_Pin_10);
  delay_nus(1500);//使用nus不使用nms，原因在于伺服电机对于时间要求更精准，使用nus误差小
  GPIO_ResetBits(GPIOD, GPIO_Pin_10);
  delay_nus(1500);
}
```

```c
#include "stm32f10x_heads.h"
#include "HelloRobot.h"

int main(void){
  BSP_Init();
  USART_Configuration();
  printf("Program Running!\n");
  
  while(1){
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    GPIO_SetBits(GPIOD, GPIO_Pin_9);
    dealy_nus(1300);//高电平持续1.3ms
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    delay_nms(20);//低电平持续20ms
}
```

### 计数并控制循环次数

```c
#include "stm32f10x_heads.h"
#include "HelloRobot.h"

int main(void){
  int counter;
  BSP_Init();
  USART_Configuration();
  printf("Program Running!\n");
  
  for(counter=1;counter<=130;counter++){
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(1700);
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    
    GPIO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(1300);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    
    delay_nms(20);
  }
  for(counter=1;counter<=130;counter++){
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(1300);
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    
    GPIO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(1700);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    
    delay_nms(20);
  }
}
```

### 利用计算机控制机器人运动

```c
#include "stm32f10x_heads.h"
#include "HelloRobot.h"

unsigned int USART_Scanf(){//串口数据接收
  u16 index = 0;
  u16 recdata;
  u16 tmp[5] = {0,0,0,0,0};
  
  while(1){
    while(USART_GetFlagStatus(USART1, USART_FLAG_RXNE)==RESET);
    tmp[index] = (USART_ReceiveData(USART1));
    if(tmp[index]=='#'){
      if(index!=0)
        break;
      else
        continue;
    }
    index++;
  }
  if(index==1)
    recdata=(tmp[0] - 0x30);
  if(index==2)
    recdata=(tmp[1] - 0x30) + ((tmp[0]-0x30)*10);
  if(index==3)
    recdata=(tmp[2] - 0x30) + ((tmp[1]-0x30)*10) + ((tmp[0]-0x30)*100);
  if(index==4)
    recdata=(tmp[3] - 0x30) + ((tmp[2]-0x30)*10) + ((tmp[1]-0x30)*100) + ((tmp[0]-0x30)*1000);
  return recdata;
}

int main(void){
  int counter;
  unsigned int PulseNumber, PulseDuration;
  BSP_Init();
  USART_Configuration();
  printf("Program Running!\n");
  
  printf("Please input pulse number:\r\n");
  PulseNumber=USART_Scanf();//计算机输入脉冲循环次数
  printf("Input pulse number is %d\r\n", PulseNumber);
  
  printf("Please input pulse duration:\r\n");
  PulseDuration=USART_Scanf();//计算机输入脉冲持续时间
  printf("Input pulse duration is %d\r\n", PulseDuration);
  
  for(counter=1;counter<PulseNumber;counter++){
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(PulseDuration);
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    delay_nms(20);
  }
  for(counter=1;counter<PulseNumber;counter++){
    GPIO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(PulseDuration);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    delay_nms(20);
  }
  while(1);
}
```

# stm32程序模块化设计与机器人运动控制

## 程序模块化设计

### 基本巡航动作

```c
#include "stm32f10x_heads.h"
#include "HelloRobot.h"

int main(void){
  int counter;
  BSP_Init();
  USART_Configuration();
  printf("Program Running!\n");
  
  for(counter=1;counter<=65;counter++){//向前
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(1700);//左轮逆时针
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    
    GPIO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(1300);//右轮顺时针
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    
    delay_nms(20);
  }
  for(counter=1;counter<=26;counter++){//向左转
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(1300);
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    
    GPIO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(1300);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    
    delay_nms(20);
  }
  for(counter=1;counter<=26;counter++){//向右转
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(1700);
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    
    GPIO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(1700);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    
    delay_nms(20);
  }
  for(counter=1;counter<=65;counter++){//后退
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(1300);
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    
    GPIO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(1700);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    
    delay_nms(20);
  }
  for(counter=1;counter<=65;counter++){//绕一个轮子原地旋转
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(1500);
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    
    GPIO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(1300);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    
    delay_nms(20);
}
```

要注意这样的程序机器人运动停止很突然，由于惯性会导致机器人倾倒，更合理的做法是令机器人能够做匀加/减速运动

```c
#include "stm32f10x_heads.h"
#include "HelloRobot.h"

void Forward(int PulseCount, int Velocity){//速度应在0～200之间
  int i;
  for(i=1;i<PulseCount;i++){
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(1500+Velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    GPO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(1500-Velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    delay_nms(20);
  }
}

void Left(int PulseCount, int Velocity){//速度应在0～200之间
  int i;
  for(i=1;i<PulseCount;i++){
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(1500-Velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    GPO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(1500-Velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    delay_nms(20);
  }
}

void Right(int PulseCount, int Velocity){//速度应在0～200之间
  int i;
  for(i=1;i<PulseCount;i++){
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(1500+Velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    GPO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(1500+Velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    delay_nms(20);
  }
}

void Backward(int PulseCount, int Velocity){//速度应在0～200之间
  int i;
  for(i=1;i<PulseCount;i++){
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(1500-Velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    GPO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(1500+Velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    delay_nms(20);
  }
}

int main(void){
  BSP_Init();
  USART_Configuration();
  printf("Program Running!\n");
  
  Forward(65, 200);
  Left(26, 200);
  Right(26, 200);
  Backward(65, 200);
  
  while(1);
}
```

上述函数中四个方向的运动代码相似，可以进一步抽象简化

```c
#include "stm32f10x_heads.h"
#include "HelloRobot.h"

void Move(int counter, int PC1_velocity, int PC0_velocity){//速度应在0～200之间
  int i;
  for(i=1;i<counter;i++){
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(1500 + PC1_velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    GPO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(1500 + PC0_velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    delay_nms(20);
  }
}

int main(void){
  BSP_Init();
  USART_Configuration();
  printf("Program Running!\n");
  
  Move(65, +200, -200);//前进参数正负
  Move(26, -200, -200);//左转参数负负
  Move(26, +200, +200);//右转参数正正
  Move(65, -200, +200);//后退参数负正
  
  while(1);
}
```

当需要机器人做大量复杂运动时，调用子函数会显得冗杂，可以使用数组存放指令调用

```c
#include "stm32f10x_heads.h"
#include "HelloRobot.h"

void Move(int counter, int PC1_velocity, int PC0_velocity){//速度应在0～200之间
  int i;
  for(i=1;i<counter;i++){
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(1500 + PC1_velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    GPO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(1500 + PC0_velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    delay_nms(20);
  }
}

int main(void){
  BSP_Init();
  USART_Configuration();
  printf("Program Running!\n");
  
  char Navigation[10] = {'F','L','F','F','R','B','L','Q'};
  int address = 0;
  while(Navigation[address]!='Q'){
    switch(Navigation[address]){
      case 'F':Move(65, +200, -200);break;
      case 'L':Move(26, -200, -200);break;
      case 'R':Move(26, +200, +200);break;
      case 'B':Move(65, -200, +200);break;
    }
    address++;
  }
  while(1);
}
```

# stm32中断编程与机器人触觉导航

## 按键输入检测

将按键与单片机的IO端口相连，当有按键被按下时，相应的端口为低电平，没有按下时为高电平。

### 按键检测

```c
#include "stm32f10x_heads.h"
#include "HelloRobot.h"

int main(void){
  BSP_Init();
  USART_Configuration();
  printf("Program Running!\n");
  while(1){
    if(GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_8)==0){
      if(GPIO_ReadOutputDataBit(GPIOB, GPIO_Pin_8)==0)
        GPIO_SetBits(GPIOB, GPIO_Pin_8);
      else
        GPIO_ResetBits(GPIOB, GPIO_Pin_8);
    }
    if(GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_9)==0){
      if(GPIO_ReadOutputDataBit(GPIOB, GPIO_Pin_9)==0)
        GPIO_SetBits(GPIOB, GPIO_Pin_9);
      else
        GPIO_ResetBits(GPIOB, GPIO_Pin_9);
    }
    if(GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_10)==0){
      if(GPIO_ReadOutputDataBit(GPIOC, GPIO_Pin_12)==0)
        GPIO_SetBits(GPIOC, GPIO_Pin_12);
      else
        GPIO_ResetBits(GPIOC, GPIO_Pin_12);
    }
    if(GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_11)==0){
      if(GPIO_ReadOutputDataBit(GPIOC, GPIO_Pin_13)==0)
        GPIO_SetBits(GPIOC, GPIO_Pin_13);
      else
        GPIO_ResetBits(GPIOC, GPIO_Pin_13);
    }
    delay_nms(120);
  }
}
```

在GPIO_Configuration中我们将PC8、PC9、PC10、PC11设置为按键输入引脚，下面将PC11设置为浮空输入

```c
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_11;
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
GPIO_InitStructure.GPIO_Spedd = GPIO_Speed_50MHz;
GPIO_Init(GPIOC, &GPIO_InitStructure);
```

## 输入端口应用

单片机复位期间和刚复位时，复用功能尚未开启，IO端口被配置成浮空输入模式，所有的端口都有外部中断能力，为了使用外部中断线，端口必须配置成输入模式。

让机器人通过触觉胡须进行导航，首先安装并测试胡须

```c
#include "stm32f10x_heads.h"
#include "HelloRobot.h"

int PE2state(void){//左边胡须状态
  return GPIO_ReadInputDataBit(GPIOE, GPIO_Pin_2);
}
int PE3state(void){//右边胡须状态
  return GPIO_ReadInputDataBit(GPIOE, GPIO_Pin_3);
}

int main(void){
  BSP_Init();
  USART_Configuration();
  preintf("Program Running!\n");
  
  while(1){
    printf("左边胡须:%d", PE2state());
    printf("右边胡须:%d", PE3state());
    delay_nms(150);
  }
}
```

### 基于胡须的导航

当机器人胡须检测当障碍物时，调用子程序使机器人倒退或旋转，然后重新直走

```c
#include "stm32f10x_heads.h"
#include "HelloRobot.h"

int PE2state(void){//左边胡须状态
  return GPIO_ReadInputDataBit(GPIOE, GPIO_Pin_2);
}
int PE3state(void){//右边胡须状态
  return GPIO_ReadInputDataBit(GPIOE, GPIO_Pin_3);
}

void Move(int counter, int PC1_velocity, int PC0_velocity){//速度应在0～200之间
  /*
  * 前进 正负
  * 左转 负负
  * 右转 正正
  * 后退 负正
  */
  int i;
  for(i=1;i<counter;i++){
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(1500 + PC1_velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    GPO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(1500 + PC0_velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    delay_nms(20);
  }
}

int main(void){
  BSP_Init();
  USART_Configuration();
  
  while(1){
    if((PE2state()==0)&&(PE3stat()==0)){//正前方有障碍物
      Move(65, -200, +200);//后退
      Move(26, -200, -200);//向左
      Move(26, -200, -200);//向左
    }else if(PE2state()==0){//右前方有障碍物
      Move(65, -200, +200);//后退
      Move(26, -200, -200);//向左
    }else if(PE3state()==0){//左前方有障碍物
      Move(65, -200, +200);//后退
      Move(26, +200, +200);//向右
    }else
      Move(65, +200, -200);//向前
  }
}
```

### 死区决策

当机器人进入墙角后，左胡须触墙右转，此时右胡须触墙又左转，会陷入死循环。所以我们天接一个计数器记录当前机器人胡须交替触动的总次数，关键是记录每个胡须的前一次触动状态并和当前状态进行对比，若状态相反则在交替总数加1，若交替总数超过了阈值表示此处是死区，需要做一个"U"型转弯，然后将计数器复位。

```c
#include "stm32f10x_heads.h"
#include "HelloRobot.h"

int PE2state(void){//左边胡须状态
  return GPIO_ReadInputDataBit(GPIOE, GPIO_Pin_2);
}
int PE3state(void){//右边胡须状态
  return GPIO_ReadInputDataBit(GPIOE, GPIO_Pin_3);
}

void Move(int counter, int PC1_velocity, int PC0_velocity){//速度应在0～200之间
  /*
  * 前进 正负
  * 左转 负负
  * 右转 正正
  * 后退 负正
  */
  int i;
  for(i=1;i<counter;i++){
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(1500 + PC1_velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    GPO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(1500 + PC0_velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    delay_nms(20);
  }
}

int main(void){
  BSP_Init();
  USART_Configuration();
  
  int counter = 1;//交替总数计数器
  int oldR = 1;//右胡须前态
  int oldL = 0;//左胡须前态
  
  while(1){
    if(PE3state()!=PE2state()){//检查是否有且只有一个胡须被触动
      if(oldR!=PE2state()&&oldL!=PE3state()){
        counter += 1;
        oldR = PE2state();
        oldL = PE3state();
        if(counter > 4){//交替总数超过4次认为进入死区
          counter = 1;//重置计数器
          Move(65, -200, +200);
          Move(26, -200, -200);
          Move(26, -200, -200);
        }
      }else
        counter = 1;
    }
    if((PE3state()==0)&&(PE2stat()==0)){//正前方有障碍物
      Move(65, -200, +200);//后退
      Move(26, -200, -200);//向左
      Move(26, -200, -200);//向左
    }else if(PE2state()==0){//右前方有障碍物
      Move(65, -200, +200);//后退
      Move(26, -200, -200);//向左
    }else if(PE3state()==0){//左前方有障碍物
      Move(65, -200, +200);//后退
      Move(26, +200, +200);//向右
    }else
      Move(65, +200, -200);//向前
  }
}
```

## 中断编程

Cortex-M3内核集成了中断控制器和中断优先级控制器，支持256个中断(16个内核+240个外部)和可编程256级中断优先级的设置。stm32f10x系列单片机的嵌套中断向量控制器(NVIC)支持68个可屏蔽中断通道(不含16个Cortex-M3内核中断)，具有16级可编程中断优先级的设置(仅使用中断优先级设置8bit中的高4位)。

编写stm32单片机的中断服务程序，首先要知道stm32f10x_it.c这个文件，打开文件可以看到***_IRQHandler函数实现，但是实际该函数实现几乎为空，这些函数是要开发者填写的中断服务（处理）函数，用到那个中断做处理就必须填写相应的中断处理函数。

### 外部中断

stm32单片机80个通用IO端口连接到19个外部中断/事件源上，PAx，PBx，PCx，PDx，PEx五个端口对应的是同一个中断/事件源EXTIx，x：0～15。另外剩下三个外部中断/事件源连接如下：

- EXTI16连接到PVD电源电压检测输出
- EXTI17连接到RTC闹钟事件
- EXTI18连接到从USB待机唤醒事件

#### 按键中断

```c
#include "stm32f10x_heads.h"
#include "HelloRobot.h"

int main(void){
  BSP_Init();
  USART_Configuration();
  
  while(1);//等待中断到来
}
```

```c
//stm32f10_it.c
#include "stm32f10x_it.h"
extern void delay_nms(unsigned long n);
···
void EXTI9_5_IRQHandler(void){
  if(GPIO_ReadOutputDataBit(GPIOB, GPIO_Pin_9)==0)
    GPIO_SetBits(GPIOB, GPIO_Pin_9);
  else
    GPIO_ResetBits(GPIOB, GPIO_Pin_9);
  
  delay_nms(10);//去抖动
  
  EXTI_ClearITPendingBit(EXTI_Line9);//中断结束时清除中断标志位
  EXTI_ClearITPendingBit(EXTI_Line5);//中断结束时清除中断标志位
}

//BSP_Init()开放了EINT4中断，如不用，为防止干扰，加入以下代码
void EXTI4_IRQHandler(void){
  EXTI_ClearITPendingBit(EXTI_Line4);//中断结束时清除中断标志位
}
```

#### 中断测试机器人触觉

```c
#include "stm32f10x_heads.h"
#include "HelloRobot.h"

int main(void){
  BSP_Init();
  USART_Configuration();
  
  while(1);
}
```

```c
//stm32f10x_it.c
#include "stm32f10x_it.h"
#include "stdio.h"

extern void delay_nms(unsigned long n);
···
void EXTI4_IRQHandler(void){
  printf("右边胡须检查到障碍\r\n")；
  if(GPIO_ReadOutputDataBit(GPIOB, GPIO_Pin_9)==0)
    GPIO_SetBits(GPIOB, GPIO_Pin_9);
  else
    GPIO_ResetBits(GPIOB, GPIO_Pin_9);
  delay_nms(200);
  
  EXTI_ClearITPendingBit(EXTI_Line4);
}

void EXTI9_5_IRQHandler(void){//中断处理函数PC9
  printf("左边胡须检查到障碍\r\n");
  if(GPIO_ReadOutputDataBit(GPIOC, GPIO_Pin_13)==0)
    GPIO_SetBits(GPIOC, GPIO_Pin_13);
  else
    GPIO_ResetBits(GPIOC, GPIO_Pin_13);
  delay_nms(200);
  
  EXTI_ClearITPendingBit(EXTI_Line5);
  
  //BSP_Init()开放EINT9，防干扰加入
  EXTI_ClearITPendingBit(EXTI_Line9);
}
```

# I/O端口综合应用与红外导航

## 使用红外探测避开障碍物

```c
#include "stm32f10x_heads.h"
#include "HelloRobot.h"

#define LeftLaunch_1 GPIO_SetBits(GPIOE, GPIO_Pin_1)//左红外发射
#define LeftLaunch_0 GPIO_ResetBits(GPIOE, GPIO_Pin_1)//左红外发射
#define RightLaunch_1 GPIO_SetBits(GPIOE, GPIO_Pin_0)//右红外发射
#define RightLaunch_0 GPIO_ResetBits(GPIOE, GPIO_Pin_0)//右红外发射
#define LeftIR GPIO_ReadInputDataBit(GPIOE, GPIO_Pin_3)//左红外接收
#define RightIR GPIO_ReadInputDataBit(GPIOE, GPIO_Pin_2)//右红外接收

void IRLaunch(unsigned char IR){
  int counter;
  if(IR=='L')
    for(counter=0;counter<1000;counter++){
      LeftLaunch_1; delay_nus(12);
      LeftLaunch_0; delay_nus(12);
    }
  if(IR=='R')
    for(counter=0;counter<1000;counter++){
      RightLaunch_1; delay_nus(12);
      RightLaunch_0; delay_nus(12);
    }
}
void Move(int counter, int PC1_velocity, int PC0_velocity){//速度应在0～200之间
  /*
  * 前进 正负
  * 左转 负负
  * 右转 正正
  * 后退 负正
  */
  int i;
  for(i=1;i<counter;i++){
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(1500 + PC1_velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    GPO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(1500 + PC0_velocity);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    delay_nms(20);
  }
}
int main(void){
  int irDetectLeft, irDetectRight;
  BSP_Init();
  USART_Configuration();
  while(1){
    IRLaunch('R');
    irDetectRight=RightIR;
    IRLaunch('L');
    irDetectLeft=LeftIR;
    if((irDetectLeft==0)&&(irDetectRight==0)){
      Move(65, -200, +200);//Back
      Move(52, -200, -200);//Left & Left
    }else if(irDetectLeft==0){
      Move(65, -200, +200);
      Move(26, +200, +200);//Right
    }else if(irDetectRight==0){
      Move(65, -200, +200);
      Move(26, -200, -200);
    }else
      Move(65, +200, -200);//Forward
  }
}
```

## 在脉冲之间采样避免碰撞

探测障碍物很重要的一点是在机器人碰撞到之前留有绕开的空间，若前方有障碍物，机器人使用脉冲命令避开，然后在此探测，若物体还在，使用另一个脉冲避开。

```c
#include "stm32f10x_heads.h"
#include "HelloRobot.h"

#define LeftLaunch_1 GPIO_SetBits(GPIOE, GPIO_Pin_1)//左红外发射
#define LeftLaunch_0 GPIO_ResetBits(GPIOE, GPIO_Pin_1)//左红外发射
#define RightLaunch_1 GPIO_SetBits(GPIOE, GPIO_Pin_0)//右红外发射
#define RightLaunch_0 GPIO_ResetBits(GPIOE, GPIO_Pin_0)//右红外发射
#define LeftIR GPIO_ReadInputDataBit(GPIOE, GPIO_Pin_3)//左红外接收
#define RightIR GPIO_ReadInputDataBit(GPIOE, GPIO_Pin_2)//右红外接收

void IRLaunch(unsigned char IR){
  略
}

int main(void){
  int pulseLeft, pulseRight;
  int irDetectLeft, irDetectRight;
  BSP_Init();
  USART_Configuration();
  do(1){
    IRLaunch('R');
    irDetectRight=RightIR;
    IRLaunch('L');
    irDetectLeft=LeftIR;
    if((irDetectLeft==0)&&(irDetectRight==0)){
      pulseLeft=1300;
      pulseRight=1700;
    }else if(irDetectLeft==0){
      pulseLeft=1700;
      pulseRight=1700;
    }else if(irDetectRight==0){
      pulseLeft=1300;
      pulseRight=1300;
    }else{
      pulseLeft=1700;
      pulseRight=1300;
    }
    GPIO_SetBits(GPIOD, GPIO_Pin_10);
    delay_nus(pulseLeft);
    GPIO_ResetBits(GPIOD, GPIO_Pin_10);
    
    GPIO_SetBits(GPIOD, GPIO_Pin_9);
    delay_nus(pulseRight);
    GPIO_ResetBits(GPIOD, GPIO_Pin_9);
    delay_nus(20);
  }while(1);
}
```

这样的实现在发送脉冲给电机之前先检查一次障碍物，改善了小车的性能。

## 边沿探测



# 通信

## SPI(串行外设端口)

一种同步串行外设端口，支持3线全双工同步传输，可选择以8或16位传输帧格式进行传输，通常SPI通过4个引脚和外部器件相连

- MISO：主设备输入/从设备输出引脚。在从模式下发送数据，主模式下接受数据
- MOSI：主设备输出/从设备输入引脚。在主模式下发送数据，从模式下接受数据
- SCK：串口时钟，作为主设备的输出，从设备的输入
- NSS：从设备选择。作为片选引脚，让主设备可以单独地与特定从设备通信，避免数据线上的冲突。

SCK信号时钟周期也是SPI时钟周期，在一个SPI时钟周期内，完成如下操作：

- 主机通过MOSI线发送1位数据，从机通过该线读取这1位数据
- 从机通过MISO线发送1位数据，主机通过该线读取这1位数据

在主从模式下，一个主机可以分时访问三个从机，


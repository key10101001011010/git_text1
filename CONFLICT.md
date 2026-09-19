#include "main.h"
#include "gpio.h"
#include "usart.h"

UART_HandleTypeDef huart1;
uint8_t buf[32];
uint16_t cnt = 0;

void SystemClock_Config(void);

int main(void)
{
  HAL_Init();
  SystemClock_Config();

  MX_GPIO_Init();
  MX_USART1_UART_Init();

  while (1)
  {
      // LED翻转 PA0
      HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_0);

      // 格式化字符串
      sprintf((char*)buf,"Count:%d\r\n",cnt);
      // 串口发送
      HAL_UART_Transmit(&huart1, buf, strlen((char*)buf),20);

      cnt++;
      if(cnt > 9999) cnt = 0;

      // 延时500ms
      HAL_Delay(500);
  }
}
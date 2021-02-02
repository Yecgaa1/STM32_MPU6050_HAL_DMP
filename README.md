# STM32_MPU6050_HAL_DMP
世人苦无Stm32+DMP+HAL库例程已久

本库开发环境：Stm32F103C8T6+MPU6050在I2C1+USART1用于发送俯仰角数据+HAL库使用STM32CubeMX配置为STM32CubeIDE在Clion下开发环境

（上面这句真的长）

## 特别感谢库来源
https://github.com/fMeow/STM32_DMP_Driver
（在其基础上做了移植和错误修正）

## 我是用HAL库但不是用这种开发环境怎么办？
### 移植要点

0.Stm32CubeMX先生成项目

1.https://github.com/Yecgaa1/STM32_mpu6050_HAL_DMP_Libarary/tree/main 
下载其中的src Inc放在你的项目文件夹内
2.如果不是stm32f1系列的，修改MPU6050/I2C.h中的

```c
#include "stm32f1xx_hal.h"
```

为（如f4系列）

```c
#include "stm32f4xx_hal.h"
```

3.在main.c中开始改写

初始化

```c
/* USER CODE BEGIN 2 */
  MPU6050_initialize();
  DMP_Init();
//记住这段在MX_I2C1_Init();后
/* USER CODE END 2 */
```

然后加上DMP读取和输出即可

```c
/* USER CODE BEGIN WHILE */
  while (1)
  {
    /* USER CODE END WHILE */
      Read_DMP();
      printf("%f\r\n",Pitch);
    /* USER CODE BEGIN 3 */
      //HAL_Delay (10);
  }
  /* USER CODE END 3 */
}
```

构建时你会遇到以下warning

```shell
In file included from E:\GitProject\stm32\mpu6050Test\Core\Src\MPU6050\I2C.c:1:
E:\GitProject\stm32\mpu6050Test\Core\Inc/MPU6050/I2C.h:22:37: warning: 'struct int_param_s' declared inside parameter list will not be visible outside of this definition or declaration
 static inline int reg_int_cb(struct int_param_s *int_param)
                                     ^~~~~~~~~~~
E:\GitProject\stm32\mpu6050Test\Core\Src\MPU6050\I2C.c: In function 'i2c_write':
E:\GitProject\stm32\mpu6050Test\Core\Src\MPU6050\I2C.c:7:25: warning: passing argument 5 of 'HAL_I2C_Mem_Write' discards 'const' qualifier from pointer target type [-Wdiscarded-qualifiers]
   I2C_MEMADD_SIZE_8BIT, data, length, 10);
                         ^~~~
In file included from E:\GitProject\stm32\mpu6050Test\Core\Inc/stm32f1xx_hal_conf.h:297,
                 from E:\GitProject\stm32\mpu6050Test\Drivers\STM32F1xx_HAL_Driver\Inc/stm32f1xx_hal.h:30,
                 from E:\GitProject\stm32\mpu6050Test\Core\Inc/MPU6050/I2C.h:2,
                 from E:\GitProject\stm32\mpu6050Test\Core\Src\MPU6050\I2C.c:1:
E:\GitProject\stm32\mpu6050Test\Drivers\STM32F1xx_HAL_Driver\Inc/stm32f1xx_hal_i2c.h:568:134: note: expected 'uint8_t *' {aka 'unsigned char *'} but argument is of type 'const uint8_t *' {aka 'const unsigned char *'}
 HAL_StatusTypeDef HAL_I2C_Mem_Write(I2C_HandleTypeDef *hi2c, uint16_t DevAddress, uint16_t MemAddress, uint16_t MemAddSize, uint8_t *pData, uint16_t Size, uint32_t Timeout);
                                                                                                                   
```

但不影响你使用（我也不会修，欢迎修好了提pull request）

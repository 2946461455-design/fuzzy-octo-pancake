# 智能自适应水下激光通信节点系统 - 算法交付包

本交付包沿用 Claude 已写的核心算法文件，并补齐后续工程代码，使其形成 STM32F4 可移植固件闭环。

## 关键入口

- STM32 主入口：`嵌入式实现/stm32_firmware/Core/Src/main.c`
- 系统调度层：`嵌入式实现/stm32_firmware/Drivers/App/ulc_app.c`
- F4 自适应控制端口：`嵌入式实现/stm32_firmware/Drivers/App/adaptive_control_f4_port.c`
- 协同网络实现：`嵌入式实现/stm32_firmware/Drivers/App/cooperative.c`
- F4 编译源清单：`嵌入式实现/stm32_firmware/source_files_f4.txt`
- F4 集成说明：`嵌入式实现/stm32_firmware/README_STM32F4.md`

## 固件闭环

```text
ADC DMA -> FE_Process -> CC_Classify -> OL_SelectAction
        -> AC_Update -> OL_Step/OL_ApplyDeltas -> COOP_Task
```

## STM32F4 默认外设

- `ADC1 + DMA`: 512 点双缓冲接收采样。
- `TIM2 TRGO`: ADC 采样触发。
- `TIM1_CH1 PWM`: 激光调制输出。
- `DAC1_CH1`: 激光功率控制。
- `DAC1_CH2`: 智能判决阈值输出。

## 验证

已在本机跑通 Q15 内存优化链路性能测试：

```text
q15_pipeline: 25.000 ms / 100000 frames
class=0 mod=4 laser_dac=1400 threshold=1998 period=512
```

完整 STM32 目标编译需要本机安装 `arm-none-eabi-gcc` 或使用 STM32CubeIDE。当前环境没有检测到 `arm-none-eabi-gcc`。

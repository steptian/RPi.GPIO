# RPi.GPIO
RPi.GPIO文档翻译

树莓派中使用python操作引脚，非常方便。但如果刚开始并没有相对系统的学习，就会有不少疑惑，所以把这些稍作翻译，供查看
##BasicUsage
基础用法；
基础用法中包含了一些示例，可以快速开始读取、写入操作。

[BasicUsage](https://github.com/steptian/RPi.GPIO/blob/master/BasicUsage.md)
##Inputs
输入（从树莓派输出）；
输入的概念跟传统的理解有点冲突，实际上是从树莓派输出，也就是从树莓派读取出数据，比如传感器的数据，如果是连接到引脚上，需要把这个引脚定义为input

[Inputs](https://github.com/steptian/RPi.GPIO/blob/master/Inputs.md)
##Outputs
输出（向树莓派输出）；
输出是指给某个引脚设定值，即通过引脚去驱动连接在其上面的外设，比如LED灯

[Outputs](https://github.com/steptian/RPi.GPIO/blob/master/Outputs.md)
##PWM
脉冲；
脉冲是一个周期性变动曲线的动作，它可以实现渐进效果，比如让LED灯渐变颜色（明暗）等

[PWM](https://github.com/steptian/RPi.GPIO/blob/master/PWM.md)
##Checking function of GPIO channels
检查通道方法列表；
每个通道的方法都可以通过一个统一的方法来获取，很方便

[Checking function of GPIO channels](https://github.com/steptian/RPi.GPIO/blob/master/Checking_function_of_GPIO_channels.md)

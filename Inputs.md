https://sourceforge.net/p/raspberry-gpio-python/wiki/Inputs/
##输入
从GPIO的输入到你的程序有很多方法。第一种也是最简单的方法就是及时检查输入值。这是一种“polling”（拉取）的方式，
如果读取的时机不对，可能会获取到错误的数据。polling这种方式是通过循环实现的，是处理器密集型方法。另外一种方式，
是使用“interrupts”（中断）读取输入（使用边沿触发）。边沿的意思是指从高电平跳转到低电平（降边）或从低电平跳转到高电平（升边）。
##上拉/下拉电阻(Pull up/Pull down)
如果你没有把引脚连接任何东西，它会“浮动”（float）。换句话说，你读到的数据是“undefined”因为他并没有连接任何东西，
除非你按下按钮或者做了切换。由于受电源干扰，它的值是不稳定的。

为了搞懂这个，我们用一个上拉/下拉电阻。这样，输入可以设置默认值。可以通过硬件也可以通过软件来实现上拉/下拉电阻。
硬件方式可以用一个10K的电阻，放在输入引脚和一个通常是0V（下拉）或者3.3V（上拉）。
RPi.GPI模块可以让我们通过配置Broadcom芯片来软件方式实现它。

```python
GPIO.setup(channel, GPIO.IN, pull_up_down=GPIO.PUD_UP)
  # or
GPIO.setup(channel, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)
```
(通道是指基于之前指定的编码系统的通道编号，BOARD或者BCM)

##测试输入（polling方式）
快速回顾一下输入：
```python
if GPIO.input(channel):
    print('Input was HIGH')
else:
    print('Input was LOW')
```
通过轮询等待按下按钮：
```python
while GPIO.input(channel) == GPIO.LOW:
    time.sleep(0.01)  # wait 10 ms to give CPU chance to do other things
```
##中断和边沿检测
边沿就是高低电平信号状态发生状态，由高到低（降边）或由低到高（升边）。
通常我们更关注输入值的变化，这种变化就是事件（event）。

下面两种方法可以避免在程序繁忙时错过按钮的按下动作：

*wait_for_edge()等待边沿方法
*event_detected()事件检测方法
*边沿呗检测到时的回调方法

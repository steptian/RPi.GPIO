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
##wait_for_edge()方法
wait_for_edge()方法是用来阻断程序的执行，直到它检测出有边沿被触发了。换句话说，上面这个等待按下按钮的离职可以重写成：
```python
GPIO.wait_for_edge(channel, GPIO.RISING)
```
如果你指向等待一段时间，那可以使用timeout参数：
```python
# wait for up to 5 seconds for a rising edge (timeout is in milliseconds)
channel = GPIO.wait_for_edge(channel, GPIO_RISING, timeout=5000)
if channel is None:
    print('Timeout occurred')
else:
    print('Edge detected on channel', channel)
```
##event_detected()方法
event_detected()方法跟polling不同，是用其他方式实现的循环，它可以保证在CPU繁忙时不会错过输入的变化。
这在使用Pygame或者PyQt的时候非常有用，因为这些框架都有一个及时监听和上报给GUI事件的主循环。
```python
GPIO.add_event_detect(channel, GPIO.RISING)  # add rising edge detection on a channel
do_something()
if GPIO.event_detected(channel):
    print('Button pressed')
```

注意，你可以检测GPIO.RISING, GPIO.FALLING or GPIO.BOTH这些事件。
##螺纹回调（Threaded callbacks）
RPi.GPIO为回调函数启用了一个新的线程。这就意味着回调函数是和主程序同时运行的，可以立即上报给边沿。例如：
```python
def my_callback(channel):
    print('This is a edge event callback function!')
    print('Edge detected on channel %s'%channel)
    print('This is run in a different thread to your main program')

GPIO.add_event_detect(channel, GPIO.RISING, callback=my_callback)  # add rising edge detection on a channel
...the rest of your program...
If you wanted more than one callback function:
def my_callback_one(channel):
    print('Callback one')

def my_callback_two(channel):
    print('Callback two')

GPIO.add_event_detect(channel, GPIO.RISING)
GPIO.add_event_callback(channel, my_callback_one)
GPIO.add_event_callback(channel, my_callback_two)
```
注意，在这种情况下，回调函数是串行而非并行的。因为在回调中只是用了一个线程，这个线程里每个回调都按他们被定义的顺序在运行。
##开关抖动（Switch debounce）
You may notice that the callbacks are called more than once for each button press. This is as a result of what is known as 'switch bounce'. There are two ways of dealing with switch bounce:
你可能也注意到了，每次按钮按下时回调被执行了多次。这就是我们成为开关抖动的原因导致的。
有两种方式可以处理这种开关抖动：

*在你的开关里，加一个0.1uF的电容
*软防抖
*两者结合
To debounce using software, add the bouncetime= parameter to a function where you specify a callback function. Bouncetime should be specified in milliseconds. For example:
软件实现防抖动的方式，可以通过在指定的回调函数中添加bouncetime=参数来实现。
bouncetime参数的单位是毫秒。
```python
# add rising edge detection on a channel, ignoring further edges for 200ms for switch bounce handling
GPIO.add_event_detect(channel, GPIO.RISING, callback=my_callback, bouncetime=200)
or
GPIO.add_event_callback(channel, my_callback, bouncetime=200)
```
##去除事件检测(Remove event detection)
如果你的程序因某种原因，不再想做边沿事件检测，可以这样停止：
```python
GPIO.remove_event_detect(channel)
```

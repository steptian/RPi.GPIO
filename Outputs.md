https://sourceforge.net/p/raspberry-gpio-python/wiki/Outputs/
##GPIO输出
1. First set up RPi.GPIO (as described here)
1、首先配置RPi.GPIO（如下）：
```python
import RPi.GPIO as GPIO
GPIO.setmode(GPIO.BOARD)
GPIO.setup(12, GPIO.OUT)
```
2、设置一个高位输出:
```python
GPIO.output(12, GPIO.HIGH)
 # or
GPIO.output(12, 1)
 # or
GPIO.output(12, True)
```
3、设置一个低位输出：
```python
GPIO.output(12, GPIO.LOW)
 # or
GPIO.output(12, 0)
 # or
GPIO.output(12, False)
```
4、同时输出多个通道值：
```python
chan_list = (11,12)
GPIO.output(chan_list, GPIO.LOW) # all LOW
GPIO.output(chan_list, (GPIO.HIGH,GPIO.LOW))  # first LOW, second HIGH
```
5、程序结尾的清理：
```python
GPIO.cleanup()
```
注意，你可以通过input()方法读取到被设置成输出的通道的当前状态值，比如我们对调一个输出：
```python
GPIO.output(12, not GPIO.input(12))
```

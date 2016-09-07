https://sourceforge.net/p/raspberry-gpio-python/wiki/PWM/
##在RPi.GPIO中使用PWM
创建一个脉冲实例：
```python
p = GPIO.PWM(channel, frequency)
```
启动脉冲：
```python
p.start(dc)   # where dc is the duty cycle (0.0 <= dc <= 100.0)
```
调整脉冲频率：
```python
p.ChangeFrequency(freq)   # where freq is the new frequency in Hz
```
调整脉冲占空比（工作周期）：
```python
p.ChangeDutyCycle(dc)  # where 0.0 <= dc <= 100.0
```
停止脉冲：
```python
p.stop()
```
如果脉冲实例超出了作用域，脉冲也会停止的

下面是一个让LED灯每两秒闪烁一次的例子：
```python
import RPi.GPIO as GPIO
GPIO.setmode(GPIO.BOARD)
GPIO.setup(12, GPIO.OUT)

p = GPIO.PWM(12, 0.5)
p.start(1)
input('Press return to stop:')   # use raw_input for Python 2
p.stop()
GPIO.cleanup()
```
让LED等变亮/变暗的例子：
```python
import time
import RPi.GPIO as GPIO
GPIO.setmode(GPIO.BOARD)
GPIO.setup(12, GPIO.OUT)

p = GPIO.PWM(12, 50)  # channel=12 frequency=50Hz
p.start(0)
try:
    while 1:
        for dc in range(0, 101, 5):
            p.ChangeDutyCycle(dc)
            time.sleep(0.1)
        for dc in range(100, -1, -5):
            p.ChangeDutyCycle(dc)
            time.sleep(0.1)
except KeyboardInterrupt:
    pass
p.stop()
GPIO.cleanup()
```

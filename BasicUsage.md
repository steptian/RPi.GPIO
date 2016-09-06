https://sourceforge.net/p/raspberry-gpio-python/wiki/BasicUsage/

##RPi.GPIO模块基础
*导入该模块

导入方法如下：
```python
import RPi.GPIO as GPIO
```
这样导入后，就可以使用GPIO替代完整写法（RPi.GPIO）了

下面的写法可以检查是否导入模块成功
```python
try:
    import RPi.GPIO as GPIO
except RuntimeError:
    print("Error importing RPi.GPIO!  This is probably because you need superuser privileges.  You can achieve this by using 'sudo' to run your script")
Pin numbering
```
##pin编号

RPi.GPIO有两种编码方式，第一种使用BOARD编码系统。这种方式是直接使用树莓派的板卡编号，使用这种方式编码的好处是无论硬件版本如何，它都可以一直有效，所以你的代码也就不用做什么改动。

第二种编码方式是BCM编码，它更靠近底层一些，是和Broadcom芯片关联的。使用这套编码方式，就不得不每次对照编码表，并且如果硬件版本改变，就得去修改代码了。

可以用下面的方法指定编码方式（强制）：
```python
GPIO.setmode(GPIO.BOARD)
  # or
GPIO.setmode(GPIO.BCM)
```

下面的代码可以检测当前使用的是何种引脚编码方式（举个例子，使用另一个Python的模块）：
```python
mode = GPIO.getmode()
The mode will be GPIO.BOARD, GPIO.BCM or None
结果有三种可能：GPIO.BOARD, GPIO.BCM 或 None
```
##警告

在你的树莓派的GPIO上面可能运行了多个脚本或电路，因此，如果RPi.GPIO检测到有一个pin引脚被配置到另外一个上面而不是默认的那个，你就会收到一个告警，如果要关掉这些告警，可以用下面的语句：
```python
GPIO.setwarnings(False)
```

##配置通道（channel）

你要使用的输入输出引脚都必须经过配置才可以使用。配置输入通道可以这样：
```python
GPIO.setup(channel, GPIO.IN)
```
（这些引脚编码都基于你之前指定的编码系统，BOARD或者BCM）

*下面是关于输入输出通道配置的更多信息

把一个通道设置成输出：
```python
GPIO.setup(channel, GPIO.OUT)
```

（这些引脚编码都基于你之前指定的编码系统，BOARD或者BCM）

还可以给输出通道一个初始值：
```python
GPIO.setup(channel, GPIO.OUT, initial=GPIO.HIGH)
```

同时配置多个通道

你可以同时设置多个通道（在0.5.8版本之后），比如：
```python
chan_list = [11,12]    # add as many channels as you want!
                       # you can tuples instead i.e.:
                       #   chan_list = (11,12)
GPIO.setup(chan_list, GPIO.OUT)
```

##输入

读取GPIO引脚数据：
```python
GPIO.input(channel)
```

（这些引脚编码都基于你之前指定的编码系统，BOARD或者BCM）返回结果要么是0 / GPIO.LOW / False 或者 1 / GPIO.HIGH / True

##输出

给引脚设置状态：
```python
GPIO.output(channel, state)
```

（这些引脚编码都基于你之前指定的编码系统，BOARD或者BCM）

状态值可以是0 / GPIO.LOW / False 或者 1 / GPIO.HIGH / True

同时设置多个通道值

你可以同时设置多个通道值（在0.5.8版本之后），比如：
```python
chan_list = [11,12]                             # also works with tuples
GPIO.output(chan_list, GPIO.LOW)                # sets all to GPIO.LOW
GPIO.output(chan_list, (GPIO.HIGH, GPIO.LOW))   # sets first HIGH and second LOW
```

##清除

在程序的尾部，去清理回收使用过的资源是个好习惯。这跟使用RPi.GOIO一样。把所有使用过的通道都重置可以避免因短路导致损坏。注意，可以仅仅清除了你在脚本中指定的的通道，如果使用GPIO.cleanup()会清除系统在用的所有引脚通道。
```python
GPIO.cleanup()
```

下面可以根据需要，只清除部分通道，某一个或者一些，都可以：
```python
GPIO.cleanup(channel)
GPIO.cleanup( (channel1, channel2) )
GPIO.cleanup( [channel1, channel2] )
```

获取树莓派板子信息和RPi.GPIO的版本信息

获取树莓派信息
```python
GPIO.RPI_INFO
```

获取树莓派板子修订信息
```python
GPIO.RPI_INFO['P1_REVISION']
GPIO.RPI_REVISION    (deprecated)
```

获取RPi.GPIO的版本信息
```python
GPIO.VERSION
```

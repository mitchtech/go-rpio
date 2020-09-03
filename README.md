你好！
很冒昧用这样的方式来和你沟通，如有打扰请忽略我的提交哈。我是光年实验室（gnlab.com）的HR，在招Golang开发工程师，我们是一个技术型团队，技术氛围非常好。全职和兼职都可以，不过最好是全职，工作地点杭州。
我们公司是做流量增长的，Golang负责开发SAAS平台的应用，我们做的很多应用是全新的，工作非常有挑战也很有意思，是国内很多大厂的顾问。
如果有兴趣的话加我微信：13515810775  ，也可以访问 https://gnlab.com/，联系客服转发给HR。
go-rpio
=======

Native GPIO-Gophers for your Pi!

go-rpio is a Go library for accessing [GPIO](http://elinux.org/Rpi_Low-level_peripherals)-pins
on the [Raspberry Pi](https://en.wikipedia.org/wiki/Raspberry_Pi).

It requires no external c libraries such as
[WiringPI](https://projects.drogon.net/raspberry-pi/wiringpi/) or [bcm2835](http://www.open.com.au/mikem/bcm2835).

There's a tiny bit of additional information over at my [blog](https://blog.eikeland.se/2013/07/30/go-gpio-library-for-raspberry-pi/).

![raspberrypi-blink](http://stianeikeland.files.wordpress.com/2013/07/animated.gif)

## Releases ##
- 1.0.0 - Supports original rpi A/B/B+
- 2.0.0 - Adds support for rpi 2, by @akramer
- 3.0.0 - Add support for /dev/gpiomem, by @dotdoom

## Usage ##

```go
import "github.com/stianeikeland/go-rpio"
```

Open memory range for GPIO access in /dev/mem

```go
err := rpio.Open()
```

Initialize a pin, run basic operations.
Pin refers to the bcm2835 pin, not the physical pin on the raspberry pi header. Pin 10 here is exposed on the pin header as physical pin 19.

```go
pin := rpio.Pin(10)

pin.Output()       // Output mode
pin.High()         // Set pin High
pin.Low()          // Set pin Low
pin.Toggle()       // Toggle pin (Low -> High -> Low)

pin.Input()        // Input mode
res := pin.Read()  // Read state from pin (High / Low)

pin.Mode(rpio.Output)   // Alternative syntax
pin.Write(rpio.High)    // Alternative syntax
```

Pull up/down/off can be set using:

```go
pin.PullUp()
pin.PullDown()
pin.PullOff()

pin.Pull(rpio.PullUp)
```

Unmap memory when done

```go
rpio.Close()
```

Also see example [examples/blinker/blinker.go](examples/blinker/blinker.go)

## Other ##

Currently, it supports basic functionality such as:
- Pin Direction (Input / Output)
- Write (High / Low)
- Read (High / Low)
- Pull (Up / Down / Off)

Would be nice to add in the future:
- PWM
- I2C
- SPI
- etc...

It works by memory-mapping the bcm2835 gpio range, and therefore require root/administrative-rights to run.

## Using without root ##

This library can utilize the new [/dev/gpiomem](https://github.com/raspberrypi/linux/pull/1112/files) 
memory range if available. 

You will probably need to upgrade to the latest kernel (or wait for the next raspbian release) if you're missing /dev/gpiomem. You will also need to add a `gpio` group, add your user to the group, and then set up udev rules. I would recommend using [create_gpio_user_permissions.py](https://github.com/waveform80/rpi-gpio/blob/master/create_gpio_user_permissions.py) if you're unsure how to do this.

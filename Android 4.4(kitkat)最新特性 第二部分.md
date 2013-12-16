[参考][http://developer.android.com/about/versions/kitkat.html](http://developer.android.com/about/versions/kitkat.html)

#Android KitKat

Android kitkat 带来了创新的，美丽的，实用的特性给各种Android设备。本文档能够快速的知道这些特性

##为所有设备设计

* 使用范围更广，适用低配置的Android设备(512M RAM)
* 提高内存管理，减少内存占用
* 对于开发者，增加` ActivityManager.isLowRamDevice()`新API
* 功能强大的内存管理工具`procstats tool`, `meminfo tool`


## 新的NFC功能，通过HCE(Host Card Emulation)


Android 4.4 提供了一个新的数据传输技术通过主卡模拟(HCE), 通过HCE可以安全的进行NFC交易；比如支付，卡信息访问，门禁卡等其他的自定义的服务；通过HCE可以在任何的android设备上模拟一个NFC，然后可以很方便的与自己的提供服务商进行交易处理； 应用也可以使用一个`Reader Model`把HCE作为一个只读者（不能交易，只看信息）  

Android HCE基于一种流行的卡，这种卡是按照`contactless ISO/IEC 14443-4 (ISO-DEP) `协议交易，目前市面上已经用的非常多了。包括NFC，AIDs (Application Identifiers); AIDs是在Manifest文件中声明的，通过Catagory的标示符，比如：‘payments’,如果很多应用都有这个标示符，就会显示一个列表供你选择支付应用；   

当用户在一个交易客户端进行触碰交易的时候，android 系统会接收之前设计好的AID，并且会路由到正确的app,这个App读取到交易终端的信息和操作，并且根据本地网络或者互联网服务来完成交易；

Android的HCE手机上必须要有NFC控制器，NFC 控制器自动的提供了HCE和SE（secure element:安全元素）之间的交易，Android 4.4 devices that support NFC will include Tap & Pay for easy payments using HCE

## 打印模块

一句话，就是能在自己的APP上添加打印机了，无论是网络打印机还是本地打印机(Intent带打印Action)

## 
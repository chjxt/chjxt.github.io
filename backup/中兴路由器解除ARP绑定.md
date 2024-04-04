```
ZXR10(config)#arp

ZXR10(config-arp)#interface gei-0/2.1

ZXR10(config-arp-if-gei-0/2.1)#arp permanent 120.1.1.3 0020.1122.3355 ＜vlan-id＞
```

_在子接口配置永久arp，与实接口类似，唯一不同的是需要在最后加上VLAN-ID_

### 6800解除arp绑定
```
ZXR10(config)#arp
ZXR10(config-arp)#no arp to-static interface gei-0/2.1
```


### 3800解除arp绑定
```
ZXR10(config)#interface gei-0/2.1
ZXR10(config-arp-if-gei-0/2.1)#no arp to-static
```

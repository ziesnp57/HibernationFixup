HibernationFixup
==================

[![Build Status](https://github.com/ziesnp57/HibernationFixup/workflows/CI/badge.svg?branch=master)](https://github.com/ziesnp57/HibernationFixup/actions) [![Scan Status](https://scan.coverity.com/projects/16402/badge.svg?flat=1)](https://scan.coverity.com/projects/16402)

一个开源内核扩展，在RTC变量和NVRAM之间提供同步。通过设计，mach内核对休眠睡眠图像进行加密，并将加密密钥写入系统注册表（PMRootDomain）中的变量“IOHibernateRTCVariables”。不知何故，这个值必须写入RTC（或SMC），以便boot.efi可以读取它。但是，如果您必须将RTC内存限制为1个银行（128字节），它不起作用：SMC/NVRAM/RTC（实际上是FakeSMC）中没有任何变量。

幸运的是，boot.efi可以从NVRAM读取密钥“IOHibernateRTCVariables”！此kext检测进入“休眠”电源状态，从系统注册表中读取变量IOHibernateRTCVariables并将其写入NVRAM。

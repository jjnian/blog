---
title: java虚拟机
cover: >-
  https://github.com/jjnian/images/blob/main/blog/%E7%81%AB%E8%BD%A6%E7%AB%99.png?raw=true
date: 2023-06-24 20:06:09
comments: true
---

## Java虚拟机

它是一种能够在不同操作系统上执行Java字节码的虚拟计算机。JVM的主要功能是解释和执行Java字节码，使得Java程序能够在不同的平台上运行。其中以Hotspot使用最为广泛，也是OpenJDK默认的Java虚拟机。

通过编译器把源文件编译成符合[JVM规范](https://docs.oracle.com/javase/specs/index.html)的class文件，就可以通过jvm运行。

### class文件

class文件是二进制文件，直接用记事本打开会是乱码，使用二进制方式打开如下图所示

```class
cafe babe 0000 0034 0038 0a00 0b00 2107
0022 0700 2308 0024 0a00 0300 250a 0002
0026 0a00 0200 2709 0028 0029 0a00 2a00
0016 0019 001a 0002 001b 0000 000a 0002
fd00 1807 001c 0114 001d 0000 0004 0001
001e 0001 001f 0000 0002 0020 
```

class文件结构如下图所示

| 类型           | 名称                                 | 解释           | 数量                  |
| -------------- | ------------------------------------ | -------------- | --------------------- |
| u4             | magic                                | 魔数           | 1                     |
| u2             | minor_version                        | 次版本号       | 1                     |
| u2             | major_version                        | 主版本号       | 1                     |
| u2             | constant_pool_count                  | 常量池个数     | 1                     |
| cp_info        | constant_pool[constant_pool_count-1] | 常量池         | constant_pool_count-1 |
| u2             | access_flags                         | 访问标记       | 1                     |
| u2             | this_class                           | 类索引         | 1                     |
| u2             | super_class                          | 父亲索引       | 1                     |
| u2             | interfaces_count                     | 接口索引数量   | 1                     |
| u2             | interfaces[interfaces_count]         | 接口内容       | interfaces_count      |
| u2             | fields_count                         | 字段表字段数量 | 1                     |
| field_info     | fields[fields_count]                 | 字段表         | fields_count          |
| u2             | methods_count                        | 方法表方法数量 | 1                     |
| method_info    | methods[methods_count]               | 方法表         | methods_count         |
| u2             | attributes_count                     | 属性表属性数量 | 1                     |
| attribute_info | attributes[attributes_count]         | 属性表         | attributes_count      |

上面u2为2个字节，u4为4个字节，通过上面的class文件结构翻译class文件的二进制内容。

### [JDK源码](https://github.com/openjdk/jdk)

文本以jdk11为例，查看jdk11下的虚拟机代码，该代码目录在src/hotspot文件下。

```shell
cpu                   # 与cpu架构相关的代码
os                    # 与操作系统相关的代码
os_cpu		            # 与cpu和操作系统相关的代码 
share
	adlc              # 平台描述语言编译器（编译cpu目录中的*.ad文件）
	aot               # AOT支持，加载验证AOT库等
	asm               # 宏汇编器，为宏形式的JIT代码生成机器代码 
	cl                # Client即时编译器   
	ci                # 编译器接口，定义JIT编译器通用的一些结构
	classfile         # 字节码文件解析和处理
	code              # 描述JIT编译后的的代码结构等
	compiler          # JIT编译器代码，虚拟机通过它选择特定的JIT编译器
	gc                # 垃圾回收， gc/shared表示共享代码，gc/gc1，gc/cms表示特定代码
	include           # 一些JVM函数和常量的导出
	interpreter       # 模版解释器和cpp解释器实现
	jfr               # 诊断工具Java Flight Record
	jvmci             # JVMCI编译器接口，可以开启Graal编译器代替C2
	libadt            # 内部使用的数据结构 
	logging           # 日志记录模块
	memory            # 内存相关，包括内存划分，metaspace划分等
	metaprogramming   # 元编程的一些type_trains
	oops              # Java类，对象在jvm的表示
	opto              # Server即时编译器（C2。JIT）
	precompiled       # 预编译文件
	prims             # JNI、JVMTI、Unsafe类具体实现
	runtime           # 包罗万象的JVM运行时模块
	services          # HeapDump、MXBean、jcmd、jinfo等辅助工具支持
	utilties          # 工具组件，如hashtable、JSON解析器、elf格式、快排算法
```

### JDK源码编译













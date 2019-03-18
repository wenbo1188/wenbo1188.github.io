---
layout: post
title: iso base media file format学习笔记
date: 2019-02-28
categories: mp4
---

## 标记、定义、缩写

### 标记和定义
+ box: 由唯一的类型和长度标记的object-oriented的构造单元(在MP4的早期版本中也称为"atom")
+ chunk: 在一个track中连续sample的集合
+ container box: 唯一作用是容纳一系列相关子box的box
+ hint track: 不包含媒体数据，而包含将一个或多个track中的数据打包成流通道(streaming channel)的指令的特殊track
+ hinter: 能够运行在只包含媒体数据的文件上，向其中加入hint track的工具
+ ISO Base Media File: 本文档中描述的文件格式的文件名称
+ leafsegment: 叶子段，不能再分割的segment
+ media data box: 包含实际媒体数据的box("mdat")
+ movie box: 包含定义元数据(metadata)的子box的container box("moov")
+ movie-fragment relative addressing: 媒体数据相对于这些movie-fragment起始处的偏移量
+ presentation: 一个或多个画面序列，可能包含音频
+ random access point (RAP): 从该sample可以正确解码音视频信息的那些sample
+ random access recovery point: 在按解码顺序对该sample之前的多个sample进行解码后可以正确解码，有时称为逐步解码刷新(gradual decoding refresh)
+ sample: 与一个单独时间戳相关的所有数据
- Note: 在非hint track中，sample指独立的视频帧，一系列包含解码顺序的视频帧，或一系列包含解码顺序的音频的压缩部分；在hint track中sample定义了一个或多个流分组(streaming packets)的形成
+ sample description: 用于定义和描述track中一些sample的结构
+ sample table: 用于描述track中sample的时序和物理布局的打包目录
+ sync sample: 如果解码在sync sample处开始，则解码顺序中的它和后续样本都可以被正确地解码，媒体格式可以为该格式提供更精确的sync sample定义
+ segment: iso base media file中的一部分，包含movie box以及相关媒体数据和其它box或者包含一个或多个movie fragment box以及相关媒体数据和其它box
+ track: 一系列包含时序关系的相关sample

### 缩写
ALC 	Asynchronous Layered Coding
FD 		File Delivery
FDT		File Delivery Table
FEC	    Forward Error Correction
FLUTE 	File Delivery over Unidirectional Transport
IANA 	Internet Assigned Numbers Authority
LCT 	Layered Coding Transport
MBMS 	Multimedia Broadcast/Multicast Service

## 面向对象结构(object-structured)的文件组织

### 文件结构
文件由一系列称为box的对象组成，所有的数据均包含在box里，除此之外文件中没有其他数据。它也包含了特殊文件格式所规定的初始的签名。  

所有的本部分提到的面向对象结构的文件都应该包含一个文件类型box

### 对象结构
这里的对象指的就是box。  

box的头部会给出该box的长度和类型，头部允许使用紧凑或扩展的长度（32或64位）和紧凑或扩展类型（32位或完整的通用唯一标识符，即UUID）。标准的box均使用紧凑型（32位），大多数盒子使用紧凑型（32位）。通常，只有media data box(es)需要64位表示长度。

box的长度是整个box的大小，包括头部，字段，和所有的包含的子box，这有助于文件的解析。

```C++
aligned(8) class Box (unsigned int(32) boxtype, 
            optional unsigned int(8)[16] extended_type) {
	unsigned int(32) size;
	unsigned int(32) type = boxtype;
	if (size==1) {
		unsigned int(64) largesize;
	} else if (size==0) {
		// box extends to end of file
	}
	if (boxtype==‘uuid’) {
		unsigned int(8)[16] usertype = extended_type;
	}
}
```

对于无法识别type的box，应直接忽略。
许多object包含version以及flags字段，定义如下:

```C++
aligned(8) class FullBox(unsigned int(32) boxtype, unsigned int(8) v, bit(24) f) 
	extends Box(boxtype) {
	unsigned int(8)	version = v;
	bit(24) flags = f;
}
```

version表示此box格式的版本，flags是一些列的flag。
对于无法识别version的box，应直接忽略。

### 文件类型box(File Type Box)

文件类型box的类型字段为:'ftyp'，属于文件层次，位置在文件最前面，至多只有一个，可将没有文件类型box的文件解读为Marjor_brand='mp41'，minor_version=0，且只兼容mp41的文件。

minor_version只用于传递major_version以外的信息，如debug等，而不用于检查版本兼容性。

文件类型box定义如下:

```C++
aligned(8) class FileTypeBox 
	extends Box(‘ftyp’) {
	unsigned int(32)	major_brand;
	unsigned int(32)	minor_version;
	unsigned int(32)	compatible_brands[];	// to end of the box
}
```



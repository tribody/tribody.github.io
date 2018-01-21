---
title: After Effects DEV
date: 2016-12-21 08:18:16
tags: AE开发
categories: 技术
---

## 简介
Effect（效果器）插件可以被用来创作、处理音视频。它是AE中的一种插件类型。

Effect（效果器）出现在Effect菜单和Effect&Presets面板中，并在PiPL文件中申明。一旦应用了Effect（效果器）,它的参数控制就会出现在效果器控制面板中（Effect Control panel, ECP）。

插件都必须包含一个Plug-in Property List（PiPL），并且插件必须放在特定的文件夹中以便AE加载。

对于所有的effect插件，AE通过发送命令选择器（command selector）到插件的入口函数（在PiPL资源文件中指定的）与插件沟通。而这些命令通过用户的一些行为触发，包括应用effect，改变参数，擦除时间轴的帧，显然序列等等。AE创建了effect的多个实例，以便于对于每个序列它的设置和输入都是独立的。但所有的实例都拥有相同的全局变量，并在它们序列的所有帧之间共享数据。一旦用户应用effectAE才会开始处理图片数据。

<!-- more -->
## 创建项目

- 第一步，要创建一个自己的effect项目，首先复制整个`\Skeleton`文件夹，重命名为`\YourName`，然后使用Ultraedit或其他编辑器批量替换`\YourName`文件夹下所有的`Skeleton`和`SKELETON`为`YourName`和`YOURNAME`。
- 第二步，在VS中配置输出目录为AE的插件目录。可以在环境变量中新建一个变量`AE_PLUGIN_BUILD_DIR`，值为`C:\Program Files\Adobe\Common\Plug-ins\7.0\MediaCore\`（可能有所区别，以具体安装版本和情况为准），然后在`配置属性>常规>输出目录`中设置输出目录。
- 第三步，在VS中配置调试文件为AE的可执行文件。在`配置属性>调试>命令`中设置AE的可执行文件路径。如`D:\Program Files\Adobe\Adobe After Effects CC 2014\Support Files\AfterFX.exe`。

调试时可选择删除配置信息。方法为在每次AE重启的时候按住`Ctrl-Alt-Shift`即可。

PiPL资源文件规定了入口函数名称，显示的名称，和插件的匹配名称（match name）。

## Effect基础
入口函数中的参数。
``` C++
PF_Err main (
PF_Cmd cmd,
PF_InData *in_data,
PF_OutData *out_data,
PF_ParamDef *params[],
PF_LayerDef *output,
void *extra)
```

每次调用入口函数，AE会更新`PF_InData`和插件的参数数组`PF_ParamDef[]`。在插件返回后，AE会检查`PF_OutData`以便更改，并且在合适的时候，使用effect渲染过的`PF_LayerDef`。

`params`和`output`分别包含当前输入输出帧（params[0]包含输入图像）。

## Effect进阶


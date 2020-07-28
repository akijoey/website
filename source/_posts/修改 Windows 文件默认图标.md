---
title: Windows 修改文件默认图标
date: 2019-11-02
tags: Python
categories: Common
keywords: Python
description: Python
cover: /img/cover_posts/python.jpg
top_img: /img/cover_posts/python.jpg
---
<strong>Windows 修改文件的默认图标.</strong>
<strong>可以分为以下两个步骤:</strong>
- <strong>批量图片格式转换</strong>
- <strong>批量修改注册表</strong>

## 批量图片格式转换

这里使用的是 vscode-icons 插件中的部分图标.
因为这里的图标比较全, 当然你也可以用自己喜欢的图标.
插件中的图标都是 svg 格式, 所以要将其转换为 ico 格式.

### 安装 Wand

这里要用到 python 的一个图片处理模块 <span style='color:#FF0000'>Wand</span>.

`$ pip install Wand`

### 安装 ImageMagick

Wand 需要与 ImageMagick 连接使用, 因此还需要安装 <span style='color:#FF0000'>ImageMagick</span>.
具体安装过程见: [http://docs.wand-py.org/en/latest/guide/install.html](http://docs.wand-py.org/en/latest/guide/install.html)

convert 命令可以直接转换图片格式.

`$ convert -background transparent a.svg a.ico`

### Source Code

```python
import os
from wand.image import Image

INPUT_PATH = r'C:\Users\10106\Desktop\svg'	# 输入路径
OUTPUT_PATH = r'C:\Users\10106\Desktop\ico'	# 输出路径

def EnumPathFiles(path, callback):	# 枚举路径下所有文件
	if not os.path.isdir(path):
		print('Error: "', path, '" is not a directory or does not exist.')
		return
	list_dirs = os.walk(path)
	for root, dirs, files in list_dirs:
		for d in dirs:
			EnumPathFiles(os.path.join(root, d), callback)
		for f in files:
			callback(root, f)

def svg2ico(path, filename):	# 格式转换
	filename = str(filename)
	i = filename.index('.')
	fname = filename[:i]
	fext = filename[i:]
	if fext !='.svg':
		return
	input_file = str(path + '\\' + filename)
	output_file = OUTPUT_PATH + '\\' + fname + '.ico'
	with Image(width = 128, height = 128, filename = input_file, background = 'transparent') as ico:
		with Image(width = 64, height = 64, filename = input_file, background = 'transparent') as sico:
			ico.sequence.append(sico)
		with Image(width = 48, height = 48, filename = input_file, background = 'transparent') as sico:
			ico.sequence.append(sico)
		with Image(width = 32, height = 32, filename = input_file, background = 'transparent') as sico:
			ico.sequence.append(sico)
		with Image(width = 16, height = 16, filename = input_file, background = 'transparent') as sico:
			ico.sequence.append(sico)
		ico.save(filename = output_file)
	print(output_file)

if __name__ == '__main__':
	EnumPathFiles(INPUT_PATH, svg2ico)
```

## 批量修改注册表

操作注册表需要用到 python 的内置模块 <span style='color:#FF0000'>winreg</span>.
官方文档: [https://docs.python.org/3.7/library/winreg.html](https://docs.python.org/3.7/library/winreg.html)

### 单次更新

思路就是修改 `HKEY_CLASSES_ROOT` 下的某些默认值
(同时实现将文件的默认打开方式设置为 VSCode)
一个文件类型具体要实现三次注册表更新, 以 .c 为例
- 修改 `.c` 的默认值为 `.c_auto_file`
- 修改 `.c_auto_file\Defaulticon` 的默认值为 <span style='color:#FF0000'>图标路径</span>
- 修改 `.c_auto_file\shell\open\command` 的默认值为 <span style='color:#FF0000'>VSCode 路径</p>

### Source Code

```python
import winreg

VSCODE_PATH = r'"D:\Tool\VSCode\Code.exe" "%1"'	# VSCode 路径
ICON_PATH = r'D:\Medium\Picture\Material\Icon\file'	# 图标路径
FILE_TYPE = [	# 要修改的文件后缀列表
	'html',
	'css',
	'js',
	'php',
	'vue',
	'ts',
	'scss',
	'json',
	'xml',
	'sql',
]

def EnumFileType(cpath, ipath):	# 枚举文件类型
	for name in FILE_TYPE:
		kpath = '.' + name
		vpath = kpath + r'_auto_file'
		registryUpdate(kpath, vpath)
		kpath = '.' + name + r'_auto_file\Defaulticon'
		vpath = ipath + r'\file_type_' + name + '.ico'
		registryUpdate(kpath, vpath)
		kpath = '.' + name + r'_auto_file\shell\open\command'
		vpath = cpath
		registryUpdate(kpath, vpath)
		print('.' + name + ' successful update')
	print('Please restart system')

def registryUpdate(kpath, vpath):	# 更新注册表
	with winreg.CreateKeyEx(winreg.HKEY_CLASSES_ROOT, kpath, 0, winreg.KEY_ALL_ACCESS) as key:
		winreg.SetValue(key, '', winreg.REG_SZ, vpath)

if __name__ == '__main__':
	EnumFileType(VSCODE_PATH, ICON_PATH)
```

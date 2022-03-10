# Ubuntu中文显示异常


很久之前解决的这个问题,不过还是要记录下来(睡前水个贴)
设置系统语言为英文后,就会发现有些中文字体很难看,后来才知道原来那些丑丑的中文符号是日本语
呕...所以赶紧改回来吧
<!--more-->

首先中文字体配置文件的位置
```
/etc/fonts/conf.avail/64-language-selector-prefer.conf
```
打开文件
```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
	<alias>
		<family>sans-serif</family>
		<prefer>
			<family>Noto Sans CJK JP</family>
			<family>Noto Sans CJK SC</family>
			<family>Noto Sans CJK TC</family>
		</prefer>
	</alias>
	<alias>
		<family>monospace</family>
		<prefer>
			<family>Noto Sans Mono CJK JP</family>
			<family>Noto Sans Mono CJK SC</family>
			<family>Noto Sans Mono CJK TC</family>
		</prefer>
	</alias>
</fontconfig>
```
可以发现JP(日本)排在第一个,所以优先级要高。
这时就需要调整一下优先级
更改为:
```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
	<alias>
		<family>sans-serif</family>
		<prefer>
			<family>Noto Sans Mono CJK SC</family>
			<family>Noto Sans Mono CJK TC</family>
			<family>Noto Sans Mono CJK JP</family>
		</prefer>
	</alias>
	<alias>
		<family>serif</family>
		<prefer>
			<family>Noto Sans Mono CJK SC</family>
			<family>Noto Sans Mono CJK TC</family>
			<family>Noto Sans Mono CJK JP</family>
		</prefer>
	</alias>
	<alias>
		<family>monospace</family>
		<prefer>
			<family>Noto Sans Mono CJK SC</family>
			<family>Noto Sans Mono CJK TC</family>
			<family>Noto Sans Mono CJK JP</family>
		</prefer>
	</alias>
</fontconfig>
```
最后重启生效
再见丑丑的日本语

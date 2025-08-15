# Linux命令

使用linux时多思考如何直接用命令行实现一些复杂操作

## wc命令

wc的使用，常常搭配pipeline，比如我查看当前目录下有多少个子目录：

```bash
ls -l | grep '^d' | wc -l
```

`ls -l` 列出详细信息，`grep '^d'` 过滤出目录行，`wc -l` 统计行数。

## sed命令

sed的几个常见使用场景：

```bash
sed -i 's/old/new/g' filename  # 替换old为new
sed -i 'Nd' filename  # 删除第N行
sed -i '/pattern/d' filename  # 删除包含pattern的行
```

例如，如果我要删除所有'.jpg'的字符

```bash
sed -i 's/\.jpg//g' filename
```

sed插入

```bash
sed -i '1i\new line' filename  # 在第一行前插入新行
```

sed追加

```bash
sed -i '1a\new line' filename  # 在第一行后插入新行
```

!!! tip
    总结一下sed的语法，一般来说会这个替换就差不多：

    - `s/old/new/g` 替换
    - `Nd` 删除第N行
    - `/pattern/d` 删除包含pattern的行
    - `Ni\text` 在第N行前插入text
    - `Na\text` 在第N行后插入text
    - `-i` 直接修改文件
    - 不加`-i`则输出到标准输出
    - `g`表示全局替换
    - `s`表示替换
  
## awk命令

awk命令的作用是按列处理文本，把输入的每行文本按指定分隔符（默认空格或制表符）分成多个字段，然后可以对这些字段进行操作。

```bash
echo "a b c" | awk '{print $1, $3}'  # 打印第一列和第三列
du -sh * | awk '{print $1, $2}'
awk '{print $2}' filename  # 打印第二列
awk -F: '{print $1}' /etc/passwd  # 打印/etc/passwd的第一列
ps aux | awk '$3 > 50 {print $1, $3}'  # 打印CPU使用率大于50的进程
ls -l | awk '/^d/ {print $9}'  # 打印所有目录的名称
cat file.txt | awk 'NF > 0'  # 删除空行 NF > 0 表示非空行
```

# Linux命令

使用linux时多思考如何直接用命令行实现一些复杂操作

## 文本统计命令 wc

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


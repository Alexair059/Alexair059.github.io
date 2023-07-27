---
comments: true
---

# 编码 | Encode

编码（encode）的目的不是为了加密，而是代表信息的另外一种表达方式。将原始信息转化为编码信息进行传输，可以解决一些特殊字符、不可见字符的传输问题

接收者将编码信息再转化为原始信息，转化的过程称之为解码（Decode）

## 进制转换

进制转换在密码学中十分常见，因为不同进制下的字符集大小不同，所以这是一种天然的编码方式

### 10进制转2/8/16进制

Python 的内置函数 `bin()`，`oct()`，`hex()` 接收 `int`，并输出对应进制下的 `str`

```python
bin(59) # '0b111011'
oct(59) # '0o73'
hex(59) # '0x3b'
```

由上述可知，如果需要定义一个特定进制下的 `int` 字面值，只需要加上对应的前缀即可

```python
bin(0b111011)   # '0b111011'
oct(0o73)   #'0o73'
hex(0x3b)   # '0x3b'
```

### 2/8/16进制转10进制

Python 的内置函数 `int()` 接收 `str`，并输出参数进制下的 `int`（常常用10进制表示，没有任何前缀）

```python
int('111011', 2)    # 59
int('73', 8)    # 59
int('3b', 16)   # 59
```

## Leet语

Leet（英文中亦称 leetspeak 或 eleet），又称黑客语，指一种发源于西方国家的BBS、在线游戏和黑客社群所使用的文字书写方式

通常是把拉丁字母转变成数字或是特殊符号，例如 E 写成 3、A 写成 4 等。或是将单字写成同音的字母或数字，如 to 写成 2、for 写成 4 等

在 CTF 中，很多线上赛的 flag 也采用的这样的写法，不仅美观而且也加强了复杂性

常见的单字转数字写法有

- A：4
- B：8
- E：3
- G：6
- I/L：1
- O：0
- S：5
- T：7
- Z：2

例如现有一个密文

> Ada is the key.

我们可以写成一个flag

```
crypto{4da_1s_7he_K3y}
```

## str 与 bytes 类型（Python）

### 常用 Trick

**`str` 类型**

```python
list(r'crypto{4da_1s_7he_K3y}')    # ['c','r','y','p','t',···,'y','}']

''.join(list(r'crypto{4da_1s_7he_K3y}'))    # 'crypto{4da_1s_7he_K3y}'

'crypto{4da_1s_7he_K3y}'.upper()    # 'CRYPTO{4DA_1S_7HE_K3Y}'

'crypto{4da_1s_7he_K3y}'.lower()    # 'crypto{4da_1s_7he_k3y}'

'1100'.zfill(8)    # '00001100'
```

`bytes` 类型

```python
list(b'crypto{4da_1s_7he_K3y}')    # [99,114,121,112,116,···,121,125]
```

### `str` 与 `bytes` 类型的转换

**字面值转换**

`str` 的字面值与 `bytes` 的字面值展示内容是一样的，只需要添加前缀 `b`

```python
type('crypto{4da_1s_7he_K3y}')    # str
type(b'crypto{4da_1s_7he_K3y}')    #bytes
```

**变量转换**

`str` 类型下有 `.encode()` 函数，`bytes` 类型下有对应的 `.decode()` 函数

```python
'crypto{4da_1s_7he_K3y}'.encode()    # b'crypto{4da_1s_7he_K3y}'

b'crypto{4da_1s_7he_K3y}'.decode()    # 'crypto{4da_1s_7he_K3y}'
```

## ASCII

一种 7 位编码标准，使用整数 0-127 来表示对应字符，其字符集便是 ASCII 码表

Python 的内置函数 `ord()` 接收单个 `str` / `chr`，返回 `int` 为其对应的 ASCII 码值

```python
ord('A')    #65
```

Python 的内置函数 `chr()` 接收 `int`，返回 `str` 作为对应的 ASCII 码

```python
chr(65)    # 'A'
```

## Hex | Base16

Hex 即 Base16 编码，Base16 指的是其字符集为 0-9、A-F 共16个字符，类似的还有 Base32，Base64编码（除开补位的 '='）

### 原理

ASCII 编码下，一般的英文字符大小均为 1byte，即 8bit，于是我们可以将其切分为两个 4bit 看待，而对于一个 占位 4bit 的信息，我们只需要 $2^4=16$ 个字符组成的集合便能一一对应，这就是 Base16 的原理

实际编码中，我们首先将 ASCII 编码下的信息，即原始信息取 ASCII 码值并二进制展开，对不足 8 位的进行高位补 0 ，得到代表原信息的二进制串，其位数必为 8 的倍数

接着我们将该二进制串按 4bit 划分为小节，每一小节代表的值便对应 Base16 码表下相应的字符

### 特点

由于 ASCII 码 8bit 代表一个字符，Base16 码 4bit 代表一个字符，即每 1 个 ASCII 字符一定可以用 2 个Base16 字符表示，因此我们可知 Base16 编码的信息字符数一般为偶数，并且很少需要 '=' 补位

### 工具

**个人实现**

虽然一般的 Base 编码都需要码表，但 Base16 码值对应的字符便是其 16 进制下的表达，所以我们可以直接借助前文的进制转换函数实现 Base16 编码

```python
def my_hex(input_str):
    bstream = [bin(ord(o))[2:].zfill(8)  for o in input_str]
    tmp = []
    for item in bstream:
        tmp.append(hex(int(item[:4], 2))[2:])
        tmp.append(hex(int(item[4:], 2))[2:])
    return ''.join(tmp)

my_hex(r'crypto{4da_1s_7he_K3y}')    # '63727970746f7b3464615f31735f3768655f4b33797d'
```

其接收 `str`，并返回对应 Base16 编码的 `str`。此处 `'crypto{4da_1s_7he_K3y}'` 的 `str` 字面值中花括号会被转义，故加上 `r` 前缀表示原始字符串

**内置函数**

比较常用的是 `bytes` 类型下的 `.hex()` 函数，它将其编码为 `str` 类型的 Base16 编码

```python
b'crypto{4da_1s_7he_K3y}'.hex()    # '63727970746f7b3464615f31735f3768655f4b33797d'
```

以及 `bytes.fromhex()` 函数，它接收一个 `str` 类型，并返回一个 `bytes` 对应原始字符串

```python
bytes.fromhex('63727970746f7b3464615f31735f3768655f4b33797d')    # b'crypto{4da_1s_7he_K3y}'
```

## Base32 & Base64

与 Base16 编码类似，Base32 与 Base64 分别用 32 个字符（占位 5bit）与 64 个字符（占位 6bit）表示原始字符

### 特点

这意味着每 5/3 个原始字符一定能用 8/4个 Base32/64 字符表示

Base32 编码长度常常是 8 的倍数，Base64 编码长度常常是 4 的倍数，且时常有 '=' 补位

### 工具

**内置函数**

Python 的标准库 `base64` 提供一系列 Base 编码函数

`base64.b16encode()` 接收 `bytes`，并返回 Base16 编码后的 `bytes`

```python
base64.b16encode(b'crypto{4da_1s_7he_K3y}')    # b'63727970746F7B3464615F31735F3768655F4B33797D'
```

与 `bytes` 内置的函数 `.hex()` 不同，它返回的是 `bytes`，并且数值中的字母都是大写

```python
base64.b16encode(b'crypto{4da_1s_7he_K3y}').decode().lower() == b'crypto{4da_1s_7he_K3y}'.hex()    # True
```

以及 `base64.b16decode()`，它接收 `str` 或 `bytes`， 并返回对应原信息的 `bytes`

```python
base64.b16decode(b'63727970746F7B3464615F31735F3768655F4B33797D')    # b'crypto{4da_1s_7he_K3y}'
```

同样它与 `bytes.fromhex()` 不同，对里面字母要求大写

Base32，Base64 的函数写法类似，不再赘述

**Pycryptodome**

该库提供一个方便快捷的函数 `bytes_to_long()`，接收一个 `bytes`，并返回十进制下的 Base16 编码值 `int`

```python
bytes_to_long(b'crypto{4da_1s_7he_K3y}')    # 37207601978879818689532919055604181433045365814425981

bytes_to_long(b'crypto{4da_1s_7he_K3y}') == int(base64.b16encode(b'crypto{4da_1s_7he_K3y}').decode(), 16)    # True
```

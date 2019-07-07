
## Python异常处理
异常即是一个事件，该事件会在程序执行过程中发生，影响了程序的正常执行。

一般情况下，在Python无法正常处理程序时就会发生一个异常。

异常是Python对象，表示一个错误。

当Python脚本发生异常时我们需要捕获处理它，否则程序会终止执行。

捕捉异常可以使用try/except语句。



```python
#语法
'''
try:
    代码  #要执行的代码
except:
    代码  #如果try中的代码出现错误，则执行except中的代码，可以在except后面加上错误的类型，如果引发此类异常，则执行except
finally:
    代码  #不论程序出没出错都要执行
'''

a = 1
b = '1'
try:
    print(a+b)
except TypeError:
    print('类型错误')
finally:
    print('结束')
    
a = 1
b = '1'
try:
    print(a+b)
except:
    print('错误')
finally:
    print('结束')
```

    类型错误
    结束
    错误
    结束
    

## Python文件读写
读写文件是最常见的IO操作。Python内置了读写文件的函数，用法和C是兼容的。

读写文件前，我们先必须了解一下，在磁盘上读写文件的功能都是由操作系统提供的，现代操作系统不允许普通的程序直接操作磁盘，所以，读写文件就是请求操作系统打开一个文件对象（通常称为文件描述符），然后，通过操作系统提供的接口从这个文件对象中读取数据（读文件），或者把数据写入这个文件对象（写文件）。

### 读文件
Python内置的open()函数可以打开一个文件。


```python
#打开文件，'r'表示只读模式
#注意：要读的文件与py文件不再同一目录下时，路径要写绝对路径，在同一目录下时，可以只写名字
f = open('1.txt', 'r')
'''
文件内容：
Hello World
'''

#读取内容，read()
print(f.read())

#关闭文件
f.close()

#f = open('2.txt', 'r')
#如果文件不存在，抛出一个IOError的错误
#FileNotFoundError                         Traceback (most recent call last)
#<ipython-input-2-fc64edc67529> in <module>
#---->  f = open('2.txt', 'r')
#
#FileNotFoundError: [Errno 2] No such file or directory: '2.txt'
```

    nice to meet you
    hello
    


```python
#由于文件的读写可能产生错误，导致文件不能正常关闭，为了保证不出现文件没关闭的情况，可以使用try...except来处理。
try:
    f = open('1.txt', 'r')
    print(f.read())
except:
    print('文件读取出错')
finally:
    if f:
        f.close()
```

    nice to meet you
    hello
    


```python
#每次都这样写可能会有点繁琐，python引入了with语句来自动帮我们调用close()方法
#as用来取别名
with open('1.txt', 'r') as f:
    print(f.read())

```

    nice to meet you
    hello
    

调用read()会一次性读取文件的全部内容，如果文件较大的话，会出现内存不够的情况，所以保险起见，可以使用read(size)的方法，每次读取一定大小的数据。也可以使用readline()，每次读取一行内容。


```python
with open('1.txt', 'r') as f:
    for line in f.readlines():
        print(line)
```

    nice to meet you
    
    hello
    


```python
 '''
其他模式：
r   :以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。
rb  :以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。这是默认模式。
r+  :打开一个文件用于读写。文件指针将会放在文件的开头。
rb+ :以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。
w   :打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。
     如果该文件不存在，创建新文件。
wb  :以二进制格式打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。
     如果该文件不存在，创建新文件。
w+  :打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。
     如果该文件不存在，创建新文件。
wb+ :以二进制格式打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。
     如果该文件不存在，创建新文件。
a   :打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。
     也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。
ab  :以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。
     也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。
a+  :打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。
     如果该文件不存在，创建新文件用于读写。
ab+ :以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。
     如果该文件不存在，创建新文件用于读写。
'''
```




    '\n其他模式：\nr   :以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。\nrb  :以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。这是默认模式。\nr+  :打开一个文件用于读写。文件指针将会放在文件的开头。\nrb+ :以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。\nw   :打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。\n    如果该文件不存在，创建新文件。\nwb  :以二进制格式打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。\n    如果该文件不存在，创建新文件。\nw+  :打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。\n    如果该文件不存在，创建新文件。\nwb+ :以二进制格式打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。\n    如果该文件不存在，创建新文件。\na   :打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。\n    也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。\nab  :以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。\n    也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。\na+  :打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。\n    如果该文件不存在，创建新文件用于读写。\nab+ :以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。\n    如果该文件不存在，创建新文件用于读写。\n'



### 写文件
写文件和读文件是一样的，唯一区别是调用open()函数时，传入标识符'w'或者'wb'表示写文本文件或写二进制文件。


```python
#w表示从开头开始编辑，即原有内容会被删除
f = open('1.txt', 'w')
f.write('nice to meet you')
f.close()
```


```python
#a:追加
with open('1.txt', 'a') as f:
    f.write('hello')
```

## Python操作excel
 python操作excel主要用到xlrd和xlwt这两个库，即xlrd是读excel，xlwt是写excel的库，xlutils能将xlrd.Book转为xlwt.Workbook，从而得以在现有xls的基础上修改数据，并创建一个新的xls，实现修改。
 
 安装：
 
 pip install xlrd
 
 pip install xlwt

 pip install xlutils


```python
#xlrd
import xlrd
#读取excel
data = xlrd.open_workbook('test.xls')

#获取所有sheet名字]
print(data.sheet_names())

#获取一个工作表
table = data.sheet_by_index(0)             #通过索引顺序获取
table = data.sheet_by_name(u'user_data')  #通过名称获取
```

    ['user_data']
    


```python
#获取总行数
table.nrows
#获取总列数
table.ncols

table.row_values(0)       #获取整行值（数组）
table.col_values(1)       #获取整列的值（数组）

#获取单元格数据
B2 = table.cell(1,1).value
C4 = table.cell(2,3).value
print(B2)
print(C4)

#使用行列索引
B3 = table.row(2)[1].value
print(B3)
```

    52.0
    https://pic4.zhimg.com/v2-85890d2e63c0232e415441b1e9edea0b_is.jpg
    5.0
    


```python
#xlwt
import xlwt
workbook = xlwt.Workbook()
worksheet = workbook.add_sheet('demo')

#写入数据
worksheet.write(1, 0, 'test')

#保存excel
workbook.save('1.xlsx')
```


```python
#修改现有的excel时，需要用到xlutils
import xlrd
from xlutils.copy import copy
import xlwt

#读取excel
data = xlrd.open_workbook('1.xls')

#将读取的excel先拷贝下来
wb = copy(data)

#在拷贝下来的数据中获取表
ws = wb.get_sheet(0)

#写入数据
ws.write(0, 0, 'changed')
 
#保存excel
wb.save('1.xls')
```


```python
#openpyxl

#打开已有的excel
from openpyxl  import load_workbook
wb1 = load_workbook('demo.xlsx')
```


```python
#创建一个excel
from  openpyxl import  Workbook 
# 实例化
wb = Workbook()
# 激活 worksheet
ws = wb.active
#写入数据
ws['A1'] = 42
ws.append([1, 2, 3])

#创建工作表
ws1 = wb.create_sheet("Mysheet")

#选择工作表
ws3 = wb["Mysheet"]
ws4 = wb.get_sheet_by_name("Mysheet")
```

    D:\Anaconda3\lib\site-packages\ipykernel_launcher.py:16: DeprecationWarning: Call to deprecated function get_sheet_by_name (Use wb[sheetname]).
      app.launch_new_instance()
    


```python
#获取单元格数据
c = ws['A1']
print(c.value)

d = ws.cell(row=4, column=2, value=10)
print(d.value)

#保存
wb.save('demo2.xlsx')
```

    42
    10
    

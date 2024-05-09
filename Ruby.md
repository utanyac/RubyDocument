# Ruby 教程

[TOC]

## Ruby 概述

### 语言特性

- 简介直观：约定优于配置。
- 灵活性：一种动态类型的语言，不需要事先声明属性。

### 参考资料

- Ruby入门教程    [Ruby 教程 | 菜鸟教程 (runoob.com)](https://www.runoob.com/ruby/ruby-tutorial.html)
- Ruby官方文档    https://www.ruby-lang.org/zh_cn/documentation/

- Rails入门教程     https://ruby-china.github.io/rails-guides/v4.1/getting_started.html

## Ruby 环境配置

### Windows 环境安装 Ruby

- Windows 系统下建议使用 RubyInstaller 来安装Ruby：https://rubyinstaller.org/downloads/

  选择带有DEVKIT的版本下载：Ruby+Devkit 3.3.1-1(x64)

  也可以走国内镜像网站下载：[RubyInstaller for Windows - 国内镜像](https://rubyinstaller.cn/)

- 双击rubyinstaller-devkit-3.3.1-1-x64.exe，选择安装路径后下载。

  安装时勾选 **Add Ruby executables to your PATH**，可以自动配置环境变量。

- 安装完成后，在弹出的命令行窗口里，输入 1，2，3 继续安装其他需要的文件。

- 以上步骤全部完成后，在任意路径下打开命令提示符，输入“ruby -v” 。输出版本表示安装成功。 

### Linux 环境安装 Ruby

*todo*

### Ruby 版本与开发工具

Ruby：3.0以上版本

Rubymine：

Rails：

### 关于 RubyGems

RubyGems 是 Ruby 的一个包管理器，它提供一个分发 Ruby 程序和库的标准格式，还提供一个管理程序包安装的工具。

RubyGems 旨在方便地管理 gem 安装的工具，以及用于分发 gem 的服务器。这类似于 Ubuntu 下的apt-get, Centos 的 yum，Python 的 pip。Ruby 1.9版本以上安装时会自带RubyGems。

```shell
# 安装包
gem install mygem
# 卸载包
gem uninstall mygem
# 列出已安装
gem list --local
# 升级gem
gem update --system
```

## Ruby 语法

### 代码格式

- Ruby 把分号和换行符解释为语句的结尾。
- Ruby 大小写敏感。
- Ruby 以 # 作为单行注释。
- Ruby 以 =begin 和 =end 作为多行注释。

------

### 数据类型

Ruby 支持的数据类型包括基本的Number（数值）、String（字符串）、Ranges（范围）、Symbols，以及true、false和nil这几个特殊值。

> 需要关注：①双引号字符串中引用变量；②Ranges类型变量的使用

```ruby
# 字符串类型 String
# 在双引号字符串中我们可以使用 #{} 井号和大括号来计算表达式的值：
name1 = "Joe"
name2 = "Mary"
puts "hello #{name1}, where is #{name2}"

# 范围类型 Range
# ..为左闭右闭区间
# ... 为左闭右开区间
range1 = (1..5).to_a    # to_a: 转换为数组
range2 = (1...5).to_a
puts "range1 = #{range1}"
puts "range2 = #{range2}"
```

------

### 变量

- **局部变量：**局部变量是在方法中定义的变量，在方法外是不可用的。
- **实例变量：**实例变量以@开头。它是实例对象的一个属性，在对象的整个生命周期都是可用的，可以在类的任何实例方法中被访问和修改。**[Ruby的动态性导致需要实例变量]**
- **类变量：**类变量以@@开头，它是类的一个属性，可以跨不同的对象使用，且使用前必须初始化。它也被共享在整个继承链中。**[类似于Java中的static静态变量]**
- **全局变量：**类变量不能跨类使用。如果您想要有一个可以跨类使用的变量，您需要定义全局变量。全局变量总是以美元符号（$）开始。
- **常数：**大写字母开头。

> 动态性：无需声明变量类型，在使用变量时，ruby 自动根据赋值确定变量类型。

------

### 方法

方法名必须以小写字母开头，且方法应在调用之前定义。

形参不需要声明类型。

如果不指定返回值，方法会默认返回最后一个语句的值；也可以通过 return 指定返回值。

```ruby
# 无参方法
def method_1
  puts "this is method 1"
end

# 有参方法
def method_2 (var1, var2)
  value = var1 + var2
  # 默认返回最后一个值
end

# 设置参数默认值
def method_3 (var1=5, var2=5)
  value = var1 + var2
  return value
end

#调用方法
method_1
a = method_2(20,30)
b = method_3(100)
puts "method 2 = #{a}"
puts "method 3 = #{b}"
```

ruby 中的类变量（@@）类似于 Java 的 static 静态变量，

ruby 中的类方法类似于 Java 的 static 方法。

与实例方法不同，类方法不需要实例化对象，可以通过类直接使用。

```ruby
class Accounts
   def reading_charge
   end
   # 类方法：写法1
   def Accounts.say_hello1
       puts "hello1"
   end
   
   # 类方法：写法2
   def self.say_hello2
       puts "hello2"
   end
end

# 调用类方法
Accounts.say_hello1
Accounts.say_hello2
```

------

### 类

类总是以关键字 *class* 开始，后跟类的名称。类名的首字母应该大写。

可以用库里预定义的 new 方法无参构造对象，也可以自定义方法实例化对象：

```ruby
class Customer
  # 类变量,多对象共用
  @@no_of_customers=0

  # 实例变量，类内共用
  def initialize(id, name, addr)
    @cust_id=id
    @cust_name=name
    @cust_addr=addr
  end

  def display_details()
    puts "Customer id #@cust_id"
    puts "Customer name #@cust_name"
    puts "Customer address #@cust_addr"
  end

  def total_no_of_customers()
    @@no_of_customers += 1
    puts "Total number of customers: #@@no_of_customers"
  end
end

# 创建对象
cust1=Customer.new("1", "John", "Wisdom Apartments, Ludhiya")
cust2=Customer.new("2", "Poul", "New Empire road, Khandala")
 
# 调用方法
cust1.display_details()
cust1.total_no_of_customers()
cust2.display_details()
cust2.total_no_of_customers()
```

------

### 运算符

与 Java 大同小异，不做赘述。

------

### 条件判断

#### if...else 语句

```ruby
# 写法1
x=1
if x > 2
   puts "x 大于 2"
elsif x <= 2 and x!=0
   puts "x 是 1"
else
   puts "无法得知 x 的值"
end

# 写法2
$debug=1
print "debug\n" if $debug
```

#### unless 语句

unless 和 if 作用相反：

```ruby
# 写法1
x=1
unless x>2
   puts "x 小于或等于 2"
 else
  puts "x 大于 2"
end
    
# 写法2
$var =  1
print "1 -- 这一行输出\n" if $var
print "2 -- 这一行不输出\n" unless $var
$var = false
print "3 -- 这一行输出\n" unless $var  
```

#### case 语句

```ruby
$age =  5
case $age
when 0 .. 2
    puts "婴儿"
when 3 .. 6
    puts "小孩"
when 7 .. 12
    puts "child"
when 13 .. 18
    puts "少年"
else
    puts "其他年龄段的"
end
```

------

### 循环

#### while 语句

```ruby
$i = 0
$num = 5

# 写法1
while $i < $num  do
   puts("在循环语句中 i = #$i" )
   $i +=1
end

#写法2
begin
   puts("在循环语句中 i = #$i" )
   $i +=1
end while $i < $num
```

#### until 语句

until 和 while 作用相反：

```ruby
$i = 0
$num = 5
begin
   puts("在循环语句中 i = #$i" )
   $i +=1;
end until $i > $num
```

#### for 语句

```ruby
for i in 0..5
   puts "局部变量的值为 #{i}"
end

# 另一种写法
(0..5).each do |i|
   puts "局部变量的值为 #{i}"
end
```

#### break 语句

终止最内部的循环：

```ruby
for i in 0..5
   if i > 2 then
      break
   end
   puts "局部变量的值为 #{i}"
end
```

#### next 语句

跳到循环的下一个迭代：

```ruby
for i in 0..5
   if i < 2 then
      next
   end
   puts "局部变量的值为 #{i}"
end
```

------

### 块与 yield 语句

块（block）是封装了一段代码的对象，可以将代码块作为参数传递给方法，并在方法内部执行。

它通常具有如下特征：

- 块需要有名称。
- 包含在大括号{} 或 do...end 内。
- 块总是从与它同名的函数中调用。例如块名称为 test，就需要用函数 test 调用块

使用 yield 语句来调用块：

```ruby
def example_method
	puts "方法开始"
	yield
	puts "方法结束"
end

example_method { puts "这是块中的代码" }

# 调用方法
example_method
```

也可以向yield传递参数：

```ruby
def example_method_with_parameters
	yield("Hello", "World")
end

example_method_with_parameters { |str1, str2| puts "#{str1} #{str2}" }

# 调用方法
example_method_with_parameters
```

块本身可以作为方法参数被传递：

```ruby
def method_accepting_block(&block)
	# 调用块
    block.call
end

block_as_parameter = proc { puts "这是作为参数传递的块" }

# 调用方法
method_accepting_block(&block_as_parameter)
```

另外，可以声明当文件被加载时要运行的代码块 BEGIN ，程序完成执行后要运行的代码块 END

```ruby
BEGIN { 
  # BEGIN 代码块
  puts "BEGIN 代码块"
} 
 
END { 
  # END 代码块
  puts "END 代码块"
}
  # MAIN 代码块
puts "MAIN 代码块"
```

这样输出结果会是：

```
BEGIN 代码块
MAIN 代码块
END 代码块
```

------

### Array 与 Hash

#### 数组 Array

Ruby 数组是任何对象的有序整数索引集合，每个元素都可以通过索引获取。

与 Java 一样，索引从 0 开始。

Ruby 中索引可以为负数，-1 表示数组的最后一个元素，-2 表示倒数第二个元素。

Ruby 中数组不需要指定大小，会随着增删元素自动调整。

##### 新建数组

```ruby
# 多种方式创建数组
names = Array.new
names = Array.new(20)    # 设置数组大小
names = Array.new(4, "mac")    # 设置4个元素值为mac
nums = Array.[](1, 2, 3, 4,5)
nums = Array[1, 2, 3, 4,5]
digits = Array(0..9)
```

##### 遍历数组

```ruby
# 常见的遍历数组操作
# 方法1：each
fruits = ['Apple', 'Banana', 'Cherry']
fruits.each do |fruit|
	puts fruit
end

# 方法2：each_with_index：同时获取元素及其索引
fruits = ['Apple', 'Banana', 'Cherry']
fruits.each_with_index do |fruit, index|
puts "#{index}: #{fruit}"
end

# 方法3：map：创建一个新数组，将原数组每个元素映射成新数组的值
numbers = [1, 2, 3]
squared_numbers = numbers.map { |number| number ** 2 #平方计算}
puts squared_numbers

# 方法4：select：选择满足特定条件的所有元素并返回一个新数组
numbers = [1, 2, 3]
even_numbers = numbers.select { |number| number.even? #是否为偶数}
puts even_numbers

# 方法5：reject：与 select 相反，返回不满足条件的元素组成的新数组：
odd_numbers = numbers.reject { |number| number.even? #是否为偶数}
puts odd_numbers
```

#### 哈希 Hash

键值对集合，类似于数组，但索引不局限于使用数字。

Ruby 1.9 之前，Hash 类似于 Java 的 HashMap，元素无序。

Ruby 2.0 之后，Hash 类似于 Java 的 LinkedHashMap, 元素有序。

##### 新建哈希

```ruby
# 使用大括号
my_hash = { 'key1' => 'value1', 'key2' => 'value2' }

# 使用Hash.new
my_hash = Hash.new
my_hash['key1'] = 'value1'
my_hash['key2'] = 'value2'
```

##### 操作哈希

```ruby
# 1. 遍历：
# each：访问每个键值对
# each_key：访问每个键
# each_value：访问每个值
my_hash.each do |key, value|
	puts "#{key}: #{value}"
end

# 2. 获取键或值的数组：
keys_array = my_hash.keys
values_array = my_hash.values
hash[key] = value    # 给对应的键赋值

# 3. 选择和拒绝元素：
# select 和 reject 方法允许你根据条件选择或拒绝哈希中的元素
selected = my_hash.select { |key, value| key == 'key1' }
rejected = my_hash.reject { |key, value| key == 'key1' }

# 4. 合并哈希：
hash1 = { 'a' => 1, 'b' => 2 }
hash2 = { 'b' => 3, 'c' => 4 }
merged_hash = hash1.merge(hash2)


```

------

### 异常

Ruby 可以在 begin/end 块中附上可能抛出异常的代码，并使用 rescue 子句告诉 Ruby 要处理的异常类型。

begin 和 rescue 类似于 Java 的 try catch，ensure 类似于 Java 的finally。

```ruby
def divide_numbers(x, y)
  begin
# 尝试执行的代码
    result = x / y
  rescue ZeroDivisionError => e
# 捕获特定类型的异常
    puts "除数不能为零: #{e.message}"
  rescue StandardError => e
# 捕获其他类型的标准错误
    puts "发生了一个标准错误: #{e.message}"
  else
# 如果没有异常发生则执行
    puts "结果是: #{result}"
  ensure
# 无论是否发生异常都会执行
    puts "这是ensure语句，无论如何都会执行"
  end
end

# 正常情况
divide_numbers(10, 2)
# 异常情况
divide_numbers(10, 0)
```

还可以使用 raise 语句手动抛出异常：

```ruby
# 简单地抛出当前异常
raise 
# 创建一个新的 RuntimeError 异常，设置它的消息为给定的字符串
raise "Error Message" 
# 使用第一个参数创建一个异常，然后设置相关的消息为第二个参数
raise ExceptionType, "Error Message"
# 再上一种形式的基础上，可添加额外的条件语句（比如 unless number_a > 0）来抛出异常
raise ExceptionType, "Error Message" condition
```

------

## Ruby 面向对象

### 基本概念

面向对象的编程语言特性包括：

- 数据封装
- 数据抽象
- 多态性
- 继承

一个面向对象的程序，涉及到的类和对象。类是用于创建对象的模板。比如，自行车是自行车类的一个实例。

以车辆为例，它包括车轮（wheels）、马力（horsepower）、燃油或燃气罐容量（fuel or gas tank capacity）。这些属性形成了车辆（Vehicle）类的数据成员。借助这些属性能把一个车辆从其他车辆中区分出来。

车辆也能包含特定的函数方法，比如暂停（halting）、驾驶（driving）、超速（speeding）。这些函数形成了车辆（Vehicle）类的数据成员。因此可以定义类为属性和函数的组合。

> 与 Java 类似，ruby 的类也包含：①构造函数；②getter、setter；③实例方法；④to_s 方法
>
> 而且因为 ruby 的动态性，实例变量不需要声明，可以直接在各个方法使用

```ruby
class Box
   # 构造器方法
   def initialize(w,h)
      @width, @height = w, h
   end
 
   # getter方法
   def getWidth
      @width
   end
   def getHeight
      @height
   end
 
   # setter方法
   def setWidth=(value)
      @width = value
   end
   def setHeight=(value)
      @height = value
   end
    
   # to_s 方法
   def to_s
      "(w:#@width,h:#@height)"  # 对象的字符串格式
   end
    
   # 实例方法
   def getArea
      @width * @height
   end   
end
 

# 创建对象
box = Box.new(10, 20)
# 使用setter方法
box.setWidth = 30
box.setHeight = 50
# 使用getter方法
x = box.getWidth()
y = box.getHeight()
# 自动调用 to_s 方法
puts "String representation of box is : #{box}"
# 调用实例方法
a = box.getArea()
puts "Area of the box is : #{a}"
```

### 访问控制

与 Java 类似，Ruby 提供了三个级别的实例方法保护：public，private，protected。

Ruby 不在实例和类变量上应用任何访问控制。

- **Public 方法：** Public 方法可被任意对象调用。默认情况下，方法都是 public 的，除了 initialize 方法总是 private 的。
- **Private 方法：** Private 方法不能从类外部访问或查看。只有类方法可以访问私有成员。
- **Protected 方法：** Protected 方法只能被类及其子类的对象调用。访问也只能在类及其子类内部进行。

```ruby
#!/usr/bin/ruby -w
 
# 定义类
class Box
   # 构造器方法
   def initialize(w,h)
      @width, @height = w, h
   end
 
   # 实例方法默认是 public 的
   def getArea
      getWidth() * getHeight
   end
 
   # 定义 private 的访问器方法
   def getWidth
      @width
   end
   def getHeight
      @height
   end
   # make them private
   private :getWidth, :getHeight
 
   # 用于输出面积的实例方法
   def printArea
      @area = getWidth() * getHeight
      puts "Big box area is : #@area"
   end
   # 让实例方法是 protected 的
   protected :printArea
end
 
# 创建对象
box = Box.new(10, 20)
 
# 调用实例方法
a = box.getArea()
puts "Area of the box is : #{a}"
 
# 尝试调用 protected 的实例方法
box.printArea()

# 输出结果：
Area of the box is : 200
test.rb:42: protected method `printArea' called for #
<Box:0xb7f11280 @height=20, @width=10> (NoMethodError)
```

### 类的继承

总的来说与 Java 一致，都不支持传统意义上的多继承，即一个类直接继承多个父类，但都有其他机制实现类似多继承的效果，可以让一个类表现出多个类的特性。

Java 通过接口（Interface）实现类似多继承的功能，一个类可以实现多个接口里的方法。

Ruby 不支持多继承，但通过使用模块（Module）和混入（Mixin）实现相同的效果

```ruby
# 继承
# 定义父类
class Box
   # 构造器方法
   def initialize(w,h)
      @width, @height = w, h
   end
   # 实例方法
   def getArea
      @width * @height
   end
end
 
# 定义子类，继承父类
class BigBox < Box
 
   # 添加一个新的实例方法
   def printArea
      @area = @width * @height
      puts "Big box area is : #@area"
   end
end
 
# 创建对象
box = BigBox.new(10, 20)
 
# 输出面积
box.printArea()
```

### 重写与重载

- Ruby 中，当子类定义了一个与父类同名的方法时，这个方法会覆盖父类的方法。这与Java中的方法重写（method overriding）是相似的概念。

- Ruby 中不支持类似于 Java 里的方法重载，因为 ruby 是动态类型语言。如果定义了两个同名方法，后一个定义会覆盖前一个定义

### 模块 Module 与 Mixin

模块（Module）是一种把方法、类和常量组合在一起的方式。模块实现了 Mixin 装置，提供了独立的命名空间，内部的方法和常量不会与其他位置的方法和常量冲突。

模块类似与类，以大写字母开头，内部定义方法的方式也相似，但它们有以下不同：

- 模块不能实例化
- 模块没有子类

```ruby
# 定义多个函数名称相同但功能不同的模块
module Driving_eco
  POWER = 0.6
  # 类方法
  def self.show_power
    puts "Economic power is #{POWER}!"
  end
  # 实例方法
  def switch_eco
    puts "Switch to economic mode!"
  end
end

module Driving_nor
  POWER = 0.8
  # 类方法
  def self.show_power
    puts "Normal power is #{POWER}!"
  end
  # 实例方法
  def switch_nor
    puts "Switch to normal mode! "
  end
end

# 调用模块的方法和变量
puts Driving_eco::POWER
puts Driving_nor::POWER
Driving_eco.show_power
Driving_nor.show_power
```

#### require 语句

如果想要使用任何已定义的模块，可以用 require 语句来加载模块：

> 这种方式类似于 Java 的 import 语句，C 和 C++ 引入头文件的 include 语句。

```ruby
require filename
```

#### include 语句

将模块 include 到类定义中，模块中的方法就会利用mixin技术，混合进入到了类中。

混合后，类的实例就可以直接调用模块里的实例方法。

```ruby
# 引入 module
class BYD
include Driving_eco
include Driving_nor
end

# 模块类似于 Java 接口，让实例具备多个类的功能特性
car = BYD.new
car.switch_nor;
car.switch_eco;
```
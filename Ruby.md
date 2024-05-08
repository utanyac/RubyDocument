# Ruby 教程

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

Rubymine

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

------

### 变量、方法与类

**Ruby 中有四种类型的变量：**

- **局部变量：**局部变量是在方法中定义的变量，在方法外是不可用的。
- **实例变量：**实例变量以@开头，是实例对象的一个属性，在对象的整个生命周期都是可用的，可以在类的任何实例方法中被访问和修改。**[Ruby的动态性导致需要实例变量]**
- **类变量：**类变量以@@开头，必须初始化。作为类的一个属性，可以跨不同的对象使用。它也被共享在整个继承链中。**[类似于Java中的static静态变量]**
- **全局变量：**类变量不能跨类使用。如果您想要有一个可以跨类使用的变量，您需要定义全局变量。全局变量总是以美元符号（$）开始。
- **常数：**大写字母开头。

------

**Ruby 中方法名必须以小写字母开头，且方法应在调用之前定义：**

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

如果不指定返回值，方法会默认返回最后一个语句的值；也可以通过 return 指定返回值。

------

**Ruby 中类总是以关键字 *class* 开始，后跟类的名称。类名的首字母应该大写。**

可以用库里预定义的 new 方法实例化对象，也可以自定义方法实例化对象：

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

### Ruby 运算符

与 Java 大同小异，不做赘述。

------

### Ruby 条件判断

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

unless式和 if作用相反

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



#### 





















## Ruby 面向对象

面向对象的编程语言特性包括：

- 数据封装
- 数据抽象
- 多态性
- 继承

一个面向对象的程序，涉及到的类和对象。类是个别对象创建的蓝图。在面向对象的术语中，您的自行车是自行车类的一个实例。

以车辆为例，它包括车轮（wheels）、马力（horsepower）、燃油或燃气罐容量（fuel or gas tank capacity）。这些属性形成了车辆（Vehicle）类的数据成员。借助这些属性您能把一个车辆从其他车辆中区分出来。

车辆也能包含特定的函数，比如暂停（halting）、驾驶（driving）、超速（speeding）。这些函数形成了车辆（Vehicle）类的数据成员。因此，您可以定义类为属性和函数的组合。
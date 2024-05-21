# Rails 教程

[TOC]



## Rails 简介

### 学习目标

通过 hello world 项目以及基础的 crud 操作演示，掌握 ruby on rails 的入门操作。

最后能够读项目的业务代码，理清 mvc 架构，在已有项目框架的基础上进行补充性质的开发。

如果想要详细学习，规范开发，参见官方文档：https://ruby-china.github.io/rails-guides/v4.1/getting_started.html

## Rails 环境搭建

更建议在linux环境搭建 Rails 项目，可以避免很多安装和配置上的问题。

```shell
ruby -v
gem -v
# 可以指定 rails 版本安装
gem install rails -v 4.2.2
# 创建以 mysql 为数据库的rails项目，会自动执行 bundle install 命令安装所需依赖，前提是有mysql环境
rails new projectxxx -d mysql
# 运行
rails server
```

## Rails 基础

### 目录结构和约定

| 文件/文件夹     | 作用                                                     |
| :-------------- | :------------------------------------------------------- |
| `app/`          | 应用的核心文件，包含模型、视图、控制器和辅助方法         |
| `app/assets`    | 应用的资源文件，例如层叠样式表（CSS）、JavaScript 和图片 |
| `bin/`          | 可执行二进制文件                                         |
| `config/`       | 应用的配置                                               |
| `db/`           | 数据库相关的文件                                         |
| `doc/`          | 应用的文档                                               |
| `lib/`          | 代码库模块文件                                           |
| `lib/assets`    | 代码库的资源文件，例如 CSS、JavaScript 和图片            |
| `log/`          | 应用的日志文件                                           |
| `public/`       | 公共（例如浏览器）可访问的文件，例如错误页面             |
| `bin/rails`     | 生成代码、打开终端会话或启动本地服务器的程序             |
| `test/`         | 应用的测试                                               |
| `tmp/`          | 临时文件                                                 |
| `vendor/`       | 第三方代码，例如插件和 gem                               |
| `vendor/assets` | 第三方资源文件，例如 CSS、JavaScript 和图片              |
| `README.rdoc`   | 应用简介                                                 |
| `Rakefile`      | 使用 `rake` 命令执行的实用任务                           |
| `Gemfile`       | 应用所需的 gem                                           |
| `Gemfile.lock`  | gem 列表，确保这个应用的副本使用相同版本的 gem           |
| `config.ru`     | [Rack 中间件](http://rack.github.io/)的配置文件          |
| `.gitignore`    | Git 忽略的文件                                           |

创建完一个新的 Rails 应用后，下一步是使用 Bundler 安装和包含该应用所需的 gem。之前执行 `rails new` 命令时会自动运行 Bundler。如果修改应用默认使用的 gem，需要再次运行 Bundler。这与 Java 中修改依赖需要执行`mvn install`差不多。

#### 控制依赖版本

依赖记录在Gemfile下（类似于Java里的pom）。如果没有指定版本号，会自动下载。

```ruby
gem 'sqlite3'    # 下载最新版本
gem 'uglifier', '>= 1.3.0'    # 安装的版本号要大于或等于 1.3.0
gem 'coffee-rails', '~> 4.0.0'   # 安装版本号大于 4.0.0，但小于 4.1
```

再次执行`bundle install`就可以下载依赖。

------

### 实现Hello World

控制器可用控制器生成器创建，想要个名为“welcome”的控制器和一个名为“index”的动作，如下所示：

```ruby
rails generate controller welcome index  # 生成器创建一个包含“index”动作的“Welcome”控制器：
```

运行上述命令后，Rails 会生成很多文件，以及一个路由。

```shell
create  app/controllers/welcome_controller.rb
 route  get 'welcome/index'
invoke  erb
create    app/views/welcome
create    app/views/welcome/index.html.erb
invoke  test_unit
create    test/controllers/welcome_controller_test.rb
invoke  helper
create    app/helpers/welcome_helper.rb
invoke  assets
invoke    coffee
create      app/assets/javascripts/welcome.js.coffee
invoke    scss
create      app/assets/stylesheets/welcome.css.scss
```

在这些文件中，控制器位于 `app/controllers/welcome_controller.rb`，视图位于 `app/views/welcome/index.html.erb`。

打开 `app/views/welcome/index.html.erb` 文件，删除全部内容，写入：

```ruby
<h1>Hello World</h1>
```

设置路由：打开 `config/routes.rb` 文件

```ruby
Rails.application.routes.draw do
  get 'welcome/index'
 
  # The priority is based upon order of creation:
  # first created -> highest priority.
  #
  # You can have the root of your site routed with "root"
  # root 'welcome#index'
  #
  # ...
```

这是程序的路由文件，使用特殊的 DSL（domain-specific language，领域专属语言）编写，告知 Rails  请求应该发往哪个控制器和动作。文件中有很多注释，举例说明如何定义路由。其中有一行说明了如何指定控制器和动作设置网站的根路由。找到以 `root` 开头的代码行，去掉注释：

```ruby
root 'welcome#index'
```

`root 'welcome#index'` 告知 Rails，访问程序的根路径时，交给 `welcome` 控制器中的 `index` 动作处理。`get 'welcome/index'` 告知 Rails，访问 http://localhost:3000/welcome/index 时，交给 `welcome` 控制器中的 `index` 动作处理。`get 'welcome/index'` 是运行 `rails generate controller welcome index` 时生成的。

如果生成控制器时停止了服务器，请再次启动（`rails server`），然后在浏览器中访问 http://localhost:3000。你会看到之前写入 `app/views/welcome/index.html.erb` 文件的内容，说明新定义的路由把根目录交给 `WelcomeController` 的 `index` 动作处理了，而且也正确的渲染了视图。

Rails 中，默认情况下，控制器方法会渲染`views 目录下`与 `控制器同名目录下`的`同方法名erb页面模板`，例如：

更详细的默认渲染可以参照本文：视图详解 - 操作举例 - 默认的渲染行为

```
controller： hello
function： hello_world
将会渲染： views/hello/hello_world.html.erb
```

------

### MVC 架构

即 “模型-视图-控制器” 架构，与 springMVC 类似。

- 模型在 app-models 目录下，类似于 springboot 里的 entity 实体类。

- 控制器在 app-controllers 目录下，类似于 springboot 里的controller。

- 视图在 app-views 目录下，类似于springboot 里resources/templates 下的 html 页面。

默认情况下，视图使用 eRuby（嵌入式 Ruby）语言编写，经由 Rails 解析后，再发送给用户。

没有 service，mapper 等文件。

------

### 路由

#### 路由的作用

Rails 路由能识别 URL，将其分发给控制器的动作进行处理，还能生成路径和 URL，无需直接在视图中硬编码字符串。

#### URL 与路由的关系

Rails 程序收到如下请求时：

```ruby
GET /patients/17
```

会查询路由，找到匹配的控制器动作。如果首个匹配的路由是：

```ruby
get '/patients/:id', to: 'patients#show'
```

那么这个请求就交给 `patients` 控制器的 `show` 动作处理，并把 `{ id: '17' }` 传入 `params`。

（这个过程就像 springboot 中的 @RequestMapping("/xxx/yyy")）

Rails 底层预设的 params 既可以获取请求体中的数据，也可以获取url中的数据。

**比如 url 中的 id 可以用 params[:id] 获取，请求体中的数据可以用 params["data"]获取。**

(类似于 Springboot 的 @RequestBody 和 @RequestParam)

#### 生成路径和 URL

通过路由可以生成路径和 URL。把路由修改成：

```ruby
get '/patients/:id', to: 'patients#show', as: 'patient'
```

as 表示的就是这个路由的名称了，可以用这个名称在 erb 模板直接调动路由触发控制器，这样就避免了硬编码 url。

在 patient 的 show 控制器中有如下代码：

```ruby
@patient = Patient.find(17)
```

在相应的视图中有如下代码：

```ruby
# 这里的 patient_path 是一个路由帮助方法，它是由 as: 'patient' 这行代码在路由文件中定义的。
# 当在路由中使用 as: 选项时，Rails 会为该路由创建一个帮助方法，名称为 patient_path。
# 这个帮助方法接受一个参数（在这个例子中是 @patient 对象），并生成一个到该资源的路径。
# Patient Record 是链接显示的文本
<%= link_to 'Patient Record', patient_path(@patient) %>
```

那么路由就会生成路径 `/patients/17`。这么做代码易于维护，如果以后路径发生变化，只需要更新路由文件，不需要一个一个去找。

在这个路由帮助方法中无需指定 ID。Rails 框架通过`约定优于配置的`原则来简化开发过程。在这个原则下，如果遵循 Rails 的默认约定来命名和组织代码，那么框架会自动为你完成很多工作，包括生成路径和URL。

在当前路由中，`:id`是一个动态段，表示任何传入`/patients/:id`路径的数字都会被视为ID参数。Rails 知道这个参数应该用来查找特定的资源实例，比如一个病人记录。

所以，在视图中使用`patient_path(@patient)`时，Rails会自动从`@patient`对象中提取ID，并将其插入到路径中，替换掉`:id`部分。不需要显式地在方法中指定id，Rails已经根据代码和路由配置知道如何处理它。

这种自动化的行为是Rails的一部分，旨在减少重复代码并使开发更加直观。这也是为什么只需要传递对象本身给方法，而不是对象的各个属性，比如ID。这样做的好处是，代码更加简洁，易于阅读和维护。如果遵循Rails的约定，就可以利用这些自动化的特性来提高开发效率。

> 视图里调用路由的写法也有很多种，建议配合 ai 问答，按照自己的节奏学习。

#### 资源路由

使用资源路由可以快速声明资源式控制器所有的常规路由，无需分别为 `index`、`show`、`new`、`edit`、`create`、`update` 和 `destroy` 动作分别声明路由，只需一行代码就能搞定。

对于特定的 HTTP 方法，例如 `GET`、`POST`、`PATCH`、`PUT` 和 `DELETE`。每个方法对应对资源的一种操作。资源路由会把一系列相关请求映射到单个路由器的不同动作上。

例如，如果 Rails 程序收到如下请求：

```ruby
DELETE /users/17
```

会查询路由将其映射到一个控制器的路由上。如果首个匹配的路由是：

```ruby
resources :users
```

那么这个请求就交给 `users` 控制器的 `destroy` 方法处理，并把 `{ id: '17' }` 传入 `params`。

------

资源式路由把 HTTP 方法和 URL 映射到控制器的动作上，还映射到数据库的 CRUD 操作上。

例如，刚刚的 `users`路由单行声明，会自动创建七个不同的默认路由，全部映射到 `Users` 控制器上：

| HTTP 方法 | 路径            | 控制器#动作   | 作用                     | 帮助方法名称         |
| --------- | --------------- | ------------- | ------------------------ | -------------------- |
| GET       | /users          | users#index   | 显示所有 user            | users_path           |
| GET       | /users/new      | users#new     | 显示新建 user 的表单     | bew_user_path        |
| POST      | /users          | users#create  | 新建 user                | users_path配合post   |
| GET       | /users/:id      | users#show    | 显示指定 user            | users_path(user)     |
| GET       | /users/:id/edit | users#edit    | 显示编辑指定 user 的表单 | edit_user_path(user) |
| PATCH/PUT | /users/:id      | users#update  | 更新指定 user            | users_path配合put    |
| DELETE    | /users/:id      | users#destroy | 删除指定 user            | users_path配合delete |

声明资源式路由后，会自动创建帮助方法，例如

- `users_path` 返回 `/users`
- `new_user_path` 返回 `/users/new`
- `edit_user_path(:id)` 返回 `/users/:id/edit`，例如 `edit_user_path(10)` 返回 `/users/10/edit`
- `user_path(:id)` 返回 `/users/:id`，例如 `user_path(10)` 返回 `/users/10`

这些帮助方法都有对应的 `_url` 形式，例如 `users_url`，返回主机、端口加路径。可以用`rake routes`命令查看当前项目的路由的对应关系。

#### 在资源路由里添加自定义路由

了解资源路由下的两个概念：

•  成员路由（Member Routes） 会自动为你提供一个包含资源 ID 的路径。当你在 resources 块中定义一个成员路由时，Rails 会生成一个路径，该路径会将你定义的路径部分附加到 /resources/:id/ 后面。这意味着成员路由是针对特定的资源实例。

•  集合路由（Collection Routes） 则不包含资源 ID。当你在 resources 块中定义一个集合路由时，Rails 会生成一个路径，该路径仅包含你定义的路径部分，附加到 /resources/ 后面。这意味着集合路由是针对资源的整个集合。  

```ruby
# 声明标准的资源路由
  resources :users do
    member do
      # url相当于/users/:id/json,erb 调用需要写 get_single_user_json_user_path(@user)
      get 'json', to: 'users#get_user_json', as: 'get_single_user_json'
    end

    collection do
      # url相当于/users/search/old_users,erb 调用需要写 search_all_old_users_users_path
      get 'search/old_users', to: 'users#search_old_user', as: 'search_all_old_users'
    end
  end
```

#### 路由的其他相关内容

如果只是为了开发，掌握最开始的as用法就能上手了。

如果不忌讳用硬编码，甚至可以不用as，直接写路径，但这违背了 Rails 的思想。

路由的约定写法细节很多，想要全部掌握需要花费些时间系统学习：https://www.kancloud.cn/wizardforcel/rails-guides/151974

------

### Scaffold 脚手架与数据库迁移

Rails 可以利用 scaffold 来创建脚手架，包含 Model，View，Controller，Migration（数据库迁移文件），Routes。

例如，如果想为 Article 创建一个脚手架，可以运行：

```shell
rails generate scaffold Article title:string body:text
```

这样对应的 mvc 架构就会被直接创造出来，里面包含了最基本的CRUD操作所需要的所有路由/控制器方法/视图模板。

但通常需要根据需求自定义生成的代码，建议仅在项目初期使用，更多的要手动去编写。

**着重了解以下操作中数据库迁移的方法：**

在配置好数据库连接的基础上：

```ruby
# 编辑config/database.yml
# 有很多种写法
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: root
  host: 192.168.12.58
  port: 3306

development:
  <<: *default
  database: mytest
```

创建一个数据模型，以创建一个名为 User 的模型为例：

```shell
rails generate scaffold User name:string age:integer
```

这时候会发现，controller，helper，model，view 等都被创建出来了：

```ruby
      invoke  active_record
      create    db/migrate/20240513013425_create_users.rb  #数据库迁移文件
      create    app/models/user.rb  # 实体类文件
      invoke    test_unit
      create      test/models/user_test.rb   
      create      test/fixtures/users.yml
      invoke  resource_route
       route    resources :users	# 资源路由
      invoke  scaffold_controller
      create    app/controllers/users_controller.rb  # 控制器
      invoke    erb  # 对应的各个页面模板
      create      app/views/users
      create      app/views/users/index.html.erb
      create      app/views/users/edit.html.erb
      create      app/views/users/show.html.erb
      create      app/views/users/new.html.erb
      create      app/views/users/_form.html.erb
      invoke    test_unit
      create      test/controllers/users_controller_test.rb
      create      test/system/users_test.rb
      invoke    helper  
      create      app/helpers/users_helper.rb  # helper，写一些便于页面使用的组件
      invoke      test_unit
      invoke    jbuilder
      create      app/views/users/index.json.jbuilder
      create      app/views/users/show.json.jbuilder
      create      app/views/users/_user.json.jbuilder
      invoke  assets  # 对应的 js 和 css
      invoke    coffee
      create      app/assets/javascripts/users.coffee
      invoke    scss
      create      app/assets/stylesheets/users.scss
      invoke  scss
      create    app/assets/stylesheets/scaffolds.scss
```

简单看一下 controller 文件，用 postman 和页面去测试这些请求，配合控制台输出日志可以看到 sql 语句（实际我们开发时可能并不需要完全参照自动模板去做）。但是具体如何配合模板去实现渲染数据还要在后面详细介绍。

```ruby
class UsersController < ApplicationController
  before_action :set_user, only: %i[ show edit update destroy ]

  # GET /users or /users.json
  def index
    @users = User.all
  end

  # GET /users/1 or /users/1.json
  def show
  end

  # GET /users/new
  def new
    @user = User.new
  end

  # GET /users/1/edit
  def edit
  end

  # POST /users or /users.json
  def create
    @user = User.new(user_params)

    respond_to do |format|
      if @user.save
        format.html { redirect_to user_url(@user), notice: "User was successfully created." }
        format.json { render :show, status: :created, location: @user }
      else
        format.html { render :new, status: :unprocessable_entity }
        format.json { render json: @user.errors, status: :unprocessable_entity }
      end
    end
  end

  # PATCH/PUT /users/1 or /users/1.json
  def update
    respond_to do |format|
      if @user.update(user_params)
        format.html { redirect_to user_url(@user), notice: "User was successfully updated." }
        format.json { render :show, status: :ok, location: @user }
      else
        format.html { render :edit, status: :unprocessable_entity }
        format.json { render json: @user.errors, status: :unprocessable_entity }
      end
    end
  end

  # DELETE /users/1 or /users/1.json
  def destroy
    @user.destroy

    respond_to do |format|
      format.html { redirect_to users_url, notice: "User was successfully destroyed." }
      format.json { head :no_content }
    end
  end

  private
    # Use callbacks to share common setup or constraints between actions.
    def set_user
      @user = User.find(params[:id])
    end

    # Only allow a list of trusted parameters through.
    def user_params
      params.require(:user).permit(:name, :age)
    end
end
```

但这时数据库里还没有表。执行数据库迁移以创建表：

```shell
rails db:migrate

== 20240513013425 CreateUsers: migrating ======================================
-- create_table(:users)
   -> 0.2724s
== 20240513013425 CreateUsers: migrated (0.2744s) =============================
```

去检查数据库，发现多了`users`表，包含`id，name，age，created_at，updated_at`字段。

它实际对应 db/migrate 目录下的迁移文件 20240513013425_create_users.rb：

```ruby
class CreateUsers < ActiveRecord::Migration[5.2]
  def change
    create_table :users do |t|
      t.string :name
      t.integer :age

      t.timestamps
    end
  end
end
```

假如对刚刚的操作不满意，比如想给`users`添加其它字段,也可以回退：  

```shell
rake db:rollback

== 20240513013425 CreateUsers: reverting ======================================
-- drop_table(:users)
   -> 0.1914s
== 20240513013425 CreateUsers: reverted (0.2223s) =============================
```

此时数据库里的 users 表已经消失了。这个操作只是影响数据库，不会影响其他文件。

打开`/db/migrate/20240513013425_create_users.rb`，修改代码，添加`t.string :email`

```ruby
class CreateUsers < ActiveRecord::Migration[5.2]
  def change
    create_table :users do |t|
      t.string :name
      t.integer :age
      t.string :email

      t.timestamps
    end
  end
end
```

保存文件再次执行数据库迁移，生成新表：

```shell
rails db:migrate

== 20240513013425 CreateUsers: migrating ======================================
-- create_table(:users)
   -> 0.1997s
== 20240513013425 CreateUsers: migrated (0.2004s) =============================
```

会发现 users 表回来了，并且多了email 字段。

同时要注意 schema_migrations 表，它用于跟踪数据库中已经执行的迁移（migrations）。这个表的主要作用是记录哪些迁移已经被应用到数据库中，帮助Rails维护数据库的当前架构版本，并管理数据库的状态，确保迁移的顺序和完整性。

每当执行一个新的迁移时，Rails会将该迁移的版本号添加到schema_migrations表中。这个版本号通常是迁移文件名的时间戳部分。

当运行rake db:migrate命令时，Rails会检查schema_migrations表，确定哪些迁移尚未运行，并按照版本号的顺序执行它们。这样可以确保所有的迁移都按照创建的顺序被应用，而且每个迁移只会被执行一次。

如果需要回滚迁移，Rails也会使用schema_migrations表来确定哪些迁移需要被回滚。通过这种方式，schema_migrations表充当了数据库架构历史的记录者，确保数据库架构的变更可以被可靠地跟踪和管理。

> 在本组开发中，因客户要求有些位置不允许放入新代码，所以我们无法完全参照正规流程创建这些代码，而是手动创建需要的各个类文件，放到单独的插件目录下。但是数据库迁移部分还是要按照正规流程去做的。

------

### 基础 CRUD 操作

#### 命名约定

创建 Model 时需要遵守以下约定：

- 数据库表名：复数，下划线分隔单词（例如 book_clubs）
- 模型类名：单数，每个单词的首字母大写（例如 BookClub）

无特殊需求时，创建好类之后，尽管类中没有任何内容，但是已经可以用数据库同名字段作为模型属性进行 crud 了。

也可以使用 alias 给各个数据库字段映射别名。

> 如果用脚手架创建就不需要去关注这些约定，如果手动创建各个文件则需要严格遵守，否则无法识别

#### Active Record

Active Record 是 MVC 中的 M（模型），负责处理数据和业务逻辑。Active Record 负责创建和使用需要持久存入数据库中的数据。Active Record 实现了 Active Record 模式，是一种对象关系映射系统。

##### 创建Active Record 模型

创建一个继承ApplicationRecord类的类，而ApplicationRecord 继承 ActiveRecord::Base，后者定义了一系列有用的方法。

> 就像 MybatisPlus，JPA等框架，封装了一系列方法，简化开发不必手写SQL语句了

```ruby
class Product < ApplicationRecord
end
```

##### 新增

```ruby
# 直接创建一条数据在对应的表中，并且保存
模型名.create(字段名: '...',...)

# new 方法实例化一个新对象，但不保存：
# 例子： 
user = User.new
user.name = "David"
user.occupation = "Code Artist"
user.save  # 调用save就可以保存到数据库了
```

##### 删除

```ruby
user = User.find_by(name: 'David')
user.destroy
```

##### 更新

```ruby
# 简单的更新
user = User.find_by(name: 'David')
user.name = 'Dave'
user.save # 调用save就可以保存到数据库了，会自动判断此操作是 update 还是 create

# 用update方法
user = User.find_by(name: 'David')
user.update(name: 'Dave')

# 批量修改
User.update_all "name = 'cwh'"
```

##### 查询

```ruby
# 返回所有用户组成的集合
users = User.all

# 返回第一个用户
user = User.first

# 返回第一个名为 David 的用户
david = User.find_by(name: 'David')
```

> 注意：记录的 update/create 时间是无需自己操作的，在表里新建 created_on 与 updated_on 的 datatime 字段，就能实现自动录入这两个时间

------

### 模型关联与进阶 CRUD 操作

使用 Active Record 关联可以轻易的建立两个模型之间的关系。

比如，要做一个 blog 项目，评论（comment ）和文章（article）之间的关联是这样的：

- **评论** 属于 **一篇文章**（一对一）
- **一篇文章** 有多个 **评论**（一对多）

声明评论属于文章：

```ruby
class Comment < ActiveRecord::Base
  belongs_to :article
end
```

另一边：

```ruby
class Article < ActiveRecord::Base
  has_many :comments
end
```

这两行声明能自动完成很多操作。例如，如果实例变量 `@article` 是一个文章对象，可以使用 `@article.comments` 取回一个数组，其元素是这篇文章的评论。

在 Active Record 关联中，约定也体现在关联名称的单复数使用上：

•  一对一关系（如 belongs_to 和 has_one 关联）使用单数形式。例如，belongs_to :user 表示一个对象属于一个用户。

•  一对多关系（如 has_many 关联）使用复数形式。例如，has_many :comments 表示一个对象拥有多个评论。

这些约定使得代码更加直观，因为它们反映了实际的数据模型关系。当看到 has_many :items 时，可以立即理解一个对象可以有多个 items。同样，当您看到 belongs_to :category 时，您知道一个对象只属于一个 category。

*（项目实际代码展示）*

> 注意：这两种关系通常一起声明，但这并不是必须的
>
> •  在 Comment 模型中，使用 belongs_to :article ，会为 Comment 对象提供方法，如 comment.article，来访问它所属的文章。
>
> •  在 Article 模型中，使用 has_many :comments ，会为 Article 对象提供方法，如 article.comments，来访问它的所有评论。
>
> 如果只是声明单边，比如如果没有 has_many :comments，就不能通过 article.comments 来获取一篇文章的所有评论。但是依然可以 comment.article 来访问它所属的文章。

#### 修改已有数据结构实现关联

刚刚的操作是新建两个有关联的表和实体类，如果对已有的两个表和实体类增加关联关系，该如何操作呢？

答案是使用Rails的迁移机制来添加外键字段。例如，如果一个用户属于一个部门（多对一关系），可以生成一个迁移文件来给`users`表添加`department`外键字段来实现关联：

```ruby
# 添加外键字段
# rails generate migration: 这是告诉Rails生成一个新的迁移文件的命令。
# AddDepartmentToUsers: 给迁移文件指定的名字。它通常遵循一个约定，即以动词+对象的形式来描述想要进行的操作
# department_id:references: 这指定了你想要添加到users表的新列的类型和名称。department是新列的名称，而:references类型意味着这个列将被设置为一个外键，用于引用另一个表
rails generate migration AddDepartmentToUsers department:references

# 运行结果
invoke  active_record
create    db/migrate/20240513055834_add_department_to_users.rb

# 执行命令后，新增的db迁移文件 db\migrate\20240513055834_add_department_to_users.rb
class AddDepartmentToUsers < ActiveRecord::Migration[5.2]
  def change
    add_reference :users, :department, foreign_key: true
  end
end

# 执行更新表的命令
# 发现 users 表多了 department_id 字段
rails db:migrate
```

#### 代码举例

controller：

```ruby
# 将新增的控制器加入到before action，会在调用前读取params中的数据，为初始化实例对象
before_action :set_user, only: %i[ show edit update destroy show_user_department ]

# 新增的关联查询
  def show_user_department
      @department = @user.department
    render json: @department
  end
```

route:

```ruby
get 'department_of_user/:id', to: 'users#show_user_department', as: 'show_user_department'
```

erb（具体工作流程参看 视图详解）：

```ruby
  <tbody>
    <% @users.each do |user| %>
      <tr>
        <td><%= user.name %></td>
        <td><%= user.age %></td>
        <td><%= link_to 'Show', user %></td>
        <td><%= link_to 'Edit', edit_user_path(user) %></td>
        <td><%= link_to 'Destroy', user, method: :delete, data: { confirm: 'Are you sure?' } %></td>
        # 调用新建的 controller，会跳转显示json
        <td><%= link_to 'Show User Department', show_user_department_path(user.id) %></td>
      </tr>
    <% end %>
  </tbody>
```

------

### 视图详解

Rails 的模板功能强大，非常灵活，对于每个原生的前端组件都准备自己的一套模板，不是短时间内就能完全掌握的，需要投入精力去学习。

官方详细教程：https://ruby-china.github.io/rails-guides/layouts_and_rendering.html

如果实在时间紧，只要掌握了模板的基本架构走向，就可以混用原生的 html，css，js ，可以不优雅地完成开发（但不推荐）。

#### 什么是 ERB

Rails 默认的模版是 ERB 模版，在 Rails 的 后端 controller 中，通过 render 渲染模版。

ERB 模版和普通的 HTML 模版的差别就是，ERB 模版支持插入 Ruby 代码，包含逻辑代码和表达式。

在所有的模板中，都能够访问当前view 或action的所有实例变量。

比如下面的 `views\users\index.html.erb`：

```ruby
<p id="notice"><%= notice %></p>

<h1>Users</h1>

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Age</th>
      <th colspan="3"></th>
    </tr>
  </thead>

  <tbody>
    <% @users.each do |user| %>
      <tr>
        <td><%= user.name %></td>
        <td><%= user.age %></td>
        <td><%= link_to 'Show', user %></td>
        <td><%= link_to 'Edit', edit_user_path(user) %></td>
        <td><%= link_to 'Destroy', user, method: :delete, data: { confirm: 'Are you sure?' } %></td>
      </tr>
    <% end %>
  </tbody>
</table>

<br>

<%= link_to 'New User', new_user_path %>
```

**控制器的实例变量是如何传递到视图的？**

通过Rails的渲染机制来完成的。当控制器的一个动作（比如index或show）被调用时，它会处理请求并设置一些实例变量。

然后，默认情况下，Rails会自动将这些实例变量传递给与动作同名的视图模板。(如果指定render渲染了别的视图模板，那控制器会自动把所有的实例变量传递给渲染的视图)

例如，如果你有一个UsersController和一个名为index的动作，像这样：

```ruby
class UsersController < ApplicationController
	def index
		@users = User.all
	end
end
```

这里，`@users`实例变量被设置为包含所有用户记录的集合。当`index`动作结束时，Rails会渲染`index.html.erb`视图模板，并将`@users`实例变量传递给它。

不是所有的实例变量都会被传递，只有在控制器动作中定义的实例变量才会被传递到视图。这意味着，如果你在控制器中没有设置一个实例变量，那么它不会在视图中可用。

另外，如果一个控制器的动作只是渲染 JSON 数据，而不是渲染 HTML 页面，那么它通常不会使用ERB 模板。在这种情况下，控制器动作会直接返回 JSON 格式的数据，而不是通过实例变量传递数据给视图模板。例如，如果有一个控制器动作：

```ruby
def show
	@user = User.find(params[:id])
	render json: @user
end
```

这个 `show` 动作会找到一个用户实例，并使用`render json:`直接将`@user`对象转换为 JSON 格式响应。这种响应通常用于API接口，供前端 JavaScript 框架或其他客户端使用，而不是传递给ERB模板。如果既需要渲染JSON数据，又需要渲染HTML页面，可以在控制器中设置条件渲染，例如:

```ruby
def show
	@user = User.find(params[:id])

	respond_to do |format|
		format.html # 默认渲染对应的ERB模板
		format.json { render json: @user }
	end
end
```

这样，根据请求的格式，控制器会决定是渲染 HTML 页面还是 JSON 数据。当请求 HTML 格式时，`@user`实例变量会被传递到`show.html.erb`模板；当请求 JSON 格式时，会直接渲染 JSON 数据。这是 Rails 中处理不同响应格式的常见方式。

再演示下渲染非默认的视图模板：

```ruby
  def show_user_department
      @department = @user.department
    render "departments/show"
    # 注意：虽然没有传递实例变量，但是 rails 的 erb 可以直接使用对应控制器动作里的实例变量
    # 所以实际上 部门 的信息已经传递过去了
  end
```

#### 操作举例

##### 创建响应

-   **调用 `render` 方法，向浏览器发送一个完整的响应（最常用）；**
-   调用 `redirect_to` 方法，向浏览器发送一个 HTTP 重定向状态码；
-   调用 `head` 方法，向浏览器发送只含 HTTP 首部的响应；

##### 使用 render 

多数情况下，`render` 方法负责渲染应用的内容。`render` 方法的行为有多种定制方式，可以渲染 Rails 模板的默认视图、指定的模板、文件、行间代码或者什么也不渲染。渲染的内容可以是文本、JSON 或 XML。而且还可以设置响应的内容类型和 HTTP 状态码。

```ruby
# 染同个控制器中的其他模板，可以把视图的名字传给 render 方法
# 如果调用 update 失败，update 动作会渲染同个控制器中的 edit.html.erb 模板。
def update
  @book = Book.find(params[:id])
  if @book.update(book_params)
    redirect_to(@book)
  else
    render "edit"
  end
end

# 也可使用符号指定要渲染的动作：
def update
  @book = Book.find(params[:id])
  if @book.update(book_params)
    redirect_to(@book)
  else
    render :edit
  end
end
```

如果想渲染其他控制器中的模板，可以用 `render` 方法，指定模板的完整路径（相对于 `app/views`）。

例如，如果控制器 `AdminProductsController` 在 `app/controllers/admin` 文件夹中，可使用下面的方式渲染 `app/views/products` 文件夹中的模板：

```ruby
render "products/show"
```

`render`方法也可以渲染文本，html，json，xml 等其他格式：

```ruby
# 渲染 html
render html: "<strong>Not Found</strong>".html_safe
# 渲染 json，xml
# Rails 支持把对象转换成 JSON，经渲染后再发送给浏览器。
# 在需要渲染的对象上无需调用 to_json 或 to_xml 方法。render 方法会自动调用它们
render json: @product
render xml: @product
# Rails 能渲染普通的 JavaScript
render js: "alert('Hello Rails');"
```

Rails 会自动为生成的响应附加正确的 HTTP 状态码（大多数情况下是 `200 OK`）。使用 `:status` 选项可以修改状态码：

```ruby
render status: 500
render status: :forbidden
```

#### 局部视图

- 局部视图把渲染过程分为多个管理方便的片段，把响应的某个特殊部分移入单独的文件。
- 局部视图的文件名以下划线开头，以便和普通视图区分开，不过引用时无需加入下划线。即便从其他文件夹中引入局部视图

```ruby
<h1>New User</h1>

<%= render 'form', user: @user %>

<%= link_to 'Back', users_path %>
```

`render` 表示渲染视图，他会优先渲染同目录下的`_form.html.erb`，同时传递了`user`变量到局部视图中。

对应的局部视图可以使用`user`去操作数据：

```ruby
<%= form_with(model: user, local: true) do |form| %>
  <% if user.errors.any? %>
    <div id="error_explanation">
      <h2><%= pluralize(user.errors.count, "error") %> prohibited this user from being saved:</h2>

      <ul>
      <% user.errors.full_messages.each do |message| %>
        <li><%= message %></li>
      <% end %>
      </ul>
    </div>
  <% end %>

  <div class="field">
    <%= form.label :name %>
    <%= form.text_field :name %>
  </div>

  <div class="field">
    <%= form.label :age %>
    <%= form.number_field :age %>
  </div>

  <div class="actions">
    <%= form.submit %>
  </div>
<% end %>

```

#### Rails 中使用 Javascript，CSS

参考教程：[在 Rails 中使用 JavaScript — Ruby on Rails 指南 (ruby-china.github.io)](https://ruby-china.github.io/rails-guides/v4.1/working_with_javascript_in_rails.html)

##### 内建 JS组件

正常来说需要 JS 的介入才能完成 ajax 异步请求，但是 Rails 只需要在模版中添加特定的属性，就可以得到相应的功能。

下面展示一个发送 Ajax 请求的例子：

```html
<form action="/things" method="post" data-remote="true">
  <!-- form fields -->
  <input type="submit" value="Create">
</form>
```

 Rails 做了一些 JS 的替代功能，比如发送 Ajax  请求。但是有很多很多的动态功能是 Rails 没有提供，也不能完成的。所以在实际的项目中需要单独引入 JS 的框架，比如React， Vue 。实际上也可以完全不使用 erb 自带的这些功能，完全用  JQuery 完成开发。

##### 约定引入 JS 与 CSS

在用脚手架生成基础文件时，这两种类型的文件放到了`app/assets`下的`javascripts`目录、`stylesheets`目录。而`app\views\layouts\application.html.erb`会引入这些总的js css文件：

```ruby
<!DOCTYPE html>
<html>
  <head>
    <title>Rails003</title>
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
  </head>

  <body>
    <%= yield %>
  </body>
</html>
```

这些文件里`require`了JavaScript目录和stylesheets目录，之后只需要继续再现有目录下添加对应的`.coffee` 和`.scss`就可以实现对视图的控制了：

```ruby
//= require rails-ujs
//= require activestorage
//= require turbolinks
//= require_tree .
    
 *
 *= require_tree .
 *= require_self
 */
```






















---
title: ThinkPHP数据模型相关操作
date: 2016-02-18 11:18:30
tags: 
- PHP
description: ThinkPHP框架添加、读取、更新数据记录

---

# 目录结构

* 初始目录结构

```
www  WEB部署目录（或者子目录）
├─index.php       入口文件
├─README.md       README文件
├─Application     应用目录，默认为空。第一次访问入口文件会自动生成，参考后面的入口文件部分。
├─Public          资源文件目录
└─ThinkPHP        框架目录
```

* 框架目录ThinkPHP

```
├─ThinkPHP 框架系统目录（可以部署在非web目录下面）
│  ├─Common       核心公共函数目录
│  ├─Conf         核心配置目录 
│  ├─Lang         核心语言包目录
│  ├─Library      框架类库目录
│  │  ├─Think     核心Think类库包目录
│  │  ├─Behavior  行为类库目录
│  │  ├─Org       Org类库包目录
│  │  ├─Vendor    第三方类库目录
│  │  ├─ ...      更多类库目录
│  ├─Mode         框架应用模式目录
│  ├─Tpl          系统模板目录
│  ├─LICENSE.txt  框架授权协议文件
│  ├─logo.png     框架LOGO文件
│  ├─README.txt   框架README文件
│  └─ThinkPHP.php    框架入口文件

```

上述应用的目录结构只是默认设置，事实上，在实际部署应用的时候，建议除了应用入口文件和Public资源目录外，其他文件都放到非WEB目录下面，具有更好的安全性。


# 添加记录

```
$user = M('User');
$data['username'] = 'ThinkPHP';
$data['email'] = 'ThinkPHP@gmail.com';
$user->create($data);
$record = $user->add();
dump($record);

```

add()返回的是插入数据的id,对于不存在的表字段，add()方法会自动过滤。


# 读取记录

在ThinkPHP中读取数据的方式很多，通常分为读取数据、读取数据集和读取字段值


```
$user = M('User');
$record = $user->where('username="ThinkPHP"')->find();
dump($record);

```

# 读取字段值
```
$user = M('User');
$record = $user->where('id=3')->getField('username');
dump($record);
```

默认情况下，当只有一个字段的时候，返回满足条件的数据表中的该字段的第一行的值.如果getField()传入多个字段，返回值将是一个关联数组：
```
$user = M('User');
$record = $user->getField('username,email');
dump($record);
```
这个数组总是以传入的第一个第一个字段为键值的。如果修改为：
```
$user = M('User');
$record = $user->getField('email,username');
dump($record);
```
将上面的两次代码分别放到testDemo()，你就会看到不一样的结果集。

# 用save()方法更新数据

```
$user = M('User');
$data['username'] = 'ThinkPHPSave';
$data['email'] = 'ThinkPHPSave@outlook.com';
$record = $user->where('id=3')->save($data);
dump($record);

```
这里的$record返回1，表示成功更改。

当然，你也可以这样：

```
$user = M('User');
$user->username = 'ThinkPHP';
$user->email = 'ThinkPHP@outlook.com';
$record = $user->where('id=3')->save();
dump($record);
```

日常开发的时候经常会遇到一些只更新某些字段的情况，可以通过下面的方式来实现：

```
$user = M("User"); 
$record = $user->where('id=4')->setField('username','ThinkPHPChangeName');

dump($record);
```

同时更新多个字段，可以将数据以数组的形式传给setField()方法：

```
$user = M('User');
$data = array('username'=>'ThinkPHPChangeArray','email'=>'ThinkPHP@array.com');
$record = $user-> where('id=6')->setField($data);
dump($record);
```

# ThinkPHP删除数据使用delete()方法

```
$user = M('User');
$record = $user->where('id=3')->delete();
dump($record);
```
或者你可以直接使用：
```
$record = $user->delete('1,2,5');
dump($record);
```
这样就达到了删除主键1,2,5这三条记录了。

# ActiveRecords

ThinkPHP实现了ActiveRecords模式的ORM模型，采用了非标准的ORM模型：表映射到类，记录映射到对象。以下实例将使用ActiveRecords重现对数据表的CURD，看看ActiveRecords给我们带来了什么好处。
```
$user = M("User");

$user->username = 'ThinkPHPWithActive';
$user->email = 'ThinkPHPActive@gmail.com';
$record = $user->add();
dump($record);
```

# 读取记录

AR最大的特点可能就是它的查询模式了，模式简单易用，因为更多情况下面查询条件都是以主键或者某个关键的字段。这种类型的查询，ThinkPHP有着很好的支持。

比如说获取主键为2的用户信息：

```
$user = M("User");
$record = $user->find(2);
dump($record);
```

直接不用where()查询了，简单友好吧。再比如：

```

$user = M("User");
$record = $user->getByUsername("jelly");
dump($record);

```
如果是查询多条记录，使用以下方式：
```
$user = M("User");
$record = $user->select('1,3,8');
dump($record);
```

# 更新记录
```
$user = M("User");
$user->find(21);
$user->username = 'TOPThinkChangeWithAR';
$record = $user->save();
dump($record);
```

# 删除记录

* 删除单条记录

```
$user = M("User");
$record = $user->delete(8);
dump($record);
```

* 删除多条记录
```
$user = M("User");
$record = $user->delete('15,16');
dump($record);
```

# 自动完成

自动完成是ThinkPHP提供用来完成数据自动处理和过滤的方法，当使用create()方法创建数据对象的时候会触发自动完成数机制。 因此，在ThinkPHP鼓励使用create()方法来创建数据对象，因为这是一种更加安全的方式，直接通过add()或者save()方法实现数据写入无法出发自动完成机制。

自动完成通常用来完成默认字段写入(比如添加时间戳)，安全字段过滤(比如加密密码)以及业务逻辑的自动处理等。可以通过模型类里面通过$_auto属性定义处理规则。下面演示如何自动完成添加时间戳：

在UserModel中，声明自动完成的定义数组$_auto ：

```
protected $_auto = array (
        array('created_at','date("Y-m-d H:i:s", time())',3,'function'),
        array('updated_at','date("Y-m-d H:i:s", time())',3,'function'),
    );
```

还有一种是理由auto()方法动态设置自动完成的机制，可以到官方文档去看看

设置完成之后，我们在testDemo()方法中创建一条用户数据：

```
$user = D('User');
$data['username'] = "ThinkPHP";
$data['email'] = "ThinkPHP@gmail.com";
$user->create($data);
$record = $user->add();
dump($record);
```

测试，如果返回记录的id值，说明用户记录创建成功。要验证数据是否自动完成，你可以直接使用：
```
$user = D('User');
$record = $user->find(id);
dump($record);
```

# 自动验证

自动验证是ThinkPHP模型层提供的一种数据验证方法，可以在使用create()创建数据对象的时候自动进行数据验证。

数据验证可以进行数据类型、业务规则、安全判断等方面的验证操作。

通常用于表单验证

数据验证有两种方式：

静态方式：在模型类里面通过$_validate属性定义验证规则。

动态方式：使用模型类的validate()方法动态创建自动验证规则。

无论是什么方式，验证规则的定义是统一的规则，定义格式为：

```
array(
     array(验证字段1,验证规则,错误提示,[验证条件,附加规则,验证时间]),
     array(验证字段2,验证规则,错误提示,[验证条件,附加规则,验证时间]),
     ......
);
```

下面以$_validate静态方式举例如何使用自动验证：

在UserController中创建register()方法，对，几乎每一个Web应用都需要实现用户注册这一步。
```
public function register()
    {
        $this->display();
    }
```

对，就是这么简单，这个方法只是将相应的视图文件渲染出来。所以接下来我们创建对应的视图文件，也就是：./Application/Home/View/User/register.html

```
<extend name="Index/base" />
<block name="main" >
<form method="post" action="__URL__/registerValidate">
    <div class="form-group">
        <label for="exampleInputName">Name</label>
        <input type="text" name="username" class="form-control" id="exampleInputName" placeholder="Name">
    </div>
    <div class="form-group">
        <label for="exampleInputEmail">Email</label>
        <input type="email" name="email" class="form-control" id="exampleInputEmail" placeholder="Email">
    </div>

    <button type="submit" class="btn btn-default">Submit</button>
</form>
</block>
```

上面就是一些HTML代码和一点模板的知识，对于模板，我们后续会讲到，但不管怎样，现在我们访问http://localhost:8999/Home/User/register，就可以看到我们的注册表单页面了。

注意到form表单中，action="__URL__/registerValidate"，这表示提交到当前的控制器的registerValidate()方法处理，所以我们在UserController中增加registerValidate()方法：

```
public function registerValidate()
    {
        $data['username'] = $_POST['username'];
        $data['email'] = $_POST['email'];
        $user = D("User");
        if ( !$user->create($data) ) {
            exit($user->getError());
        }
        //todo: validation passes, add data to database and redirect somewhere
        echo 'validation passes';
    }
```

这里的if ( !$user->create($data) )会触发自动验证并判断验证是否通过验证。你可以尝试在表单里填写不同的数据来进行测试，也可以修改一下验证规则，更多规则可以到官网查看：

http://document.thinkphp.cn/manual_3_2.html#auto_validate

# 关联模型

通常我们所说的关联关系包括下面三种：

```
一对一关联 ：ONE_TO_ONE，包括HAS_ONE 和 BELONGS_TO
一对多关联 ：ONE_TO_MANY，包括HAS_MANY 和 BELONGS_TO
多对多关联 ：MANY_TO_MANY
```

# 关联定义

ThinkPHP可以很轻松的完成数据表的关联CURD操作，目前支持的关联关系包括下面四种： HAS_ONE、BELONGS_TO、HAS_MANY和MANY_TO_MANY。 

一个模型根据业务模型的复杂程度可以同时定义多个关联，不受限制，所有的关联定义都统一在模型类的 $_link 成员变量里面定义，并且可以支持动态定义。要支持关联操作，模型类必须继承Think\Model\RelationModel类，关联定义的格式类似于：

```
namespace Home\Model;
use Think\Model\RelationModel;
class UserModel extends RelationModel{
     protected $_link = array(
        '关联'  =>  array(
            '关联属性1' => '定义',
            '关联属性N' => '定义',
        ),
     );
}
```

关于关联属性的定义和值，你可以到官方文档仔细查看，我们下面也会给出一些最常用的。

在我们的讲解例子中，会采用HAS_MANY和BELONGS_TO来演示，对于其他的几个关系模型，可以参考官方文档举一反三。

首先我们知道数据库里面有两张表，用户表和文章表，并且我们也为其创建了不同的模型(UserModel ArticelModel)。

现在我们仔细来想想他们之间的对应关系：一个用户可以拥有多篇文章，而每一篇文章都属于某个特定的用户。所以我们可以分别为这两种关系添加关联模型：

在UserModel中：

```
protected $_link = array(
        'Article' => self::HAS_MANY
    );
```

在ArticleModel中：

```
protected $_link = array(
        'User' => self::BELONGS_TO
    );
```

以上者两种都是最简洁的模型关联声明。因为在最开始设计数据库的时候，我们遵守了ThinkPHP的官方的规范：

外键的默认规则是当前数据对象名称_id，例如：UserModel对应的可能是表think_user，那么think_user表的外键默认为user_id，如果你的外键不是user_id，而是其他自定义的字段如：user_identify，那么就必须在定义关联的时候定义 foreign_key 。如下：

在UserModel中：

```
protected $_link = array(
        'mapping_type'  => self::HAS_MANY,
        'class_name'    => 'Article',
        'foreign_key'   => 'user_identify',
    );
```

更多自定义的关联模型参数可以到官网查看。

有了以上的定义之后，我们就可以在检索用户数据的同时将属于他的文章也一起检索出来，使用relation()。

同样是在testDemo()这个方法中：

```
$user = D('User');
$record = $user->relation(true)->find(4);
dump($record); 
```

---
title: PHP的一些优秀开源库
date: 2016-02-18 11:18:30
tags: 
- PHP
description: PHP的一些优秀开源库，来自Github

---
## 1.Dispatch – 微框架

[Dispatch](https://github.com/badphp/dispatch)是一个PHP小框架。它并没有给你完整的MVC设置，但你可以定义URL规则和方法，以便更好组织应用程序。这对API、简单的站点或原型来说是完美的。

```
//包含库
include 'dispatch.php';
 
// 定义你的路由
get('/greet', function () {
　　
//渲染视图
    render('greet-form');
});
 
//post处理
post('/greet', function () {
    $name = from($_POST, 'name');
　　
// render a view while passing some locals
    render('greet-show', array('name' => $name));
});
 
// serve your site
dispatch();
```

你可以匹配特定类型的HTTP请求和路径，渲染视图或做更多事情。如果你合并Dispatch和其他框架，那你就可以拥有一个相当强大并且轻量级的程序！


## 2. Klein – PHP快如闪电的路由

[Klein](https://github.com/chriso/klein.php)是另一款针对PHP5.3+版本的轻量级路由库。虽然它有一些比Dispatch冗长的语法，但它相当快。这有一个例子：

```
respond('/[:name]', function ($request) {
    echo 'Hello ' . $request->name;
});
```

你也可以定制来指定HTTP方法和使用正则表达式作为路径。


```
respond('GET', '/posts', $callback);
respond('POST', '/posts/create', $callback);
respond('PUT', '/posts/[i:id]', $callback);
respond('DELETE', '/posts/[i:id]', $callback);
 
//匹配多种请求方法:
respond(array('POST','GET'), $route, $callback);
 
//你或许也想在相同的地方处理请求
respond('/posts/[create|edit:action] /[i:id] ', function ($request, $response) {
    switch ($request->action) {
        
// do something
    }
});

```

对于小型项目来说这是很棒的，但当你把一个像这样的库用于大型应用时，你不得不遵守规矩，因为你的代码可能很快就变得不可维护。所以你最好搭配一个像[Laravel](http://www.laravel.com/)或者[CodeIgniter](http://www.codeigniter.com/)这样完全成熟的框架。


##  3. Ham – 带缓存的路由库

[Ham](https://github.com/radiosilence/Ham)也是一款轻量级的路由框架，但是它利用缓存甚至获得了更快的速度。它通过把任何I/O相关的东西缓存进XCache/APC。下面是一个例子：

```
require '../ham/ham.php';
 
$app = new Ham('example');
$app->config_from_file('settings.php');
 
$app->route('/pork', function($app) {
    return "Delicious pork.";
});
 
$hello = function($app, $name='world') {
    return $app->render('hello.html', array(
        'name' => $name
    ));
};
$app->route('/hello/<string>', $hello);
$app->route('/', $hello);
 
$app->run();
```

这个库要求你至少安装了XCache和APC其中的一个，这可能意味着，在大多数主机提供商提供的主机上它可能用不了。但是如果你拥有一个安装它们其一的主机，或者你可以操控你的web服务器，你应该尝试这款最快的框架。



## 4. Assetic – 资源管理

[Assetic](https://github.com/kriswallsmith/assetic)是一个PHP的资源管理框架，用于合并和减小了CSS/JS资源。下面是例子。

 
```
use Assetic\Asset\AssetCollection;
use Assetic\Asset\FileAsset;
use Assetic\Asset\GlobAsset;
 
$js = new AssetCollection(array(
    new GlobAsset('/path/to/js/*'),
    new FileAsset('/path/to/another.js'),
));
 
//当资源被输出时，代码会被合并
echo $js->dump();
```

以这种方式合并资源是一个好主意，因为它可以加速站点。不仅仅总下载量减小了，也消除了大量不必要的HTTP请求(这是最影响页面加载时间的两件事)

## 5. ImageWorkshop – 带层的图片处理

[ImageWorkshop](http://phpimageworkshop.com/)是一个让你操控带层图片的开源库。借助它你可以重定义尺寸、裁剪、制作缩略图、打水印或做更多事情。下面是一个例子：

```
// 从norway.jpg图片初始化norway层
$norwayLayer = ImageWorkshop::initFromPath('/path/to/images/norway.jpg'); 
 
// 从watermark.png图片初始化watermark层(水印层)
$watermarkLayer = ImageWorkshop::initFromPath('/path/to/images/watermark.png'); 
 
$image = $norwayLayer->getResult(); 
// 这是生成的图片!
 
header('Content-type: image/jpeg');
imagejpeg($image, null, 95); 
// We choose to show a JPG with a quality of 95%
exit;
```
ImageWorkshop被开发用于使一些PHP中最通用的处理图片的案例简化，如果你需要一些更强大的东西，你应该看下Imagine library！

## 6. Snappy – 快照/PDF库

[Snappy](https://github.com/KnpLabs/snappy)是一个PHP5库，可以生成快照、URL、HTML、PDF。它依赖于wkhtmltopdf binary（在Linux，Windows和OSX上都可用）。你可以像这样使用它们：

```
require_once '/path/to/snappy/src/autoload.php'; 
 
use Knp\Snappy\Pdf; 
 
//通过wkhtmltopdf binary路径初始化库
$snappy = new Pdf('/usr/local/bin/wkhtmltopdf'); 
 
//通过把Content-type头设置为pdf来在浏览器中展示pdf
 
header('Content-Type: application/pdf');
header('Content-Disposition: attachment; filename="file.pdf"'); 
 
echo $snappy->getOutput('http://www.github.com');
```

要记得，你的主机提供商可能不允许调用外部二进制程序。

## 7. Idiorm – 轻量级ORM库

[Idiorm](https://github.com/j4mie/idiorm/)是个人之前在本网站教程中用过最喜爱的一款。它是一款轻量级的ORM库，一个建立在PDO之上的PHP5查询构造器。借助它，你可以忘记如何书写乏味的SQL：

 
```
$user = ORM::for_table('user')
    ->where_equal('username', 'j4mie')
    ->find_one();
 
$user->first_name = 'Jamie';
$user->save();
 
$tweets = ORM::for_table('tweet')
    ->select('tweet.*')
    ->join('user', array(
        'user.id', '=', 'tweet.user_id'
    ))
    ->where_equal('user.username', 'j4mie')
    ->find_many();
 
foreach ($tweets as $tweet) {
    echo $tweet->text;
}
```
diorm有一个姊妹库叫Paris，Paris是一个基于Idiorm的Active Record实现。

## 8. Underscore – PHP的工具腰带

[Underscore](http://brianhaveri.github.com/Underscore.php/)是原始Underscore.js的一个接口 – Javascript应用的工具腰带。PHP版本没有让人失望，而且支持了几乎所有原生功能。下面是一些例子：

__::each(array(1, 2, 3), function($num) { echo $num . ','; }); 
// 1,2,3,
 
$multiplier = 2;
__::each(array(1, 2, 3), function($num, $index) use ($multiplier) {
  echo $index . '=' . ($num * $multiplier) . ',';
});
// prints: 0=2,1=4,2=6,
 
__::reduce(array(1, 2, 3), function($memo, $num) { return $memo + $num; }, 0); 
// 6
 
__::find(array(1, 2, 3, 4), function($num) { return $num % 2 === 0; }); 
// 2
 
__::filter(array(1, 2, 3, 4), function($num) { return $num % 2 === 0; }); 
// array(2, 4)
这个库也支持链式语法，这使得它更为强大。

## 9. Requests – 简单HTTP请求

[Requests](https://github.com/rmccue/Requests)是一个简化HTTP请求的库。如果你和我一样，几乎从来都记不住传递给Curl的各种各样的参数，那么它就是为你准备的：
```
$headers = array('Accept' => 'application/json');
$options = array('auth' => array('user', 'pass'));
$request = Requests::get('https://api.github.com/gists', $headers, $options);
 
var_dump($request->status_code);
// int(200)
 
var_dump($request->headers['content-type']);
// string(31) "application/json; charset=utf-8"
 
var_dump($request->body);
// string(26891) "[…]"
```
借助这个库，你可以发送HEAD、GET、POST、PUT、DELTE和PATCH HTTP请求，你可以通过数组添加文件和参数，并且可以访问所有相应数据。

## 10. Buzz – 简单的HTTP请求库

[Buzz](https://github.com/kriswallsmith/Buzz)是另一个完成HTTP请求的库。下面是一个例子：
```
$request = new Buzz\Message\Request('HEAD', '/', 'http://google.com');
$response = new Buzz\Message\Response();
 
$client = new Buzz\Client\FileGetContents();
$client->send($request, $response);
 
echo $request;
echo $response;
```
因为它缺乏文档，所以你不得不阅读源码来获知它支持的所有参数。

 

## 11. Goutte – Web抓取库

[Goutte](https://github.com/fabpot/Goutte)是一个抓取网站和提取数据的库。它提供了一个优雅的API，这使得从远程页面上选择特定元素变得简单。

 
```
require_once '/path/to/goutte.phar'; 
 
use Goutte\Client; 
 
$client = new Client();
$crawler = $client->request('GET', 'http://www.symfony-project.org/'); 
 
//点击链接
$link = $crawler->selectLink('Plugins')->link();
$crawler = $client->click($link); 
 
//使用一个类CSS语法提取数据
$t = $crawler->filter('#data')->text(); 
 
echo "Here is the text: $t";
 
 ```

## 12. Carbon – DateTime 库

[Carbon](https://github.com/FriendsOfPHP/Goutte) 是 DateTime API 的一个简单扩展。
```
printf("Right now is %s", Carbon::now()->toDateTimeString());
printf("Right now in Vancouver is %s", Carbon::now('America/Vancouver'));
 
$tomorrow = Carbon::now()->addDay();
$lastWeek = Carbon::now()->subWeek();
$nextSummerOlympics = Carbon::createFromDate(2012)->addYears(4);
 
$officialDate = Carbon::now()->toRFC2822String();
 
$howOldAmI = Carbon::createFromDate(1975, 5, 21)->age;
 
$noonTodayLondonTime = Carbon::createFromTime(12, 0, 0, 'Europe/London');
 
$endOfWorld = Carbon::createFromDate(2012, 12, 21, 'GMT');
 
//总是以UTC对比
if (Carbon::now()->gte($endOfWorld)) {
    die();
}
 
if (Carbon::now()->isWeekend()) {
    echo 'Party!';
}
 
echo Carbon::now()->subMinutes(2)->diffForHumans(); 
// '2分钟之前'
```

## 13. Ubench – 微型基准库

[Ubench](https://github.com/devster/ubench) 是一个用于评测PHP代码的微型库，可监控（代码）执行时间和内存使用率。下面是范例：
```
use Ubench\Ubench;
 
$bench = new Ubench;
 
$bench->start();
 
//执行一些代码
 
$bench->end();
 
//获取执行消耗时间和内存
echo $bench->getTime(); 
// 156ms or 1.123s
echo $bench->getTime(true); 
// elapsed microtime in float
echo $bench->getTime(false, '%d%s'); 
// 156ms or 1s
 
echo $bench->getMemoryPeak(); 
// 152B or 90.00Kb or 15.23Mb
echo $bench->getMemoryPeak(true); 
// memory peak in bytes 内存峰值
echo $bench->getMemoryPeak(false, '%.3f%s'); 
// 152B or 90.152Kb or 15.234Mb
 
//在结束标识处返回内存使用情况
echo $bench->getMemoryUsage(); 
// 152B or 90.00Kb or 15.23Mb
```
(仅)在开发时运行这些校验是一个好主意。

## 14. Validation – 输入验证引擎

[Validation](https://github.com/Respect/Validation) 声称是PHP库里最强大的验证引擎。但是，它能名副其实吗？看下面：
```
use Respect\Validation\Validator as v; 
 
//简单验证
$number = 123;
v::numeric()->validate($number); 
//true 
 
//链式验证
$usernameValidator = v::alnum()->noWhitespace()->length(1,15);
$usernameValidator->validate('alganet'); 
//true 
 
//验证对象属性
$user = new stdClass;
$user->name = 'Alexandre';
$user->birthdate = '1987-07-01'; 
 
//在一个简单链中验证他的属性
$userValidator = v::attribute('name', v::string()->length(1,32))
                  ->attribute('birthdate', v::date()->minimumAge(18)); 
 
$userValidator->validate($user); 
//true
```
你可以通过这个库验证你的表单或其他用户提交的数据。除此之外，它内置了很多校验，抛出异常和定制错误信息。

## 15. Filterus – 过滤库

[Filterus](https://github.com/ircmaxell/filterus)是另一个过滤库，但它不仅仅可以验证，也可以过滤匹配预设模式的输出。下面是一个例子：
```
$f = Filter::factory('string,max:5');
$str = 'This is a test string'; 
 
$f->validate($str); 
// false
$f->filter($str); 
// 'This '
```
Filterus有很多内建模式，支持链式用法，甚至可以用独立的验证规则去验证数组元素。

## 16. Faker – 假数据生成器

[Faker](https://github.com/fzaninotto/Faker) 是一个为你生成假数据的PHP库。当你需要填充一个测试数据库，或为你的web应用生成测试数据时，它能派上用场。它也非常容易使用：
```
//引用Faker 自动加载器
require_once '/path/to/Faker/src/autoload.php';
 
//使用工厂创建来创建一个Faker\Generator实例
$faker = Faker\Factory::create();
 
//通过访问属性生成假数据
echo $faker->name; 
// 'Lucy Cechtelar';
 
echo $faker->address;
  
// "426 Jordy Lodge
  
// Cartwrightshire, SC 88120-6700"
 
echo $faker->text;
  
// Sint velit eveniet. Rerum atque repellat voluptatem quia ...
只要你继续访问对象属性，它将继续返回随机生成的数据。
```

## 17. Mustache.php – 优雅模板库

[Mustache.php](https://github.com/bobthecow/mustache.php)是一款流行的模板语言，实际已经在各种编程语言中得到实现。使用它，你可以在客户端或服务段重用模板。 正如你猜得那样，Mustache.php 是使用PHP实现的。

$m = new Mustache_Engine;
echo $m->render('Hello {{planet}}', array('planet' => 'World!')); 
// "Hello World!"
建议看一下官方网站Mustache docs 查看更多高级的例子。

## 18. Gaufrette – 文件系统抽象层

[Gaufrette](https://github.com/KnpLabs/Gaufrette)是一个PHP5库，提供了一个文件系统的抽象层。它使得以相同方式操控本地文件，FTP服务器，亚马逊 S3或更多操作变为可能。它允许你开发程序时，不用了解未来你将怎么访问你的文件。

 
```
use Gaufrette\Filesystem;
use Gaufrette\Adapter\Ftp as FtpAdapter;
use Gaufrette\Adapter\Local as LocalAdapter; 
 
//本地文件:
$adapter = new LocalAdapter('/var/media'); 
 
//可选地使用一个FTP适配器
// $ftp = new FtpAdapter($path, $host, $username, $password, $port); 
 
//初始化文件系统
$filesystem = new Filesystem($adapter); 
 
//使用它
$content = $filesystem->read('myFile');
$content = 'Hello I am the new content';
$filesystem->write('myFile', $content);
```
也有缓存和内存适配器，并且随后将会增加更多适配器。

## 19. Omnipay – 支付处理库

[Omnipay](https://github.com/adrianmacneil/omnipay)是一个PHP支付处理库。它有一个清晰一致的API，并且支持数十个网关。使用这个库，你仅仅需要学习一个API和处理各种各样的支付处理器。下面是一个例子：
```
use Omnipay\CreditCard;
use Omnipay\GatewayFactory;
 
$gateway = GatewayFactory::create('Stripe');
$gateway->setApiKey('abc123');
 
$formData = ['number' => '4111111111111111', 'expiryMonth' => 6, 'expiryYear' => 2016];
$response = $gateway->purchase(['amount' => 1000, 'card' => $formData]);
 
if ($response->isSuccessful()) {
　　
//支付成功:更新数据库
    print_r($response);
} elseif ($response->isRedirect()) {
　　
//跳转到异地支付网关
    $response->redirect();
} else {
　　
//支付失败:向客户显示信息
    exit($response->getMessage());
}
```
使用相同一致的API，可以很容易地支持多种支付处理器，或在需要时进行切换。

## 20. Upload – 处理文件上传

[Upload](https://github.com/brandonsavage/Upload)是一个简化文件上传和验证的库。上传表单时，这个库会校验文件类型和尺寸。
```
$storage = new \Upload\Storage\FileSystem('/path/to/directory');
$file = new \Upload\File('foo', $storage);
 
//验证文件上传
$file->addValidations(array(
　　
//确保文件类型是"image/png"
    new \Upload\Validation\Mimetype('image/png'),
 
　　
//确保文件不超过5M(使用"B","K","M"或者"G")
    new \Upload\Validation\Size('5M')
));
 
//试图上传文件
try {
    
//成功
    $file->upload();
} catch (\Exception $e) {
    
//失败!
    $errors = $file->getErrors();
}
```
它将减少不少乏味的代码。

## 21. HTMLPurifier – HTML XSS 防护

[HTMLPurifier](http://htmlpurifier.org/)是一个HTML过滤库，通过强大的白名单和聚集分析，保护你代码远离XSS攻击。它也确保输出标记符合标准。 (源码在[github](https://github.com/ezyang/htmlpurifier)上)
```
require_once '/path/to/HTMLPurifier.auto.php';
 
$config = HTMLPurifier_Config::createDefault();
$purifier = new HTMLPurifier($config);
$clean_html = $purifier->purify($dirty_html);
```
如果你的网站允许用户提交 HTML 代码，不修改就展示代码的话，那这时候就是用这个库的时候了。

## 22. ColorJizz-PHP – 颜色操控库

[ColorJizz](https://github.com/mikeemoo/ColorJizz-PHP)是一个简单的库，借助它你可以转换不同的颜色格式，并且做简单的颜色运算
```
use MischiefCollective\ColorJizz\Formats\Hex;
 
$red_hex = new Hex(0xFF0000);
$red_cmyk = $hex->toCMYK();
echo $red_cmyk; 
// 0,1,1,0
 
echo Hex::fromString('red')->hue(-20)->greyscale(); 
// 555555
```
它已经支持并且可以操控所有主流颜色格式了

## 23. PHP Geo – 地理位置定位库

[phpgeo](https://github.com/mjaschen/phpgeo)是一个简单的库，用于计算地理坐标之间高精度距离。例如：

 ```
use Location\Coordinate;
use Location\Distance\Vincenty;
 
$coordinate1 = new Coordinate(19.820664, -155.468066); 
// Mauna Kea Summit 茂纳凯亚峰
$coordinate2 = new Coordinate(20.709722, -156.253333); 
// Haleakala Summit
 
$calculator = new Vincenty();
$distance = $calculator->getDistance($coordinate1, $coordinate2); 
// returns 128130.850 (meters; ≈128 kilometers)
```

它将在使用地理位置数据的app里出色工作。你可以试译 HTML5 Location API，雅虎的API（或两者都用，我们在weather web app tutorial中这样做了），来获取坐标。

## 24. ShellWrap – 优美的命令行包装器

借助 [ShellWrap](https://github.com/MrRio/shellwrap) 库，你可以在PHP代码里使用强大的 Linux/Unix 命令行工具。

 ```
require 'ShellWrap.php';
use \MrRio\ShellWrap as sh; 
 
//列出当前文件下的所有文件
echo sh::ls(); 
 
//检出一个git分支
sh::git('checkout', 'master'); 
 
//你也可以通过管道把一个命令的输出用户另一个命令
//下面通过curl跟踪位置，然后通过grep过滤’html’管道来下载example.com网站
echo sh::grep('html', sh::curl('http://example.com', array(
    'location' => true
))); 
 
//新建一个文件
sh::touch('file.html'); 
 
//移除文件
sh::rm('file.html'); 
 
//再次移除文件(这次失败了,然后因为文件不存在而抛出异常)
try {
    sh::rm('file.html');
} catch (Exception $e) {
    echo 'Caught failing sh::rm() call';
}
```
当命令行里发生异常时，这个库抛出异常，所以你可以及时对之做出反应。它也可以通过管道让你一个命令的输出作为另一个命令的输入，来实现更强的灵活性。



[参考资料](http://1ke.co/course/483)

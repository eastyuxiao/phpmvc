一、文件结构 
建立3个文件夹 
controller文件夹存放控制器文件 
view文件夹存放视图文件 
model文件夹存放数据文件 
建立1个index.php 作为唯一入口 
二、控制器 
我们在controller文件夹下建立一个democontroller.php文件，文件内容如下 
复制代码代码如下:

<?php 
class DemoController 
{ 
function index() 
{ 
echo('hello world'); 
} 
} 
/* End of file democontroller.php */ 

这个文件里面我们只是建立了一个名为DemoController的对象并包含一个index的方法，该方法输出hello world。下面在index.php中执行DemoController中index方法。 
index.php的代码如下 
复制代码代码如下:

<?php 
require('controller/democontroller.php'); 
$controller=new DemoController(); 
$controller->index(); 
/* End of file index.php */ 

运行index.php，ok如愿我们看到了我们久违的hello world。这两个文件非常简单，但也揭示了一点点mvc的本质，通过唯一入口运行我们要运行的控制器。当然controller部分应该是由uri来决定的，那么我们来改写一下index.php使他能通过uri来决定运行那个controller。 
index.php改写代码如下： 
复制代码代码如下:

<?php 
$c_str=$_GET['c']; 
//获取要运行的controller 
$c_name=$c_str.'Controller'; 
//按照约定url中获取的controller名字不包含Controller，此处补齐。 
$c_path='controller/'.$c_name.'.php'; 
//按照约定controller文件要建立在controller文件夹下，类名要与文件名相同，且文件名要全部小写。 
$method=$_GET['a']; 
//获取要运行的action 
require($c_path); 
//加载controller文件 
$controller=new $c_name; 
//实例化controller文件 
$controller->$method(); 
//运行该实例下的action 
/* End of file index.php */ 

在浏览器中输入http://localhost/index.php?c=demo&a=index，得到了我们的hello world。当然如果我们有其他的controller并且要运行它，只要修改url参数中的c和a的值就可以了。 
这里有几个问题要说明一下。 
一、php是动态语言，我们直接可以通过字符串new出我们想要的对象和运行我们想要的方法，即上面的new $c_name，我们可以理解成new 'DemoController'，因为$c_name本身的值就是'DemoController'，当然直接new 'DemoController'这么写是不行的，其中的'DemoController'字符串必须通过一个变量来中转一下。方法也是一样的。 
二、我们在url中c的值是demo，也就是说$c_name 的值应该是demoController呀，php不是区分大小写吗，这样也能运行吗？php区分大小写这句话不完整，在php中只有变量（前面带$的）和常量(define定义的)是区分大小写的，而类名方，法名甚至一些关键字都是不区分大小写的。而true，false，null等只能全部大写或全部小写。当然我们最好在实际编码过程中区分大小写。 
三、视图 
我们在前面的controller中只是输出了一个“hello world”，并没有达到mvc的效果，下面我将在此基础上增加视图功能，相信到这里大家基本已经能想到如何添加视图功能了。对，就是通过万恶的require或者include来实现。 
首先我们在view文件夹下建立一个index.php，随便写点什么（呵呵，我写的还是hello world）。之后我们改写一下我们之前的DemoController。代码如下： 
复制代码代码如下:

<?php 
class DemoController 
{ 
function index() 
{ 
require('view/index.php'); 
} 
} 
/* End of file democontroller.php */ 

再在浏览器中运行一下，看看是不是已经输出了我们想要的内容了。 
接着我们通过controller向view传递一些数据看看，代码如下： 
复制代码代码如下:

<?php 
class DemoController 
{ 
function index() 
{ 
$data['title']='First Title'; 
$data['list']=array('A','B','C','D'); 
require('view/index.php'); 
} 
} 
/* End of file democontroller.php */ 

view文件夹下index.php文件代码如下： 
复制代码代码如下:

<html> 
<head> 
<title>demo</title> 
</head> 
<body> 
<h1><?php echo $data['title'];?></h1> 
<?php 
foreach ($data['list'] as $item) 
{ 
echo $item.'<br>'; 
} 
?> 
</body> 
</html> 

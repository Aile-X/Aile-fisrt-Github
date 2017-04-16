# github_fist

activity的生命周期:
oncreate() 创建调用一次
onrestart() onstop()之后,onstart()之前
onstart() 用户所见
on resume() 获取焦点并且可操作
on pause() 部分挡住
on stop() 全部挡住
on destory() finish()

函数调用过程：
1:启动第一个Activity的时候：
第一次创建onCreate()-->Activity可见了onStart()-->Activity可以操作了onResume()。
 
2:点击第一个Activity上的按钮通过Intent跳到第二个Activity：
第一个Activity暂停onPause()-->创建第二个ActivityonCreate()-->Activity可见onStart()-->Activity可操作onResume()-->第一个Activity被第二个Activity完全遮盖onStop()(如果调用了finish(),或者系统资源紧缺，则会被销毁onDestory())。

3:点击系统返回功能建，从第二个Activity回到第一个Activity ：
第二个Activity暂停onPause()-->第一个Activity重启动OnRestart()(并没有被销毁,如果销毁了则要创建onCreate())-->第一个Activity可见onStart()-->第一个Activity可操作onResume()-->第二个Activity被完全遮盖onStop()(如果调用了finish(),或者系统资源紧缺，则会被销毁onDestory())

横竖屏切换时候activity的生命周期
1、不设置Activity的android:configChanges时，切屏会重新调用各个生命周期，切横屏时会执行一次，切竖屏时会执行两次
2、设置Activity的android:configChanges="orientation"时，切屏还是会重新调用各个生命周期，切横、竖屏时只会执行一次
3、设置Activity的android:configChanges="orientation|keyboardHidden"时，切屏不会重新调用各个生命周期，只会执行onConfigurationChanged方法

activity启动模式
standdard 只要startactivity就创建新实例
singletop 看栈顶有无实例,有则重用,无则创建
singletast 在一个新task中,只会重用一个实例
singleinstance 只能有一个实例,不能其他

退出activity
单一activity就用finish即可,很多activity可以使用application,将activity创建集合,用于一次清理

Fragment的生命周期:
onAttach()、onCreate()、onCreateView()、onActivityCreated()、onstart()、onResume()、onPause()、onStop()、onDestroyView()、onDestroy()、onDetach()
1:切换到该Fragment，分别执行onAttach()、onCreate()、onCreateView()、onActivityCreated()、onstart()、onResume()方法。
2:锁屏，分别执行onPause()、onStop()方法。
3:亮屏，分别执行onstart()、onResume()方法。
4:覆盖，切换到其他Fragment，分别执行onPause()、onStop()、onDestroyView()方法。
5:从其他Fragment回到之前Fragment，分别执行onCreateView()、onActivityCreated()、onstart()、onResume()方法。

启动service
1:继承service类
2:在mainfest.xml注册
3:通过startservice和bindservice启动,响应用stopservice和unbindservice停止
4:5个内部自动调用的方法
onCreat()           创建服务
onStartCommand()    开始服务
onDestroy()         销毁服务
onBind()            绑定服务
onUnbind()          解绑服务

service生命周期
1) startService()生命周期
onCreate() -> onStart() -> onDestroy()
onCreate() 创建时调用一次
onStart() 多次调用startService()方法尽管不会多次创建服务，但onStart() 方法会被多次调用。
onDestroy() 服务被终止时调用。
2) bindService()生命周期方法
onCreate() -> onBind() -> onUnbind() -> onDestroy()
onBind() 多次调用Context.bindService()方法并不会导致该方法被多次调用。
onUnbind() 在调用者与服务解除绑定时被调用

intentservice和service
intentservice是service子类,拥有工作线程方法onHandleIntent(),比service安全

BroadcastReceiver生命周期
只有一个回调方法onReceive(),所以不要在里面做耗时操作,只能有10s响应时间,一旦完成任务就可能死掉

contentprovider,contentresolver,contentobserve:
ContentProvider：内容提供者，对外提供数据的操作，contentProvider.notifyChanged(uir)更新数据
contentResolver：内容解析者，解析ContentProvider返回的数据
ContentObServer:内容监听者，监听数据的改变，contentResolver.registerContentObServer()

Android引入广播机制的用意
a:从MVC的角度考虑(应用程序内).照搬的web那一套MVC架构
b：程序通信(例如在自己的应用程序内监听系统来电)
c：效率上(参考UDP的广播协议在局域网的方便性)
d：设计模式上(IOC反转控制的一种应用，类似监听者模式)

BroadcastReceiver注册
静态注册 常驻广播,生命周期长,但是优先级不如动态注册
动态注册 生命周期同应用程序,但是优先级比静态注册高

Android中activity，context，application
1:activity和application都是context的子类
2:Context：当前上下文对象，保存的是上下文中的参数和变量，它可以让更加方便访问到一些资源
3:Context通常与activity的生命周期是一样的，application表示整个应用程序的对象
4:对于一些生命周期较长的，不要使用context，可以使用application

ANR
1:不在activity oncreate和onresume方法中做耗时操作,时间限制5s,解决方法为handler+thread
2:不要在broadcastreceiver onreceive方法做耗时操作,时间限制10s,解决方法为开一个service
3:不要在普通service里做耗时操作,时间限制20s

单线程模型handler,looper,messagequeue,message关系
handler 拥有looper和messagequeue的引用,用来发送和处理消息
looper 用于messagequeue的数据管理和交换
messagequeue 用于存储message
message 具体消息

Android中touch事件的传递机制
dispatchTouchEvent 分派
onTouchEvent 处理
onInterceptTouchEvent 拦截 


Android动画
帧动画 原理就是将一张张单独的图片连贯的进行播放，从而在视觉上产生一种动画的效果,多个item
补间动画 lpha,translate,scale,rotate之间过渡动画,Interpolator 控制速率
属性动画 机制是通过不断地对值进行操作来实现的，而初始值和结束值之间的动画过渡就是由ValueAnimator这个类来负责计算的

ListView的优化:
1:重用convertview.
2:getview少使用逻辑
3:使用viewholder
4:分页加载
5:WeakRefrence引用ImageView对象
6:recycleview替代

android 进行优化
对 listview 的优化。
对图片的优化。
对内存的优化。
尽量不要使用过多的静态类 static
数据库使用完成后要记得关闭 cursor
广播使用完之后要注销

Android 线程通信有共享内存（变量）,文件，数据库,Handler,Java 里的 wait()，notify()，notifyAll()
Android进程通信intent,Content Provider,广播（Broadcast）,AIDL服务
一个 Android 程序开始运行时，会单独启动一个Proces,Activity或者Service都会跑在这个Process,但一个Process下却可以有许多个Thread。
一个 Android 程序开始运行时，就有一个主线程Main Thread被创建,对于比较费时的工作，应该设法交给子线程去做
Android UI操作并不是线程安全的并且这些操作必须在UI线程中执行。如果在子线程中直接修改UI，会导致异常。

内存泄漏和内存溢出
内存泄漏 没用使用的对象一直没被GC,所以久了会累积导致最终内存溢出,例如数据库cusor没有关闭,io流没有关闭,没有及时recycle bitmap等.
内存溢出 系统分配的内存不够了,例如图片加载会出现这个问题,解决方案就是变小加载,软引用加载

Android架构框架
Linux Kernel(Linux内核)、Libraries(系统运行库)、ApplicationFramework(框架层)、Applications (应用程序)

你在工作中是怎样解决一个bug
1:已有经验.log打印
2:复制error信息到百度Google查找
3:CSDN，github，stack overflow
4:论坛（开源中国），qq技术群，博客（teachcource）
5:师傅或同事

开发中都使用过哪些框架、平台
xUtils（网络、图片、ORM）
友盟（统计平台）
极光推送
百度地图
Gson（解析 json 数据框架）
volley


一般在开发项目中都使用什么设计模式
为常用的就是单例设计模式(数据库操作)，工厂设计模式以及观察者设计模式(contentobserve)


sim卡EF就是作存储并和手机通讯用的,
android属于软实时操作系统,
一条最长的短信息中文70,英文160字节, 
DDMS是程序执行查看器，TraceView是程序性能分析器,heap内存泄漏检查工具,allocation tracker内存分配跟踪工具
将一个Activity设置成窗口的样式(android :theme="@android:style/Theme.Dialog" ),
android view是在UI线程中绘制,surfaceview是在新开线程绘制,
sleep不释放锁,wait释放锁资源,
通过windowmanager的getdefaltdisplay管理分辨率,
消息推送的方式有极光,友盟,XMPP协议,MQTT协议,HTTP轮循方式,
gravity是组件内元素的对齐,而layout_gravity：相对于父类容器该组件的对齐方式
后台的Activity由于某原因被系统回收了,在onPuase方法中调用onSavedInstanceState()回收之前保存当前状态
Fragment对象有一个getActivity的方法，通过该方法与activity交互,
使用framentmentManager.findFragmentByXX可以获取fragment对象，在activity中直接操作fragment对象
使用adapter的notifyDataSetChanged方法更新listview数据
intent可传递基本数据类型,数组类型,bundle类型，但是bundle类型的数据需要实现Serializable或者parcelable接口
Serializable 和 Parcelable 的区别是Parcelable效率高,不会产生大量临时变量
ListView实现分页加载具体设置 ListView 的滚动监听器：setOnScrollListener(new OnScrollListener{….})方法
ListView通过lv.setSelection(listView.getPosition())方法定位到指定位置
在 ScrollView 中如何嵌入 ListView最好方法是自定义 ListView，重载 onMeasure()方法，设置全部显示。
ListView中图片错位的问题是由于convertview重用
将SQLite数据库(dictionary.db文件)与apk文件一起发布,把这个文件放在/res/raw目录下即可
发消息给UI线程更新UI,可以通过handelr,asyctask,runable
在子线程中不能new handler,否则RuntimeException
修改 Activity 进入和退出动画可以通过定义Activity的主题theme
android与服务器交互有对称加密和非对称加密,对称加密-DES,非对称加密-RSA,例子SSL,SSH
属性动画控件移动后事件相应就在控件移动后本身进行处理
Android验证码登陆有服务器端获取图片,短信验证
andorid应用第二次登录实现自动登录,把长效token保存到SharedPreferences
可以把大文件通过post断点上传
activity是phonwindow,phonewindow又是view
view绘制流程为measure,layout,ondraw
get是从指定的资源请求数据,post是向指定的资源提交要被处理的数据



android NDK
NDK是一系列工具的集合,NDK 提供了一份稳定、功能有限的 API 头文件声明,使 “Java+C” 的开发方式终于转正，成为官方支持的开发方式,
NDK 将是 Android 平台支持 C 开发的开端

android JNI
1)安装和下载Cygwin，下载 Android NDK
2)在ndk项目中JNI接口的设计
3)使用C/C++实现本地方法
4)JNI生成动态链接库.so文件
5)将动态链接库复制到java工程，在java工程中调用，运行java工程

Android数字签名
(1)所有的应用程序都必须有数字证书
(2)Android程序包使用的数字证书可以是自签名的
(3)必须使用一个合适的私钥生成的数字证书来给程序签名
(4)数字证书都是有有效期的

final, finally, finalize的区别
final 用于声明属性，方法和类，分别表示属性不可变，方法不可覆盖，类不可继承。
finally是异常处理语句结构的一部分，表示总是执行。
finalize是Object类的一个方法，在垃圾收集器执行的时候会调用被回收对象的此方法，可以覆盖此方法提供垃圾收集时的其他资源回收，例如关闭文件等。

abstract class和interface
abstract class可以定义和实现方法,其子类可以重用和不实现方法,
interface 只能定义方法,其子类必须实现方法

String，StringBuffer，StringBuilder
1:速度方面的比较：StringBuilder >  StringBuffer  >  String
2.单线程操作字符串缓冲区 下操作大量数据 = StringBuilder
3.多线程操作字符串缓冲区 下操作大量数据 = StringBuffer

Java GC 原理
JVM的堆是Java对象的活动空间，程序中的类的对象从中分配空间,在堆中，找到已经无用的对象，并把这些对象占用的空间收回使其可以重新利用

移动端跨平台开发
Web 流：也被称为 Hybrid 技术，它基于 Web 相关技术来实现界面及功能：PhoneGap
虚拟机流：通过将某个语言的虚拟机移植到不同平台上来运行 Android
代码转换流：将某个语言转成 Objective-C、Java 或 C#，然后使用不同平台下的官方工具来开发

java算法
public static void selectsort(int[] arr) {
        for (int i = 0; i < arr.length - 1; i++) {
            int minIndex = i;
            for (int j = i + 1; j < arr.length; j++) {
                if (arr[j] < arr[minIndex]) {
                    minIndex = j;
                }
            }
            int t = arr[i];
            arr[i] = arr[minIndex];
            arr[minIndex] = t;
        }
    }
    public static void insertsort(int[] arr) {
        for (int i = 0; i < arr.length - 1; i++) {
            for (int j = i + 1; j > 0; j--) {
                if (arr[j] < arr[j - 1]) {
                    int temp = arr[j - 1];
                    arr[j - 1] = arr[j];
                    arr[j] = temp;
                }
            }
        }
    }
    public static void bubblesort(int[] arr) {
        for (int i = 0; i < arr.length - 1; i++) {
            for (int j = arr.length - 1; j > i; j--) {
                if (arr[j] < arr[j - 1]) {
                    int t = arr[j];
                    arr[j] = arr[j - 1];
                    arr[j - 1] = t;
                }
            }
        }
    }
    public static int binarySearch(int[] arr, int target) {
        int start = 0;
        int end = arr.length - 1;
        while (start <= end) {
            int mid = (start + end) / 2;
            if (arr[mid] > target) {
                end = mid - 1;
            } else if (arr[mid] < target) {
                start = mid + 1;
            } else {
                return mid;
            }
        }
        return -1;
    }
    public static int zhishusum(int start, int end) {
        int i, j;
        int sum = 0;
        for (i = start; i < end; i++) {
            for (j = 2; j <= i / 2; j++) {
                if (i % j == 0)
                    break;
            }
            if (j > i / 2) {
                sum += i;
            }
        }
        return sum;
    }
    public static long FBNQ(long n) {
        if (n == 1 || n == 2) {
            return 1;
        }
        return FBNQ(n - 1) + FBNQ(n - 2);
    }

ProgressBar计时10s代码:
public class ProgressBarStu extends Activity {
	private ProgressBar progressBar = null;
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.progressbar);
		progressBar = (ProgressBar)findViewById(R.id.progressBar);
		Thread thread = new Thread(new Runnable() {
			public void run() {
				int progressBarMax = progressBar.getMax();
				try {
					while(progressBarMax!=progressBar.getProgress())
					{
						int stepProgress = progressBarMax/10;
						int currentprogress = progressBar.getProgress();
						progressBar.setProgress(currentprogress+stepProgress);
						Thread.sleep(1000);
					}
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
		});
		thread.start();
	}

单例模式
ublic class Singleton {
    private static Singleton singleton;

    private Singleton() {

    }

    public static synchronized Singleton getSingleton() {
        if (singleton == null) {
            singleton = new Singleton();
        }

        return singleton;
    }
}

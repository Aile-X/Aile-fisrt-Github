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

启动service
1:继承service类
2:在mainfest.xml注册
3:通过startservice和bindservice启动,响应用stopservice和unbindservice停止

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
intentservice拥有工作线程,比service安全

BroadcastReceiver生命周期
只有一个回调方法onReceive(),所以不要在里面做耗时操作,只能有10s响应时间,一旦完成任务就可能死掉

contentprovider,contentresolver,contentobserve
contentprovider 提供URI供外接调用数据
contentresolver 管理数据的工具
contentobserve 监管制定URI数据的更新

Android引入广播机制的用意
a:从MVC的角度考虑(应用程序内).照搬的web那一套MVC架构
b：程序通信(例如在自己的应用程序内监听系统来电)
c：效率上(参考UDP的广播协议在局域网的方便性)
d：设计模式上(IOC反转控制的一种应用，类似监听者模式)

BroadcastReceiver注册
静态注册 常驻广播,生命周期长,但是优先级不如动态注册
动态注册 生命周期同应用程序,但是优先级比静态注册高

ANR
1:不在activity oncreate和onresume方法中做耗时操作,时间限制5s
2:不要在broadcastreceiver onreceive方法做耗时操作,时间限制10s
3:不要在普通service里做耗时操作,时间限制20s
4:若有耗时操作就需要使用handler和asyctask去实现

单线程模型handler,looper,messagequeue,message关系
handler 拥有looper和messagequeue的引用,用来发送和处理消息
looper 用于messagequeue的数据管理和交换
messagequeue 用于存储message
message 具体消息

ListView的优化:
1，多次重用convertview.
2，使用viewholder
3，分页加载

内存泄漏和内存溢出
内存泄漏 没用使用的对象一直没被GC,所以久了会累积导致最终内存溢出,例如数据库cusor没有关闭,io流没有关闭,没有及时recycle bitmap等.
内存溢出 系统分配的内存不够了,例如图片加载会出现这个问题,解决方案就是变小加载,软引用加载

Android架构框架
Linux Kernel(Linux内核)、Libraries(系统运行库)、ApplicationFramework(框架层)、Applications (应用程序)

sim卡EF就是作存储并和手机通讯用的,android属于软实时操作系统,一条最长的短信息中文70,英文160字节, DDMS是程序执行查看器，
TraceView是程序性能分析器,将一个Activity设置成窗口的样式(android :theme="@android:style/Theme.Dialog" ),android
view是在UI线程中绘制,surfaceview是在新开线程绘制

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

final, finally, finalize的区别。 
final 用于声明属性，方法和类，分别表示属性不可变，方法不可覆盖，类不可继承。
finally是异常处理语句结构的一部分，表示总是执行。
finalize是Object类的一个方法，在垃圾收集器执行的时候会调用被回收对象的此方法，可以覆盖此方法提供垃圾收集时的其他资源回收，例如关闭文件等。

abstract class和interface
abstract class可以定义和实现方法,其子类可以重用和不实现方法,
interface 只能定义方法,其子类必须实现方法

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

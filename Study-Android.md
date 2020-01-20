##1、四大组件以及应用场景？

1. Activity【活动】：在Android应用中负责与用户交互的组件。
2. Service【服务】：后台运行服务，不提供界面呈现。常用于为其他组件提供后台服务或者监控其他组件的运行状态。经常用来执行一些耗时操作。
3. BroadcastReceiver【广播接收器】：用来接收广播。用于监听应用程序中的其他组件。
4. Content Provider【内容提供商】：支持在多个应用中存储和读取数据，相当于数据库。Android应用程序之间实现实时数据交换。

##2. Activity生命周期图

![ActivityLifecyle](https://img-blog.csdn.net/20180405184622584?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMzQ0MzEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


方法|描述|可被杀死
---|:---|:---
onCreate|在activity第一次被创建的时候调用。这里是你做所有初始化设置的地方──创建视图、绑定数据至列表等。如果曾经有状态记录（参阅后述[Saving Activity State](https://blog.csdn.net/itachi85/article/details/7426451#actstate)。），则调用此方法时会传入一个包含着此activity以前状态的包对象做为参数。 总继之以onStart()。|否
onRestart|在Activity被停止后，再次启动调用。总继之以onStart()。|否
onStart|当Activity被用户可见时。
onResume|当Activity可与用户交互时|否
onPause|当系统将要启动另一个activity时调用。此方法主要用来将未保存的变化进行持久化，停止类似动画这样耗费CPU的动作等。这一切动作应该在短时间内完成，因为下一个activity必须等到此方法返回后才会继续。当activity重新回到前台是继以onResume()。|是
onStop|当activity不再为用户可见时调用此方法。这可能发生在它被销毁或者另一个activity（可能是现存的或者是新的）回到运行状态并覆盖了它。|是
onDetroy|在activity销毁前调用。这是activity接收的最后一个调用。这可能发生在activity结束（调用了它的 finish() 方法）或者因为系统需要空间所以临时的销毁了此acitivity的实例时。你可以用isFinishing() 方法来区分这两种情况。|是

##3、横竖屏切换时候activity的生命周期

- 不设置Activity的android:configChanges时,切屏会重新调用各个生命周期,切横屏时会执行一次,切竖屏时会执行两次. 
- 设置Activity的android:configChanges="orientation"时,切屏还是会重新调用各个生命周期,切横、竖屏时只会执行一次. 
- 设置Activity的android:configChanges="orientation|keyboardHidden"时,切屏不会重新调用各个生命周期,只会执行onConfigurationChanged方法.

##4. Fragment生命周期图

![FragmentLifecyle](https://img-blog.csdn.net/20180405184635884?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMzQ0MzEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##5. Service的生命周期：首先Service有两种启动方式，而在这两种启动方式下，它的生命周期不同。

- 通过startService()方法启动的服务。初始化结束后系统会调用 void onStart(Intent intent) 方法，用于处理传递给startService()的Intent对象。如音乐服务会打开Intent 来探明将要播放哪首音乐，并开始播放。注意：多次调用startService()方法会多次触发onStart()方法。
- 通过bindService ()方法启动的服务。初始化结束后系统会调用 IBinder onBind(Intent intent) 方法，用来绑定传递给bindService 的Intent 的对象。注意：多次调用bindService()时，如果该服务已启动则不会再触发此方法。

##6.广播接收者生命周期

- 一个广播接收者有一个回调方法：void onReceive(Context curContext, Intent broadcastMsg)。当一个广播消息到达接收者是，Android调用它的onReceive()方法并传递给它包含消息的Intent对象。广播接收者被认为仅当它执行这个方法时是活跃的。当onReceive()返回后，它是不活跃的。
- 有一个活跃的广播接收者的进程是受保护的，不会被杀死。但是系统可以在任何时候杀死仅有不活跃组件的进程，当占用的内存别的进程需要时。
- 这带来一个问题，当一个广播消息的响应时费时的，因此应该在独立的线程中做这些事，远离用户界面其它组件运行的主线程。如果onReceive()衍生线程然后返回，整个进程，包括新的线程，被判定为不活跃的（除非进程中的其它应用程序组件是活跃的），将使它处于被杀的危机。解决这个问题的方法是onReceive()启动一个服务，及时服务做这个工作，因此系统知道进程中有活跃的工作在做。

##7、Activity的四种启动模式对比？

- Standard:标准的启动模式，如果需要启动一个activity就会创建该activity的实例。也是activity的默认启动模式。
- SingeTop:如果已经有一个实例位于Activity栈的顶部时，就不产生新的实例，而只是调用Activity中的newInstance()方法。如果不位于栈顶，会产生一个新的实例。
- SingleTask:设置了singleTask启动模式的activity在启动时，如果位于activity栈中，就会复用该activity，这样的话，在该实例之上的所有activity都依次进行出栈操作，即执行对应的onDestroy()方法，直到当前要启动的activity位于栈顶。一般应用在网页的图集，一键退出当前的应用程序。
- singleInstance:如果使用singleInstance启动模式的activity在启动的时候会复用已经存在的activity实例。不管这个activity的实例是位于哪一个应用当中，都会共享已经启动的activity的实例对象。使用了singlInstance的启动模式的activity会单独的开启一个共享栈，这个栈中只存在当前的activity实例对象。

##8、Activity在有Dialog时按Home键的生命周期？

1. 当我们的Activity上弹出Dialog对话框时，程序的生命周期依然是onCreate() ---> onStart() ---> onResume()，在弹出Dialog的时候并没有onPause()和onStop()方法。而在此时我们按下Home键，才会继续执行onPause()和onStop()方法。这说明对话框并没有使Activity进入后台，而是在点击了Home键后Activity才进入后台工作。
2. 原因就是，其实Dialog是Activity的一个组件，此时Activity并不是不可见，而是被Dialog组件覆盖了其他的组件，此时我们无法对其他组件进行操作而已。

##9、两个Activity 之间跳转时必然会执行的是哪几个方法？

1. 首先定义两个Activity，分别为A和B。
2. 当我们在A中激活B时，A调用onPause()方法，此时B出现在屏幕时，B调用onCreate()、onStart()、onResume()。
3. 这个时候B【B不是一个透明的窗体或对话框的形式】已经覆盖了A的窗体，A会调用onStop()方法。

##10、 前台切换到后台，然后再回到前台，Activity生命周期回调方法。弹出Dialog，生命周期回调方法？

1. 首先定义两个Activity，分别为A和B。
2. 完整顺序为：A调用onCreate()方法 ---> onStart()方法 ---> onResume()方法。当A启动B时，A调用onPause()方法，然后调用新的Activity B，此时调用onCreate()方法 ---> onStart()方法 ---> onResume()方法将新Activity激活。之后A再调用onStop()方法。当A再次回到前台时，B调用onPause()方法，A调用onRestart()方法 ---> onStart()方法 ---> onResume()方法，最后调用B的onStop()方法 ---> onDestory()方法。
3. 弹出Dialog时，调用onCreate()方法 ---> onStart()方法 ---> onResume()方法。

##11、fragment各种情况下的生命周期？

1. 由于Fragment的生命周期与Activity的生命周期有着牵扯，所以把两者的图放到一起作为对比理解。
![FragmentLifecyle](https://img-my.csdn.net/uploads/201211/29/1354170682_3824.png)

2. 接下来就不同情况下的Fragment生命周期做一简单介绍：

Fragment在Activity中replace
新替换的Activity：onAttach() ---> onCreate() ---> onCreatView() ---> onViewCreated ---> onActivityCreated() ---> onStart --->onResume()

被替换的Activity：onPause() ---> onStop() ---> onDestoryView() ---> onDestory() ---> onDetach()

Fragment在Activity中replace，并addToBackStack
新替换的Fragment（没有在BackStack中）：onAttach > onCreate > onCreateView > onViewCreated > onActivityCreated > onStart > onResume

新替换的Fragment（已经在BackStack中）：onCreateView > onViewCreated > onActivityCreated > onStart > onResume

被替换的Fragment：onPause > onStop > onDestroyView

Fragment在ViewPager中切换
我们称切换前的的Fragment称为PreviousFragment，简称PF；切换后的Fragment称为NextFragment，简称NF；其他Fragment称为OtherFragment，简称OF。

（在ViewPager中setUserVisibleHint能反映出Fragment是否被切换到后台或前台，所以在这里也当作生命周期）

如果相关的Fragment没有被加载过：
NF： setUserVisibleHint(false)【用户不可见】 > onAttach > onCreate > setUserVisibleHint(true)【用户可见】 > onCreateView > onViewCreated > onActivityCreated > onStart > onResume

OF跟NF相邻： setUserVisibleHint(false) > onAttach > onCreate > onCreateView > onViewCreated > onActivityCreated > onStart > onResume

如果相关的Fragment已经被加载过：
NF跟PF相邻  ：setUserVisibleHint(true)

NF跟PF不相邻：setUserVisibleHint(true) > onCreateView > onViewCreated > onActivityCreated > onStart > onResume

PF跟NF相邻  ：setUserVisibleHint(false)

PF跟NF不相邻：setUserVisibleHint(false) > onPause > onStop > onDestroyView

OF跟PF相邻：onPause > onStop > onDestroyView

OF跟NF相邻：onCreateView > onViewCreated > onActivityCreated > onStart > onResume

OF夹在PF和NF中间：不调用任何生命周期方法

NF跟PF相邻  ：setUserVisibleHint(true)

NF跟PF不相邻：setUserVisibleHint(true) > onCreateView > onViewCreated > onActivityCreated > onStart > onResume

PF跟NF相邻  ：setUserVisibleHint(false)

PF跟NF不相邻：setUserVisibleHint(false) > onPause > onStop > onDestroyView

OF跟PF相邻：onPause > onStop > onDestroyView

OF跟NF相邻：onCreateView > onViewCreated > onActivityCreated > onStart > onResume

OF夹在PF和NF中间：不调用任何生命周期方法

如果重写了FragmentPagerAdapter的DestroyItem方法，并且相关的Fragment已经加载过：
相互切换时只会调用setUserVisibleHint

Fragment进入了运行状态：
Fragment在进入运行状态时，以下四个生命周期会随它所属的Activity一起被调用：

onPause() ---> onStop() ---> onStart() ---> onResume()

关于Fragment的onActivityResult方法：
使用Fragment的startActivity方法时，FragmentActivity的onActivityResult方法会回调相应的Fragment的onActivityResult方法，所以在重写FragmentActivity的onActivityResult方法时，注意调用super.onActivityResult。

##12、 如何实现Fragment的滑动？
将Fragment与viewpager绑定，通过viewpager中的touch事件，会进行move事件的滑动处理。

Fragment布局

```

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@android:color/holo_red_light">
 
 
    <TextView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:text="Fragment One" />
</LinearLayout>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@android:color/holo_red_light">
 
 
    <TextView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:text="Fragment Two" />
</LinearLayout>
```
Fragment代码：

```

public class FragmentOne extends Fragment {
 
    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, Bundle savedInstanceState) {
       return inflater.inflate(R.layout.fragment_one, container, false);
    }
}
public class FragmentTwo extends Fragment {
 
    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, Bundle savedInstanceState) {
       return inflater.inflate(R.layout.fragment_Two, container, false);
    }
}
```
Viewpager布局：

```

xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.example.spreadtrumshitaoli.fragmentscroll.MainActivity">
 
    <android.support.v4.view.ViewPager
        android:id="@+id/view_pager"
        android:layout_height="match_parent"
        android:layout_width="match_parent"/>
 
</android.support.constraint.ConstraintLayout>
```

MainActivity代码：

```

public class MainActivity extends AppCompatActivity {
 
    private FragmentOne fragmentOne;
    private FragmentTwo fragmentTwo;
 
    private ViewPager viewPager;
 
    private ArrayList<Fragment> mFragmentList = new ArrayList <Fragment>();
    private FragmentPagerAdapter fragmentPagerAdapter;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        init();
 
    }
 
    private void init() {
 
        viewPager = (ViewPager) findViewById(R.id.view_pager);
        fragmentOne = new FragmentOne();
        fragmentTwo = new FragmentTwo();
 
        mFragmentList.add(fragmentOne);
        mFragmentList.add(fragmentTwo);
 
//将adapter和fragment绑定在一起。
        fragmentPagerAdapter = new FragmentPagerAdapter(getSupportFragmentManager()) {
            @Override
            public Fragment getItem(int i) {
                return mFragmentList != null ? mFragmentList.get(i) : null;
            }
 
            @Override
            public int getCount() {
                return mFragmentList != null ? mFragmentList.size() : 0;
            }
        };
        viewPager.setAdapter(fragmentPagerAdapter);
        viewPager.setOnPageChangeListener(new ViewPager.OnPageChangeListener() {
            @Override
            public void onPageScrolled(int i, float v, int i1) {
 
            }
 
            @Override
            public void onPageSelected(int i) {
                //TODO:
            }
 
            @Override
            public void onPageScrollStateChanged(int i) {
 
            }
        });
 
    }
 
}
```
在这段代码中，我们

首先fragment以及viewpager都实例化；

再将fragment添加到泛型arraylist里；

最后将带有fragment的arraylist和adapter绑定。

##13、fragment之间传递数据的方式？

###方法一：

1. 在MainFragment中设置一个setData()方法，在方法中设置更改按钮名称;
//MainFragment.java文件中

```

public void setData(String string) {

bt_main.setText(string);

}
```

2. 在MenuFragment中的ListView条目点击事件中通过标签获取到MainFragment，并调用对应的setData()方法，将数据设置进去，从而达到数据传递的目的。

```

lv.setOnItemClickListener(new AdapterView.OnItemClickListener() {

@Override

public void onItemClick(AdapterView<?> parent, View view, int position, long id) {

MainFragment mainFragment =

(MainFragment) getActivity()

.getSupportFragmentManager()

.findFragmentByTag("mainFragment");

mainFragment.setData(mDatas.get(position));

}

});
```
只需上面区区两步即可达到数据传递的目的。
###方法二：
采取接口回调的方式进行数据传递。

step1: 在Menuragment中创建一个接口以及接口对应的set方法：

```
 
//MenuFragment.java文件中

public interface OnDataTransmissionListener {

public void dataTransmission(String data);

}

public void setOnDataTransmissionListener(OnDataTransmissionListener mListener) {

this.mListener = mListener;

}
```
step2: 在MenuFragment中的ListView条目点击事件中进行接口进行接口回调

```
 
//MenuFragment.java文件中

lv.setOnItemClickListener(new AdapterView.OnItemClickListener() {

@Override

public void onItemClick(AdapterView<?> parent, View view, int position, long id) {

/**

* 方法二：采取接口回调的方式进行数据传递

*/

if (mListener != null) {

mListener.dataTransmission(mDatas.get(position));

}

}

});
```

step3: 在MainActivity中根据menuFragment获取到接口的set方法，在这个方法中进行进行数据传递，具体如下：

//在MainActivity.java中

```

menuFragment.setOnDataTransmissionListener(new MenuFragment.OnDataTransmissionListener() {

@Override

public void dataTransmission(String data) {

mainFragment.setData(data); //注：对应的mainFragment此时应该要用final进行修饰

}

});

```
通过上面的三步也可以轻松做到Fragment数据之间的传递。

###方法三：

使用三方开源框架：EventBus
那么问题来了：EventBus是个啥东西？？？
简单来说，EventBus是一款针对Android优化的发布/订阅（publish/subscribe）事件总线。主要功能是替代Intent,Handler,BroadCast在Fragment，Activity，Service，线程之间传递消息。简化了应用程序内各组件间、组件与后台线程间的通信。优点是开销小，代码更优雅，以及将发送者和接收者解耦。比如请求网络，等网络返回时通过Handler或Broadcast通知UI，两个Fragment之间需要通过Listener通信，这些需求都可以通过EventBus实现。
下面我们就用EventBus来实现以下Fragment之间的数据传递：

step1：引入EventBus


```
compile 'org.greenrobot:eventbus:3.0.0'

```
step2：注册事件接收者
这里MainFragment是要接收MenuFragment发送来的数据，所以我们在MainFragment中的onCreateView()方法中进行注册：

```

EventBus.getDefault().register(this);

```
step3：发送事件
注：发送事件之前其实还有一步定义事件类型，这里我们传递的数据只有一个类型，所以这一步取消了。
MenuFragment发送数据给MainFragment，所以我们在MenuFragment中将要传递的数据进行发送事件操作：

 
```

lv.setOnItemClickListener(new AdapterView.OnItemClickListener() {

@Override

public void onItemClick(AdapterView<?> parent, View view, int position, long id) {

EventBus.getDefault().post(mDatas.get(position));

}

});

```
step4：接收消息并处理
在MainFragment中我们接收来自MenuFragment传递过来的数据,并进行对应的处理（注：EventBus 3.0版本这一步必须要写注解@Subscribe (与2.4版本有所区别)）：


```

@Subscribe

public void onEvent(String data) {

bt_main.setText(data);

}

```
通过上面这一步即可完成数据之间的传递，需要注意的是在销毁的时候我们要注销事件接收。
step5：注销事件接收

 
//MainFragment.java中


```
@Override

public void onDestroy() {

super.onDestroy();

EventBus.getDefault().unregister(this);

}

```
以上五步完成了Fragment之间的数据传递，看似比上面两个方法要复杂的多，但当我们涉及到复杂的Fragment之间数据传递（例如Fragment中嵌套多层Fragment）时，就会体会到EventBus的爽快之处~~~这里不进行赘述了。

##14、Activity 怎么和Service 绑定？

这需要实现service中的onBind()函数以返回service实例给activity

1. 创建service类和activity类。

2. 在service类中定义一个内部类继承自Binder()类：

```

public class MyBinder extends Binder{  

        public Service1 getService(){  

            return Service1.this;  

        }  

    }  
    
```
实例化onBind()方法：


```

private final IBinder binder = new MyyBinder();

@Override

public IBinder onBind(Intent intent){

        Log.i(LOG，"onBind......");

        return binder;

}

```

3. 在activity中完成绑定

```

Intent intent = new Intent(Activity1.this,Activity2.class);

          bindService(intent,conn,Context.BIND_AUTO_CREATE);

```

bindService的第二个参数是一个ServiceConnection类型的参数。service和其他组件之间的连接都表示为一个ServiceConnection，要想将service和其他组件进行绑定，就需要实现一个新的ServiceConnection。

```

public ServiceConnection conn= new ServiceConnection() {  

          

        @Override  

        public void onServiceDisconnected(ComponentName name) {  

//当连接意外断开时调用

            Log.i(LOG, "onServiceDisconnected>>>>>>>>");  

            myservice = null;  

        }  

          

        @Override  

        public void onServiceConnected(ComponentName name, IBinder service) {  

//当建立连接时调用

            Log.i(LOG, "onServiceConnected>>>>>>>>");  

            myservice = ((Service1.MyBinder)service).getService();  

        }  

    }; 

```

bindService的第三个参数是一个flag。
   可以使用的flag有：

BIND_AUTO_CREATE：绑定完成后就启动目标service

BIND_DEBUG_UNBIND：这只在debug时使用，跟unbind有关。

BIND_NOT_FOREGROUND：确保被绑定的service永远不会有运行于前台的优先级，因为默认情况下，绑定一个service会提高它的优先级

BIND_ABOVE_CLIENT：确保客户端处于前台时，绑定的service也变为前台进程

BIND_ALLOW_OOM_MANAGEMENT：允许系统在低内存等状态下删除该service（这是自己对源码中注释的理解）

BIND_WAIVE_PRIORITY：绑定service时不改变其优先级

BIND_ADJUST_WITH_ACTIVITY：系统根据service所绑定的activity的重要程度来调整这个service的优先级。

##15、service生命周期？

###Service分为两种：

- 本地服务，属于同一个应用程序，通过startService来启动或者通过bindService来绑定并且获取代理对象。
如果只是想开个服务在后台运行的话，直接startService即可，如果需要相互之间进行传值或者操作的话，就应该通过bindService。

- 远程服务（不同应用程序之间），通过bindService来绑定并且获取代理对象。

1. 被启动的服务(startService())的生命周期。
             如果一个Service被某个Activity 调用Context.startService() 方法启动，那么不管是否有Activity使用bindService()绑定或unbindService()解除绑定到该Service，该Service都在后台运行。如果一个Service被多次执行startService()，它的onCreate()方法只会调用一次，也就是说该Service只会创建一个实例，而它的onStartCommand()将会被调用多次(对应调用startService()的次数)。该Service将会一直在后台运行，直到被调用stopService()，或自身的stopSelf方法。当然如果系统资源不足，系统也可能结束服务。
2. 被绑定的服务(bindService())的生命周期。
             如果一个Service被调用 Context.bindService ()方法绑定启动，不管调用bindService()调用几次，onCreate()方法都只会调用一次，而onStartCommand()方法始终不会被调用，这时会调用onBind()方法。当连接建立之后，Service将会一直运行，除非调用Context.unbindService() 断开连接或者之前调用bindService() 的 Context 不存在了(如该Activity被finish)，系统将会自动停止Service，对应onDestroy()将被调用。
3. 被启动又被绑定的服务的生命周期。
             如果一个Service又被启动又被绑定，则该Service将会一直在后台运行。调用unbindService()将不会停止Service，而必须调用stopService()或Service的stopSelf()方法来停止服务。
4. 当服务被停止时清除服务。
            当一个Service被终止时，Service的onDestroy()方法将会被调用，在这里应当做一些清除工作，如停止在Service中创建并运行的线程等。
            
###注意
Service默认是运行在main线程的，因此Service中如果需要执行耗时操作（大文件的操作，数据库的拷贝，网络请求，文件下载等）的话应该在子线程中完成。

！特殊情况是：Service在清单文件中指定了在其他进程中运行。

##16、 activity和service的绑定方式以及怎么在Activity 中启动自己对应的Service？

1、activity能进行绑定得益于Serviece的接口。为了支持Service的绑定，实现onBind方法。

2、Service和Activity的连接可以用ServiceConnection来实现。需要实现一个新的ServiceConnection，重写onServiceConnected和OnServiceDisconnected方法，一旦连接建立，就能得到Service实例的引用。

3、执行绑定，调用bindService方法，传入一个选择了要绑定的Service的Intent(显示或隐式)和一个你实现了的ServiceConnection的实例

##17、Service的启动方式？

采用Context.startService()方法启动服务，在服务未被创建时，系统会先调用服务的onCreate()方法，接着调用onStart()方法。如果调用startService()方法前服务已经被创建，多次调用startService()方法并不会导致多次创建服务，但会导致多次调用onStart()方法。采用startService()方法启动的服务，只能调用Context.stopService()方法结束服务，服务结束时会调用onDestroy()方法。


采用Context.bindService()方法启动服务，在服务未被创建时，系统会先调用服务的 onCreate()方法，接着调用onBind()方法。这个时候调用者和服务绑定在一起，调用者退出了，系统就会先调用服务的onUnbind()方 法，接着调用onDestroy()方法。如果调用bindService()方法前服务已经被绑定，多次调用bindService()方法并不会导致 多次创建服务及绑定(也就是说onCreate()和onBind()方法并不会被多次调用)。如果调用者希望与正在绑定的服务解除绑定，可以调用 unbindService()方法，调用该方法也会导致系统调用服务的onUnbind()-->onDestroy()方法。 

##18、谈谈ContentProvider、ContentResolver、ContentObserver之间的关系？

ContentProvider：
* 四大组件的内容提供者，主要用于对外提供数据

* 实现各个应用程序之间的（跨应用）数据共享，比如联系人应用中就使用了ContentProvider，你在自己的应用中可以读取和修改联系人的数据，不过需要获得相应的权限。其实它也只是一个中间人，真正的数据源是文件或者SQLite等

* 一个应用实现ContentProvider来提供内容给别的应用来操作，通过ContentResolver来操作别的应用数据，当然在自己的应用中也可以

ContentResolver：
* 内容解析者，用于获取内容提供者提供的数据

* ContentResolver.notifyChange（uri）发出消息

ContentObserver：
* 内容监听器，可以监听数据的改变状态

* 目的是观察（捕捉）特定Uri引起的数据库的变化，继而做一些相应的处理，它类似于数据库技术中的触发器（Trigger），当ContentObserver所观察的Uri发生变化时，便会触发它。触发器分为表触发器、行触发器，相应地ContentObsever也分为表ContentObserver、行ContentObserver，当然这是与它所监听的Uri MIME Type有关的

* ContentResolver.registerContentObserver()监听消息

##19、广播的分类

- 无序广播：通过Context.sendBroadcast()方法来发送，它是完全异步的。所有的receivers（接收器）的执行顺序不确定，因此所有的receivers（接收器）接收broadcast的顺序不确定。这种方式效率更高，但是BroadcastReceiver无法使用setResult系列、getResult系列及abortbroadcast（中止）系列API。广播不能被终止,数据不能被修改。
- 有序广播：即从优先级别最高的广播接收器开始接收，接收完了如果没有丢弃，就下传给下一个次高优先级别的广播接收器进行处理，依次类推，直到最后。如果多个应用程序设置的优先级别相同，则谁先注册的广播，谁就可以优先接收到广播。通过Context.sendOrderedBroadcast()方法来发送，sendOrderedBroadcast(intent, receiverPermission, resultReceiver, scheduler, initialCode, initialData, initialExtras);，其中的参数resultReceiver，可以自己重写一个类，作为一个最终的receive 最后都能够接收到广播，最终的receiver 不需要再清单文件里面配置，initialData可以作为传输的数据。广播可以被终止，数据传输过程中可以被修改。

##19、注册广播接收者两种方式的区别

- 静态注册：在manifest文件中进行注册，静态注册的广播不管你的程序有没有启动有会起作用
- 动态注册：在java代码中进行注册，程序动态注册的接收者只在程序运行过程中有效

##20、Android 动画的分类

- 属性动画：属性动画是在Android3.0之后提供的动画，可以对一个对象的属性进行操作，从而达到动画的效果，包括没有被渲染到屏幕上的，该系统是可扩展的，让您可以将定制类型的属性进行动画化。
- 帧动画：可绘制的动画需要一个接一个地显示可绘制的资源，就像胶卷一样。如果您希望用可绘制的资源来表示更容易表示的动画，比如位图的进展，那么这种动画方法是非常有用的。



- 视图动画：视图动画是较老的系统，只能用于视图，设置和提供足够的能力来满足许多应用程序的需求是相对容易的。在3.0之前，视图动画是被广泛应用，但3.0后属性动画的出现，使得视图动画的缺点也就被放大了。主要一个缺点就是当视图移动后，其响应事件的位置还依然在动画前的地方，所以视图动画只能做普通的动画展示效果，避免交互的发生。主要提供了AlphaAnimation,RotateAnimation,TranslateAnimation,ScaleAnimation四种动画方式，也提供了AnimationSet动画集合，混合使用多种动画。

###注意：常规分类一般不包含视图动画

- 视图动画与属性动画的区别：视图动画只能支持简单的缩放、平移、旋转、透明度基本的动画，且有一定的局限性。视图动画系统的另一个缺点是它只修改了视图被绘制的地方，而不是实际的视图本身。例如对一个imageview添加事件后，平移那么事件还是会在之前的位置。属性动画将不存在这些限制，您可以对任何对象(视图和非视图)的任何属性进行动画，并且对象本身实际上是被修改的。视图动画的使用比较简单。

##21、一条最长的短信约占多少字节

中文70（包括标点），英文160个字节

##22、Handler消息机制：
[Android Handler机制](https://www.jianshu.com/p/3d8f7ec1017a)
### 为什么要使用Handler？
- 因为屏幕的刷新频率是60Hz，大概16毫秒会刷新一次，所以为了保证UI的流畅性，耗时操作需要在子线程中处理，子线程不能直接对UI进行更新操作。因此需要Handler在子线程发消息给主线程来更新UI。
这里再深入一点，Android中的UI控件不是线程安全的，因此在多线程并发访问UI的时候会导致UI控件处于不可预期的状态。Google不通过锁的机制来处理这个问题是因为：

1. 引入锁会导致UI的操作变得复杂
2. 引入锁会导致UI的运行效率降低

- 因此，Google的工程师最后是通过单线程的模型来操作UI，开发者只需要通过Handler在不同线程之间切花就可以了。

### 概述一下Android中的消息机制？
Android中的消息机制主要是指Handler的运行机制。Handler是进行线程切换的关键，在主线程和子线程之间切换只是一种比较特殊的使用情景而已。其中消息传递机制需要了解的东西有Message、Handler、Looper、Looper里面的MessageQueue对象。

如上图所示，我们可以把整个消息机制看作是一条流水线。其中：

![Handler](/Users/isos/Study/Study-Android/Handler.png)


- MessageQueue是传送带，负责Message队列的传送与管理
- Looper是流水线的发动机，不断地把消息从消息队列里面取出来，交给Handler来处理
- Message是每一件产品
- Handler就是工人。但是这么比喻不太恰当，因为发送以及最终处理Message的都是Handler

###为什么在子线程中创建Handler会抛异常？
Handler的工作是依赖于Looper的，而Looper（与消息队列）又是属于某一个线程（ThreadLocal是线程内部的数据存储类，通过它可以在指定线程中存储数据，其他线程则无法获取到），其他线程不能访问。因此Handler就是间接跟线程是绑定在一起了。因此要使用Handler必须要保证Handler所创建的线程中有Looper对象并且启动循环。因为子线程中默认是没有Looper的，所以会报错。



正确的使用方法是：
```

    Handler handler = null;
    new Thread(new Runnable() {

        private Looper mLooper;

        @Override
        public void run() {
            //必须调用Looper的prepare方法为当前线程创建一个Looper对象，然后启动循环
            //prepare方法中实质是给ThreadLocal对象创建了一个Looper对象
            //如果当前线程已经创建过Looper对象了，那么会报错
            Looper.prepare();
            handler = new Handler();
            //获取Looper对象
            mLooper = Looper.myLooper();
            //启动消息循环
            Looper.loop();

            //在适当的时候退出Looper的消息循环，防止内存泄漏
            mLooper.quit();
        }
    }).start();
```

####主线程中默认是创建了Looper并且启动了消息的循环的，因此不会报错：
应用程序的入口是ActivityThread的main方法，在这个方法里面会创建Looper，并且执行Looper的loop方法来启动消息的循环，使得应用程序一直运行。

###子线程中可以通过Handler发送消息给主线程吗？
可以。有时候出于业务需要，主线程可以向子线程发送消息。子线程的Handler必须按照上述方法创建，并且关联Looper。


##23、ThreadLocal

- ThreadLocal类用来提供线程内部的局部变量，这种变量在多线程环境下访问(通过get或set方法访问)时能保证各个线程里的变量相对独立于其他线程内的变量。ThreadLocal实例通常来说都是private static 类型，用于关联线程。
- Android中的ThreadLocal和Java 的ThreadLocal是一致的，每个线程都有自己的局部变量，一个线程的本地变量对其他线程是不可见的，ThreadLocal不是用与解决共享变量的问题，不是为了协调线程同步而存在，而是为了方便每个线程处理自己的状态而引入的一个机制

###ThreadLocal共有三个成员变量

- reference：通过弱引用存储ThreadLocal本身，主要是防止线程自身所带的数据都无法释放，避免OOM；
- hashCounter：是线程安全的加减操作，getAndSet(int newValue)取当前的值，并设置新的值；
- hash：里面的0x61c88647 * 2的作用是：在Value存在数据的主要存储数组table上，而table被设计为下标为0，2，4...2n的位置存放key，而1，3，5...(2n-1)的位置存放的value，0x61c88647*2保证了其二进制最低位为0，也就是在计算key的下标时，一定是偶数位。

##24、什么是嵌入式操作系统，Android操作系统属于实时操作系统吗？

嵌入式实时操作系统是指当外界事件或数据产生时，能够接受并以足够快的速度予以处理，其处理的结果又能在规定的时间之内来控制生产过程或对处理系统作出快速响应，并控制所有实时任务协调一致运行的嵌入式操作系统。主要用于工业控制、 军事设备、 航空航天等领域对系统的响应时间有苛刻的要求，这就需要使用实时系统。又可分为软实时和硬实时两种，而android是基于linux内核的，因此属于软实时。

##25、Android中的线程与线程，进程与进程之间是如何通信的？

1. 一个 Android 程序开始运行时，会单独启动一个Process。默认情况下，所有这个程序中的Activity或者Service都会跑在这个Process。默认情况下，一个Android程序也只有一个Process，但一个Process下却可以有许多个Thread。
2. 一个 Android 程序开始运行时，就有一个主线程Main Thread被创建。该线程主要负责UI界面的显示、更新和控件交互，所以又叫UI Thread。一个Android程序创建之初，一个Process呈现的是单线程模型--即Main Thread，所有的任务都在一个线程中运行。所以，Main Thread所调用的每一个函数，其耗时应该越短越好。而对于比较费时的工作，应该设法交给子线程去做，以避免阻塞主线程（主线程被阻塞，会导致程序假死 现象）。
3. Android单线程模型：Android UI操作并不是线程安全的并且这些操作必须在UI线程中执行。如果在子线程中直接修改UI，会导致异常。

##26、Android dvm的进程和Linux的进程, 应用程序的进程是否为同一个概念

DVM指dalivk的虚拟机。每一个Android应用程序都在它自己的进程中运行，都拥有一个独立的Dalvik虚拟机实例。而每一个DVM都是在Linux 中的一个进程，所以说可以认为是同一个概念。

##27、sim卡的EF 文件有何作用

sim卡的文件系统有自己规范，主要是为了和手机通讯，sim本身可以有自己的操作系统，EF就是作存储并和手机通讯用的。

##28、让Activity变成一个窗口：Activity属性设定

讲点轻松的吧,可能有人希望做出来的应用程序是一个漂浮在手机主界面的东西，那么很 简单你只需要设置 一下Activity的主题就可以了在AndroidManifest.xml 中定义 Activity的 地方一句话： 
　　Xml代码 
　　
```
　　1. android :theme="@android:style/Theme.Dialog" 
```

　　这就使你的应用程序变成对话框的形式弹出来了，或者 

　　Xml代码 

```
　　1. android:theme="@android:style/Theme.Translucent" 
```

　　就变成半透明的，[友情提示-.-]类似的这种activity的属性可以在android.R.styleable 类的AndroidManifestActivity 方法中看到，AndroidManifest.xml中所有元素的属性的介绍都可以参考这个类android.R.styleable 

　　上面说的是属性名称，具体有什么值是在android.R.style中 可以看到，比如这个"@android:style/Theme.Dialog" 就对应于android.R.style.Theme_Dialog ,('_'换成'.' < --注意：这个是文章内容不是笑脸)就可以用在描述文件 中了,找找类定义和描述文件中的对应关系就都明白了。
　　
##28、如何将SQLite数据库(dictionary.db文件)与apk文件一起发布

可以将dictionary.db文件复制到Eclipse Android工程中的res aw目录中。所有在res aw目录中的文件不会被压缩，这样可以直接提取该目录中的文件。可以将dictionary.db文件复制到res aw目录中。

##29、如何打开res aw目录中的数据库文件

在Android中不能直接打开 res aw 目录中的数据库文件，而需要在程序第一次启动时将文件复制到手机内存或SD卡的中，然后再打开该数据库文件。复制的基本方法是使用getResources().openRawResource方法获得res aw 目录中资源的 InputStream 对象，然后将该 InputStream 对象中的数据写入其它的目录中相应的文件中。在Android SDK 中可以使用 SQLiteDatabase.openOrCreateDatabase 方法来打开任意目录中的SQLite数据库文件。

##30、在Android中mvc的具体体现
Android 的官方建议应用程序的开发采用 MVC 模式，何谓 MVC？
MVC 是 Model、View、Controller 的缩写，总共分为三部分:

- 模型（Model）对象：是应用程序的主题部分，所有的业务逻辑都应该写在该层；
- 视图（View）对象：是应用程序中负责生成用户界面的部分。也是在整个 MVC 架构中用户唯一可以看到的一层，接收用户的输入，显示处理接口；
- 控制器（Control）对象：是根据用户的输入，控制用户界面数据显示及跟新Model对象状态的部分，控制器更重要的一种导航功能，像用户发出的相关事件，交给 M 来处理。

###Android 鼓励弱耦合的组件的重用，在Android中 MVC 的具体体现如下：

- 试图层（View）：一般采用xml文件进行界面的描述，使用的时候可以非常方便的引入，如果你对Android了解的比较多的话，就一定可以想到在Anndroid中也可以是JavaScript+HTML的方式作为View层，当然这里需要进行 Java 和 JavaScript 之间的通信，幸运的是，Android 提供了它们之间非常方便的通信实现。
- 控制层（Controller）：Android控制层的重任通常落在了众多的Activity上，这句话也就暗含了布套在Activity中写代码，要通过Activity交个Model业务逻辑层处理，这样做的另外一个原因是Android中的Activity的响应时间是5s，如果耗时的操作放在这里，程序就很容易被回收掉。
- 模型层（Model）：对数据库的操作、对网络等的操作都应该在Model里边处理，当然对业务计算等操作也是必须放在该层的。

##31、Android系统架构

###Android系统架构和其操作系统一样，采用了分层的架构。从架构图看，Android分为四个层，从高层到底层分别是应用程序层、应用程序框架层、系统运行层和Linux核心层。

###1、应用程序层

- Android 会同一系列核心应用程序包一起发布，该应用程序包括 Email 客户端，SMS短消息程序包，日历，地图，浏览器，联系人管理程序等。所有的应用程序都是使用Java语言编写的。

###2、应用程序框架层

开发人员也可以完全访问核心应用程序所使用的 API 框架。该应用程序的架构设计简化了组件的重用；任何一个应用程序都可以发布它的功能块并且任何其它的应用程序都可以使用其所发布的功能块（不过得遵循框架的安全性限制）。同样，该应用程序重用机制也使用可以方便的替换程序组件。

隐藏在每个应用后面的是一系列的服务和系统，其中包括；

- 丰富而又可扩展的视图（Views），可以用来构建应用程序，它包括列表（lists）， 网格（grids），文本框（text boxes），按钮（buttons），甚至可嵌入的 web 浏览器。
- 内容提供者（ContentProvider）使得应用程序可以访问另一个应用程序的数据（例如：联系人数据库），或者共享他们自己的数据
- 资源管理器（Resource Manager）提供非代码资源的访问，入本地字符串，图形和布局文件（Layout Files）。
- 通知管理器（Notification Manager）使得应用程序可以在状态栏中显示自定义的提示信息。
- 活动管理器（Activity Manager）用来管理应用程序生命周期并提供常用的导航回退功能。

###3、系统运行库

####程序库
Android 包含一些 C/C++库，这些库能被 Android 系统中不同的组件使用。他们通过 Android 应用程序框架为开发者提供服务。以下是一些核心库：

- 系统 C 库：一个从 BSD 继承来的标准 C 系统函数库（libc），它是专门为基于 embedded Linux 的设备定制的；
- 媒体库：基于 PacketVide OpenCORE；该库支持多种常用的音频、视频格式回放和录制，同时支持静态图像文件。编码格式包括 MPEG4，H.264，MP3，AAC，AMR，JPG，PNG。
- Surface Manager：对显示子系统的管理，并且为多个应用程序提供了2D和3D图层的无缝融合。
- LibWebCore：一个最新的 web 浏览器引擎，支持 Android 浏览器和一个可嵌入的 web 视图。
- SGL：底层的 2D 图形引擎。
- 3D libraries：基于 OpenGl ES 1.0 APIs；该库可以使用硬件 3D 加速（如果可用）或者使用高度优化的 3D 软加速。
- FreeType：位图（bitmap）和矢量（vector）字体显示。
- SQLite：一个对于所有应用程序可用，功能强劲的轻型关系型数据库引擎。

####Android 运行库

- Android 包括了一个核心库，该核心库提供了 Java 变成语言核心库的大多数功能。
- 每一个 Android 应用程序都在它自己的进程中运行，都拥有一个独立的 Dalvik 虚拟机事例。Dalvik 被设计成一个设备可以同时搞笑地运行多个虚拟系统。Dalvik虚拟机执行（.dex）的 Dalvik 可执行文件，改格式文件针对小内存使用做了优化。同时虚拟机是基于寄存器的，所有的类都经由 Java 编译器编译，然后通过 SDK 中的 “dx” 工具转化成 .dex 格式由虚拟机执行。
- Dalvik 下半年及依赖于 Linux 内核的一些功能，比如线程机制和底层内存管理机制。

###4、Linux 内核

Android 的核心系统服务依赖于 Linux 2.6 的内核，如安全性，内存管理，进程管理，网络协议栈和驱动模型。Linux 内核也同时作为硬件和软件栈之间的抽象层。

##32、Android 常用控件

###1、单选框（RadioButton与RadioGroup）：

- RadioGroup 用于对单选框进行分组，相同组内的单选框只有一个单选框被选中；
- 事件：setOnCheckedChangeListener(),处理单选框被选择事件。把 RadioGroup.OnCheckedChangeListener 实例作为参数掺入。

###多选框（CheckBox）：

- 每个多选框都是独立的，可以通过迭代所有的多选框，然后根据其状态是否被选中在获取其值。
- 事件：setOnCheckChangeListener处理多选框被选择事件。把 CompoundButton.OnCheckChangeListener 实例作为参数传入

###下拉列表框(Spring)：

- Spinner.getItemAtPosition(Spinner.getSelectedItemPosition());获取下拉列表框的值。
- 事件：setOnItemSelectedListener(),处理下拉列表框被选择事件把AdapterView.OnItemSelectedListener实例作为参数传入；

###拖动条(SeekBar)：

- SeekBar.getProgress()获取拖动条当前值
- 事件:setOnSeekBarChangeListener()，处理拖动条值变化事件，把SeekBar.OnSeekBarChangeListener实例作为参数传入。

###菜单(Menu):

- 重写Activity的onCreatOptionMenu(Menu menu)方法，该方法用于创建选项菜单，咋用户按下手机的"Menu"按钮时就会显示创建好的菜单，在onCreatOptionMenu(Menu Menu)方法内部可以调用Menu.add()方法实现菜单的添加。
- 重写Activity的onMenuItemSelected()方法，该方法用于处理菜单被选择事件。

###进度对话框(ProgressDialog)：

- 创建并显示一个进度对话框：ProgressDialog.show(ProgressDialogActivity.this,"请稍等"，"数据正在加载中...."，true)；
- 设置对话框的风格：setProgressStyle()
- ProgressDialog.STYLE_SPINNER  旋转进度条风格(为默认风格)
- ProgressDialog.STYLE_HORIZONTAL 横向进度条风格


##33、Android常用的5种布局

Android 布局是应用界面开发的重要一环，在 Android 中，共有5中布局方式，分别是：FrameLayout（框架布局），LinearLayout（线性布局），AbsoluteLayout（绝对布局），RelativeLayout （相对布局），TableLayout（表格布局）。

###FrameLayout

- 这个布局可以看成是墙角堆东西，有一个四方的矩形的左上角墙角，我们在放了第一个东西，要再放一个，那就放在原来放的位置的上面，这样一次的放，会盖住原来的东西。这个布局比较简单，也只能放一点比较简单的东西。

###LinearLayout

- 线性布局，这个东西，从外框上可以理解为一个 div，他首先是一个一个从上往下罗列在屏幕上。每一个 LinearLayout 里面又可分为垂直布局（android:orientation="vertical")和水平布局（android:orientation="horizontal")。当垂直布局时，每一行就只有一个元素，多个元素一次垂直往下；水平布局时，只有一行，每一个元素一次向右排列。
- LinearLayout 中有一个重要的属性 android:layout_weight="1"，这个weight在垂直布局时，代表行距；水平的时候代表列宽；weight值越大就越大

###AbsoluteLayout

- 绝对布局由于 div 指定了 absolute 属性，用X，Y坐标和指定元素的位置，android:layout_x="20px",android:layout_y="12px"，这种布局方式也比较简单，但是在垂直随便切换是，往往会出问题，而且多个元素的时候，计算也比较麻烦。

###RelativeLayout

- 相对布局可以理解为某一个元素为参照物，来定位的布局方式。主要属性有：相对于某一个元素    

    android:layout_below="@id/aaa" 该元素在 id为aaa的下面    

    android:layout_toLeftOf="@id/bbb" 改元素的左边是bbb    

     相对于父元素的地方    

     android:layout_alignParentLeft="true"  在父元素左对齐    

     android:layout_alignParentRight="true" 在父元素右对齐
     
###TableLayout

- 表格布局类似Html里面的Table。每一个TableLayout里面有表格行TableRow，TableRow里面可以具体定义每一个元素，设定他的对齐方式 android:gravity="" 。
- 每一个布局都有自己适合的方式，另外，这五个布局元素可以相互嵌套应用，做出美观的界面。

##34、ListView优化

###工作原理

ListView 针对 List 中每个 Item。要求 adapter “给我一个是”（getView）。
一个新的视图被返回并显示。

###如果我们有上亿个项目要显示怎么？为每个项目创建一个新视图？

实际上Android为你缓存了视图。
Android 中有个叫做 Recycler 的构建。

1. ListView 先请求一个 type1 视图（getView）然后请求其它可见的项目。convertView 在 getView 中是空的。
2. 当 item1 滚出屏幕，并且一个新的项目从屏幕底端上来时，ListView 再请求一个 type1 视图。convertView 此时不是空值了，它的值是 item1。你只需设定新的数据然后返回 convertView，不必重新创建一个视图。

##35、设计模式和IoC（控制反转）

Android 框架魅力的源泉在于 IoC，在开发 Android 的过程中你会时刻感受到 Ioc 带来的巨大方便，就拿 Activity 来说，下面的函数是框架调用自动调用的：

```
protected void onCreate(Bundle savedInstanceState)；
```

不是程序编写者主动去调用，反而是用户写的代码被框架调用，这也就反转了！当然 Ioc 本身的内涵远远不止这些，但是从这个例子中也可以窥视出 IoC 带来的巨大好处。此类的例子在 Android 随处可见，例如说数据库的管理类，例如说 Android 中 SAX 的 Handler 的调用等。有时间，甚至需要自己编写简单的 IoC 实现，上面展示的多线程现在就是一个说明。

##36、Android 中长度单位详解

现在这里介绍一下 dp 和 sp。dp 就是 dip。这个和 sp 基本类似。如果设置表示长度、高度等属性时可以使用 dp 或 sp。但如果设置字体，需要使用 sp。dp 是与密度无关，sp 除了与密度无关外，还与 scale 无关。如果屏幕密度为160，这时 dp 、sp 和 px 是一样的，1dp=1sp=1px。 但是如果使用px做单位，如果屏幕大小不变（假设还是3.2寸），而屏幕密度变成了320。那么原来TextView的宽度设成160px，在密度为320的3.2寸屏幕里看要比在密度为160的3.2寸屏幕上看短了一半。但如果设置成160dp或者160sp的话。系统会自动将width属性值设置成320px的。也就是160*320/160。其中320/160可称为密度比例因子。也就是说，如果使用dp和sp，系统会根据屏幕密度的变化自动进行转换。

下面看一下其它单位的含义

- px：表示屏幕时机的像素。例如，320*480的屏幕在横向有320个像素，在纵向有480个像素。
- in：表示英寸，是屏幕的物理尺寸。每英寸等于2.54厘米。例如，形容手机屏幕大小，经常说3.2（英）寸、3.5（英）寸、4（英）寸就是指这个单位。这些尺寸是屏幕的对角线长度。如果手机的屏幕是3.2英寸，表示手机的屏幕（可视区域）对角线长度是3.2*2.54=8.128厘米。

##36、ANR简介

- ANR：Application Not Responding，5秒
- 在 Android 中，活动管理器和窗口管理器这两个系统服务负责监视应用程序的响应。当出现下列情况时，Android 就会显示 ANR 对话框了：

对输入事件（如按键、触摸屏事件）的响应应超过5秒
意向接收器（IntentReceiver）超过10s中仍未执行完毕

- Android应用程序完全运行在一个独立的线程中（例如main）。这就意味着，任何在主线程中运行的，需要消耗大量时间的操作都会引发 ANR。因为此时，你的应用程序已经没有机会去响应输入事件和意向广播（Intent broadcast）。
- 因此，任何运行在主线程中的方法，都要尽可能的只做少量的工作。特别是活动生命周期中的重要方法，如onCreate和onResume等更应如此。潜在的比较耗时的操作，如访问网络和数据库；或者是开销很大的计算，比如改变位图的大小，需要再一个单独的子线程中完成（或者是使用异步请求，如数据库操作）。但这并以为着你的主线程需要进入阻塞状态已等待子线程结束--也不要调用Thread.wait或者Thread.sleep方法。取而代之的是，主线程为子线程提供一个句柄（Handler），让子线程在即将结束的时候调用它。使用这种方法涉及你的应用程序，能够保证你的程序对输入保持良好的响应，从而避免因为输入事件超过5秒不被处理而产生的ANR。这种实践需要应用到所有显示用户界面的线程，因为他们都面临这同样的超时问题。

##37、Android Intent

- 在一个Android应用中，主要是由一些组件组成的，（Activity、Service、ContentProvider、BroadcastReceiver）在这些组件之间的通讯，由Intent协助完成。
- Intent 负责对应用中一次操作的动作、动作涉及数据、附加数据进行描述，Android则根据此Intent的描述，负责找到对应的组件，将Intent传递给调用的组件，并完成组件的调用。Intent在这里起着实现调用者与被调用者之间的解耦作用。
- Intent传递过程中，要找到目标消费者（另一个 Activity、IntentReceiver或者Service），也就是Intent的响应者，有两种方法来匹配：
- 1、显示匹配，启动时指定对应的组件，例如：context.startActivity(new Intent(context, TestActivity.class));
- 隐式匹配：隐式匹配，首先要匹配Intent的几项值：Action，Category，Data/Type，Component（如果填写了Component就是上例中的TestActivity.class)，这就形成了显示匹配。所以此部分只降前几种匹配。匹配规则为最大匹配规则。
- 1、如果你填写了Action，如果有一个程序的Manifest.xml中的某一个Activity的IntentFilter中定义了包含了相同的Action，那么这个Intent就与这个目标Action匹配，如果这个Filter中没有定义Type/Category，那么这个Activity就匹配了。但是如果手机中有两个以上的程序匹配，那个就会弹出一个对话框来提示说明。Action的中值在Android中有很多预定义，如果你想直接转到你自己定义的Intent接收者，可以再接收者的IntentFilter中加入一个自定义的Action值（同事要设定Category值为“android.intent.category.DEFAULT"），在你的Intent中设定该值为Intent的Action，就直接能跳转自己的Intent接收者中。因为这个Action在系统中是唯一的。
- 2、Data/Type，可以用Uri来作为data，比如

```
Uri uri = new Uri("http://www.google.com);
Intent intent = new Intent(Intent.Action_VIEW, uri);
```

在手机的Intent分发过程中，会根据http://www.google.com的scheme判断出数据类型的type，手机的Brower则能匹配到它，在Brower的Manifest.xml中的IntentFilter中首先有ACTION_VIEW Action，也能处理http:的type

- 3、分类Category，一般不要去在Intent设置他，如果你写Intent的接收者，就在Manifest的Activity的IntentFilter中包含android.category.DEFAULT，这样所有不设置Category（Intent.addCategory(String c);）的Intent都会与这个Category匹配
- extras（附件信息），是其它所有附件信息的集合。使用extras可以为组件提供扩展信息，比如，如果要执行“发送电子邮件”这个动作，可以将电子邮件的标题、正文等保存在extras里，传给电子邮件发送组件。

##38、如果后台的Activity由于某种原因被系统回收了，如何在被系统回收之前保存当前状态？

- 当你的程序中某一个Activity A 在运行时中，主动或被动运行另一个新的Activity B，这个时候A会执行

```
public void onSaveInstanceState(Bundle outState){
	super.onSaveInstanceState(outState);
}
```

B完成以后又会来找A，这个时候就有两种情况，一种是A被回收，一种是没有被回收，被回收的A就要重新调用onCreate方法，不同于直接启动的是这回onCreate()里是带上参数saveInstanceState，没被回收的就还是onResume就好了。
saveInstanceState是一个Bundle对象，你基本可以把它理解为系统帮你维护的一个Map对象。在onCreate里，你可能会用到它，如果正常启动onCreate就不会有它，所以用的时候要判断一下是否为空。

- 就像官方的Notepad教程里的情况，你正在编辑某一个note，忽然被中断，那么就把这个note的id记住，再起来的时候就可以根据这个id去把那个note取出来，程序就完整一些。这也是看你的应用需不需要保存什么，比如你的界面就是读取一个列表，那就不需要特殊记住什么

##39、如果退出Activity

###对于单一Activity的应用来说，退出很简单，直接finish即可。当然，也可以用killProcess和System.exit这样的方法。先提供几个方法，仅供参考：

- 抛异常强制退出：该方法通过抛异常，使程序 Force Close。验证可以，但是，需要解决的问题是，如何是程序结束掉，而不弹出 Force Close的窗口
- 记录打开的Activity：每打开一个Activity，就记录下来。在需要退出时，关闭每一个Activity即可
- 发送特定的广播：在需要结束应用时，发送一个特定的广播，每个Activity收到广播后，关闭即可
- 递归退出在打开新的activity时使用startActivityForResult，然后加上标志，在onActivityResult中进行处理，递归关闭。除了第一个，都是想办法把每一个Activity结束掉，简洁达到目的。但是这样做同样不完美。你会发现，如果自己的应用程序对每一个Activity都设置了nosensor，在两个Activity结束的间隙，sensor可能有效了。但至少，我们的目的达到了，而且没有影响用户的使用。为了变成的方便，最好定义一个Activity基类，处理这些共通问题。

##40、Message、Handler、Message Queue、Looper之间的关系

- Message Queue(消息队列）：用来存放通过 Handler 发布的消息，通常附属于某一个创建它的线程，可以通过 Looper.myQueue 得到当前线程的消息队列
- Handler：可以发布或者处理一个消息或者操作一个Runnable，通过Handler发布消息，消息将只会发送到与它关联的消息队列，也只能处理该消息队列中的消息
- Looper：是Handler和消息队列之间通讯的桥梁，程序组件首先通过Handler把消息传递给Looper，Looper把消息放入队列。Looper也把消息队列里的消息广播给所有的Handler，Handler接收到消息后调用handleMessgae进行处理
- Message：消息，在Handler类中的handlerMessage方法中得到单个的消息进行处理


##41、你如何评价Android系统？优缺点。

###优点

1. 学习的开源性
2. 软件兼容性比较好
3. 软件发展迅速
4. 面布局好

###缺点

1. 版本过多
2. 先有软件少
3. 商务性能差

##42、谈谈android数据存储方式。

###Android提供了5种方式存储数据：

1. 使用SharedPreferences存储数据；它是Android提供的用来存储一些简单配置信息的一种机制，采用了XML格式将数据存储到设备中。只能在同一个包内使用，不能在不同的包之间使用。
2. 文件存储数据；文件存储方式是一种较常用的方法，在Android中读取/写入文件的方法，与Java中实现I/O的程序是完全一样的，提供了openFileInput()和openFileOutput()方法来读取设备上的文件。
3. SQLite数据库存储数据；SQLite是Android所带的一个标准的数据库，它支持SQL语句，它是一个轻量级的嵌入式数据库。
4. 使用ContentProvider存储数据；主要用于应用程序之间进行数据交换，从而能够让其他的应用保存或读取此Content Provider的各种数据类型。
5. 网络存储数据；通过网络上提供给我们的存储空间来上传(存储)和下载(获取)我们存储在网络空间中的数据信息。

　　

##43、Android中Activity, Intent, Content Provider, Service各有什么区别

- Activity： 活动，是最基本的android应用程序组件。一个活动就是一个单独的屏幕，每一个活动都被实现为一个独立的类，并且从活动基类继承而来。
- Intent： 意图，描述应用想干什么。最重要的部分是动作和动作对应的数据。
- Content Provider：内容提供器，android应用程序能够将它们的数据保存到文件、SQLite数据库中，甚至是任何有效的设备中。当你想将你的应用数据和其他应用共享时，内容提供器就可以发挥作用了。
- Service：服务，具有一段较长生命周期且没有用户界面的程序。

##44、View, surfaceView, GLSurfaceView有什么区别。

- view是最基础的，必须在UI主线程内更新画面，速度较慢。
- SurfaceView 是view的子类，类似使用双缓机制，在新的线程中更新画面所以刷新界面速度比view快
- GLSurfaceView 是SurfaceView的子类，opengl 专用的

##45、Manifest.xml文件中主要包括哪些信息？

- manifest：根节点，描述了package中所有的内容。
- uses-permission：请求你的package正常运作所需赋予的安全许可。
- permission： 声明了安全许可来限制哪些程序能你package中的组件和功能。
- instrumentation：声明了用来测试此package或其他package指令组件的代码。
- application：包含package中application级别组件声明的根节点。
- activity：Activity是用来与用户交互的主要工具。
- receiver：IntentReceiver能使的application获得数据的改变或者发生的操作，即使它当前不在运行。
- service：Service是能在后台运行任意时间的组件。
- provider：ContentProvider是用来管理持久化数据并发布给其他应用程序使用的组件。

##46、根据自己的理解描述下Android数字签名。

1. 所有的应用程序都必须有数字证书，Android系统不会安装一个没有数字证书的应用程序
2. Android程序包使用的数字证书可以是自签名的，不需要一个权威的数字证书机构签名认证
3. 如果要正式发布一个Android ，必须使用一个合适的私钥生成的数字证书来给程序签名，而不能使用adt插件或者ant工具生成的调试证书来发布。
4. 数字证书都是有有效期的，Android只是在应用程序安装的时候才会检查证书的有效期。如果程序已经安装在系统中，即使证书过期也不会影响程序的正常功能。

##47、AIDL的全称是什么?如何工作?能处理哪些类型的数据?

###AIDL全称Android Interface Definition Language（AndRoid接口描述语言）是一种借口描述语言; 编译器可以通过aidl文件生成一段代码，通过预先定义的接口达到两个进程内部通信进程跨界对象访问的目的.AIDL的IPC的机制和COM或CORBA类似, 是基于接口的，但它是轻量级的。它使用代理类在客户端和实现层间传递值. 如果要使用AIDL, 需要完成2件事情: 1. 引入AIDL的相关类.; 2. 调用aidl产生的class.理论上, 参数可以传递基本数据类型和String, 还有就是Bundle的派生类, 不过在Eclipse中,目前的ADT不支持Bundle做为参数,

###具体实现步骤如下:

1. 创建AIDL文件, 在这个文件里面定义接口, 该接口定义了可供客户端访问的方法和属性。
2. 编译AIDL文件, 用Ant的话, 可能需要手动, 使用Eclipse plugin的话,可以根据adil文件自动生产java文件并编译, 不需要人为介入.
3. 在Java文件中, 实现AIDL中定义的接口. 编译器会根据AIDL接口, 产生一个JAVA接口。这个接口有一个名为Stub的内部抽象类，它继承扩展了接口并实现了远程调用需要的几个方法。接下来就需要自己去实现自定义的几个接口了.
4. 向客户端提供接口ITaskBinder, 如果写的是service，扩展该Service并重载onBind ()方法来返回一个实现上述接口的类的实例。
5. 在服务器端回调客户端的函数. 前提是当客户端获取的IBinder接口的时候,要去注册回调函数, 只有这样, 服务器端才知道该调用那些函数

###AIDL语法很简单,可以用来声明一个带一个或多个方法的接口，也可以传递参数和返回值。 由于远程调用的需要, 这些参数和返回值并不是任何类型.下面是些AIDL支持的数据类型:

1. 不需要import声明的简单Java编程语言类型(int,boolean等)
2. String, CharSequence不需要特殊声明
3. List, Map和Parcelables类型, 这些类型内所包含的数据成员也只能是简单数据类型, String等其他比支持的类型.
(另外: 我没尝试Parcelables, 在Eclipse+ADT下编译不过, 或许以后会有所支持).

###实现接口时有几个原则:

1. 抛出的异常不要返回给调用者. 跨进程抛异常处理是不可取的.
2. IPC调用是同步的。如果你知道一个IPC服务需要超过几毫秒的时间才能完成地话，你应该避免在Activity的主线程中调用。也就是IPC调用会挂起应用程序导致界面失去响应. 这种情况应该考虑单起一个线程来处理.
3. 不能在AIDL接口中声明静态属性。

###IPC的调用步骤:

1. 声明一个接口类型的变量，该接口类型在.aidl文件中定义。
2. 实现ServiceConnection。
3. 调用ApplicationContext.bindService(),并在ServiceConnection实现中进行传递.
4. 在ServiceConnection.onServiceConnected()实现中，你会接收一个IBinder实例(被调用的Service). 调用YourInterfaceName.Stub.asInterface((IBinder)service)将参数转换为YourInterface类型。
5. 调用接口中定义的方法。你总要检测到DeadObjectException异常，该异常在连接断开时被抛出。它只会被远程方法抛出。
6. 断开连接，调用接口实例中的ApplicationContext.unbindService()
[参考：](http://buaadallas.blog.51cto.com/399160/372090)

##48、android:gravity与android:layout_gravity的区别
LinearLayout有两个非常相似的属性：android:gravity与android:layout_gravity。他们的区别在 于：android:gravity用于设置View组件的对齐方式，而android:layout_gravity用于设置Container组件的 对齐方式。

##49、padding与margin的区别
padding填充的意思，指的是view中的content与view边缘的距离，类似文本中的indent
而margin表示的是view的左边缘与parent view的左边缘的距离
margin一般用来描述控件间位置关系，而padding一般描述控件内容和控件的位置关系。

简单，padding是站在父 view的角度描述问题，它规定它里面的内容必须与这个父view边界的距离。margin则是站在自己的角度描述问题，规定自己和其他（上下左右）的 view之间的距离，如果同一级只有一个view，那么它的效果基本上就和padding一样了。

##50、Dalvik基于JVM的改进

1. 几个class变为一个dex，constant pool，省内存
2. Zygote，copy-on-write shared,省内存，省cpu，省电
3. 基于寄存器的bytecode，省指令，省cpu，省电
4. Trace-based JIT,省cpu，省电,省内存

##51、android中有哪几种解析xml的类,官方推荐哪种？以及它们的原理和区别.

###DOM解析

- 优点:

1. XML树在内存中完整存储,因此可以直接修改其数据和结构. 
2. 可以通过该解析器随时访问XML树中的任何一个节点. 
3. DOM解析器的API在使用上也相对比较简单.

- 缺点:如果XML文档体积比较大时,将文档读入内存是非常消耗系统资源的.

- 使用场景:DOM 是用与平台和语言无关的方式表示 XML 文档的官方 W3C 标准.DOM 是以层次结构组织的节点的集合.这个层次结构允许开发人员在树中寻找特定信息.分析该结构通常需要加载整个文档和构造层次结构,然后才能进行任何工作.DOM是基于对象层次结构的.

###SAX解析

- 优点:

SAX 对内存的要求比较低,因为它让开发人员自己来决定所要处理的标签.特别是当开发人员只需要处理文档中所包含的部分数据时,SAX 这种扩展能力得到了更好的体现.

- 缺点:

用SAX方式进行XML解析时,需要顺序执行,所以很难访问到同一文档中的不同数据.此外,在基于该方式的解析编码过程也相对复杂.

- 使用场景:

对于含有数据量十分巨大,而又不用对文档的所有数据进行遍历或者分析的时候,使用该方法十分有效.该方法不用将整个文档读入内存,而只需读取到程序所需的文档标签处即可.

###Xmlpull解析

android SDK提供了xmlpull api,xmlpull和sax类似,是基于流（stream）操作文件,然后根据节点事件回调开发者编写的处理程序.因为是基于流的处理,因此xmlpull和sax都比较节约内存资源,不会象dom那样要把所有节点以对橡树的形式展现在内存中.xmlpull比sax更简明,而且不需要扫描完整个流.

##52、Android系统中GC什么情况下会出现内存泄露呢？

###出现情况:

1. 数据库的cursor没有关闭
2. 构造adapter时,没有使用缓存contentview, 衍生listview的优化问题-----减少创建view的对象,充分使用contentview,可以使用一静态类来优化处理getview的过程
3. Bitmap对象不使用时采用recycle()释放内存
4. activity中的对象的生命周期大于activity

调试方法: DDMS==> HEAPSZIE==>dataobject==>[Total Size]

##53、谈谈对Android NDK的理解

1. NDK 全称: Native Development Kit 
2. 误解 

	- 误解一: NDK 发布之前, Android 不支持进行 C 开发。在Google 中搜索 “NDK” ,很多 “Android 终于可以使用 C++ 开发 ” 之类的标题,这是一种对 Android 平台编程方式的误解.其实, Android 平台从诞生起,就已经支持 C . C++ 开发.众所周知, Android 的 SDK 基于 Java 实现,这意味着基于 Android SDK 进行开发的第三方应用都必须使用 Java 语言.但这并不等同于 “ 第三方应用只能使用 Java” .在 Android SDK 首次发布时, Google 就宣称其虚拟机 Dalvik 支持 JNI 编程方式,也就是第三方应用完全可以通过 JNI 调用自己的 C 动态库,即在 Android 平台上, “Java+C” 的编程方式是一直都可以实现的。当然这种误解的产生是有根源的:在Android SDK 文档里,找不到任何 JNI 方面的帮助.即使第三方应用开发者使用 JNI 完成了自己的 C 动态链接库（ so ）开发,但是 so 如何和应用程序一起打包成 apk 并发布？这里面也存在技术障碍.我曾经花了不少时间,安装交叉编译器创建 so ,并通过 asset （资源）方式,实现捆绑 so 发布.但这种方式只能属于取巧的方式,并非官方支持.所以,在 NDK 出来之前,我们将 “Java+C” 的开发模式称之为灰色模式,即官方既不声明 “ 支持这种方式 ” ,也不声明 “ 不支持这种方式 ”。
	- 误解二:有了 NDK ,我们可以使用纯 C 开发 Android 应用 
 Android SDK采用 Java 语言发布,把众多的 C 开发人员排除在第三方应用开发外（ 注意:我们所有讨论都是基于“ 第三方应用开发 ” , Android 系统基于 Linux ,系统级别的开发肯定是支持 C 语言的. ）.NDK 的发布,许多人会误以为,类似于 Symbian . WM ,在 Android 平台上终于可以使用纯 C . C++ 开发第三方应用了！其实不然, NDK 文档明确说明: it is not a good way .因为 NDK 并没有提供各种系统事件处理支持,也没有提供应用程序生命周期维护.此外,在本次发布的 NDK 中,应用程序 UI 方面的 API 也没有提供.至少目前来说,使用纯 C . C++ 开发一个完整应用的条件还不完备.

3. NDK 是一系列工具的集合.

	- NDK提供了一系列的工具,帮助开发者快速开发 C （或 C++ ）的动态库,并能自动将 so 和 java 应用一起打包成 apk .这些工具对开发者的帮助是巨大的. 
NDK集成了交叉编译器,并提供了相应的 mk 文件隔离 CPU .平台. ABI 等差异,开发人员只需要简单修改 mk 文件（指出 “ 哪些文件需要编译 ” . “ 编译特性要求 ” 等）,就可以创建出 NDK可以自动地将 so 和 Java 应用一起打包,极大地减轻了开发人员的打包工作. 

4. NDK 提供了一份稳定.功能有限的 API 头文件声明.

	- Google明确声明该 API 是稳定的,在后续所有版本中都稳定支持当前发布的 API .从该版本的 NDK 中看出,这些 API 支持的功能非常有限,包含有: C 标准库（ libc ）.标准数学库（ libm ）.压缩库（ libz ）. Log 库（ liblog ）.

5. NDK 带来什么 

	- NDK 的发布,使 “Java+C” 的开发方式终于转正,成为官方支持的开发方式.
	- 使用NDK ,我们可以将要求高性能的应用逻辑使用 C 开发,从而提高应用程序的执行效率. 
	- 使用NDK ,我们可以将需要保密的应用逻辑使用 C 开发.毕竟, Java 包都是可以反编译的. 
	- NDK促使专业 so 组件商的出现.（乐观猜想,要视乎 Android 用户的数量） 

6. NDK 将是 Android 平台支持 C 开发的开端. NDK提供了的开发工具集合,使开发人员可以便捷地开发.发布 C 组件.同时, Google承诺在 NDK 后续版本中提高 “ 可调式 ” 能力,即提供远程的 gdb 工具,使我们可以便捷地调试 C 源码.在支持 Android 平台 C 开发,我们能感觉到 Google 花费了很大精力,我们有理由憧憬 “C 组件支持 ” 只是 Google Android 平台上C 开发的开端.毕竟, C 程序员仍然是码农阵营中的绝对主力,将这部分人排除在 Android 应用开发之外,显然是不利于 Android 平台繁荣昌盛的.


##54、java中 ==和equals的区别：

- 值类型是存储在内存中的堆栈（以后简称栈），而引用类型的变量在栈中仅仅是存储引用类型变量的地址，而其本身则存储在堆中。 ==操作比较的是两个变量的值是否相等，对于引用型变量表示的是两个变量在堆中存储的地址是否相同，即栈中的内容是否相同。 equals操作表示的两个变量是否是对同一个对象的引用，即堆中的内容是否相同。  ==比较的是2个对象的地址，而equals比较的是2个对象的内容。 显然，当equals为true时，==不一定为true； 

##55、android view绘制简单描述

1. 简单描述可以解释为：计算大小（measure），布局坐标计算（layout），绘制到屏幕（draw）；下面看看每一步的动作到底是什么。

	- 第一步：当activity启动的时候，触发初始化view过程的是由Window对象的DecorView调用View（具体怎样从xml中读取是用LayoutInflater.from(context).inflate）对象的 public final void measure(int widthMeasureSpec, int heightMeasureSpec)方法开始的，这个方法是final类型的，也就是所有的子类都不能继承该方法，保证android初始化view的原理不变。具体参数类值，后面会介绍。
	- 第二步：View的measure方法 onMeasure(widthMeasureSpec, heightMeasureSpec)，该方法进行实质性的view大小计算。注意：view的大小是有父view和自己的大小决定的，而不是单一决定的。这也就是为什么ViewGroup的子类会重新该方法，比如LinearLayout等。因为他们要计算自己和子view的大小。View基类有自己的实现，只是设置大小。其实根据源码来看，measure的过程本质上就是把Match_parent和wrap_content转换为实际大小
	- 第三步：当measure结束时，回到DecorView，计算大小计算好了，那么就开始布局了，开始调用view的 public final void layout(int l, int t, int r, int b)，该还是也是final类型的，目的和measure方法一样。layout方法内部会调用onlayout(int l, int t, int r, int b )方法，二ViewGroup将此方法abstract的了，所以我们继承ViewGroup的时候，需要重新该方法。该方法的本质是通过measure计算好的大小，计算出view在屏幕上的坐标点
	- 第四步：measure过了，layout过了，那么就要开始绘制到屏幕上了，所以开始调用view的  public void draw(Canvas canvas)方法，此时方法不是final了，原因是程序员可以自己画，内部会调用ondraw，我们经常需要重写的方法。 简单描述可以解释为：计算大小（measure），布局坐标计算（layout），绘制到屏幕（draw）；

	
	
Android 的消息机制也就是 handler 机制,创建 handler 的时候会创建一个 looper ( 通过 looper.prepare() 来创建 ),looper 一般为主线程 looper.
handler 通过 send 发送消息 (sendMessage) ,当然 post 一系列方法最终也是通过 send 来实现的,在 send 方法中handler 会通过 enqueueMessage() 方法中的 enqueueMessage(msg,millis )向消息队列 MessageQueue 插入一条消息,同时会把本身的 handler 通过 msg.target = this 传入.
Looper 是一个死循环,不断的读取MessageQueue中的消息,loop 方法会调用 MessageQueue 的 next 方法来获取新的消息,next 操作是一个阻塞操作,当没有消息的时候 next 方法会一直阻塞,进而导致 loop 一直阻塞,当有消息的时候,Looper 就会处理消息 Looper 收到消息之后就开始处理消息: msg.target.dispatchMessage(msg),当然这里的 msg.target 就是上面传过来的发送这条消息的 handler 对象,这样 handler 发送的消息最终又交给他的dispatchMessage方法来处理了,这里不同的是,handler 的 dispatchMessage 方法是在创建 Handler时所使用的 Looper 中执行的,这样就成功的将代码逻辑切换到了主线程了.
Handler 处理消息的过程是:首先,检查Message 的 callback 是否为 null,不为 null 就通过 handleCallBack 来处理消息,Message 的 callback 是一个 Runnable 对象,实际上是 handler 的 post 方法所传递的 Runnable 参数.其次是检查 mCallback 是 否为 null,不为 null 就调用 mCallback 的handleMessage 方法来处理消息。

	
电话面试题
1.ArrayList 和 HashMap 简单说一些,区别,底层的数据结构.
2.Handler 消息机制
3.引起内存泄漏的场景
4.多线程的使用场景?
5.常用的线程池有哪几种?
6.在公司做了什么?团队规模?为什么离职?
面试中实际涉及到的问题
第一轮
1.知道哪些单例模式,写一个线程安全的单例,并分析为什么是线程安全的?
2.Java中的集合有哪些?解释一下HashMap?底部的数据结构?散列表冲突的处理方法,散列表是一个什么样的数据结构?HashMap是采用什么方法处理冲突的?
3.解释一下什么是MVP架构,画出图解,一句话解释MVP和MVC的区别?
4.Handle消息机制?在使用Handler的时候要注意哪些东西,是否会引起内存泄漏?画一下Handler机制的图解?
5.是否做过性能优化?已经采取了哪些措施进行优化?
6.引起内存泄漏的原因是什么?以及你是怎么解决的?
这些问题应该都是比较基础的问题,每个开发者都应该是非常熟悉并能详细叙述的.这一轮的面试官问的技术都是平时用到的.
第二轮
1.关于并发理解多少?说几个并发的集合?
2.Handler 消息机制图解?
3.在项目中做了哪些东西?
4.画图说明View 事件传递机制?并举一个例子阐述
5.类加载机制,如何换肤,换肤插件中存在的问题?hotfix是否用过,原理是否了解?
6.说说项目中用到了哪些设计模式,说了一下策略模式和观察者模式?
7.会JS么?有Hybid开发经验么?
8.说一下快排的思想?手写代码
9.堆有哪些数据结构?
对于这轮米那是明显感觉到压力,知识的纵向了解也比较深,应该是个leader.
Activity状态保存与恢复。
Service的生命周期，启动方法，有什么区别。
service和activity怎么进行数据交互。
怎么保证service不被杀死。
广播使用的方式和场景以及广播的几种分类。
Intent的使用方法，可以传递哪些数据类型。
ContentProvider使用方法。
ContentProvider、ContentResolver、ContentObserver 之间的关系。
Thread、AsycTask、IntentService的使用场景与特点。
FrameLayout 、 LinearLayout 、 RelativeLayout 各自特点及绘制效率对比。
Android的数据存储形式。
Android两种序列化的区别和作用。
Sqlite的基本操作。
Android中的MVC、MVP模式。
Merge、ViewStub的作用。
动画有哪几类，各有什么特点？
Android的消息机制，子线程更新UI的方法和原理。
Android怎么加速启动Activity。
App的启动过程。
Android优化方法。
如何防止内存泄漏？
Android中弱引用与软引用的应用场景。
Bitmap的四种属性，如何加载大图（inJustDecodeBounds）。
View与View Group分类。自定义View过程：onMeasure()、onLayout()、onDraw()。
View刷新机制和绘制流程。
Activity、Window、View的联系和理解。
invalidate和requestLayout的区别及使用。
Touch事件分发机制和冲突处理。
Android IPC:Binder原理。
Android5.0（UI库）、6.0（权限）、7.0特性。








































##启动模式：

###1、standard：标准化启动模式

- 每启动一个Activity，都会重新创建Activity的新的实例，将其放在栈的顶部。不需要考虑这个实例是否已经存在。
- 每一次启动，它的onCreate()、onStart()、onResume()方法都会被依次调用。

###2、singleTop：栈顶复用模式

- 当前栈中已经有该Activity实例，并且该实例位于栈顶时，会去调用onNewIntent()方法。
- 当前栈中已有该Activity的实例但是该实例不在栈顶时，依然会去创建Activity。
- 当前栈中不存在该Activity实例时，会去新创建一个该Activity。
- 应用场景：IM对话框、新闻客户端推送。

###3、singleTask：栈内复用模式

- 它主要检测整个栈中是否已经存在当前想要启动的Activity，存在的话直接将该Activity置于栈顶，之前位于该Activity上面的Activity将被销毁，同时调用onNewIntent()方法，而不存在的话进行创建。

应用场景：应用主界面。

###4、singleInstance：

        一个人独享一个任务栈。当该Activity启动时，系统会创建一个新的任务栈，同时将Activity放到这个新的任务栈当中，有别的应用来启动该Activity时，由于栈内复用的特性，不会再去创建相应Activity任务栈，而是这两个应用独享一个Activity实例。

        例如：应用A中现有两个Activity E、Activity F，为standard启动模式，应用B中有一个Activity G，但其启动模式是singleInstance。应用A想用应用B任务栈当中的Activity G，尽管在不同的应用下，但是应用A仍然会直接复用Activity G。

        特性：
1、以SingleInstance模式启动的Activity具有全局唯一性【全局唯一性即指在整个系统当中只会存在一个这样的实例】；
2、如果在启动这样一个Activity时，【整个系统都是单例的】，已经存在了一个实例；
3、以SingleInstance模式启动的Activity具有独占性。

应用场景：呼叫来电。

问题：onNewIntent()调用时机？

singleTop：如果新Activity已经位于任务栈的栈顶，就不会重新创建，并回调 onNewIntent(intent) 方法。
singleTask：只要该Activity在一个任务栈中存在，都不会重新创建，并回调 onNewIntent(intent) 方法。

#网络协议：

协议：【协议指计算机通信网络中两台计算机之间进行通信所必须共同遵守的规定或规则】
HTTP协议

- 基本概念：【超文本传输协议】允许将HTML（超文本标记语言）文档从Web服务器传送到客户端的浏览器。HTTP协议是基于TCP/IP通信协议来传输数据的，可以从服务器端获取图片等数据资源。
- URI：【uniform resource identifier】统一的资源标识符，用来唯一的标识一个资源。强调资源！！！
- 组成部分：1）访问资源的命名机制；file
			2）存放资源的主机名；
			3）资源自身的名称，由路径表示，着重于强调资源。

         例：file://a:1234/b/c/d.txt   表示资源目标在a主机下的1234端口的b目录下的c目录下的d.txt文件。

        URL：【uniform resource Location】统一资源定位器，是一种具体的URI。即URL可以用来标识一个资源，而且还指明了如                       何定位这个资源。强调路径！！！

        组成部分：

                        1）协议；

                        2）存有该资源的主机IP地址；

                        3）主机资源的具体地址。

        HTTP协议特点：

                                   1）简单快速；

                                   2）无连接；【限制每次链接只处理一个请求，服务器处理完客户的请求之后会收到客户端的应答，再断开链                                          接，节省了重复的时间】；

                                   3）无状态：【没有记忆能力，】

       HTTP协议的request/response请求头原理剖析：

       request有可能经过代理服务器到达web服务器

       代理服务器最主要的作用：提高访问速度【大部分代理服务器都具有缓存功能，当你再次访问前一个网络请求时，就可以直          接从代理服务器中获取，而不需要请求我们的web服务器】。

       HTTP协议容易混淆知识点：

       （1）http1.0与http1.1的具体区别：

                  http处于计算机网络的应用层。

                   1）缓存处理

                   2）带宽优化及网络连接的使用

                   3）Host头使用：1.1上请求操作和响应操作都支持Host头，而且在请求消息中如果没有Host头的话会报告一个400错                      误。

                   4）长连接：在一个TCP连接上，可以传送多个HTTP请求和响应，而不是发送一个HTTP请求就断开一个连接，再发                      送一个HTTP请求再建立一个连接。

                存在的问题：

                                    1）传输数据时，每次都需要重新创建连接，增加了大量的延迟时间；

                                    2）传输数据时，所有传输的内容都是明文，客户端和服务器端都无法验证对方的身份；

                                    3）使用时，header里携带的内容过大，增加了传输成本。

       （2）get / post方法的区别：

                  1）提交的数据：get提交的数据一般会放在我们的URL之后，用“  ？”来分割；而post提交数据都会放在我们entity-                           body【消息主体】当中。

                  2）提交的数据大小是否有限制：get提交的数据是有限制的，因为url是有限制的，不能无限制的输入一个url地址；而                     post方法提交的是body，因此没有限制。

                  3）取得变量的值：get方法通过Request.QueryString()来取得变量的值，而post方法通过Request.Form来取得变量的                     值。

                  4）安全问题：get提交数据一定会带来安全问题

       （3）Cookie和Session的区别：

                  1）cookie【用户身份的标识】：客户端的解决方案。是由服务器发给客户端的特殊信息，而这些信息以文本文件的方                   式存放在客户端，然后客户端每次向服务器发送请求的时候都会带上这些特殊的信息。存放在响应头里面。

                       客户端 向服务端发送一个HTTP Request请求；
                       服务端给客户端一个HTTP Response ，并且把它的cookies设置给我们的客户端；
                       客户端将HTTP Request和cookies打包给我们的服务端；
                       服务端会根据客户端给我们的cookies来进行指定的判断，返回HTTP Response给我们的客户端。
                       此方法弥补了我们HTTP协议无状态的不足。之前当上述请求响应操作完成后，服务端和客户端就断开连接，服务端                        就无法从连接上跟踪我们所想要的客户端。

                  2）session：另一种记录客户状态的限制，cookie保存在客户端浏览器中，而session保存在服务器上。客户端浏览器                     访问服务器时，服务器把客户端信息以某种形式记录在服务器上。

                        创建session；
                        在创建session同时，服务器会为该session生成唯一的session id；
                        在session被创建之后，就可以调用session相关的方法往session中增加内容；
                        当客户端再次发送请求的时候，会将这个session id带上，服务器接收到请求之后就会依据session id找到相                           应的session。
                  3）区别：

                                 存放的位置不同；

                                 存取的方式不同【cookie保存的是Ascll码字符串，而session中能够保存任何类型的数据】；

                                 安全性上的不同【cookie存放在客户端浏览器中，对我们客户端可见，客户端的一些程序就有可能去修改我们                                  cookie的内容，而session则不然，它存储在服务端上，对客户端是透明的，不存在敏感信息泄露的风险】；

                                 有效期上的不同【一般我们会设置cookie过期时间， session依赖 id，若id设置为-1，当关掉浏览器session就                                  会失效】；

                                 对服务器的造成的压力不同【cookie保存在客户端不占用客户资源，session保存在服务端，每一个用户都会                                    产生一个session。在并发很多用户时cookie是一个很好的选择】。

        HTTPS协议：

        基本概念：对工作在以加密连接（SSL / TLS）上的常规HTTP协议。通过在TCP和HTTP之间加入TLS【Transport Layer               Security】来加密。

        SSL / TLS协议：安全传输协议，TLS是SSL的升级版，也是现阶段所使用的协议；

        HTTPS传输速度：

                                      1）通信慢；

                                      2）SSL必须进行加密处理。

        TLS / SSL握手：

                                      1）密码学原理

                                            对称加密：加密数据所用的密钥和解密数据所用的密钥相同。

                                            非对称加密：分私有密钥和公有密钥。

                                      2）数字证书：互联网通讯中标志通讯各方身份信息的一串数字。

                                      3）握手过程

#Handler【Android SDK提供给开发者方便进行异步消息处理的类】：
          AsyncTask、retrofit都对Handler进行了封装。

       （1）Handler四大组件

                 1）Message

                       Message是在线程之间传递的消息，它可以在内部携带少量的信息，用于在不同线程之间交换数据。

                       例：Message的what字段、arg1字段、arg2字段来携带整型数据，obj字段携带一个Object对象。

                 2）Handler

                       处理者，它主要用来发送和处理消息。发送消息一般是使用Handler的sendMessage()方法，消息经过处理后，最终                        传递到Handler的handlerMessage()方法中。

                 3）MessageQueue

                      消息队列，它主要用来存放所有通过Handler发送的消息，这部分消息会一直存在于消息队列中，等待被处理。

                      注意：每个线程中只会有一个MessageQueue对象。

                 4）Looper

                     是每个线程中MessageQueue的管家，调用Looper的loop()方法后，就会进入到一个无限循环当中，每当发现                                  MessageQueue中存在一条消息，就会将其取出传递到Handler的handleMessage()方法当中。

                     注意：每个线程中只会有一个Looper对象。

                     异步消息处理流程：

                                                       1）在主线程当中创建一个Handler对象；

                                                       2）重写handleMessage()方法；

                                                       3）当子线程需要进行UI操作时，创建一个Message对象，并通过Handler将消息发送出去；

                                                       4）消息添加到MessageQueue的队列中等待被处理；

                                                       5）Looper在MessageQueue中取出待处理消息，发回Handler的handleMessage()方法中。

                    【由于Handler是在主线程中创建的，因此我们的handleMessage()方法中的代码也会在主线程中执行，避免了异常的                       产生】




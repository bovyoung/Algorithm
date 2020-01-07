##1、四大组件以及应用场景？
1. Activity【活动】：在Android应用中负责与用户交互的组件。
2. Service【服务】：后台运行服务，不提供界面呈现。常用于为其他组件提供后台服务或者监控其他组件的运行状态。经常用来执行一些耗时操作。
3. BroadcastReceiver【广播接收器】：用来接收广播。用于监听应用程序中的其他组件。
4. Content Provider【内容提供商】：支持在多个应用中存储和读取数据，相当于数据库。Android应用程序之间实现实时数据交换。

##2、四个组件的生命周期？

1. Activity生命周期图

![ActivityLifecyle](https://img-blog.csdn.net/20180405184622584?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMzQ0MzEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

2. Fragment生命周期图

![FragmentLifecyle](https://img-blog.csdn.net/20180405184635884?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMzQ0MzEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

3. Service的生命周期：首先Service有两种启动方式，而在这两种启动方式下，它的生命周期不同。

- 通过startService()方法启动的服务。初始化结束后系统会调用 void onStart(Intent intent) 方法，用于处理传递给startService()的Intent对象。如音乐服务会打开Intent 来探明将要播放哪首音乐，并开始播放。注意：多次调用startService()方法会多次触发onStart()方法。
- 通过bindService ()方法启动的服务。初始化结束后系统会调用 IBinder onBind(Intent intent) 方法，用来绑定传递给bindService 的Intent 的对象。注意：多次调用bindService()时，如果该服务已启动则不会再触发此方法。

##3、Activity的四种启动模式对比？

- Standard:标准的启动模式，如果需要启动一个activity就会创建该activity的实例。也是activity的默认启动模式。
- SingeTop:如果启动的activity已经位于栈顶，那么就不会重新创建一个新的activity实例。而是复用位于栈顶的activity实例对象。如果不位于栈顶仍旧会重新创建activity的实例对象。
- SingleTask:设置了singleTask启动模式的activity在启动时，如果位于activity栈中，就会复用该activity，这样的话，在该实例之上的所有activity都依次进行出栈操作，即执行对应的onDestroy()方法，直到当前要启动的activity位于栈顶。一般应用在网页的图集，一键退出当前的应用程序。
- singleInstance:如果使用singleInstance启动模式的activity在启动的时候会复用已经存在的activity实例。不管这个activity的实例是位于哪一个应用当中，都会共享已经启动的activity的实例对象。使用了singlInstance的启动模式的activity会单独的开启一个共享栈，这个栈中只存在当前的activity实例对象。


##4、Activity在有Dialog时按Home键的生命周期？

1. 当我们的Activity上弹出Dialog对话框时，程序的生命周期依然是onCreate() ---> onStart() ---> onResume()，在弹出Dialog的时候并没有onPause()和onStop()方法。而在此时我们按下Home键，才会继续执行onPause()和onStop()方法。这说明对话框并没有使Activity进入后台，而是在点击了Home键后Activity才进入后台工作。
2. 原因就是，其实Dialog是Activity的一个组件，此时Activity并不是不可见，而是被Dialog组件覆盖了其他的组件，此时我们无法对其他组件进行操作而已。

##5、两个Activity 之间跳转时必然会执行的是哪几个方法？

1. 首先定义两个Activity，分别为A和B。
2. 当我们在A中激活B时，A调用onPause()方法，此时B出现在屏幕时，B调用onCreate()、onStart()、onResume()。
3. 这个时候B【B不是一个透明的窗体或对话框的形式】已经覆盖了A的窗体，A会调用onStop()方法。

##6、 前台切换到后台，然后再回到前台，Activity生命周期回调方法。弹出Dialog，生命周期回调方法？

1. 首先定义两个Activity，分别为A和B。
2. 完整顺序为：A调用onCreate()方法 ---> onStart()方法 ---> onResume()方法。当A启动B时，A调用onPause()方法，然后调用新的Activity B，此时调用onCreate()方法 ---> onStart()方法 ---> onResume()方法将新Activity激活。之后A再调用onStop()方法。当A再次回到前台时，B调用onPause()方法，A调用onRestart()方法 ---> onStart()方法 ---> onResume()方法，最后调用B的onStop()方法 ---> onDestory()方法。
3. 弹出Dialog时，调用onCreate()方法 ---> onStart()方法 ---> onResume()方法。

##7、fragment各种情况下的生命周期？

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

##8、 如何实现Fragment的滑动？
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

##9、fragment之间传递数据的方式？

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

##10、Activity 怎么和Service 绑定？

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

##11、service生命周期？

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

##12、 activity和service的绑定方式以及怎么在Activity 中启动自己对应的Service？

1、activity能进行绑定得益于Serviece的接口。为了支持Service的绑定，实现onBind方法。

2、Service和Activity的连接可以用ServiceConnection来实现。需要实现一个新的ServiceConnection，重写onServiceConnected和OnServiceDisconnected方法，一旦连接建立，就能得到Service实例的引用。

3、执行绑定，调用bindService方法，传入一个选择了要绑定的Service的Intent(显示或隐式)和一个你实现了的ServiceConnection的实例

##13、Service的启动方式？

采用Context.startService()方法启动服务，在服务未被创建时，系统会先调用服务的onCreate()方法，接着调用onStart()方法。如果调用startService()方法前服务已经被创建，多次调用startService()方法并不会导致多次创建服务，但会导致多次调用onStart()方法。采用startService()方法启动的服务，只能调用Context.stopService()方法结束服务，服务结束时会调用onDestroy()方法。


采用Context.bindService()方法启动服务，在服务未被创建时，系统会先调用服务的 onCreate()方法，接着调用onBind()方法。这个时候调用者和服务绑定在一起，调用者退出了，系统就会先调用服务的onUnbind()方 法，接着调用onDestroy()方法。如果调用bindService()方法前服务已经被绑定，多次调用bindService()方法并不会导致 多次创建服务及绑定(也就是说onCreate()和onBind()方法并不会被多次调用)。如果调用者希望与正在绑定的服务解除绑定，可以调用 unbindService()方法，调用该方法也会导致系统调用服务的onUnbind()-->onDestroy()方法。 

##14、谈谈ContentProvider、ContentResolver、ContentObserver之间的关系？

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

##15、广播的分类？

- 分为有序广播和无序广播两类。

1. 无序广播发送代码：
public class MainActivity extends Activity {
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
 
    public void click(View v){
        //启动界面 startActivity
        //发送广播 sendBroadcast
 
        Intent intent = new Intent();
 
        intent.setAction("com.itheima.cctv.action.NEWS");   
        intent.putExtra("data", "我是一个无须的广播");
        sendBroadcast(intent);  
    }
}
2. 无序广播的监听代码：

```

public class CctvReceiver extends BroadcastReceiver {
 
    private static final String TAG = "CctvReceiver";
 
    @Override
    public void onReceive(Context context, Intent intent) {
        String data = intent.getStringExtra("data");
        Log.d(TAG, "data==="+data);
    }
 
}
<receiver android:name="com.itheima.cctv.CctvReceiver">
            <intent-filter >
            <!--这里监听的广播就是上面发送广播设置的intent.setAction("com.itheima.cctv.action.NEWS");-->
                <action android:name="com.itheima.cctv.action.NEWS"/>
            </intent-filter>
        </receiver>     
```

有序广播发送：

```
public class ShengReceiver extends BroadcastReceiver {
 
    @Override
    public void onReceive(Context context, Intent intent) {
        Log.d("vivi", "我是省级部门，我收到的指令是："+getResultData());
        //getResultData()是用来获取有序广播里面的数值.这里的信息是:
        //主席讲话：每人奖励10斤土豆
 
 
        setResultData("主席讲话：每人奖励7斤土豆");//有序广播的数值,可以被修改,后面的程序在接受到这个广播,就会变成,现在我们改变的值了
 
        //有序广播传输是可以终止的.但是最终的接受者就算在终止之后,也是可以接受到数据的
        //abortBroadcast();
    }
}
 

public class ShiReceiver extends BroadcastReceiver {
 
    @Override
    public void onReceive(Context context, Intent intent) {
        Log.d("vivi", "我是市级部门，我收到的指令是："+getResultData());
    //因为上面修改了数据,所以这里获取到的数据是:主席讲话：每人奖励7斤土豆
    }
 
}
 

<!--有序广播的优先级别使用 android:priority=""来指定,最高是1000,最低是-1000-->
        <receiver android:name="com.itheima.region.ShengReceiver">
            <intent-filter android:priority="1000">
                <action android:name="com.itheima.gov.action.POTATO"/>
            </intent-filter>
        </receiver>
 
        <receiver android:name="com.itheima.region.ShiReceiver">
           <intent-filter android:priority="500">
                <action android:name="com.itheima.gov.action.POTATO"/>
            </intent-filter>
        </receiver>  
```

有序接收：

```

public class MyReceiver extends BroadcastReceiver {
 
    @Override
    public void onReceive(Context context, Intent intent) {
        Log.d("vivi", "我是恩恩主席的内线，我收到的指令是："+getResultData());
    }
 
}

```

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


##Handler消息机制：

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
主线程中默认是创建了Looper并且启动了消息的循环的，因此不会报错：
应用程序的入口是ActivityThread的main方法，在这个方法里面会创建Looper，并且执行Looper的loop方法来启动消息的循环，使得应用程序一直运行。


###子线程中可以通过Handler发送消息给主线程吗？
可以。有时候出于业务需要，主线程可以向子线程发送消息。子线程的Handler必须按照上述方法创建，并且关联Looper。
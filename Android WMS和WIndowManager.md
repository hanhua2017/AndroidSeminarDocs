# Window 属性
###     应用程序窗口
    Activity就是一个典型的应用程序窗口，应用程序窗口包含的类型如下所示。 
    public static final int FIRST_APPLICATION_WINDOW = 1;//1
    public static final int TYPE_BASE_APPLICATION   = 1;//窗口的基础值，其他的窗口值要大于这个值
    public static final int TYPE_APPLICATION        = 2;//普通的应用程序窗口类型
    public static final int TYPE_APPLICATION_STARTING = 3;//应用程序启动窗口类型，用于系统在应用程序窗口启动前显示的窗口。
    public static final int TYPE_DRAWN_APPLICATION = 4;
    public static final int LAST_APPLICATION_WINDOW = 99;//2
--------------------- 
### 子窗口
    子窗口，顾名思义，它不能独立的存在，需要附着在其他窗口才可以，PopupWindow就属于子窗口。子窗口的类型定义如下所示：

    public static final int FIRST_SUB_WINDOW = 1000;//子窗口类型初始值
    public static final int TYPE_APPLICATION_PANEL = FIRST_SUB_WINDOW;
    public static final int TYPE_APPLICATION_MEDIA = FIRST_SUB_WINDOW + 1;
    public static final int TYPE_APPLICATION_SUB_PANEL = FIRST_SUB_WINDOW + 2;
    public static final int TYPE_APPLICATION_ATTACHED_DIALOG = FIRST_SUB_WINDOW + 3;
    public static final int TYPE_APPLICATION_MEDIA_OVERLAY  = FIRST_SUB_WINDOW + 4; 
    public static final int TYPE_APPLICATION_ABOVE_SUB_PANEL =FIRST_SUB_WINDOW + 5;
    public static final int LAST_SUB_WINDOW = 1999;//子窗口类型结束值
    子窗口的Type值范围为1000到1999。
--------------------- 

### 系统窗口
    Toast、输入法窗口、系统音量条窗口、系统错误窗口都属于系统窗口。系统窗口的类型定义如下所示：

    public static final int FIRST_SYSTEM_WINDOW     = 2000;//系统窗口类型初始值
    public static final int TYPE_STATUS_BAR         = FIRST_SYSTEM_WINDOW;//系统状态栏窗口
    public static final int TYPE_SEARCH_BAR         = FIRST_SYSTEM_WINDOW+1;//搜索条窗口
    public static final int TYPE_PHONE              = FIRST_SYSTEM_WINDOW+2;//通话窗口
    public static final int TYPE_SYSTEM_ALERT       = FIRST_SYSTEM_WINDOW+3;//系统ALERT窗口
    public static final int TYPE_KEYGUARD           = FIRST_SYSTEM_WINDOW+4;//锁屏窗口
    public static final int TYPE_TOAST              = FIRST_SYSTEM_WINDOW+5;//TOAST窗口
    public static final int LAST_SYSTEM_WINDOW      = 2999;//系统窗口类型结束值

    系统窗口的类型值有接近40个，这里只列出了一小部分， 系统窗口的Type值范围为2000到2999。
--------------------- 

### Window的标志
    Window的标志也就是Flag，用于控制Window的显示，同样被定义在WindowManager的内部类LayoutParams中，一共有20多个，这里我们给出几个比较常用。

| Flag        | 描述   |  
| :---------:   | :----: | 
| FLAG_ALLOW_LOCK_WHILE_SCREEN_ON     | 只要窗口可见，就允许在开启状态的屏幕上锁屏 |  
| FLAG_NOT_FOCUSABLE     | 窗口不能获得输入焦点，设置该标志的同时，FLAG_NOT_TOUCH_MODAL也会被设置 | 
| FLAG_NOT_TOUCHABLE	| 窗口不接收任何触摸事件
| FLAG_NOT_TOUCH_MODAL| 	在该窗口区域外的触摸事件传递给其他的Window,而自己只会处理窗口区域内的触摸事件| 
| FLAG_KEEP_SCREEN_ON| 	只要窗口可见，屏幕就会一直亮着| 
| FLAG_LAYOUT_NO_LIMITS	| 允许窗口超过屏幕之外| 
| FLAG_FULLSCREEN	| 隐藏所有的屏幕装饰窗口，比如在游戏、播放器中的全屏显示| | FLAG_SHOW_WHEN_LOCKED	| 窗口可以在锁屏的窗口之上显示| 
| FLAG_IGNORE_CHEEK_PRESSES| 	当用户的脸贴近屏幕时（比如打电话），不会去响应此事件| 
| FLAG_TURN_SCREEN_ON	| 窗口显示时将屏幕点亮| 


    设置Window的Flag有三种方法，第一种是通过Window的addFlags方法：
    Window mWindow =getWindow(); 
    mWindow.addFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN);

    第二种通过Window的setFlags方法:

    Window mWindow =getWindow();            
    mWindow.setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN,WindowManager.LayoutParams.FLAG_FULLSCREEN);

    其实Window的addFlags方法内部会调用setFlags方法，因此这两种方法区别不大。 
    第三种则是给LayoutParams设置Flag，并通过WindowManager的addView方法进行添加，如下所示。

    WindowManager.LayoutParams mWindowLayoutParams =
                new WindowManager.LayoutParams();
    mWindowLayoutParams.flags=WindowManager.LayoutParams.FLAG_FULLSCREEN;
    WindowManager mWindowManager =(WindowManager) getSystemService(Context.WINDOW_SERVICE);  
    TextView mTextView=new TextView(this);
    mWindowManager.addView(mTextView,mWindowLayoutParams);`
--------------------- 

### 软键盘相关模式
    窗口和窗口的叠加是非常常见的场景，但如果其中的窗口是软键盘窗口，可能就会出现一些问题，比如典型的用户登录界面，默认的情况弹出的软键盘窗口可能会盖住输入框下方的按钮，这样用户体验会非常糟糕。 
    为了使得软键盘窗口能够按照期望来显示，WindowManager的静态内部类LayoutParams中定义了软键盘相关模式，这里给出常用的几个：

| SoftInputMode| 	描述| 
| :---------:   | :----: | 
| SOFT_INPUT_STATE_UNSPECIFIED| 	没有指定状态,系统会选择一个合适的状态或依赖于主题的设置| 
| SOFT_INPUT_STATE_UNCHANGED| 	不会改变软键盘状态| 
| SOFT_INPUT_STATE_HIDDEN	| 当用户进入该窗口时，软键盘默认隐藏| 
| SOFT_INPUT_STATE_ALWAYS_HIDDEN	| 当窗口获取焦点时，软键盘总是被隐藏| 
| SOFT_INPUT_ADJUST_RESIZE| 	当软键盘弹出时，窗口会调整大小| 
| SOFT_INPUT_ADJUST_PAN| 	    

    当软键盘弹出时，窗口不需要调整大小，要确保输入焦点是可见的从上面给出的SoftInputMode 
    可以发现，它们与AndroidManifest中Activity的属性android:windowSoftInputMode是对应的。
    因此，除了在AndroidMainfest中为Activity设置android:windowSoftInputMode以外还
    可以在Java代码中为Window设置SoftInputMode：
    getWindow().setSoftInputMode(WindowManager.LayoutParams.SOFT_INPUT_ADJUST_RESIZE);

--------------------- 

# WindowMangerService 的诞生

#### frameworks/base/services/java/com/android/server/SystemServer.java
#### SystemServer的main方法创建SystemServer,  run方法初始化各种系统服务

    
    public static void main(String[] args) {
       new SystemServer().run();
    }

    private void run() {
         try {
            System.loadLibrary("android_servers");//1
            ...
            mSystemServiceManager = new SystemServiceManager(mSystemContext);//2
            mSystemServiceManager.setRuntimeRestarted(mRuntimeRestart);
            LocalServices.addService(SystemServiceManager.class, mSystemServiceManager);
            // Prepare the thread pool for init tasks that can be parallelized
            SystemServerInitThreadPool.get();
        } finally {
            traceEnd();  // InitBeforeStartServices
        }
        try {
            traceBeginAndSlog("StartServices");
            startBootstrapServices();//3
            startCoreServices();//4
            startOtherServices();//5
            SystemServerInitThreadPool.shutdown();
        } catch (Throwable ex) {
            Slog.e("System", "******************************************");
            Slog.e("System", "************ Failure starting system services", ex);
            throw ex;
        } finally {
            traceEnd();
        }
    ...
    }


#### startOtherServices() 服务创建WMS

    private void startOtherServices() {
            ...
            traceBeginAndSlog("InitWatchdog");
            final Watchdog watchdog = Watchdog.getInstance();//1
            watchdog.init(context, mActivityManagerService);//2
            traceEnd();
            traceBeginAndSlog("StartInputManagerService");
            inputManager = new InputManagerService(context);//3
            traceEnd();
            traceBeginAndSlog("StartWindowManagerService");
            ConcurrentUtils.waitForFutureNoInterrupt(mSensorServiceStart, START_SENSOR_SERVICE);
            mSensorServiceStart = null;
            wm = WindowManagerService.main(context, inputManager,
                    mFactoryTestMode != FactoryTest.FACTORY_TEST_LOW_LEVEL,
                    !mFirstBoot, mOnlyCore);//4
            ServiceManager.addService(Context.WINDOW_SERVICE, wm);//5
            ServiceManager.addService(Context.INPUT_SERVICE, inputManager);//6
            traceEnd();   
           ... 
           try {
            wm.displayReady();//7 用来初始化显示信息
               } catch (Throwable e) {
            reportWtf("making display ready", e);
              }
           ...
           try {
            wm.systemReady();//用来通知WMS，系统的初始化工作已经完成，其内部调用了WindowManagerPolicy的systemReady方法。
               } catch (Throwable e) {
            reportWtf("making Window Manager Service ready", e);
              }
            ...      
    }
--------------------- 

#### frameworks/base/services/core/java/com/android/server/wm/WindowManagerService .java
#### WindowManagerService#main创建WMS

    public static WindowManagerService main(final Context context,
            final InputManagerService im,
            final boolean haveInputMethods, final boolean showBootMsgs,
            final boolean onlyCore) {
        final WindowManagerService[] holder = new WindowManagerService[1];
        DisplayThread.getHandler().runWithScissors(new Runnable() {
            @Override
            public void run() {
                holder[0] = new WindowManagerService(context, im,
                        haveInputMethods, showBootMsgs, onlyCore);
            }
        }, 0);
        return holder[0];
    }
    
    在DisplayThread线程,前台线程。创建WMS，
    

#### WMS的构造函数

    private WindowManagerService(Context context, InputManagerService inputManager,
            boolean haveInputMethods, boolean showBootMsgs, boolean onlyCore, WindowManagerPolicy policy) {
       ...
       mInputManager = inputManager;//1
       ...
        mDisplayManager = (DisplayManager)context.getSystemService(Context.DISPLAY_SERVICE);
        mDisplays = mDisplayManager.getDisplays();//2每个显示设备都有一个Display实例
        for (Display display : mDisplays) {
            createDisplayContentLocked(display);//Display封装成DisplayContent，DisplayContent用来描述一快屏幕。
        }
       ...
         mActivityManager = ActivityManager.getService();//4
       ...
        mAnimator = new WindowAnimator(this);//5
        mAllowTheaterModeWakeFromLayout = context.getResources().getBoolean(
                com.android.internal.R.bool.config_allowTheaterModeWakeFromWindowLayout);
        LocalServices.addService(WindowManagerInternal.class, new LocalService());
        initPolicy();//6
        // Add ourself to the Watchdog monitors.
        Watchdog.getInstance().addMonitor(this);//7
     ...
    }

#### initPolicy(); 初始化 WindowManagerPolicy

    private void initPolicy() {
        //UiThread 
        UiThread.getHandler().runWithScissors(new Runnable() {
            @Override
            public void run() {
                WindowManagerPolicyThread.set(Thread.currentThread(), Looper.myLooper());

                mPolicy.init(mContext, WindowManagerService.this, WindowManagerService.this);
            }
        }, 0);
    }
    
#### WMS一些字段

    mPolicy：WindowManagerPolicy
    WindowManagerPolicy（WMP）类型的变量。WindowManagerPolicy是窗口管理策略的接口类，用来定义一个窗口策略所要遵循的通用规范，并提供了WindowManager所有的特定的UI行为。它的具体实现类为PhoneWindowManager，这个实现类在WMS创建时被创建。WMP允许定制窗口层级和特殊窗口类型以及关键的调度和布局。
    
    mSessions：ArraySet
    ArraySet类型的变量，元素类型为Session。在Android解析WindowManager（三）Window的添加过程这篇文章中我提到过Session，它主要用于进程间通信，其他的应用程序进程想要和WMS进程进行通信就需要经过Session，并且每个应用程序进程都会对应一个Session，WMS保存这些Session用来记录所有向WMS提出窗口管理服务的客户端。
    mWindowMap：WindowHashMap
    WindowHashMap类型的变量，WindowHashMap继承了HashMap，它限制了HashMap的key值的类型为IBinder，value值的类型为WindowState。WindowState用于保存窗口的信息，在WMS中它用来描述一个窗口。综上得出结论，mWindowMap就是用来保存WMS中各种窗口的集合。
    
    mFinishedStarting：ArrayList
    ArrayList类型的变量，元素类型为AppWindowToken，它是WindowToken的子类。要想理解mFinishedStarting的含义，需要先了解WindowToken是什么。WindowToken主要有两个作用：
    
    可以理解为窗口令牌，当应用程序想要向WMS申请新创建一个窗口，则需要向WMS出示有效的WindowToken。AppWindowToken作为WindowToken的子类，主要用来描述应用程序的WindowToken结构，
    应用程序中每个Activity都对应一个AppWindowToken。
    WindowToken会将相同组件（比如Acitivity）的窗口（WindowState）集合在一起，方便管理。
    mFinishedStarting就是用于存储已经完成启动的应用程序窗口（比如Acitivity）的AppWindowToken的列表。
    除了mFinishedStarting，还有类似的mFinishedEarlyAnim和mWindowReplacementTimeouts，其中mFinishedEarlyAnim存储了已经完成窗口绘制并且不需要展示任何已保存surface的应用程序窗口的AppWindowToken。mWindowReplacementTimeout存储了等待更换的应用程序窗口的AppWindowToken，如果更换不及时，旧窗口就需要被处理。
    
    mResizingWindows：ArrayList
    ArrayList类型的变量，元素类型为WindowState。
    mResizingWindows是用来存储正在调整大小的窗口的列表。与mResizingWindows类似的还有mPendingRemove、mDestroySurface和mDestroyPreservedSurface等等。其中mPendingRemove是在内存耗尽时设置的，里面存有需要强制删除的窗口。mDestroySurface里面存有需要被Destroy的Surface。mDestroyPreservedSurface里面存有窗口需要保存的等待销毁的Surface，为什么窗口要保存这些Surface？这是因为当窗口经历Surface变化时，窗口需要一直保持旧Surface，直到新Surface的第一帧绘制完成。
    
    mAnimator：WindowAnimator
    WindowAnimator类型的变量，用于管理窗口的动画以及特效动画。
    
![我的头像](https://s2.ax1x.com/2019/05/28/VexoRA.png)    
--------------------- 


# Activity 创建到加载到视图上

## AMS启动Activity
    android.app.ActivityThread.java的scheduleLaunchActivity()
    

    public final void scheduleLaunchActivity(Intent intent, IBinder token, int ident, ... ) {
            ... 
            //调用handleLaunchActivity 方法
            sendMessage(H.LAUNCH_ACTIVITY, r);
    }
        
    private void handleLaunchActivity(ActivityClientRecord r, Intent customIntent, String reason) {
        ...
        // Initialize before creating the activity 初始化全局视图处理工具
        WindowManagerGlobal.initialize();
        // 准备一个Activity
        Activity a = performLaunchActivity(r, customIntent);
        ...
        // 开始显示Activity 到Window上
        handleResumeActivity(r.token, false, r.isForward,
                !r.activity.mFinished && !r.startsNotResumed, r.lastProcessedSeq, reason);
            
    }
    
## WindowManagerGlobal  初始化与WMS简历连接  

    public static void initialize() { // 初始化
        getWindowManagerService();
    }
    
    // 获取WMS引用
    public static IWindowManager getWindowManagerService() {
        synchronized (WindowManagerGlobal.class) {
            if (sWindowManagerService == null) {
                sWindowManagerService = IWindowManager.Stub.asInterface(
                        ServiceManager.getService("window"));
                try {
                    sWindowManagerService = getWindowManagerService();
                    ValueAnimator.setDurationScale(sWindowManagerService.getCurrentAnimatorScale());
                } catch (RemoteException e) {
                    throw e.rethrowFromSystemServer();
                }
            }
            return sWindowManagerService;
        }
    }

    //和WMS进行通信
    public static IWindowSession getWindowSession() {
        synchronized (WindowManagerGlobal.class) {
            if (sWindowSession == null) {
                try {
                    InputMethodManager imm = InputMethodManager.getInstance();
                    IWindowManager windowManager = getWindowManagerService();
                    // 获取sWindowSession 
                    sWindowSession = windowManager.openSession(
                            new IWindowSessionCallback.Stub() {
                                @Override
                                public void onAnimatorScaleChanged(float scale) {
                                    ValueAnimator.setDurationScale(scale);
                                }
                            },
                            imm.getClient(), imm.getInputContext());
                } catch (RemoteException e) {
                    throw e.rethrowFromSystemServer();
                }
            }
            return sWindowSession;
        }
    }
    
    //WMS 中创建一个Client
    public IWindowSession openSession(IWindowSessionCallback callback, IInputMethodClient client,
            IInputContext inputContext) {
        if (client == null) throw new IllegalArgumentException("null client");
        if (inputContext == null) throw new IllegalArgumentException("null inputContext");
        Session session = new Session(this, callback, client, inputContext);
        return session;
    }
 ------
   
    
##  performLaunchActivity   初始化Activity的试图  

    private Activity performLaunchActivity(ActivityClientRecord r, Intent customIntent) {
        ...
        // 创建Activity
        Activity activity = null;
        try {
            java.lang.ClassLoader cl = r.packageInfo.getClassLoader();
            activity = mInstrumentation.newActivity(
                    cl, component.getClassName(), r.intent);
           ...
        } catch (Exception e) {
            ...
        }

        try {
            Application app = r.packageInfo.makeApplication(false, mInstrumentation);
            ...
            if (activity != null) {
                Context appContext = createBaseContextForActivity(r, activity);
                ...
                //TODO 执行部分初始化操作
                activity.attach(appContext, this, getInstrumentation(), r.token,
                        r.ident, app, r.intent, r.activityInfo, title, r.parent,
                        r.embeddedID, r.lastNonConfigurationInstances, config,
                        r.referrer, r.voiceInteractor, window);
                ....
                // 绑定View到当前 执行setContentView
                if (r.isPersistable()) {
                    mInstrumentation.callActivityOnCreate(activity, r.state, r.persistentState);
                } else {
                    mInstrumentation.callActivityOnCreate(activity, r.state);
                }
            
            }
            mActivities.put(r.token, r);
        } 
        ...
        return activity;
    }
    
### Activity#attach 初始化操作    
    
    final void attach(Context context, ActivityThread aThread,
            Instrumentation instr, IBinder token, int ident,
            Application application, Intent intent, ActivityInfo info,
            CharSequence title, Activity parent, String id,
            NonConfigurationInstances lastNonConfigurationInstances,
            Configuration config, String referrer, IVoiceInteractor voiceInteractor,
            Window window) {
        mFragments.attachHost(null /*parent*/);
        // 设置一个PhoneWindow用来添加view, Activity#onCreate 添加view到此处
        mWindow = new PhoneWindow(this, window);
        ...
        mUiThread = Thread.currentThread(); // UI线程
        mMainThread = aThread; // 运行 AcitityThread 的线程
        ...
        // 设置WindowMange , WindowManagerImpl
        mWindow.setWindowManager(
                (WindowManager)context.getSystemService(Context.WINDOW_SERVICE),
                mToken, mComponent.flattenToString(),
                (info.flags & ActivityInfo.FLAG_HARDWARE_ACCELERATED) != 0);
        if (mParent != null) {
            mWindow.setContainer(mParent.getWindow());
        }
        // 新建的LoacalWindowManager,  父parent 是 context.getSystemService(Context.WINDOW_SERVICE)
        // WindowManagerImpl 
        mWindowManager = mWindow.getWindowManager();
        mCurrentConfig = config;
    }
    
    android.view.Window
    public void setWindowManager(WindowManager wm, IBinder appToken, String appName,
            boolean hardwareAccelerated) {
        mAppToken = appToken;
        mAppName = appName;
        mHardwareAccelerated = hardwareAccelerated
                || SystemProperties.getBoolean(PROPERTY_HARDWARE_UI, false);
        if (wm == null) {
            wm = (WindowManager)mContext.getSystemService(Context.WINDOW_SERVICE);
        }
        mWindowManager = ((WindowManagerImpl)wm).createLocalWindowManager(this);
    }
    
    public WindowManager getWindowManager() {
        return mWindowManager;
    }
    
    android.view.WindowManagerImpl
    public WindowManagerImpl createLocalWindowManager(Window parentWindow) {
        return new WindowManagerImpl(mContext, parentWindow);
    }

### callActivityOnCreate 调用Activity的onCreate,渲染布局到PhoneWindow上
    onCreate方法一般是执行一些Window一些属性调用
    调用setContentView方法，把试图渲染到view上
    
    public void setContentView(@LayoutRes int layoutResID) {
        getWindow().setContentView(layoutResID);// 调用PhoneWindow方法
        initWindowDecorActionBar();
    }

    
    com.android.internal.policy.PhoneWindow 
    
    public void setContentView(View view, ViewGroup.LayoutParams params) {
        // Note: FEATURE_CONTENT_TRANSITIONS may be set in the process of installing the window
        // decor, when theme attributes and the like are crystalized. Do not check the feature
        // before this happens.
        if (mContentParent == null) {
            installDecor(); //初始化一个父View就是ViewGroup,FramLayout
        } else if (!hasFeature(FEATURE_CONTENT_TRANSITIONS)) {
            mContentParent.removeAllViews();
        }

        if (hasFeature(FEATURE_CONTENT_TRANSITIONS)) {
            view.setLayoutParams(params);
            final Scene newScene = new Scene(mContentParent, view);
            transitionTo(newScene);
        } else {
            mContentParent.addView(view, params);
        }
        mContentParent.requestApplyInsets();
        final Callback cb = getCallback();
        if (cb != null && !isDestroyed()) {
            cb.onContentChanged();
        }
        mContentParentExplicitlySet = true;
    }
    
------

## handleResumeActivity    
    final void handleResumeActivity(IBinder token,
            boolean clearHide, boolean isForward, boolean reallyResume, int seq, String reason) {
        ...
        if (r != null) {
            final Activity a = r.activity;
            if (r.window == null && !a.mFinished && willBeVisible) {
                //TODO 获取PhoneWindow
                r.window = r.activity.getWindow();
                // 要添加到手机屏幕的视图
                View decor = r.window.getDecorView();
                decor.setVisibility(View.INVISIBLE);
                // WindowManagerImpl
                ViewManager wm = a.getWindowManager();
                WindowManager.LayoutParams l = r.window.getAttributes();
                ...
                if (a.mVisibleFromClient && !a.mWindowAdded) {
                    a.mWindowAdded = true;
                    // TODO 通知WMS更新当前视图
                    wm.addView(decor, l);
                }
            ...
            if (!r.activity.mFinished && willBeVisible
                    && r.activity.mDecor != null && !r.hideForNow) {
                
                WindowManager.LayoutParams l = r.window.getAttributes();
               
                    if (r.activity.mVisibleFromClient) {
                        ViewManager wm = a.getWindowManager();
                        View decor = r.window.getDecorView();
                        //TODO 更新视图
                        wm.updateViewLayout(decor, l);
                    }
            }

            // onReuse 执行完毕
            // Tell the activity manager we have resumed.
            if (reallyResume) {
                try {
                    ActivityManagerNative.getDefault().activityResumed(token);
                } catch (RemoteException ex) {
                    throw ex.rethrowFromSystemServer();
                }
            }

        } 
    }

    
###    WindowManagerImpl#addView(decor, l);
    public void updateViewLayout(@NonNull View view, @NonNull ViewGroup.LayoutParams params) {
        applyDefaultToken(params);
        mGlobal.updateViewLayout(view, params);
    }
        调用WindowManagerGlobal#updateViewLayout
    
    public void addView(View view, ViewGroup.LayoutParams params,
            Display display, Window parentWindow) {
        ...
        ViewRootImpl root;
        View panelParentView = null;

        synchronized (mLock) {
            ...
            //创建一个 PhoneWindow和WMS一个桥梁用来传递参数
            root = new ViewRootImpl(view.getContext(), display);
            view.setLayoutParams(wparams);
            mViews.add(view);
            mRoots.add(root);
            mParams.add(wparams);
        }

        // do this last because it fires off messages to start doing things
        try {
            // 通知WMS
            root.setView(view, wparams, panelParentView);
        } catch (RuntimeException e) {
            ...
        }
    }
    
###    android.view.ViewRootImpl
    public ViewRootImpl(Context context, Display display) {
        mContext = context;
        //TODO 通过WMS 获取Session 每个进程一个Session
        mWindowSession = WindowManagerGlobal.getWindowSession();
        mDisplay = display; // 对应的屏幕
        mFirst = true; // true for the first time the view is added
        mAdded = false; // 是否添加过子View
        mWindow = new W(this); // WMS移动视图的代理对象
        //处理手机按键，音量等
        mFallbackEventHandler = new PhoneFallbackEventHandler(context);
        // 定时刷新屏幕
        mChoreographer = Choreographer.getInstance();
    }
    
    public void setView(View view, WindowManager.LayoutParams attrs, View panelParentView) {
        synchronized (this) {
        
                ...
                // Compute surface insets required to draw at specified Z value.
                // TODO: Use real shadow insets for a constant max Z.
                if (!attrs.hasManualSurfaceInsets) {
                    attrs.setSurfaceInsets(view, false /*manual*/, true /*preservePrevious*/);
                }

                ...
                mAdded = true;
                int res; /* = WindowManagerImpl.ADD_OKAY; */

                // TODO 测量布局
                // Schedule the first layout -before- adding to the window
                // manager, to make sure we do the relayout before receiving
                // any other events from the system.
                requestLayout();
               
                ...
                
                    mOrigWindowType = mWindowAttributes.type;
                    mAttachInfo.mRecomputeGlobalAttributes = true;
                    collectViewAttributes();
                    //TODO  通知 WMS 进行更新当前UI信息
                    res = mWindowSession.addToDisplay(mWindow, mSeq, mWindowAttributes,
                            getHostVisibility(), mDisplay.getDisplayId(),
                            mAttachInfo.mContentInsets, mAttachInfo.mStableInsets,
                            mAttachInfo.mOutsets, mInputChannel);
                ...
            }
        }
    }

####  requestLayout()
    public void requestLayout() {
        if (!mHandlingLayoutInLayoutRequest) {
            //TODO 检测当前线程
            checkThread();
            mLayoutRequested = true;
            //TODO 遍历布局,最后调用 performTraversals
            scheduleTraversals();
        }
    }    
    
    void checkThread() {
        if (mThread != Thread.currentThread()) {
            throw new CalledFromWrongThreadException(
                    "Only the original thread that created a view hierarchy can touch its views.");
        }
    }
    
    
    private void performTraversals() {
            ...
            try {
                ...
                //TODO
                relayoutResult = relayoutWindow(params, viewVisibility, insetsPending);
            }{}
            ...
            //TODO surfaceView 更新视图
            if (mSurfaceHolder != null) {
                // The app owns the surface; tell it about what is going on.
                if (mSurface.isValid()) {
                    // XXX .copyFrom() doesn't work!
                    //mSurfaceHolder.mSurface.copyFrom(mSurface);
                    mSurfaceHolder.mSurface = mSurface;
                }
                ...
            }

            if (!mStopped || mReportNextDraw) {
                    ...
                    //TODO View测量操作
                     // Ask host how big it wants to be
                    performMeasure(childWidthMeasureSpec, childHeightMeasureSpec);
                    ...
            }
        } 

        final boolean didLayout = layoutRequested && (!mStopped || mReportNextDraw);
        boolean triggerGlobalLayoutListener = didLayout
                || mAttachInfo.mRecomputeGlobalAttributes;
        if (didLayout) {
            //TODO 布局内部View 坐标调整
            performLayout(lp, mWidth, mHeight);
        }

    

        if (computesInternalInsets) {
            ...
            try {
                //TODO 更新view,可视区域，可点击区域等等
                mWindowSession.setInsets(mWindow, insets.mTouchableInsets,
                        contentInsets, visibleInsets, touchableRegion);
            } catch (RemoteException e) {
            }
        }
        ...
        //TODO 准备绘制
        performDraw();
    }

#### mWindowSession.addToDisplay
    调用WMS#addWindow 方法
    
    public int  addWindow(Session session, IWindow client, int seq,
            WindowManager.LayoutParams attrs, int viewVisibility, int displayId,
            Rect outContentInsets, Rect outStableInsets, Rect outOutsets,
            InputChannel outInputChannel) {

            //TODO 检查权限
            int res = mPolicy.checkAddPermission(attrs, appOp);
            if (res != WindowManagerGlobal.ADD_OKAY) {
                return res;
            }

            //TODO 添加到那个视图上
            final DisplayContent displayContent = getDisplayContentLocked(displayId);
            ...
            //TODO 检测type 类型
            if (type >= FIRST_SUB_WINDOW && type <= LAST_SUB_WINDOW) {
                attachedWindow = windowForClientLocked(null, attrs.token, false);
                ...
            }
            ...    
            //TODO 创建WS 用来移动View， 更新屏幕显示
            WindowState win = new WindowState(this, session, client, token,
                    attachedWindow, appOp[0], seq, attrs, viewVisibility, displayContent);
            ...  
            //TODO 调整参数
            mPolicy.adjustWindowParamsLw(win.mAttrs);
            ...  
            //
            res = mPolicy.prepareAddWindowLw(win, attrs);
            if (res != WindowManagerGlobal.ADD_OKAY) {
                return res;
            }
            ...
            //TODO 添加视图到屏幕上
            win.attach();
    }
    
    WindowState#attach
    void attach() {
        if (WindowManagerService.localLOGV) Slog.v(
            TAG, "Attaching " + this + " token=" + mToken
            + ", list=" + mToken.windows);
        mSession.windowAddedLocked();
    }
    
    Seesioin#windowAddedLocked
    // 创建SurfaceComposerClient对象， 作为跟SurfaceFlinger通信的代理对象。
    void windowAddedLocked() {
        if (mSurfaceSession == null) {
            if (WindowManagerService.localLOGV) Slog.v(
                TAG_WM, "First window added to " + this + ", creating SurfaceSession");
            mSurfaceSession = new SurfaceSession();
            if (SHOW_TRANSACTIONS) Slog.i(
                    TAG_WM, "  NEW SURFACE SESSION " + mSurfaceSession);
            mService.mSessions.add(this);
            if (mLastReportedAnimatorScale != mService.getCurrentAnimatorScale()) {
                mService.dispatchNewAnimatorScaleLocked(this);
            }
        }
        mNumWindow++;
    }

    SurfaceSession.java 
    /** Create a new connection with the surface flinger. */
    public SurfaceSession() {
        mNativeClient = nativeCreate();
    }
    
-------    
    




# Activity 创建到加载到视图上
    Activity销毁view，从ActivityThread#handleDestroyActivity() 说起
    
    private void handleDestroyActivity(IBinder token, boolean finishing,
            int configChanges, boolean getNonConfigInstance) {
            ...
            WindowManager wm = r.activity.getWindowManager();
            View v = r.activity.mDecor;// PhoneWindow的mDecor 
                ...
            wm.removeViewImmediate(v); // 移除当前视图,WindowManagerImpl#removeViewImmediate
                ...
            r.activity.mDecor = null;   
                ...
    }
    
    
    WindowManagerImpl#removeViewImmediate
    public void removeViewImmediate(View view) {
        //WindowManagerGlobal
        mGlobal.removeView(view, true);
    }

    WindowManagerGlobal#removeView
    public void removeView(View view, boolean immediate) {
        if (view == null) {
            throw new IllegalArgumentException("view must not be null");
        }

        synchronized (mLock) {
            // 寻找第几个View
            int index = findViewLocked(view, true);
            View curView = mRoots.get(index).getView();
            // 执行删除操作
            removeViewLocked(index, immediate);
            if (curView == view) {
                return;
            }

            throw new IllegalStateException("Calling with view " + view
                    + " but the ViewAncestor is attached to " + curView);
        }
    }

    private void removeViewLocked(int index, boolean immediate) {
        ViewRootImpl root = mRoots.get(index);
        View view = root.getView();

        if (view != null) {
            InputMethodManager imm = InputMethodManager.getInstance();
            if (imm != null) {
                imm.windowDismissed(mViews.get(index).getWindowToken());
            }
        }
        // 是否需要延迟删除，ViewRootImpl#die,若果当前正在准备绘制则延迟删除
        boolean deferred = root.die(immediate);
        if (view != null) {
            view.assignParent(null);
            if (deferred) {
                mDyingViews.add(view);
            }
        }
    }
    
    
    /**
     * @param immediate True, do now if not in traversal. False, put on queue and do later.
     * @return True, request has been queued. False, request has been completed.
     */
    boolean die(boolean immediate) {
        // Make sure we do execute immediately if we are in the middle of a traversal or the damage
        // done by dispatchDetachedFromWindow will cause havoc on return.
        if (immediate && !mIsInTraversal) {
            doDie();
            return false;
        }

        if (!mIsDrawing) {
            destroyHardwareRenderer();
        } else {
            Log.e(mTag, "Attempting to destroy the window while drawing!\n" +
                    "  window=" + this + ", title=" + mWindowAttributes.getTitle());
        }
        mHandler.sendEmptyMessage(MSG_DIE);
        return true;
    }
    
    
    void doDie() {
        checkThread();
        if (LOCAL_LOGV) Log.v(mTag, "DIE in " + this + " of " + mSurface);
        synchronized (this) {
            if (mRemoved) {
                return;
            }
            mRemoved = true;
            if (mAdded) {
                // 有子View，调用View的 onDetachedFromWindow
                dispatchDetachedFromWindow();
            }
            ...
        }
        WindowManagerGlobal.getInstance().doRemoveView(this);
    }


    void dispatchDetachedFromWindow() {
        if (mView != null && mView.mAttachInfo != null) {
            mAttachInfo.mTreeObserver.dispatchOnWindowAttachedChange(false);
            //调用的View的onDetach
            mView.dispatchDetachedFromWindow();
        }
        ...
        try {
            //又回到了WMS调用了
            mWindowSession.remove(mWindow);
        } catch (RemoteException e) {
        }
        ...
        
        unscheduleTraversals();
    }
    
    
    mWindowSession.remove(mWindow)此方法最终会调用到WMS的removeWindowLocked(WindowState win, boolean keepVisibleDeadWindow) 方法
    
   
    void removeWindowLocked(WindowState win, boolean keepVisibleDeadWindow) {
        ...
        //需要执行一些转场动画执行
        if (win.mHasSurface && okToDisplay()) {
            ...
        }
        // 移除视图
        removeWindowInnerLocked(win);
        // Removing a visible window will effect the computed orientation
        // So just update orientation if needed.
        if (wasVisible && updateOrientationFromAppTokensLocked(false)) {
            mH.sendEmptyMessage(H.SEND_NEW_CONFIGURATION);
        }
        Binder.restoreCallingIdentity(origId);
    }


    void removeWindowInnerLocked(WindowState win) {
        ...
        for (int i = win.mChildWindows.size() - 1; i >= 0; i--) {
            WindowState cwin = win.mChildWindows.get(i);
            Slog.w(TAG_WM, "Force-removing child win " + cwin + " from container " + win);
            removeWindowInnerLocked(cwin);
        }

        ...
        //清除系统视图
        mPolicy.removeWindowLw(win);
        //销毁连接
        win.removeLocked();
        // 
        mWindowMap.remove(win.mClient.asBinder());
        ...
    }

------



ActivityThread#handleLaunchActivity
    WindowManagerGlobal.initialize(); 处理视图管理工具类
        初始化WindowManagerGlobal, 
        getWindowManagerService() 获取WMS对象
        getWindowSession() 获取Binder对象
        
    ActivityThread#performLaunchActivity
        Activity#attach
            Activity attach 初始化PhoneWindow , WindowManager(WindowManagerImpl)，以及二者建立联系。
            mWindow.setWindowManager，
            Activity 持有WindowManager 
        mInstrumentation.callActivityOnCreate
            执行Activity的OnCreate 方法， 设置布局添加到PhoneWindow中。PhoneWindow#decorView 的创建
        
    ActivityThread#handleResumeActivity
        wm.addView(decor, l);
        WindowManagerImpl#addView
            WIndowManagerGloable#addView
                ViewRootImpl  //The top of a view hierarchy, implementing the needed protocol between View and the WindowManager
                    new ViewRootImpl(view.getContext(), display);
                        mWindowSession = WindowManagerGlobal.getWindowSession();
                            windowManager.openSession(); // 获取一个IBinder对象，用来和WMS进行通信
                        mWindow = new W(this); // 操纵视图进行移动
                        
                ViewRootImpl#setView 进行
                    attrs.setSurfaceInsets() //Z轴阴影绘制
                    requestLayout();
                        checkThread();// 检测是不是UI线程
                        doTraversal()
                            performTraversals()
                                relayoutWindow(params, viewVisibility, insetsPending) // 重新布局窗口
                                performMeasure(childWidthMeasureSpec, childHeightMeasureSpec);//计算宽高
                                performLayout(lp, mWidth, mHeight);// 执行当前视图的布局
                                performDraw();// 绘制视图
                                
                    mWindowSession.addToDisplay()
                        WMS#addWindow()
                             WindowState win = new WindowState()
                             mPolicy.prepareAddWindowLw(win, attrs);
                             win.attach();//调用JNI 
                        
                
        
        
        
        
    ActivityThread#handleDestroyActivity()
        wm.removeViewImmediate(v);
            WindowManagerImpl#removeViewImmediateWindowManagerImpl
                WindowManagerGlobal#removeView
                    removeViewLocked(index, immediate);
                        root.die(immediate);
                        ViewRootImpl#die(immediate)
                            doDie()
                                dispatchDetachedFromWindow();
                                    mWindowSession.remove(mWindow);
                                    WMS#removeWindow()
                                        removeWindowInnerLocked(win);
                                            mPolicy.removeWindowLw(win);//清除系统视图
                                            win.removeLocked();//销毁连接
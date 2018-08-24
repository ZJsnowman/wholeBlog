---
title: Android四大组件之Service
date: 2016-07-28 18:38:49
tags: [Android]
---
 
 关于Service你所应该知道的一切<!-- more -->

## 导图
![](https://ws3.sinaimg.cn/large/006tNc79gy1fqjamqw192j3107075jtw.jpg)

##   服务的两种形式 ##
- 启动
当应用组件（如 Activity）通过调用 startService() 启动服务时，服务即处于“启动”状态。一旦启动，服务即可在后台无限期运行，**即使启动服务的组件已被销毁也不受影响**。 已启动的服务通常是执行单一操作，而且不会将结果返回给调用方。例如，它可能通过网络下载或上传文件。 操作完成后，服务会自行停止运行。

- 绑定
当应用组件通过调用 bindService() 绑定到服务时，服务即处于“绑定”状态。绑定服务提供了一个客户端-服务器接口，允许组件与服务进行交互、发送请求、获取结果，甚至是利用进程间通信 (IPC) 跨进程执行这些操作。 仅当与另一个应用组件绑定时，绑定服务才会运行。 **多个组件可以同时绑定到该服务，但全部取消绑定后，该服务即会被销毁**。


----------
PS:服务在其托管进程的**主线程**中运行，它既不创建自己的线程，也不在单独的进程中运行（除非另行指定）。 这意味着，如果服务将执行任何 CPU 密集型工作或阻止性操作（例如 MP3 播放或联网），则应在服务内创建新线程来完成这项工作。通过使用单独的线程，可以降低发生“应用无响应”(ANR) 错误的风险，而应用的主线程仍可继续专注于运行用户与 Activity 之间的交互。
 
## 使用清单文件声明服务 ##

```xml
<manifest ... >
  ...
  <application ... >
      <service android:name=".ExampleService" />
      ...
  </application>
</manifest>
```



- 为了确保应用的安全性，请始终使用**显式 Intent** 启动或绑定 Service，且不要为服务声明 Intent 过滤器。


- 此外，还可以通过添加 android:exported 属性并将其设置为 "false"，确保服务仅适用于您的应用。这可以有效阻止其他应用启动您的服务，即便在使用显式 Intent 时也如此

## 创建启动服务
启动服务由另一个组件通过调用 startService() 启动，这会导致调用服务的 onStartCommand() 方法。

服务启动之后，其生命周期即独立于启动它的组件，并且可以在后台无限期地运行，即使启动服务的组件已被销毁也不受影响。 因此，**服务应通过调用 stopSelf() 结束工作来自行停止运行，或者由另一个组件通过调用 stopService() 来停止它。**  //如何结束自己

应用组件（如 Activity）可以通过调用 startService() 方法并传递 Intent 对象 （指定服务并包含待使用服务的所有数据）来启动服务。服务通过 onStartCommand() 方法接收此 Intent。

例如，假设某 Activity 需要将一些数据保存到在线数据库中。该 Activity 可以启动一个协同服务，并通过向 startService() 传递一个 Intent，为该服务提供要保存的数据。服务通过 onStartCommand() 接收 Intent，连接到 Internet 并执行数据库事务。事务完成之后，服务会自行停止运行并随即被销毁。

###  拓展IntentService ###
IntentService执行以下操作
- 创建默认的工作线程，用于在应用的主线程外执行传递给 onStartCommand() 的所有 Intent。
- 创建工作队列，用于将一个 Intent 逐一传递给 onHandleIntent() 实现，这样您就永远不必担心多线程问题。
- 在处理完所有启动请求后停止服务，因此您永远不必调用 stopSelf()。
- 提供 onBind() 的默认实现（返回 null）。
- 提供 onStartCommand() 的默认实现，可将 Intent 依次发送到工作队列和 onHandleIntent() 实现。

综上所述，您只需实现 onHandleIntent() 来完成客户端提供的工作即可。（不过，您还需要为服务提供构造函数,
这个构造函数就是用来**定义子线程的名字**）

```java
public class HelloIntentService extends IntentService {

  /**
   * A constructor is required, and must call the super IntentService(String)
   * constructor with a name for the worker thread.
   */
  public HelloIntentService() {
      super("HelloIntentService");
  }

  /**
   * The IntentService calls this method from the default worker thread with
   * the intent that started the service. When this method returns, IntentService
   * stops the service, as appropriate.
   */
  @Override
  protected void onHandleIntent(Intent intent) {
      // Normally we would do some work here, like download a file.
      // For our sample, we just sleep for 5 seconds.
      long endTime = System.currentTimeMillis() + 5*1000;
      while (System.currentTimeMillis() < endTime) {
          synchronized (this) {
              try {
                  wait(endTime - System.currentTimeMillis());
              } catch (Exception e) {
              }
          }
      }
  }
}
```

### 推展Service

```java 
public class HelloService extends Service {
  private Looper mServiceLooper;
  private ServiceHandler mServiceHandler;

  // Handler that receives messages from the thread
  private final class ServiceHandler extends Handler {
      public ServiceHandler(Looper looper) {
          super(looper);
      }
      @Override
      public void handleMessage(Message msg) {
          // Normally we would do some work here, like download a file.
          // For our sample, we just sleep for 5 seconds.
          long endTime = System.currentTimeMillis() + 5*1000;
          while (System.currentTimeMillis() < endTime) {
              synchronized (this) {
                  try {
                      wait(endTime - System.currentTimeMillis());
                  } catch (Exception e) {
                  }
              }
          }
          // Stop the service using the startId, so that we don't stop
          // the service in the middle of handling another job
          stopSelf(msg.arg1);
      }
  }

  @Override
  public void onCreate() {
    // Start up the thread running the service.  Note that we create a
    // separate thread because the service normally runs in the process's
    // main thread, which we don't want to block.  We also make it
    // background priority so CPU-intensive work will not disrupt our UI.
    HandlerThread thread = new HandlerThread("ServiceStartArguments",
            Process.THREAD_PRIORITY_BACKGROUND);
    thread.start();

    // Get the HandlerThread's Looper and use it for our Handler
    mServiceLooper = thread.getLooper();
    mServiceHandler = new ServiceHandler(mServiceLooper);
  }

  @Override
  public int onStartCommand(Intent intent, int flags, int startId) {
      Toast.makeText(this, "service starting", Toast.LENGTH_SHORT).show();

      // For each start request, send a message to start a job and deliver the
      // start ID so we know which request we're stopping when we finish the job
      Message msg = mServiceHandler.obtainMessage();
      msg.arg1 = startId;
      mServiceHandler.sendMessage(msg);

      // If we get killed, after returning from here, restart
      return START_STICKY;
  }

  @Override
  public IBinder onBind(Intent intent) {
      // We don't provide binding, so return null
      return null;
  }

  @Override
  public void onDestroy() {
    Toast.makeText(this, "service done", Toast.LENGTH_SHORT).show();
  }
}
```
这里待续...

### 启动服务
启动服务
您可以通过将 Intent（指定要启动的服务）传递给 startService()，从 Activity 或其他应用组件启动服务。Android 系统调用服务的 onStartCommand() 方法，并向其传递 Intent。（切勿直接调用 onStartCommand()。）

例如，Activity 可以结合使用**显式** Intent 与 startService()，启动上文中的示例服务 (HelloSevice)：

```java
Intent intent = new Intent(this, HelloService.class);
startService(intent);
```
- startService() 方法将立即返回，且 Android 系统调用服务的 onStartCommand() 方法。如果服务尚未运行，则系统会先调用 onCreate()，然后再调用 onStartCommand()。
- 多个服务启动请求会导致多次对服务的 onStartCommand() 进行相应的调用。但是，要停止服务，只需一个服务停止请求（使用 stopSelf() 或 stopService()）即可

### 停止服务
启动服务必须管理自己的生命周期。也就是说，除非系统必须回收内存资源，否则系统不会停止或销毁服务，而且服务在 onStartCommand() 返回后会继续运行。因此，服务必须通过调用 stopSelf() 自行停止运行，或者由另一个组件通过调用 stopService() 来停止它。

### 创建绑定服务
绑定服务允许应用组件通过调用 bindService() 与其绑定，以便创建长期连接（通常不允许组件通过调用 startService() 来启动它）。
```java
 bindService(new Intent(this, MyService.class), mConnection, BIND_AUTO_CREATE);
```
多个客户端可以同时绑定到服务。客户端完成与服务的交互后，会调用 unbindService() 取消绑定。一旦没有客户端绑定到该服务，系统就会销毁它。

## 生命周期
![生命周期](https://ws3.sinaimg.cn/large/006tNc79gy1fqjamrkg9qj30at0e3tan.jpg)

- 服务的整个生命周期从调用 onCreate() 开始起，到 onDestroy() 返回时结束。与 Activity 类似，服务也在 onCreate() 中完成初始设置，并在 onDestroy() 中释放所有剩余资源。例如，音乐播放服务可以在 onCreate() 中创建用于播放音乐的线程，然后在 onDestroy() 中停止该线程。
无论服务是通过 startService() 还是 bindService() 创建，都会为所有服务调用 onCreate() 和 onDestroy() 方法。

- 服务的有效生命周期从调用 onStartCommand() 或 onBind() 方法开始。每种方法均有 Intent 对象，该对象分别传递到 startService() 或 bindService()。
对于启动服务，有效生命周期与整个生命周期同时结束（即便是在 onStartCommand() 返回之后，服务仍然处于活动状态）。对于绑定服务，有效生命周期在 onUnbind() 返回时结束。


 
PS:尽管启动服务是通过调用 stopSelf() 或 stopService() 来停止，但是该服务并无相应的回调（没有 onStop() 回调）。因此，除非服务绑定到客户端，否则在服务停止时，系统会将其销毁—onDestroy() 是接收到的唯一回调。
图 2 说明了服务的典型回调方法。尽管该图分开介绍通过 startService() 创建的服务和通过 bindService() 创建的服务，但是请记住，不管启动方式如何，任何服务均有可能允许客户端与其绑定。因此，最初使用 onStartCommand()（通过客户端调用 startService()）启动的服务仍可接收对 onBind() 的调用（当客户端调用 bindService() 时）。






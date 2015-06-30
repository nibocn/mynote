# android编程权威指南

@(Android)[note|android]

## 第3章 Activity的生命周期
* 停止(onStop)的activity能够存在多久，谁也无法保证。如果系统需要回收内存，它将首先销毁那些停止的activity。

### 设备旋转与 Activity 生命周期
* FrameLayout是一种最简单的ViewGroup组件，它不以特定方式安排其子视图的位置。 FrameLayout子视图的位置排列都是由它们各自的android:layout_gravity属性决定的。
* 只要在应用运行中设备配置发生了改变，Android就会销毁当前activity，然后再新建一个activity。

### 设备旋转前保存数据
* 覆盖`onSaveInstanceState(Bundle outState)`在设备运行中发生配置变更时，如设备旋转，保存以前的数据。
* 我们在Bundle中存储和恢复的数据类型只能是`基本数据类型（primitive type）`以及可以实现`Serializable`接口的对象。创建自己的定制类时，如需在`onSaveInstanceState(...)`方法中保存类对象，记得实现`Serializable`接口。

### 再探 Activity 生命周期
* activity只有在`暂停`或`停止`状态下才可能会被销毁。
* 暂存的activity记录到底可以保留多久？用户按了后退键后，系统会彻底销毁当前的activity。此时，暂存的activity记录同时被清除。此外，系统重启或长时间不使用activity时，暂存的activity记录通常也会被清除。

## 第5章 第二个Activity
### 启动Activity
* activity调用startActivity(...)方法时，调用请求实际发给了操作系统。准确地说，该方法调用请求是发送给操作系统的`ActivityManager`。 **ActivityManager负责创建Activity实例并调用其onCreate(...)方法。**

## 第6章 Android SDK版本与兼容
### Android编程与兼容性问题
* `Build.VERSION.SDK_INT`常量代表了Android设备的版本号。

## 第7章 UI fragment与fragment管理器
### 托管UI fragment
* fragment生命周期与activity生命周期的一个关键区别就在于，fragment的生命周期方法是由托管activity而不是操作系统调用的。
* 在activity中托管一个UI fragment，有如下两种方式：
    * 添加fragment到activity布局中；
    * 在activity代码中添加fragment；

### 添加 UI fragment 到 FragmentManager
* FragmentManager类具体管理的是：
    * fragment队列；
    * fragment事务的回退栈；
* FrameLayout组件的资源ID。容器视图资源ID主要有两点作用：
    * 告知FragmentManagerfragment视图应该出现在activity视图的什么地方；
    * 是FragmentManager队列中fragment的唯一标识符；
* activity被销毁时，它的FragmentManager会将fragment队列保存下来。

## 第9章 使用ListFragment显示列表
### ListFragment、 ListView 及 ArrayAdapter
* adapter负责：
    * 创建必要的视图对象；
    * 用模型层数据填充视图对象；
    * 将准备好的视图对象返回给ListView；

## 第11章 使用ViewPager
* ViewPager默认加载当前屏幕上的列表项，以及左右相邻页面的数据，从而实现页面滑动的快速切换。可调用`setOffscreenPageLimit(int)`方法，定制预加载相邻页面的数目。
* FragmentPagerAdapter与FragmentStatePagerAdapter使用方法基本相同，**唯一的区别就在于二者在卸载不再需要的fragment时**，所采用的处理方法不同：
    * 使用FragmentStatePagerAdapter会销毁掉不需要的fragment。事务提交后，可将fragment从activity的FragmentManager中彻底移除。
    * FragmentPagerAdapter的做法大不相同。对于不再需要的fragment ，FragmentPagerAdapter则选择调用事务的detach(Fragment)方法，而非remove(Fragment)方法，FragmentPagerAdapter只是销毁了fragment的视图，但仍将fragment实例保留在FragmentManager中。因此， FragmentPagerAdapter创建的fragment永远不会被销毁。
* **通常来说，使用FragmentStatePagerAdapter更节省内存。如果用户界面只需要少量固定的fragment，则FragmentPagerAdapter是个安全且合适的选择。**

## 第14章 fragment的保留
* 在fragment的onCreate方法中设置setRetainInstance(boolean)方法保存fragment实例，在Activity销毁时fragment不会随其销毁。

  **设置setRetainInstance后，屏幕旋转等操作不会再调用fragment的onDestroy()方法。**

  fragment生命周期差异，如图：

  ![](http://img.hb.aicdn.com/c6ca9fa226dfb680d339c81a193557a15a0c01991ad0b-CpYp5c_fw658)
* fragment必须同时满足两个条件才能进入保留状态：
  * 已调用了fragment的setRetainInstance(true)方法
  * 因设备配置改变（通常为设备旋转），托管activity正在被销毁

  Fragment处于保留状态的时间非常短暂，即fragment脱离旧activity到重新附加给立即创建的
新activity之间的一段时间。

* 如果activity是因操作系统需要回收内存而被销毁，则所有被保留的fragment也会被随之销毁。
* 如果activity或fragment中有需要长久保存的东西，则应覆盖onSaveInstanceState(Bundle)方法，将其状态保存下来。这样，由于同activity记录的生命周期保持了同步，后续可在需要时对其进行恢复。

## 第16章 操作栏
* 注意，虽然android:showAsAction属性是在API 11级引入的，但Android Lint并没有报出兼容性问题。**不同于Java代码，XML属性是不需要注解保护的。在早期API级别的设备上，后期新版本引入的XML属性会被自动忽略。**
* `setHasOptionsMenu(boolean)`在fragment中设置显示菜单。

## 第17章 存储与加载本地文件
* Android设备上的所有应用都有一个放置在沙盒中的文件目录。每个应用的沙盒目录都是设备/data/data目录的子目录，且默认以应用包命名。例如，
CriminalIntent应用的沙盒目录全路径为： /data/data/com.bignerdranch.android.criminalintent。
* Context类提供的基本文件或目录处理方法

| 方法  | 使用目的
| ---- | ------- |
| File getFilesDir() | 获取/data/data/<packagename>/files目录
| FileInputStream openFileInput(String name) | 打开现有文件进行读取
| FileOutputStream openFileOutput(String name, int mode) | 打开文件进行写入，如不存在就创建它
| File getDir(String name, int mode) | 获取/data/data/<packagename>/目录的子目录（如不存在就先创建它）
| String[] fileList() | 获取/data/data/<packagename>/files目录下的文件列表。可与其他方法配合使用，例如openFileInput(String)
| File getCacheDir() | 获取/data/data/<packagename>/cache目录。应注意及时清理该目录，并节约使用空间
*注意，表中大多数方法返回了标准Java类实例，如java.io.File*

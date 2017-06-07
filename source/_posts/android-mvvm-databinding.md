---
title: android-architecture-todo-mvvm-databinding学习笔记
---
项目源码地址：[android-architecture-todo-mvvm-databinding](https://github.com/googlesamples/android-architecture/tree/todo-mvvm-databinding/)
### 1. TasksActivity
根据`AndroidManifest.xml`得知
```android
com.example.android.architecture.blueprints.todoapp.tasks.TasksActivity
```
为启动界面。
#### 1. implements
该`Activity`实现了2个接口`TaskItemNavigator`和`TasksNavigator`,分别为查看TO-DO详情和增加TO-DO。实现方式也很简单，启动对应的`Activity`并在`onActivityResult`中处理。
#### 2. onCreate
`onCreate`中调用了多个方法：

*   `setupToolbar()` 初始化标题栏，Pass
*   `setupNavigationDrawer()` 初始化侧滑控件，Pass
*   `TasksFragment tasksFragment = findOrCreateViewFragment();` 初始化`TasksFragment`，在该方法中首先是通过Id获取Fragment对象,如果没有获取到`Fragment`则创建一个新的并添加到当前的`Activity`中。这么做的原因是当`Activity`被回收时,系统会把`Fragment`保存起来。`Activity`恢复的时候会重新走`onCreate`方法,这时候如果不加判断直接`new`一个`Fragment`并尝试添加到`Activity`中时会失败(不会报错),并且当调用在`Fragment`中定义的方法时,会抛出`NullPointException`。
*   `mViewModel = findOrCreateViewModel();` 这里是使用了一个没有UI的`Fragment`来保存`ViewModel`层,如果该`Fragment`为空或保存的`ViewModel`为空时创建一个新的`Fragment`并添加到当前的`Activity`中。
*   `mViewModel.setNavigator(this);` 将点击事件和实现接口绑定,具体的在`TasksFragment`部分分析.
*   `tasksFragment.setViewModel(mViewModel);` 将`ViewModel`与`Fragment`的布局绑定起来。

#### 3. onDestroy
清除了`ViewModel`层持有的当前`Activity`的引用以防内存泄漏
 
### 2. TasksFragment
#### 1. onActivityCreated
#### 2. onCreateView
```android
mTasksFragBinding = TasksFragBinding.inflate(inflater, container, false);
```
首先,获取自动生成的`TasksFragBinding`对象。开启`dataBinding`后,将布局文件定义成如下格式可以生成binding对象,命名方式为布局名称+Binding,这里的布局命名为`tasks_frag.xml`,所以生成的类名为`TasksFragBinding`：
```xml
<layout>
    <data></data>
    <LinearLayout></LinearLayout>
</layout>
```
在布局文件`<data>`标签中定义了2个`<variable>`标签:
```xml
<variable name="view"
          type="com.example.android.architecture.blueprints.todoapp.tasks.TasksFragment" />

<variable name="viewmodel"
          type="com.example.android.architecture.blueprints.todoapp.tasks.TasksViewModel" />
```
对应代码中：
```android
mTasksFragBinding.setView(this);

mTasksFragBinding.setViewmodel(mTasksViewModel);
```
`setHasOptionsMenu(true);` 设置显示菜单。对应的实现方法为`onCreateOptionsMenu`和`onOptionsItemSelected`。
###暂停

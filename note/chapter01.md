## Android Studio 使用导航

目前新版本的 Android Studio 在创建新项目的时候没有「Use AndroidX artifacts」这个选项了，这已经是默认的行为，不要勾选「Use legacy android.support libraries」即可。



## 用户界面设计

视图是用户界面的构造模块，显示在屏幕上的一切都是视图。

用户能看到并与之交互的视图称为部件（widget）。

有些部件可以用来显示文字或图像，有些部件（比如按钮）可以点击以触发事件任务。



Android SDK 内置了多种部件，通过配置各种部件可获得应用所需的外观及行为。

每一个部件都是 View 类或其子类的一个具体实例。



ViewGroup 是一种特殊的 View，它包含并布置其他视图。

ViewGroup 视图本身不显示内容，它规划其他视图内容应该显示在哪里。

ViewGroup 通常又称为布局。



### 视图层级关系

部件包含在视图对象的层级结构中，这种结构又称作视图层级结构（view hierarchy）。

作为根元素的部件必须指定 Android XML 资源文件的命名空间属性。



### 部件属性

1. **android:layout_width** 和 **android:layout_height** 属性

   几乎每类部件都需要这两个属性。以下是它们的两个常见属性值（二选一）。

   - match_parent：视图与其父视图大小相同。
   - wrap_content：视图将根据其显示内容自动调整大小。

   

2. **android:text** 属性，该属性指定部件要显示的文字内容。

   android:text 属性值不是字符串值，而是以 **@string/** 语法形式对字符串资源（string resource）的引用。

   字符串资源包含在一个独立的名叫 strings 的 XML 文件中（strings.xml），虽然可以硬编码设置部件的文本属性值，但这通常不是个好办法。

   比较好的做法是将文字内容放置在独立的字符串资源 XML 文件中，然后引用它们。

   这样会方便应用的本地化。

   

### 预览布局

在布局文件页面，工具栏的左上角，有个层级按钮，可以通过它的下拉菜单切换显示不同的布局预览模式——设计（Design）预览或蓝图（Blueprint）预览，或者并排显示设计预览和蓝图预览。



设计预览模式，用来展示布局在设备上的效果，也包括主题样式。

蓝图预览模式，用来展示部件的尺寸以及它们之间的位置关系。



在设计预览模式下，可以查看布局在不同的设备配置下的样子。

通过预览窗口上方的面板，可以指定设备类型、Android 模拟器版本、设备主题以及设备使用区域，查看布局的不同渲染结果，也可以模拟某个语言区域的自右到左的文字显示模式。



也可以直接使用布局编辑器摆放部件，布置布局。

项目窗口左边有个面板，包括了 Android 所有的内置部件。可以将它们从面板拖曳到视图上，或者拖到左下方的部件树上，更精准地控制如何摆放部件。



在使用ConstraintLayout 时，图形化布局编辑器非常有用。



## 从布局 XML 到视图对象

```kotlin
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle

class MainActivity : AppCompatActivity() {
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
    
}
```



AppCompatActivity 是一个 Activity 子类，能为 Android 旧版本系统提供兼容支持。



activity 子类的实例创建后，`onCreate(Bundle?)` 函数会被调用。

activity 创建后，它需要获取并管理用户界面。

要获取 activity 的用户界面，可以调用以下 Activity 函数：

> Activity.setContentView(layoutResID: Int)

根据传入的布局资源 ID 参数，该函数生成指定布局的视图并将其放置在屏幕上。

布局视图生成后，布局文件包含的部件也随之以各自的属性定义完成实例化。



**资源与资源 ID**

布局是一种资源。**资源是应用非代码形式的内容**，比如图像文件、音频文件以及 XML 文件等。

项目的所有资源文件都存放在目录 `app/res` 的子目录下。

在项目工具窗口中可以看到，`activity_main.xml` 布局资源文件存放在 `res/layout/` 目录下。

`strings.xml` 字符串资源文件存放在 `res/values/` 目录下。

可以使用资源 ID 在代码中获取相应的资源。

`activity_main.xml` 布局的资源 ID 为 `R.layout.activity_main`。

activity_main 是 R 的内部类 layout 里的一个整型常量名。



注意，从 Android Studio 3.6 开始，出于性能优化方面的考虑，已经不再生成 `R.java` 文件，AGP 现在直接生成 R 类字节码。

[https://android-developers.googleblog.com/2020/02/android-studio-36.html](https://android-developers.googleblog.com/2020/02/android-studio-36.html#:~:text=Additionally%2C%20Android%20Gradle%20plugin%20has%20made%20significant%20performance%20improvement%20for%20annotation%20processing/KAPT%20for%20large%20projects.%20This%20is%20caused%20by%20AGP%20now%20generating%20R%20class%20bytecode%20directly%2C%20instead%20of%20.java%20files.)



为按钮添加资源 ID：`android:id="@+id/true_button"`。

注意，android:id 属性值前面有一个 **+** 标志，android:text 属性值则没有。

这是因为在创建资源 ID，而对字符串资源只是做引用。



## 部件的实际应用

```kotlin
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.Button

class MainActivity : AppCompatActivity() {

    private lateinit var trueButton: Button
    private lateinit var falseButton: Button

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        trueButton = findViewById(R.id.true_button)
        falseButton = findViewById(R.id.false_button)
    }

}
```



在 activity 中，可以调用 `Activity.findViewById(Int)` 函数引用已生成的部件。

该函数以部件的资源 ID 作为参数，返回一个视图对象。



只有在 onCreate 函数里调用 setContentView 函数后，视图对象才会实例化到内存里。

在属性声明时，使用 lateinit 修饰符。这实际是告诉编译器，在使用属性内容时，会保证提供非空的 View 值。

然后在onCreate 中，找到视图对象并赋值给对应的视图属性。





Android 应用属于典型的事件驱动类型。

不像命令行或脚本程序，事件驱动型应用启动后，即开始等待行为事件的发生，比如用户点击某个按钮。

事件也可以由操作系统或其他应用触发，但用户触发的事件更直观。



应用等待某个特定事件的发生，也可以说应用正在「监听」特定事件。

为响应某个事件而创建的对象叫作**监听器**（listener）。

监听器会实现特定事件的**监听器接口**（listener interface）。


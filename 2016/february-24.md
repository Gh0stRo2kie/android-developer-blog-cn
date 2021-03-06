 
# Android Support Library 23.2

> wizChen翻译 原文链接[http://android-developers.blogspot.jp/2016/02/android-support-library-232.html](http://android-developers.blogspot.jp/2016/02/android-support-library-232.html)

来自 [Ian Lake](https://plus.google.com/+IanLake)，开发人员

### Android Support Library 23.2

当谈及到Android支持库的时候，必须重要的意识到这不是一个单一的库，而是为了寻求提供向后兼容各种版本API的所有库的[集合](http://developer.android.com/tools/support-library/features.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)，以及在最新版本中提供独特的特性。23.2在支持库添加了很多新的特新，并且对已存在的库进行了向下兼容。

[视频戳我](https://youtu.be/3PIc-DuEU2s?list=PLWz5rJ2EKKc9e0d55YHgJFHXNZbGHEXJX)

### 支持Vector Drawables和Animated Vector Drawables

[Vector drawables](https://www.youtube.com/watch?v=wlFVIIstKmA&utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)可以让你在XML中定义，用单个的矢量图形替代多个png的图片资源。虽然之间荣Lollipop和更高版本，但通过**support-vector-drawable**和 **support-animated-vector-drawable**支持库，现在*VectorDrawable* 和 *AnimatedVectorDrawable* 分别都是向下兼容的。

[Android Studio 1.4](http://android-developers.blogspot.com/2015/09/android-studio-14.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)针对矢量图形通过[在构建时生成png资源](https://www.youtube.com/watch?v=8e3I-PYJNHg&t=221&utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)提供了有局限的支持。为了禁用该功能（获得真正的优势和该支持库的空间节约），**你需要添加 *vectorDrawables.useSupportLibrary = true* 到你的 *build.gradle* 文件中**：

```
// Gradle Plugin 2.0+  
 android {  
   defaultConfig {  
     vectorDrawables.useSupportLibrary = true  
    }  
 }  
```

你将会发现这个新的属性只存在于版本号为2.0的Gradle插件中。如果你的Gradle版本为1.5，你可以这样使用：

```
 // Gradle Plugin 1.5  
 android {  
   defaultConfig {  
     generatedDensities = []  
  }  

  // This is handled for you by the 2.0+ Gradle Plugin  
  aaptOptions {  
    additionalParameters "--no-version-vectors"  
  }  
 }  
```

在API版本7及以上你可以使用VectorDrawableCompat，API版本11及以上可以使用AnimatedVectorDrawableCompat。由于Android drawable的加载机制，不是每一个允许放一个drawable id（比如在一个XML文件中）的地方都支持加载矢量图片。

首先，当使用AppCompat和[ImageView ](http://developer.android.com/reference/android/widget/ImageView.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)（或者是其子类，例如[ImageButton](http://developer.android.com/reference/android/widget/ImageButton.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)和[ FloatingActionButton](http://developer.android.com/reference/android/support/design/widget/FloatingActionButton.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)），你可以使用新的属性 **app:srcCompat** 去指向矢量图片资源（正如其他的drawable资源可以用[android:src](http://developer.android.com/reference/android/widget/ImageView.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog#attr_android:src)引用一样）：

```
 <ImageView  
  android:layout_width="wrap_content"  
  android:layout_height="wrap_content"  
  app:srcCompat="@drawable/ic_add" />  
```

如果你想在运行的时候改变图片资源，你可以使用一个和之前相同（功能）的方法[ setImageResource()](http://developer.android.com/reference/android/widget/ImageView.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog#setImageResource(int)) ——（和之前）没有变化。**使用AppCompat 和 *app:srcCompat* 是将矢量图形集成到你的app中最简单的方法**。

你会发现直接使用*app:srcCompat*设置矢量资源无法在Lollipop之前的版本上生效。然而，AppCompat提供了加载矢量资源的支持，现在矢量图可以被其他drawable容器引用，比如[StateListDrawable](http://developer.android.com/reference/android/graphics/drawable/StateListDrawable.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog), [InsetDrawable](http://developer.android.com/reference/android/graphics/drawable/InsetDrawable.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog), [LayerDrawable](http://developer.android.com/reference/android/graphics/drawable/LayerDrawable.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog), [LevelListDrawable](http://developer.android.com/reference/android/graphics/drawable/LevelListDrawable.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog) 和 [RotateDrawable](http://developer.android.com/reference/android/graphics/drawable/RotateDrawable.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)。当然，也可以间接使用该属性，在这些不被正常支持矢量图片资源情况比如[TextView](http://developer.android.com/reference/android/widget/TextView.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)的[android:drawableLeft](http://developer.android.com/reference/android/widget/TextView.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog#attr_android:drawableLeft)属性下你却可以引用矢量图片资源。

### AppCompat DayNight主题
对于AppCompat来说，允许使用矢量图形在你的app里面已经是非常大的改变了，在这次发布中我们还在AppCompat中添加了这样的主题：**Theme.AppCompat.DayNight**。

![](https://3.bp.blogspot.com/-PCq6in0WXBs/Vsyp7EVfSsI/AAAAAAAACmQ/fMHWVrVibf0/s640/image05.png)
![](https://1.bp.blogspot.com/-Ru64P9N2S_M/VsyqB1Fw5gI/AAAAAAAACmU/dYegY5HFn58/s640/image01.png)

在API14之前，使用 *DayNight* 主题和它的扩展 *DayNight.NoActionBar*，*DayNight.DarkActionBar*，*DayNight.Dialog*等，将会变成Light主题。但是在API版本14及其之后的设备上，**这个主题允许应用可以非常容易的支持 *Light* 和 *Dark* 主题**，可以很轻松的切换日、夜间主题。

默认情况下，判断是否是‘夜间’是根据系统设置来的（从[UiModeManager.getNightMode()](http://developer.android.com/reference/android/app/UiModeManager.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog#getNightMode())可以获取），但是你可以用[ AppCompatDelegate](http://developer.android.com/reference/android/support/v7/app/AppCompatDelegate.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)里面的方法覆写那个值。你可以用静态方法*AppCompatDelegate.setDefaultNightMode()*在你整个应用中设置默认值或者通过[ getDelegate()](http://developer.android.com/reference/android/support/v7/app/AppCompatActivity.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog#getDelegate())和*setLocalNightMode()*方法只改变当前的*Activity*和*Dialog*的主题。

当使用AppCompatDelegate.MODE_NIGHT_AUTO时，系统会根据你所在位置和时间自动切换日夜模式，使用*MODE_NIGHT_NO* 和 *MODE_NIGHT_YES* 将会分别强制主题变成日间模式和夜间模式。

比较关键的一点是，在你使用*DayNight*主题当作硬编码颜色的情况下，当你彻底的测试你的应用时，发现会很容易导致不可读的文字和图标。如果你使用的是标准的[TextAppearance.AppCompat](http://developer.android.com/reference/android/support/v7/appcompat/R.style.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog#TextAppearance_AppCompat)样式应用到你的文字或者颜色例如[android:textColorPrimary](http://developer.android.com/reference/android/R.attr.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog#textColorPrimary)，你会发现这些（颜色）都是会自动更新的。

然而，如果你更愿意为了夜间模式去特地的自定义资源，AppCompat复用了[夜间限定文件夹](http://developer.android.com/guide/topics/resources/providing-resources.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog#NightQualifier)，让自定义你需要的一些资源成为可能。请考虑在AppCompat使用标准的颜色或者推荐的着色支持，因为这更容易。

### 设计支持库：Bottom Sheets
[设计支持库](http://android-developers.blogspot.com/2015/05/android-design-support-library.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)提供了[material design](https://www.google.com/design/spec/material-design?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)许多样式的实现。这次发布允许开发者简单的在应用中增加[bottom sheet](https://www.google.com/design/spec/material-design?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)。

![](https://4.bp.blogspot.com/-tHhmGm8q1Qs/VsyqSo_IBDI/AAAAAAAACmY/EWy2HbMmGYg/s640/image06.png)

通过附加一个*BottomSheetBehavior*给一个[CoordinatorLayout](http://developer.android.com/reference/android/support/design/widget/CoordinatorLayout.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)的子视图（比如*app:layout_behavior=”android.support.design.widget.BottomSheetBehavior”*），你会得到五个状态之间的适当的触摸检测转换：

- STATE_COLLAPSED：这是默认的属性，沿着布局的底部显示。高度可以由
  *app:behavior_peekHeight* 属性控制(默认为0)
- STATE_DRAGGING：当用户同时直接拖动底部向上或者向下的中间状态
- STATE_SETTLING：当视图被确定和设置到它的位置之间的那片刻时间
- STATE_EXPANDED：底部的表单完全被伸展的状态，也就是整个底部表单可见（如果它的高度比CoordinatorLayout容器小的话）或者整个CoordinatorLayout被填充的时候。
- STATE_HIDDEN：默认下不可用的（启用该属性的方法：*app:behavior_hideable attribute*），开启该状态允许用户下拉底部表单去隐藏它。

记住在你底部表单的可滑动容器内必须支持嵌套滑动（例如，[NestedScrollView](http://developer.android.com/reference/android/support/v4/widget/NestedScrollView.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)，[RecyclerView](http://developer.android.com/reference/android/support/v7/widget/RecyclerView.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)，或者是API版本21以上的 ListView/ScrollView）

如果你需要接收到状态的回调，你可以增加一个BottomSheetCallback:

```
 // The View with the BottomSheetBehavior  
 View bottomSheet = coordinatorLayout.findViewById(R.id.bottom_sheet);  
 BottomSheetBehavior behavior = BottomSheetBehavior.from(bottomSheet);  
 behavior.setBottomSheetCallback(new BottomSheetCallback() {  
    @Override  
    public void onStateChanged(@NonNull View bottomSheet, int newState) {  
      // React to state change  
    }  
      @Override  
      public void onSlide(@NonNull View bottomSheet, float slideOffset) {  
       // React to dragging events  
   }  
 });  
```

虽然BottomSheetBehavior获取到[持久的底部表单](https://www.google.com/design/spec/components/bottom-sheets.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog#bottom-sheets-persistent-bottom-sheets)实例，这次库的发布也提供了*BottomSheetDialog* 和 *BottomSheetDialogFragment*去填充模态底部表单。简单的用[AppCompatDialog](http://developer.android.com/reference/android/support/v7/app/AppCompatDialog.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)或者[AppCompatDialogFragment](http://developer.android.com/reference/android/support/v7/app/AppCompatDialogFragment.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)的底部表单替换可以让你的自定义dialog有一样的底部表单（样式）。

### Support v4: MediaBrowserServiceCompat

[Support v4](http://developer.android.com/tools/support-library/features.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog#v4)库作为许多支持库的基础服务，它包含了在最新版平台上介绍的许多框架特性（也有一大部分独特的特点）。

添加之前发布的[MediaSessionCompat](https://www.youtube.com/watch?v=FBC1FgWe5X4&utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)类为媒体播放提供了一个坚实的基础，这次发布添加了 *MediaBrowserServiceCompat* 和 *MediaBrowserCompat* ，给所有API版本4以上的设备提供了一个兼容的方案，带去了最新的API（甚至是添加在Marshmallow里面的）。提供一个你可以用来连接媒体播放服务和应用UI的标准接口，这样会让在Android Auto上支持[音频播放](http://developer.android.com/training/auto/audio/index.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)和在Android Wear浏览媒体（文件）更加容易。

### RecyclerView

[RecyclerView](http://developer.android.com/reference/android/support/v7/widget/RecyclerView.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)这个部件为创建lists和grids（布局）提供了一个高级的灵活的基础，当然也支持[动画](https://www.youtube.com/watch?v=imsr8NrIAMs&utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)。这次发布带来了一个令人激动的新特性的*LayoutManager* API：自动测量！这将允许一个RecyclerView自己基于它的内容去测量自己的大小，例如使用*WRAP_CONTENT*作为RecyclerView的尺寸现在也是可能的。你会发现所有的LayoutManagers构建现在已支持自动测量。

由于这一次的改变，请确保仔细检查你的item视图的布局参数：之前被忽略的布局参数（如在滚动位置的MATCH_PARENT）现在均已被支持。

如果你有一个自定义的LayoutManager不是继承自LayoutManagers里面构建的（任意）一个，这是一个**可选的**的API——你可以调用*setAutoMeasureEnabled(true)* 方法，做了什么改变请参考官方文档。

记住虽然*RecyclerView*可以让它的子视图做动画，但是他不会因边界的改变发生动画，如果你希望你的RecyclerView边界改变时有动画，你可以使用[过度的API](http://developer.android.com/training/transitions/overview.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)。

### Custom Tabs

[Custom Tabs](http://android-developers.blogspot.com/2015/09/chrome-custom-tabs-smooth-transition.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)让App无缝的过度到网页内容成为可能。随着这一次的发布，你现在可以在网页内容的底部添加一个功能条。

![](https://1.bp.blogspot.com/-z_TM7Ch8fE0/VsyqZz2okNI/AAAAAAAACmc/3HT9_R_IhYU/s640/image04.png)

使用addToolbarItem()函数，你最多可以添加5（MAX_TOOLBAR_ITEMS）个按钮在底部功能条，并且可以用setToolbarItem()方法在开始运行的时候更新他们，类似以前的setToolbarColor()这种方法，你也能找到例如setSecondaryToolbarColor()这种方法，去自定义背景色和底部功能条

### Leanback for Android TV

[Leanback Library](http://developer.android.com/tools/support-library/features.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog#v17-leanback)提供给你把你的应用简单的[迁移到Android TV](https://www.youtube.com/watch?v=yT4ADuZGEVY&utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)的工具，发布了很多针对电视的标准组件，[GuidedStepFragment](http://developer.android.com/reference/android/support/v17/leanback/app/GuidedStepFragment.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)得到了很大的提升。

![](https://4.bp.blogspot.com/-YmvEulQtB5o/VsyqlGkmHXI/AAAAAAAACmg/DZxERRItP0Q/s640/image02.png)

最明显的改变可能是引入了一个两列可用的Action（通过重写onCreatButtonActions()或者调用setButtonActions()添加的）。这使得到达列表底部更加简单通过滚动[GuidedActions](http://developer.android.com/reference/android/support/v17/leanback/widget/GuidedAction.html?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)。

对于GuidedActions，有大量的新特性帮助你在输入时放入提示，通过descriptionEditable()，为确定添加动作（使用subActions()），还有一个GuidedDatePickerAction。

![](https://4.bp.blogspot.com/-FahHAG7DavY/VszPhjBLnzI/AAAAAAAACm0/i7OXfxFI3-Q/s640/tv_combined_image.png)

这些功能帮助你最大化帮助用户输入和制作优雅的交互。

### 现在已经可以获取

可以通过你的Android SDK Manager和Android Studio得到23.2版本的Android支持库。从现在开始利用所有的新特性，开始进行bug修复!像往常一样,bug报告需要在[b.android.com](https://code.google.com/p/android/issues/entry?template=Support%20Library%20bug&utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog) 进行或者联系其他在[Android Development Google+ 社区](https://plus.google.com/communities/105153134372062985968?utm_campaign=android_launch_supportlibrary23.2_022216&utm_source=anddev&utm_medium=blog)的开发人员。
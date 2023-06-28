# Easy Controller

## Engaging Hook(Mix Cut)

It's a powerful tool package designed specifically for Cocos Creator 3.8 or above, offering a range of features and functionalities to enhance your game development process.

A simple drag-and-drop operation will allow you to control camera and character actions smoothly on your phone and computer.

Alright, now that we know what EasyController has offered, let's explore how to use it in your projects. Here's a step-by-step guide to get you started quickly.

## Greeting (Facing Camera)

大家好，欢迎观看本视频，我是麒麟子。

今天给大家分享一个我编写的，特别实用的虚拟摇杆工具包，它可以让你在五分钟就完成摄像机和角色的操控，极大地提升开发效率。

更重要的是，它是开源免费的，你可以在 Cocos Store 上免费获得。

话不多说，我们现在开始。

## How to get it

打开 Cocos DashBoard，切换到商城标签。

在搜索栏输入 'EasyController'， 回车，你可以发现它在这里了。

点击图标进入到详情页面。

由于它是免费的，直接点击 获取 按钮就可以添加到自己的资源库。

然后点击 创建项目，输入一个项目名称。 点击确认，可以看到它已经开始下载了。

当下载完成后，会弹出确定提示。

在这里，你可以点击 ’打开项目‘按钮，直接启动 Cocos Creator。

也可以点击 取消，回到 项目 标签，找到刚刚新建的 项目，点击并启动。

## Project Structure

提醒一下，这个工具包需要 Cocos Creator 3.8 以上才能打开，不要忘了安装对应的版本哦。

第一次启动项目，会花一点时间准备，等等就好了。

OK，当看到这个 Cocos Creator 编辑器界面的时候，就表示已经好了。

接下来，我们看看它里面都有些什么。

material, textures, scripts 等这些文件夹，看名字就知道是装的什么，就不多讲了。

我们主要关注 `kylins_easy_controller` 这个文件夹。它包含了我们今天要讲的内容的核心。

接下来，我们打开 scenes 目录下的 rooster_jump 场景。

可以看到，有一个地面，一只公鸡和一些物体。

点击编辑器上方的预览按钮启动预览。

唉，你看，我们可以控制这个小鸡上窜下跳，还可以旋转和调节摄像机。

[5 seconds show with music](music)
哎哟，它还有把伞。

好啦，接下来，让麒麟子手把手教你怎么使用这个虚拟摇杆工具包。 我们一步步来实现刚刚的场景功能。

## How to use

首先，我们需要创建一个新的场景作为测试，起名为 test.

### Introduce to Joystick prefab

#### create

我们找到 kylins_easy_controller 目录下的 ui_joystick_panel 预制体，它就是今天的主角，一切的事件驱动都是由它提供的。

它的使用方法非常简单，只需要把它拖到场景树中就可以了。

咦，我们可以看到，这里生成了一个和预制体同名的节点，并挂在了一个 Canvas 节点下。

这个Canvas 节点也是自动生成的。 不要觉得奇怪。 Cocos Creator 中所有的 2D 对象的渲染都需要在 Canvas 下进行，当我们创建一个 2D 对象时，如果场景树中没有 Canvas 节点，便会自动创建一个。

我们双击刚刚创建的节点，或者选中它按F键，可以将视角定位到这个节点。

嗯，这样看不是很方便。

对于 2D 对象，我们需要点击左上角的模式切换按钮，切换到 2D 模式。 这样就方便多了。

#### content

展开 `ui_joystick_panel` 节点，我们可以看到它下面有 4 个子节点。
`checker_camera`, `checker_movement`, `ctrl`, 和 `buttons`.

`checker_camera` 节点用来标注摄像机操作范围，当用户用鼠标或者手指在这个节点区域内拖动时，就会触发摄像机旋转事件，在脚本中监听这个事件就可以控制摄像机了。

我们可以击活它的 Sprite 组件来查看它的实际范围。 这里的白色区域就是了。

我们可以通过改变这个白色区域的大小来调节摄像机操作的范围。

它也支持使用多点触控进行摄像机缩放，比如在手机上，或者在苹果笔记本上。

当然，我们也可用鼠标来操作摄像机的缩放。

`checker_movement` 节点用来标注摇杆的触发范围，当用户在这个节点范围内点击时，摇杆就会显示出来。

同样的，可以击活它的 Sprite 组件来查看它的实际范围，这里的红色区域就是。

我们可以通过改变这个红色区域的大小来调节摇杆的触发范围。

`ctrl` 节点就是我们的真正的虚拟摇杆了， 它由摇杆底底和摇杆指针两个部分组成。

当我们在 `checker_movement` 内点击时，它就会出现，然后摇杆的指针会跟着鼠标或者手指的位置移动。

同时，它会基于摇杆指针和摇杆中心点的位置关系，派发移动事件。

`buttons` 是行为按钮的根节点，我们可以看到这里预先定义了 5 个按钮。 我们一般用它来触法角色行为，比如 跳跃，攻击，技能 等等。 你可以根据自己的需求进行添加和删除。

在这个 DEMO 中，我们只需要操作角色的跳跃，所以我们隐藏其余 4 个，只保留一个就好。

### Add a Character

接下来，我们看看如何用它操控我们的摄像机和角色。

为了让效果更直观，我们搭建一个简单的场景。

首先，切换到 3D 模式。

新建一个平面，然后调整它的缩放为 20，1，20。 然后把 ground 材质给它。

找到 assets/rooster_man 目录下的 rooster_man_skin，把它拖到场景中。 双击节点定位，可以看到这只带着伞的鸡。

### Use ThirdPersonCamera

找到主摄像机  Main Camera，然后把 ThirdPersonCamera 组件添加上去。

把刚刚的 rooster_man 节点拖到它的 target 属性上。

可以看到， 它有一些可以调节的参数。

`Look At Offset`：是指目标的偏移，我们给0，0.5，0 就好了。
`Zoom Sensitivity`: 是指摄像机缩放的灵敏度，值越大，缩放速度越快。 这里我们设置为 0.1。
Len Min 和 Lin Max 和 Len 分别是指摄像机的最近、最远和初始距离，我们保持默认就行。
`Rotate VHSeparately`:表示水平和竖直方向是否分离旋转，当勾选后，每一帧只会选择改变量最大的方向进行旋转。
`Tween Time`: 是摄像机数值变化时的缓动时间，保持默认就行。

再次启动预览。

我们可以试着操作摄像机的旋转和缩放了。
[5秒钟的秀](5秒钟的秀)

### Use CharacterMovement

我们再来试试摇杆。

咦，可以看到，它没有反应。

这是因为我们还没有处理摇杆事件。

接下来，我们回到 Cocos Creator 界面.

找到这个 CharacterMovement 组件，把它拖到 rooster_man 节点上。

可以看到这个组件有很多属性。

别怕，我们一个个来看。

`Main Camera` 是指这个场景的主摄像机节点，它用来计算移动方向，我们把主摄像机拖过来就可以了。

`velocity` 是指移动速度，我们可以设置为 2

`jump velocity` 是起跳的初速度，我们可以设置为 4

`max-jump-times` 是指在落地前可以跳跃的最大次数，用于实现多次跳跃。 如果是 0 表示不能跳。 我们设置为 2，让它可以2段跳。

下面的 idle, move, jump begin, jump loop, and jump land 动画片段用于对应的状态下的动作切换。

我们把需要的动画一个个拖过去。

好啦，我们再次启动，看看效果。

我们调节一下摄像机的位置，仔细看。
当我们的虚拟摇杆偏移量很小时，主角的移动速度会跟着变小。 当摇杆的偏移量很大时，主角的移动速度会跟着变大。

这个特性非常有用，特别在一些需要精准走位的游戏中。

## Use Physics System

我们的 CharacterMovement 组件也支持和物理系统交互。接下来我们来添加物理相关组件。

首先，给这只公鸡添加一个 RigidBody 组件，然后再添加一个 capsule collider 组件。 调节一下这个 collider 的参数，让它恰好包裹住主角的模型，

这样会让物理碰撞检测更精准。

然后，我们还要给地面添加一个碰撞体，不然主角会掉下去。

最后我再添加一些物体，让这个场景有一点交互性。

搞定，我们启动看看。

[play 10 seconds](xxx)

## Showcase and Conclusion

好啦，今天的分享就完啦。该讲的都讲完啦

啊？ 【转头】

代码还没有讲。

不过，麒麟写的代码像诗一样。

想要研究代码细节的小伙伴们就自己看啦。

如果有什么疑问，欢迎在评论区留言。

好啦，我们下次见，拜拜。

哦，不要忘了一键三连和关注哦！

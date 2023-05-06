# Use Custom Render Pipeline in your own project

这篇文章将会讲解 Cocos Creator 的新特性，**自定义渲染管线（Custom Render Pipeline）**，并演示如何将 Cocos Cyberpunk 中已经写好的自定义管线应用到自己的项目中。

![01.png](https://files.mdnice.com/user/21366/daad3e63-8e6c-41e5-8149-4ef543ad2318.png)

Cocos Cyberpunk 是 Cocos 引擎官方团队以展示引擎重度 3D 游戏制作能力，提升社区学习动力而推出的完整开源 TPS 3D游戏，支持 Web, IOS, Android 多端发布。

本系列文章将从各个方面对源码进行解读，提升大家的学习效率。希望能够帮助大家在 3D 游戏开发的路上更进一步。

工程源码免费下载页面:
[https://store.cocos.com/app/detail/4543](https://store.cocos.com/app/detail/4543)

看着 **Cocos Cyberpunk** 项目中的光影效果，不少朋友眼馋。

麒麟子收到的最多的咨询是：

**如何将 Cocos Cyberpunk 的自定义渲染技术用到自己的项目中。**

今天这篇文章就专门满足大家这个需求，主要分享下面几个点：

1. **CRP Graph**
2. **管线选择**
3. **目录与代码结构**
4. **在自己的项目中使用 Cocos Cyberpunk 中的 CRP**

>为了方便描述，在麒麟子的文章中，暂时都简称为 **CRP**。这样既能很省事，又显得高端大气上档次。

## 先报个喜

在看了麒麟子写的关于 **Cocos Cyberpunk** 的源码导读的文章后，很多开发者表示很有用，纷纷加入了学习之旅。

**“Cocos Cyberpunk 源码学习交流群”** 在入群门槛极高的情况下，也迎来了不少人加入，真心希望大家都能通过这个项目收获满满。

很多朋友问我的留言功能怎么没有了，之前的文章怎么不见了。

在这里，请给我一个机会解释一下：

之前的号因为主体注销，所以公众号不能用了。目前这个号是新号。

以前老号上的文章，会根据情况用最新版的 Cocos Creator 重写，敬请期待！

**欢迎新老朋友关注这个新号。**

话不多说，我们正式开始吧。

## 自定义渲染管线 - CRP

![02.png](https://files.mdnice.com/user/21366/1d2121bb-6098-4e4d-b1ac-ff26bae7c091.png)

**Cocos Cyberpunk** 作为引擎官方重度 **3D** 游戏 **DEMO**，在渲染方面下的功夫显然不会少。

这里最重要的特性就是：**自定义渲染管线（Custom Render Pipeline）**。

虽然在 **Cocos Creator** 3.7 中 **CRP** 仍然作为实验性功能提供。

但在 **Cocos Cyberpunk** 项目中，为了以最小的性能开销达到目标画质，整个 **Cocos Cyberpunk** 项目的渲染基于 **CRP** 构建，实现了**前向渲**染、**延迟渲染**、**后期效果渲染**。

并且支持**原生**和 **web** 平台发布。

不管是作为 **CRP** 模板引用到自己的项目，还是作为 **CRP** 学习资料，都是一个不错的示范。

从今天开始，麒麟子会分享自己对于 **Cocos Cyberpunk** 渲染方面的理解，希望能够给大家带来帮助。

## CRP Graph

为了让大家有一个舒适的开始，我们从简单易懂看得见摸得着的部分开始。

![03.png](https://files.mdnice.com/user/21366/26e98dc9-be98-4f70-8079-f561989cbbd0.png)

在 **Cocos Cyberpunk** 中，为了方便查看渲染管线流程和状态，负责渲染的大佬实现了一个可视化的渲染管线流程图，如上图所示。

只需要三个步骤就可以打开这个流程图：

1. 新建一个空节点
2. 将 **pipeline/graph/pipeline-graph.ts** 组件拖到这个节点上
3. 勾选组件上的 **Edit** 复选框，就会弹出 **RenderPipeline Graph** 窗口
![04.png](https://files.mdnice.com/user/21366/56569972-cef2-48d6-a0c1-a340d9f04039.png)

从图中我们可以看出，它实现了三个渲染管线：**forward**、**main** 和 **reflection probe**。

管线中每个 **Pass** 具体的实现细节，会在后面的文章中逐一讲解。

今天我们先来看看三个管线的基本内容。

### forward 管线

![05.png](https://files.mdnice.com/user/21366/116a1d38-e91e-425d-b7f3-6e5e9b99e0ba.png)

**forward** 管线主要用于实现 **UI** 渲染，它非常简单，只有两个 **Pass**：

- **ForwardPass**：使用前向渲染方式渲染 3D 场景
- **ForwardPostPass**：用于前向渲染管线的后期着色阶段
  
### main 管线

![06.png](https://files.mdnice.com/user/21366/0bdbd7ec-f048-47f3-99ca-d8f3e825746b.png)

从名字就可以看出，这是 **Cocos Cyberpunk** 主要的管线，它一共由 8 个 **Pass** 构成：

- **CustomShadowPass**：
- **DeferredGBufferPass**：延迟渲染 GBuffer 阶段
- **DeferredLightingPass**：延迟渲染光照阶段
- **custom.BloomPass**：全屏泛光、辉光
- **TAAPass**：TAA抗锯齿
- **FSRPass**：超分技术
- **ZoomScreenPass**：屏幕缩放
- **DeferredPostPass**：用于延迟渲染管线的后期着色阶段

### reflection probe 管线

![07.png](https://files.mdnice.com/user/21366/50535eba-772b-47fb-9a59-d964a458432d.png)

这个管线原本用来烘焙反射探针的时候用的，但目前烘焙反射探针的时候也用的 **main** 管线，**目前没有使用**。它一共包含了三个 **Pass**：

- **DeferredGBufferPass**
- **DeferredLightingPass**
- **DeferredPostPass**

**Cocos Cyberpunk** 中的反射探针部分的内容特别棒，麒麟子后面会专门用一篇文章来分享。

## 管线选择

接下来我们看看，在 **Cocos Cyberpunk** 项目中是如何决定使用哪一条渲染管线的。

为了更好地表述，我们需要区分一下两种摄像机。

- **运行时摄像机**：就是我们自己在场景上创建的摄像机节点，它用来在项目运行时，渲染场景中的节点。
- **编辑器内置摄像机**：这是编辑器自带的摄像机，用于渲染场景编辑器中的预览，它有一个固定的名称叫：**Editor Camera**。

在 **pipeline/pipeline-manager.ts** 的 **renderCamera** 函数中，我们可以看到以下代码：

```ts
if (!pipelineName) {
    pipelineName = 'forward';
    if (cameraSetting) {
        pipelineName = cameraSetting.pipeline;
    }
    else if (camera.name === 'Editor Camera') {
        if (camera.projectionType === renderer.scene.CameraProjection.ORTHO) {
            pipelineName = 'forward';
        }
        else {
            pipelineName = 'main';
        }
    }
}
```

从上面的代码可以看出：

1、当场景中的运行时摄像机没有挂接 **CameraSetting** 组件时，会按照 **forward** 方式执行渲染，否则会按 **CameraSetting** 组件指定的方式渲染。

2、当编辑器摄像机在渲染 **UI** 时，会采用 **forward** 管线渲染，其它情况则使用 **main** 管线渲染。 这就是为什么大家在场景编辑器中能实时看到Bloom/TAA等效果的原因。

接下来，我们看看 **Cocos Cyberpunk** 中使用到的 3 个 运行时摄像机。

### 1、UI摄像机

打开 **scene-game-start** 场景，定位到 **init/canvas/Camera** 摄像机节点，这个摄像机用于渲染项目中的 **UI**。

可以看到它有一个 **CameraSetting** 组件，**Pipeline** 属性的值为 **forward**。

### 2、漫游摄像机

打开 **scene** 场景，定位到 **Main Camera/Camera**，可以看到它有一个 **CameraSetting** 组件，**Pipeline** 属性的值为 **main**。

这个摄像机就是当游戏开始时，玩家没有点 START 前，用于漫游场景的那个摄像机。

![08.png](https://files.mdnice.com/user/21366/c79e6938-1058-4200-8598-4f5776b0e572.png)

### 3、主角摄像机

打开 **assets/resources/obj/player-tps.prefab**， 定位到 **player-tps/camera_root/camera_player**， 可以看到它有一个 **CameraSetting** 组件， **Pipeline** 属性值为 **main**。

这个摄像机就是挂在主角身上，用来操控游戏视角的摄像机。

可以看到，运行时摄像机也是：**UI** 使用 **forward** 管线，**3D** 场景使用 main 管线。

## 目录与代码结构

![09.png](https://files.mdnice.com/user/21366/0064bee7-3ab8-4727-9f18-eec2fbed89b9.png)

为了方便大家快速上手，在这篇文章里，我们先来看看代码结构，让大家快速定位想要看的内容。

![10.png](https://files.mdnice.com/user/21366/d5b059af-35cc-46e5-b581-6790dea77ef4.png)

在前面的文章中麒麟子有提到过，为了方便复用，负责 Cocos Cyberpunk 渲染工作的大佬，把 CRP 相关的内容以 extensions 的方式做了隔离。大家在 **资源窗口（Assets）** 中可以看到三个文件夹，assets、internal、pipeline。 其中 pipeline 就是我们的自定义管线内容。

### 目录内容

pipeline 目录内主要容如下：

- **pipeline-manager.ts**：主入口文件
- **components**：自定义渲染管线中需要用到的脚本代码组件
- **graph**：自定义渲染管线图相关内容
- **lib**：第三方库，目前用到的是 detect-gpu，用于检查 GPU 参数
- **resources**：自定义渲染管线需要用到的 effect 文件、材质文件、生成的 CRP Graph文件，pipeline 预制体
- **settings**：硬件相关的设置
- **passes**：自定义渲染管线各个 Pass 的具体实现
- **utils**：一些工具类函数，如：math、debug、event 等

### 入口

在 **pipeline-manager.ts** 中定义了一个类：**CustomPipelineBuilder**， 这就是我们的主入口。

在文件的底部，有两行代码，如下。

```ts
//将自定义渲染管线注册到引擎渲染器
if (director.root && director.root.device && director.root.device.gfxAPI !== gfx.API.WEBGL) {
    rendering.setCustomPipeline('Deferred', new CustomPipelineBuilder)
}

//设置 CC_PIPELINE_TYPE 为延迟渲染
game.on(Game.EVENT_RENDERER_INITED, () => {
    director.root.pipeline.setMacroInt('CC_PIPELINE_TYPE', 1);
})
```

rendering.setCustomPipeline 的原型如下：

```ts
setCustomPipeline(name: string, builder: PipelineBuilder): void;
```

参数

- name：自定义渲染管线名字
- builder：自定义渲染管线实例

PipelineBuilder 的原型为：

```ts
export interface PipelineBuilder {
    setup(cameras: renderer.scene.Camera[], pipeline: Pipeline): void;
}
```

它只要求用户实现 setup 函数，这个函数在渲染的时候，每帧都会被调用。 你可以把它看作 render，会更容易理解。

## 把 CRP 搬到自己的项目中

接下来，我们来讲讲大家最想要的部分：**如何将 Cocos Cyberpunk 中的自定义渲染管线搬到自己的项目**。

### 搬

#### 步骤1、选择一个项目或者新建一个项目

不论是已经存在的项目 ，还是新建的项目，请确认项目使用的 Cocos Creator 版本与 Cocos Cyberpunk 使用的版本一致，否则可能会出现一些兼容问题。

#### 步骤2、复制 pipeline 到项目中

找到 Cocos Cyberpunk 工程目录下的 extensions，将它下面的 pipeline 文件夹，复制到自己的项目的 extensions 文件夹下。

如果自己的项目下没有 extensions 文件夹，请先新建一个。

#### 步骤3、开启自定义管线

![11.png](https://files.mdnice.com/user/21366/5a8485d6-462c-4bf8-a7f4-a629e450386b.png)

找到菜单 **项目**(Project)->**项目设置**(Project Settings)， 在 **功能裁剪**（Feature Cropping）标签下，勾选 **自定义渲染管线**（Custom Render Pipeline）

切换到 **宏配置**（Macro Configurations) 标签，在 **CUSTOM_PIPELINE_NAME** 处填上 **Deferred**。

![12.png](https://files.mdnice.com/user/21366/4bb83c95-3fbb-40e5-8770-2fb633c8f2fd.png)
> 这个 **Deferred** 对应的就是 **pipeline-manager.ts** 中注册到引擎的管线名称。有兴趣的朋友可以自己改一个名字尝试。

#### 步骤4、重启编辑器

有两种方式：

1. 关闭编辑器，再次打开
2. CTRL + R

### 验证

接下来，我们来做一个辉光（Glow）效果，来验证一下自定义管线是否生效。

#### 步骤1、新建一个场景，并创建一些球或者立方体

为了方便观察，麒麟子建了一个 **Plane**，在 **Plane** 上面放了两个立方体。

#### 步骤2、新建材质

**Cocos Cyberpunk** 中的自定义管线属于深度自定义，实现了一套延迟渲染。材质引用的 **effect** 必须使用 **pipeline/resources/surface/custom-surface**。

所以我们新建了三个材质，并把它们的 **effect** 都指定好，拖动给所有的对象。

#### 步骤3、调节材质

为了方便观察，我们把 **Plane** 材质的 **Albedo** 调暗。可以得到下面这样的效果。
![13.png](https://files.mdnice.com/user/21366/6995d135-acf7-4751-9bef-a26242e5ca86.png)

接下来，我们把其中一个立方体的材质的 **Emissive** 更换一下颜色，并加大 **Emissive Scale** 的值。 你会发现它发光了。

![14.png](https://files.mdnice.com/user/21366/235f2a86-0a66-446d-b242-fb2b6ce320b0.png)

启动网页预览，也能出现同样的效果，这就说明，集成成功了。

好了，接下来，朋友们可以自己玩了。

## 总结

今天这篇文章，只是对 **Cocos Cyberpunk** 中已经写好的自定义渲染管线做了简单的讲解，并教会了大家如何集成到自己的项目中。

后面的文章中，麒麟子会介绍 **Cocos Creator** 3.7 版本中的自定义渲染管线机制，并结合 **Cocos Cyberpunk** 进行实例讲解。

希望能够让更多想要为自己项目定制渲染管线，或者想通过自定义渲染管线研究3D渲染技术的朋友们带来一些帮助。

今天的文章就到这里，感谢大家的观看，新老朋友记得关注麒麟子，后续内容更精彩。

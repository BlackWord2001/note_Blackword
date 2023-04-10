# <center>函数</center>

## Application.OpenURL()
`Application.OpenURL()` 是用于在 Unity 应用程序中打开指定的 URL。这个 API 是跨平台的，可以支持文件处理程序，使 Unity 开发人员不必为每个平台编写自己的处理程序。当玩家在游戏中点击链接时，游戏开发人员通常使用 OpenUrl 来启动本地系统的 Web 浏览器
~~~cs
Application.OpenURL("https://unity.cn/"); // 打开url（网页）
~~~

## Application.Quit()
`Application.Quit()` 用于退出应用程序。在编辑器中调用 `Application.Quit()` 不会有任何效果。
~~~ cs
Application.Quit();
~~~

## Debug.Log()
在Unity中，Debug.Log()是一个用于打印消息到控制台的函数1。你可以使用它来打印任何你想要的消息，例如变量的值、对象的名称等等

~~~cs
Debug.Log("Hello World!");
~~~

你还可以使用其他的Debug函数，例如 `Debug.LogWarning()` 和 `Debug.LogError()`，来打印不同类型的消息。

# <center>变量</center>

## Application.dataPath
`Application.dataPath` 用于获取包含游戏数据文件夹的路径取决于运行时所基于的平台，该路径是只读的而且游戏编译后会加密压缩。

需要注意的是，在移动端上，不允许使用 Application.dataPath 来读写数据，否则会直接卡住

## Application.persistentDataPath
`Application.persistentDataPath` 持久化文件路径，相当于获取不同操作系统给应用分配的一个存储空间（可读写），用于保存游戏对应的一些数据或者资料。

## Application.streamingAssetsPath
StreamingAssets 文件夹只读，但是不会被加密，主要存储一些比如游戏的配置文件等，StreamingAssets 这个目录需要自己手动在Assets下创建。

## Application.temporaryCachePath
Application.temporaryCachePath 是获取当前平台的临时文件目录。
如windows下的 `C:/Users/UserName/AppData/Local/Temp/DefaultCompany/项目名称`

## Application.runInBackground
`Application.runInBackground` 控制游戏是否可以在后台运行（默认为True）。

## Time.deltaTime
`Time.deltaTime` 是一个变量，用于获取当前帧和上一帧之间经过的时间。它用于使游戏与帧率无关，以便以相同的速度运行游戏。

~~~cs
float timer = 0;

void Update()
    {
        timer += Time.deltaTime;

        if (timer > 3)
        {
            Debug.Log("大于3秒");
        }
    }
~~~

## Time.time
Time.time 用于获取从游戏开始到现在所用的时间，它是一个静态变量，可以直接访问，而不需要实例化任何对象。

与 Time.deltaTime 不同，Time.time 不是用于计算帧之间的时间差。它是一个浮点数，表示从游戏开始到现在所用的时间（以秒为单位）。

您可以使用 Time.time 来计算游戏运行的时间，或者在游戏中使用它来控制某些事情以恒定速率发生，而不受（可能极度波动的）帧速率的影响。

~~~cs
Debug.Log(Time.time); // 游戏开始到现在所花的时间
~~~

## Time.timeScale
`Time.timeScale` 用于控制游戏时间缩放比例，它是一个浮点数，表示时间流逝的速度。

当 Time.timeScale 的值为 1 时，时间流逝的速度与现实时间相同。当 Time.timeScale 的值为 0.5 时，时间流逝的速度是现实时间的一半。当 Time.timeScale 的值为 0 时，游戏暂停。

您可以使用 Time.timeScale 来控制游戏中的时间流逝速度，例如在游戏中实现慢动作效果或加速效果。此外，您还可以使用 Time.timeScale 来控制游戏中所有与时间有关的操作，例如动画、物理模拟等。

~~~cs
Debug.Log(Time.timeScale); // 时间缩放值，默认是1倍
~~~

## Time.fixedDeltaTime
`Time.fixedDeltaTime` 用于控制物理和其他固定帧率更新（如 MonoBehaviour 的 FixedUpdate）的时间间隔，它是一个浮点数，表示时间流逝的速度

`Time.fixedDeltaTime` 的默认值为 0.02 秒，可以通过编辑器中的 Project Settings -> Time 来修改

需要注意的是，`Time.fixedDeltaTime` 的时间间隔是相对于受 `Time.timeScale` 影响的游戏内时间的时间间隔。此外，为了读取增量时间，建议改用 `Time.deltaTime`，因为当您位于 FixedUpdate 函数或 Update 函数中时，它会自动返回正确的增量时间

~~~cs
Debug.Log(Time.fixedDeltaTime); // 固定时间间隔
~~~
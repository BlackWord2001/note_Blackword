# <center>函数/变量</center>

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


## Debug.Log()
在Unity中，Debug.Log()是一个用于打印消息到控制台的函数1。你可以使用它来打印任何你想要的消息，例如变量的值、对象的名称等等

~~~cs
Debug.Log("Hello World!");
~~~

你还可以使用其他的Debug函数，例如 `Debug.LogWarning()` 和 `Debug.LogError()`，来打印不同类型的消息。


## SceneManager.GetActiveScene()
`SceneManager.GetActiveScene();` 可以获取当前场景或返回当前场景
~~~cs
using UnityEngine.SceneManagement; // 需要using此模块

// 获取当前场景
Scene scene = SceneManager.GetActiveScene();
Debug.Log(scene.name);
~~~

## SceneManager.CreateScene();
`SceneManager.CreateScene` 是 Unity 的脚本 API 中的一个静态方法，用于在运行时使用给定名称创建一个新的空场景。您可以使用它在运行时创建新场景。
~~~cs
Scene newScene = SceneManager.CreateScene("newScene");
~~~

## SceneManager.sceneCount
`SceneManager.sceneCount` 可以获取当前以加载的场景数量并返回。
~~~cs
Debug.Log(SceneManager.sceneCount);
~~~

## SceneManager.UnloadSceneAsync()
`SceneManager.UnloadSceneAsync()` 销毁所有与给定场景关联的游戏对象，并将场景从 SceneManager 中移除。

提供的场景名称可以是完整场景路径（Build Settings 窗口中 显示的路径），也可以只是场景名称。如果仅提供场景名称， 此方法将卸载场景列表中匹配的第一个场景。如果您有 多个名称相同但路径不同的场景，应该使用完整的场景路径。 支持的格式示例：\ "Scene1"\ "Scene2"\ "Scenes/Scene3"\ "Scenes/Others/Scene3"\ "Assets/scenes/others/scene3.unity"\ \ 注意：这不区分大小写。由于是异步场景，不保证完成时间。 \ 注意：目前不会卸载资源。要释放资源内存，请调用 Resources.UnloadUnusedAssets。 \ 注意：如果没有可加载的场景，则无法执行 UnloadSceneAsync。例如，只包含一个场景的项目无法使用此静态成员。
~~~cs
SceneManager.UnloadSceneAsync(newScene);
~~~

## SceneManager.LoadScene()
`SceneManager.LoadScene` 用于加载场景，
参数 | 解释
:- | :-
sceneName | 要加载的场景的名称或路径。
sceneBuildIndex	| Build Settings 中要加载场景的索引。
mode | 允许您指定是否以累加方式加载场景。
~~~cs
// 加载场景（替换当前场景）
SceneManager.LoadScene("Learn1", LoadSceneMode.Single);

// 加载场景（添加到当前场景）
SceneManager.LoadScene("Learn1", LoadSceneMode.Additive);
~~~
注意：注意：在大多数情况下，为了避免在加载时出现暂停或性能中断现象， 您应该使用此命令的异步版，即： `LoadSceneAsync`。

## Scene.isLoaded
`scene.isLoaded` 此变量可以检测场景是否已经加载并返回为bool值
~~~cs
Debug.Log(scene.isLoaded);
~~~

## Scene.path
`Scene.path` 可以获取当前场景的本地路径
~~~cs
Debug.Log(scene.path);
~~~

## Scene.buildIndex
`buildIndex` 是 `Scene` 类的一个属性，它返回场景在 Build Settings 中的索引。索引从零开始，因此第一个场景的索引为零。例如，如果 Build Settings 中有五个场景，则它们的索引为 0 1 2 3 4
~~~cs
Debug.Log(scene.buildIndex);
~~~

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
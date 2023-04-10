# <center>C# 脚本<center>

## 生命周期方法
> 以下按照调用先后顺序排序

方法名称 | 调用时间
:-|:-
Awake | 最早调用，所以一般可以在此实现单例模式
OnEnable | 组件激活后调用。在Awake后会调用一次
Start | 在Update之前调用一次， 在OnEnablo之后调用，可以在此设置一些初始值
FixedUpdate | 固定频率调用方法，每次调用与上次调用的时间间隔相同（默认为0.02ms调用一次，可以在 `项目设置` → `时间` → `固定时间步进` 中修改它）
Update | 帧率调用方法，每帧调用一次，每次调用与上次调用的时间间隔不相同
LateUpdate | 在Update每调用完一次后， 紧跟着调用一次
OnDisable | 与onEnable相反，组件未激活时调用
OnDestroy | 被销毁后调用一次


## 动态修改物体属性物体类的使用
~~~cs
// 获取当前脚本所挂载的游戏物体
GameObject go = this.gameObject;
Debug.Log(go.name);
// 更简单的获取方式
Debug.Log(gameObject.name);

// 打印标签
Debug.Log(gameObject.tag);

// 打印图层
Debug.Log(gameObject.layer);

// 打印立方体名称
Debug.Log(Cube.name);

// 激活状态
Debug.Log(Cube.activeInHierarchy); // 当前真正的激活状态
Debug.Log(Cube.activeSelf);        // 自生激活状态

// 获取Transform组件
Debug.Log(transform.position);

// 获取其他组件
BoxCollider bc = GetComponent<BoxCollider>();
gameObject.AddComponent<AudioSource>(); // 给自身“Empty”添加组件
Cube.AddComponent<AudioSource>();       // 给“Cube”添加组件

// 通过游戏物体的名称获取游戏物体
GameObject test1 = GameObject.Find("Test");
Debug.Log(test1.name);

// 通过标签名称获取游戏物体
GameObject test2 = GameObject.FindWithTag("Enemy");
Debug.Log(test2.name);
// 设置激活状态
test2.SetActive(false);

// 通过预设体来实列化一个游戏物体
// Instantiate(Prefab);
// Instantiate(Prefab, transform);
GameObject go2 = Instantiate(Prefab, Vector3.zero,  Quaternion.identity); // 把Prefab放到世界原点，不旋转(四元数)
// 销毁 
Destroy(go2);
~~~

# <center>函数</center>

## Debug.Log()
在Unity中，Debug.Log()是一个用于打印消息到控制台的函数1。你可以使用它来打印任何你想要的消息，例如变量的值、对象的名称等等

~~~cs
Debug.Log("Hello World!");
~~~

你还可以使用其他的Debug函数，例如 `Debug.LogWarning()` 和 `Debug.LogError()`，来打印不同类型的消息。

# <center>变量</center>

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
Time.time 是一个变量，用于获取从游戏开始到现在所用的时间，它是一个静态变量，可以直接访问，而不需要实例化任何对象。

与 Time.deltaTime 不同，Time.time 不是用于计算帧之间的时间差。它是一个浮点数，表示从游戏开始到现在所用的时间（以秒为单位）。

您可以使用 Time.time 来计算游戏运行的时间，或者在游戏中使用它来控制某些事情以恒定速率发生，而不受（可能极度波动的）帧速率的影响。

~~~cs
Debug.Log(Time.time); // 游戏开始到现在所花的时间
~~~

## Time.timeScale
`Time.timeScale` 是一个变量，用于控制游戏时间缩放比例，它是一个浮点数，表示时间流逝的速度。

当 Time.timeScale 的值为 1 时，时间流逝的速度与现实时间相同。当 Time.timeScale 的值为 0.5 时，时间流逝的速度是现实时间的一半。当 Time.timeScale 的值为 0 时，游戏暂停。

您可以使用 Time.timeScale 来控制游戏中的时间流逝速度，例如在游戏中实现慢动作效果或加速效果。此外，您还可以使用 Time.timeScale 来控制游戏中所有与时间有关的操作，例如动画、物理模拟等。

~~~cs
Debug.Log(Time.timeScale); // 时间缩放值，默认是1倍
~~~

## Time.fixedDeltaTime
`Time.fixedDeltaTime` 是一个变量，用于控制物理和其他固定帧率更新（如 MonoBehaviour 的 FixedUpdate）的时间间隔，它是一个浮点数，表示时间流逝的速度

`Time.fixedDeltaTime` 的默认值为 0.02 秒，可以通过编辑器中的 Project Settings -> Time 来修改

需要注意的是，`Time.fixedDeltaTime` 的时间间隔是相对于受 `Time.timeScale` 影响的游戏内时间的时间间隔。此外，为了读取增量时间，建议改用 `Time.deltaTime`，因为当您位于 FixedUpdate 函数或 Update 函数中时，它会自动返回正确的增量时间

~~~cs
Debug.Log(Time.fixedDeltaTime); // 固定时间间隔
~~~
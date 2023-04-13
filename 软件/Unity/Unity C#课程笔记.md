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

## 场景
~~~cs
// 两个类，场景类，场景管理类
// SceneManager.LoadScene("Learn1");

// 获取当前场景
Scene scene = SceneManager.GetActiveScene();
Debug.Log(scene.name);

// 场景是否已经加载
Debug.Log(scene.isLoaded);

// 场景路径
Debug.Log(scene.path);

// 场景索引
Debug.Log(scene.buildIndex);

// 获取场景下所有的子物体
GameObject[] gos = scene.GetRootGameObjects();
Debug.Log(gos.Length);

/* 场景管理类 */
// 创建新的场景
Scene newScene = SceneManager.CreateScene("newScene");

// 当前以加载的场景数量
Debug.Log(SceneManager.sceneCount);

// 卸载场景（销毁）
SceneManager.UnloadSceneAsync(newScene);

// 加载场景（替换当前场景）
// SceneManager.LoadScene("Learn1", LoadSceneMode.Single);

// 加载场景（添加到当前场景）
SceneManager.LoadScene("Learn1", LoadSceneMode.Additive);
~~~

## 位置旋转缩放

获取位置缩放旋转
~~~ cs
Debug.Log(transform.position); // 全局坐标
Debug.Log(transform.localPosition); // 相对坐标

// 获取旋转
Debug.Log(transform.rotation);
Debug.Log(transform.localRotation);
Debug.Log(transform.eulerAngles);
Debug.Log(transform.localEulerAngles);

// 获取缩放
Debug.Log(transform.localScale);

// 向量（物体的方向）
Debug.Log(transform.forward);
Debug.Log(transform.right);
Debug.Log(transform.up);
~~~

变换

~~~cs
// 时时刻刻看向世界中心点 (0, 0, 0)
transform.LookAt(Vector3.zero);

// 每帧率旋转1°
transform.Rotate(Vector3.up, 1);

// 绕某个物体旋转
transform.RotateAround(Vector3.zero, Vector3.up, 1);
~~~
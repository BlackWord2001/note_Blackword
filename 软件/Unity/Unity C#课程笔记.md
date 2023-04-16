# <center>C# 脚本<center>


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

父子关系
~~~cs
// 获取父物体
// transform.parent.gameObject

// 子物体个数
Debug.Log(transform.childCount);

// 解除与子物体的父子关系
transform.DetachChildren();

// 获取子物体
Transform trans = transform.Find("Child");
transform.GetChild(0);

// 判断一个物体是不是另一个物体的子物体
bool res = trans.IsChildOf(transform);
Debug.Log(res);

// 设置父物体
trans.SetParent(transform);
~~~

## 按键输入检测
~~~cs
// 鼠标输入 0左键 1右键 2滚轮
if (Input.GetMouseButtonDown(0))
{
    Debug.Log("按下了鼠标左键");
}

if (Input.GetMouseButton(0))
{
    Debug.Log("长按鼠标左键");
}

if(Input.GetMouseButtonUp(0))
{
    Debug.Log("抬起了鼠标左键");
}

// 键盘输入（枚举）
if (Input.GetKeyDown(KeyCode.A))
{
    Debug.Log("按下了A");
}
if (Input.GetKey(KeyCode.A))
{
    Debug.Log("长按A");
}
if (Input.GetKeyUp(KeyCode.A))
{
    Debug.Log("松开了A");
}

if (Input.GetKeyDown(KeyCode.A))
{
    Debug.Log("按下了A");
}
// 键盘输入（字符）可以使用小写不能使用大写
if (Input.GetKeyDown("b"))
{
    Debug.Log("按下了B");
} 

~~~

## 虚拟轴
那么虚拟轴到底是什么?简单来说，虚拟轴就是一-个数值在-1~ 1内的数轴，这个数轴上重要的数值就是-1.0和1。
当使用按键模拟一个完整的虚拟轴时需要用到两个按键,即将按键1设置为负轴按键,按键2设置为正轴按键。在没有按下任何按键的时候，虚拟轴的数值为0;在按下按键1的时候，虚拟轴的数值会从0~-1进行过渡;在按下按键2的时候，虚拟轴的数值会从0~ 1进行过渡


    按键1           没按键          按键2
    |_______________|_______________|
    -1              0               1

`项目设置` → `输入管理` → `轴线` 可以查看虚拟轴设置

获取虚拟轴
~~~cs
float horizontal = Input.GetAxis("Horizontal");
float vertical = Input.GetAxis("Vertical");
Debug.Log(horizontal + "   " + vertical);
~~~

获取虚拟按键
~~~cs
if(Input.GetButtonDown("Jump"))
{
    Debug.Log("空格");
}
~~~

## 音乐音效
再添加音效c#代码前先给物体增加一个 `Audio Source` 组件才能让物体正常播放音频

代码中 `music1` 为音乐 `music2` 为音效
~~~cs
public class AudioTest : MonoBehaviour
{
    public AudioClip music1;
    public AudioClip music2;

    // 播放器组件
    private AudioSource player;

    void Start()
    {
        player = GetComponent<AudioSource>();
        // 设定播放的音频片段
        player.clip = music1;
        // 循环
        player.loop = true;
        // 音量
        player.volume = 0.5f;
        // 播放
        player.Play();
    }

    // Update is called once per frame
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            // 如果当前正在播放
            if (player.isPlaying)
            {
                // 暂停播放
                player.Pause();

                // 停止播放
                // player.Stop();
            }
            else
            {
                // 暂停
                player.UnPause();

                // 开始播放
                // player.Play();
            }
        }

        // 按鼠标左键播放声音
        if (Input.GetMouseButtonDown(0))
        {
            // 播放音效
            player.PlayOneShot(music2);
        }
    }
}
~~~

## 角色控制初步实现

在添加C#脚本之前先给我们的角色创建一个Character Controller的组件

~~~cs
private CharacterController player;

void Start()
{
    player = GetComponent<CharacterController>();
}

void Update()
{
    // 水平
    float horizontal = Input.GetAxis("Horizontal");
    // 垂直
    float vertical = Input.GetAxis("Vertical");
    // 创建成一个方向向量
    Vector3 dir = new Vector3(horizontal, 0, vertical);
    // Debug.DrawRay(transform.position, dir, Color.red);
    // 朝向该方向移动（受重力影响）
    player.SimpleMove(dir * 2);
}
~~~

## 火球物理和爆炸
1. 先从unity资源商店中下载名为 `Procedural fire` 的火焰资源包
2. 随便挑选一个火焰的预制体然后在上面挂上名为 `FireTest` 的脚本
3. 在 `Explosion` 预制体上挂一个名为`ExplosionTest`的脚本，用于编写爆炸效果销毁的功能

FireTest.cs
~~~cs
// 创建一个爆炸的预设体
public GameObject Perfab;

// 监听发生碰撞
private void OnCollisionEnter(Collision other) 
{
    // 创建一个爆炸物体 (爆炸物体, 生成位置，旋转)
    Instantiate(Perfab, transform.position, Quaternion.identity);
    // 销毁自身
    Destroy(gameObject);

    // 获取碰撞到的物体并打印
    Debug.Log(other.gameObject.name);
}

// 持续碰撞中
private void OnCollisionStay(Collision other) 
{
}

// 结束碰撞
private void OnCollisionExit(Collision other) 
{
}
~~~

ExplosionTest.cs
~~~cs
float timer = 0;

void Update()
{
    timer += Time.deltaTime;
    if (timer > 1)
    {
        Destroy(gameObject); // 销毁爆炸
    }
}
~~~

## 触发器

~~~cs
// 监听触发
private void OnTriggerEnter(Collider other) {
    GameObject door = GameObject.Find("door");
    if (door != null) 
    {
        door.SetActive(false);
    }
}

// 持续触发
private void OnTriggerStay(Collider other) 
{
}

// 触发结束
private void OnTriggerExit(Collider other) 
{
}
~~~

## 铰链、弹簧
Hinge Joint（铰链组件）：非常适合制作门，但也可用于制作链条、摆锤等模型。 

Spring Joint （弹簧组件）：但允许它们之间的距离发生变化，就好像它们被弹簧连接一样。 

## 物理材质
物理材质 (Physic Material) 用于调整碰撞对象的摩擦力和反弹效果。

要创建物理材质，请从菜单栏中选择 Assets > Create > Physic Material。

![image](./images/PhysicMaterial-1.png)

然后物理材质可以拖拽到相应的碰撞器中（Collider）

![image](./images/PhysicMaterial-2.png)

---
layout: post
title: "Unity3d 常用API"
categories: unity3d
tags: unity3d
---

* content
{:toc}

## 1.Event Function:事件函数 
```
    1. Reset() :被附加脚本时、在游戏物体的组件上按Reset时会触发该事件函数
    2. Start() :在游戏初始化时会执行一次
    3. Update() :每一帧都会运行这个方法
    4. FixedUpdate(): 会在指定帧调用该方法多少次
    5. LateUpdate(): 晚于Update的运行顺序，但是FPS和Update是一样的
    6. Awake() Start() : 都是在游戏物体初始化运行一次，但是Awake的运行顺序高于Start的，并且只要脚本中存在Awake方法，则无论是否挂载了该脚本都会执行该方法
    7. OnEnable(): 当将物体的SetActive设置为true时就会自动调用调用该方法
    8. OnDestory(): 当关闭游戏则会调用该方法
```   
  
## 2.Time时间类函数
```
   1. Time.time 表示从游戏开发到现在的时间，会随着游戏的暂停而停止计算。
   2. Time.timeSinceLevelLoad 表示从当前Scene开始到目前为止的时间，也会随着暂停操作而停止。
   3. Time.deltaTime 表示从上一帧到当前帧时间，以秒为单位。【一般用来控制角色、动画的运动】
   4. Time.fixedTime 表示以秒计游戏开始的时间，固定时间以定期间隔更新（相当于fixedDeltaTime）直到达到time属性。
   5. Time.fixedDeltaTime 表示以秒计间隔，在物理和其他固定帧率进行更新，在Edit->ProjectSettings->Time的Fixed Timestep可以自行设置。
   6. Time.SmoothDeltaTime 表示一个平稳的deltaTime，根据前 N帧的时间加权平均的值。
   7. Time.timeScale 时间缩放，默认值为1，若设置<1，表示时间减慢，若设置>1,表示时间加快，可以用来加速和减速游戏，回放等、非常有用。如果游戏中控制运动的都是使用了Time.deltatime的话，则可以通过设置Time.timeScale=0来暂停其运动等。
   8. Time.frameCount 总帧数
   9. Time.realtimeSinceStartup 表示自游戏开始后的总时间，即使暂停也会不断的增加。【一般用作性能测试】
   10. Time.captureFramerate 表示设置每秒的帧率，然后不考虑真实时间。
   11. Time.unscaledDeltaTime 以秒计算，完成最后一帧的时间 不考虑timescale时候与deltaTime相同，若timescale被设置，则无效。
   12. Time.unscaledTime 从游戏开始到现在所用的时间 不考虑timescale时候与time相同，若timescale被设置，则无效。
```   

## 3.GameObject类
【1】创建游戏物体的三种方法
```
	1. 通过其构造器来创建 GameObject go=new GameObejct("游戏物体名"); //一般是用来创建空的游戏来存放其他东西的。
	2. Instantiate GameObject.Instantiate(prefab) //根据Prefab或者是另外一个游戏物体来创建（克隆Colon），可以实例粒子、等其他的游戏物体，很是常用的
	3. CreattePrimitive GameObject.CreatePrimitive(PrimitiveType.**) //创建原始的游戏物体，基本的几何体
```
	
【2】 为游戏物体添加组件
```
       其中组件可以是我们自己自定义的脚本GameObject.AddComponent<组件名>
```
【3】属性、变量
```
	1. GameObject.activeInHierarchy 游戏物体是否处于激活状态，与父类有关，父类被取消激活，则子类也是取消激活的
	2. GameObject.activeSelf 自身的激活状态，与父类无关，只与自身有关。【控制组件的激活与取消激活则使用.enable=false/true】
	3. GameObject.tag 游戏物体的tag标签，具体的由程序员自定义设置
	4. GameObject.SetActive(false/true) 通过参数的控制来设置其游戏物体的激活状态，true为激活状态，反之为取消激活状态。
```
	
【4】UnityEngine.Object中的共有方法与变量
```
	1. name: 名字，调用该变量，则无论是通过GameObject还是Component都是返回游戏物体的名字
	2. Destroy() :删除游戏物体，但是不会立马在unity中删除，而是会先进行回收，等确定没对象使用的时候，在进行删除
	3. DontDestroyOnLoad() : 当加载新的场景的时候，不删除这个场景中的某个游戏物体
	4. FindObjectType<>
	5. FindObjectsType<> : t通过类型来进行查找，是进行全局的查找，则就是在整个场景中进行查找
	6. FindGameObjectWithTag :如果查到的是多个，则只返回查找到的第一个
	7. FindGameObejctsWithTag 返回查找到的游戏物体集合
```
	
【5】、消息的发送
```
	1. BroadcastMessage() 广播发送消息，则该物体上对应的方法会被调用，同时这个游戏物体上的子物体上对应的方法也会被调用的
	2. SendMessage() 发送消息，只会对这个游戏物体中脚本上的方法发送消息
	3. SendMessageUpwards() 广播发送消息，但是和BroadcastMessage()是相反的，在调用自身的方法时也会向上传递，调用其父类的方法
```
	
【6】、游戏组件的查找
```
	1. Cube cube = target.GetComponent<Cube>(); 返回一个对应的组件，如果有多个，则只返回第一个
	2. Cube[]cc= target.GetComponents<Cube>(); 返回该游戏物体上所有符合条件的组件，返回一个组件数组
	3. Cube[] xx = target.GetComponentsInChildren<Cube>(); 返回该游戏物体上的对应组件，同时返回该游戏物体的子类上对应的组件
	4. Cube[] yy = target.GetComponentsInParent<Cube>(); 返回该游戏物体上的对应组件，同时返回该游戏物体的父类上对应的组件
```
	
## 4.MonoBehaviours的类

   【1】继承的变量成员
 ```  
        1. enabled: 返回该组件是否被激活或者是被禁用，可以通过该变量来进行设置
        2. isActiveAndEnabled: 只能返回该组件是否激活的标志位，不能设置该变量，为只读的
        3. tag :该组件所对应的游戏物体的标签
        4. name :该组件所对应的游戏物体的名字
```
	
   【2】Invoke等方法、变量 将添加要调用的方法添加到等待队列中，然后等待用户设定的时间后，进行队列中的方法调用。
```   
        1. Invoke("方法1"，float time): 在等待time的时间后调用方法1
        2. bool i= IsInvoking("方法1") 返回bool值，如果方法被添加到队列中，但没有被运行则返回true,如果经过一段时间后该方法被调用了则会返回false;
        3. InvokeRepeating("方法1",time,number): 等待time时间后，会重复开始运行方法1，每秒钟运行number次。
        4. CancelInvoke（） 会暂停通过Involve/InvokeRepeating的运行，但是一般来说CancelInvoke会和InvokeRepeating组合调用。参数由自己设定
        扩充： 在脚本的类前添加[ExecuteInEditMode]：则该脚本不用按游戏运行按钮就会开始编译，只限在编辑模式里面
            在脚本的共有变量前添加[HideInInspector]:则该共有变量不会在Inspector面板进行显示
```
	 
## 5.Coroutines 协程
```
    1、定义协程：IEnumerator 方法名()
        {
            yield return 0/null ;
            yield return new WaitForSeconds(1.0f); //等待一定时间在运行下面的代码
        }
    2、开启协程：StartCoroutines(方法名());
        说明：协程开启会继续执行下面代码，不会等协程方法运行完再执行接下来的方法
     
    3、开启与关闭协程时，StartCoriutine(参数)、StopCoroutine(参数) 其中的参数要互相对应，如果传递的是方法名，则两个方法中的参数就要是方法名，如果是IEnumerator的返回值，则其中两个方法发的参数就要是IEnumerator的返回值
    1、 private IEnumerator coroutine;
    coroutine = WaitAndPrint();
    StartCoroutine(coroutine);
    StopCoroutine(coroutine);
     
    2、StartCoroutine("WaitAndPrint");
        StopCoroutine("WaitAndPrint");
    4、StopAllCoroutines() 停止所有的协程，不管你是怎么调用的
``````

## 6.OnMousexx鼠标触发事件
```
     如果是通过Collider进行触发检测的话，则要在设置中打开允许进行射线检测。
	1. OnMouseDown(): 当鼠标按下的时候触发，按一次触发一次
	2. OnMouseDrag(): 当鼠标按住不放的时候一直触发，是每一帧进行触发
	3. OnMouseUp(): 当鼠标抬起的时候触发，只执行一次
	4. OnMouseEnter(): 当鼠标进入的时候触发，进入一次触发一次
	5. OnMousetOver(): 当鼠标在触发物体的上面时，则一直触发
	6. OnMouseExit(): 当鼠标移出的时候触发
	7. OnMouseUpAsButton() 相当于是按钮的功能，当鼠标在同一个游戏物体上按下抬起的时候才会触发，按下与抬起不在同一个游戏上的话则不会进行触发。
```

## 7.Mathf类 所有的成员均为静态的
```
    Mathf.Abs() 返回绝对值的
    Mathf.Ceil() 向上取整的，10.1--->11
    Mathf.Clamp(value,min,max) 如果value的值在min--max之间的话就返回value,但是如果value的值小于min,则返回min,如果value的值大于max,则返回max，一般是用在控制角色血量，当玩家的血量减少的时候，不会出现出现低于0和大于100的情况 hp= Mathf.Clamp(hp,0,100);
    Mathf.ClosePowerOfTwo(value): 取得离value的2次方最近的值
    Mathg.DeltaAngke: 取得两个角度之间的最小夹角
    Mathf.Floor 向下取整
    Mathf.Pow(i,j) 取得i的j次方
    Mathf.MoveToWards() 一般用来做移动控制，是匀速的运动，加速度固定的
    Mathf.Lerp() 差值运算，一般是用来控制动画、运动，越往后运行的越慢的。
    Mathf.PingPong(t,maxValue) 类似乒乓球的来回运动，起始 值是0，通过t变量来控制值由0向maxValue移动，当t大于maxValue的时候又向0进行移动，然后就这样的来回往复运动，一般t变量用时间Time.deltatime来进行控制的。
```
 
## 8.Input输入类
```
    GetKey() 按键一直按着时触发
    GetKeyDown 按键被按下那一刻进行触发
    GetKeyUp 按键被按下后抬起时触发
    GetMouseButton(0/1/2) 1:左键 2:右键 3:中键 鼠标一直按着时触发
    GetMouseButtonDown() 鼠标按下那一刻触发、
    GetMouseButtonUp() 鼠标抬起的那一刻时触发
    GetButtonDown()
    GetButton()
    GetButtonUp() 这三个的参数是用户自定义的虚拟按键进行触发，其他的和上面的一样
    GetAxis("虚拟轴名") 通过按下的虚拟轴来返回-1~1之间的值，开始值是0，然后向-1/1进行渐渐的变化，有一定的加速度。一般用来控制运动的，比如是赛车的加速运动等
    GetAxisRaw() 其他的和GetAxis差不多，就是少了渐变效果，返回值只有 0 1 -1三个
    anyKeyDown 当任何按键被按下（包括鼠标按键）时返回true
    anyKey 当任何按键被按着（包括鼠标）时返回true
    mousePosition 返回鼠标在屏幕上的像素坐标，【屏幕坐标】z轴衡为0的
```
 
## 9.Vector2 二维向量
```
    magnitude: 返回向量的长度
    normalized； 返回这个向量长度为1的矢量，不管这个向量多长，也是返回1的矢量，只是返回值，不对原向量的值产生影响
    Normalize() 无参数的，也是向量化，但是调用该方法会改变原向量值，使其的值被向量化 了
    ClampMagnitude() ;将一个向量限制在参数中指定的长度之间
    MoveToWards() 用来做匀速的运动，由一个位置向另一个位置进行移动
    sqrMagnitude 对求向量的的长度时不进行开平方根运算了，减少性能的损耗，一般是用来比较两个向量的长度大小的。
    其他的参考API文档即可，较为简单。
    扩充：向量是结构体，为值类型，修改其中的变量的时候要整体进行修改，不能单独的进行单个变量的赋值修改
```
 
## 10.Vector3 三维变量
```
    Cross() 插乘运算【左手法则】，通过两个向量来获得另一个向量的方向，然后进行相关的判断
    Project() 投影运算
    Reflect() 反射运算
    Slerp() 按照角度进行插值，与lerp的按照位置信息进行插值的，一般用在炮台的旋转，使旋转的更加平滑
```
 
## 11.Random随机数类
```
    InitState（）： 通过参数指定的种子，然后再调用Range()产生随机数的时候会依据种子来进行生成，则每一次运行所生成的随机数都是一样的，是伪随机数。一般要生成的随机数不同，可以设置参数为System.DataTime.Now.Ticks：通过时间戳来完成 
    insideUnitFCircle :在单位为1的园内随机生成一个位置信息，如果要在更大的圆中生成，则可以在后面*圆的半径信息。一般用来控制随机生成敌人的位置信息
    insideUnitSphere: 在单位为1的球内随机生成一个位置信息，如果要在更大的球中生成，则可以在后面*圆的半径信息。
```
 
## 12.四元数 Quaternion
```
    欧拉角【eylarAngles】与面板中的值对应和四元数【rotation】之间是可以进行转换的，一般欧拉角是用来让用户可以直观的看到的，而四元数是用来控制内部的运算 的。
    .eulerAngles 将四元数转变为欧拉角
    Euler() 将欧拉角转变为四元数
    .LookRotation() 让玩家通过设置四元数来进行望向敌人的旋转，将向量方向转变为四元数  
    Vector3 temp = enemy.position - player.position; //获得两个位置信息之间的变量，是主角望向敌人，所以要设置向量的方向是指向敌人的
    enemp.y = 0; //如果不想主角在望向他的时候出现低头的情况，也就是y轴的值出现了变化了。
    player.rotation= Quaternion.LookRotation(temp);
    slerp() 在做朝向的旋转的时候，不建议使用lerp，而是建议使用slerp,使其的旋转朝向更为平滑，更加的自然
    Quaternion target= Quaternion.LookRotation(temp);
    player.rotation = Quaternion.Slerp(player.rotation, target, Time.deltaTime); //插值的缓慢旋转
```
	 
## 13.Rigidbody 刚体组件 控制角色的移动
```
    .position: 可以通过刚体来控制运动，在控制运动方面，使用rigibody.positon比transform.porition计算要快的多，相关的物理计算也是在刚体中计算好了，但是不建议使用这个方法来持续的控制物体的运动，不平滑，控制一两次的时候还可以使用
    MovePosition() 对position的优化，其中利用了插值运算，一般持续运动的则使用这个方法，不出现卡顿的现象
    ,rotation:
    MoveRotation 用来控制刚体的旋转的，一般不建议使用rotation,比较耗性能，建议使用MoveRotation(),然后配合Quaternion,slerp()进行使用，使其更加的平滑
    AddForce() 为刚体添加力，一般可以用在赛车游戏中，当进行短时的加速则可以给以限定时间的AddForce方法
```
 
## 14.Camera 相机组件
```
    当相机的标签是main cream时，可以通过Camer.main来进行主相机cream组件的查找射线，用来检测鼠标在屏幕上的位置信息，以及触碰到什么
        Ray ray = cameraMain.ScreenPointToRay(Input.mousePosition); //获得相机到鼠标之间的射线
    RaycastHit hit; //用来存放射线检测到的游戏物体的信息的
    bool temp = Physics.Raycast(ray, out hit); //进行射线检测
```
 
## 15.Application
```
    SreeamingAcsets: 该文件下的资源不会被压缩，导入是什么类型还是什么类型，【主要是音频、视频资源】
     
    dataPath: 工程文件路径
    streamingAssetsPath: 可以通过文件流来进行读取的文件路径
    persistenDataPath :可以实例的文件路径
    tempporaryCachePath :临时的文件路径
     
    Application.OpenURL("") 打开指定的网址
    Application.Quit() 退出游戏的运行
    .CapturScreenshot("游戏截图") 用来截图的，字符串为截图fileName
     
     
    Application.identifier 标识名
    .companyName 公司名
    productName 产品名
    instalMode 安装包名
    isEditor 是否在编辑器模式
    isFocused 是否在焦点
    isMoliePlatform 是否是移动平台
    isPlaying
    isWebPlayer
    platform 编辑器的平台
    unityVersion unity版本号
    version 项目文件版本号
    runInBackground 是否可以在后台运行
     
    UnityEditor.EditorApplication.isPlaying=false; //在编辑器模式下推出编辑状态
```
 
 
## 16.SceneManager场景类
```
    SceneManager.LoadScene() 加载下一个场景，一般是用在另一个场景不是太大的情况下
    SceneManager.LoadSceneAsync() 异步加载下一个场景，返回AsyncOperation类型，里面包含了加载的信息，加载的进度条等等。可以让用户缓解等待加载场景的时间
    sceneCount 获得当前加载的场景个数
    sceneCountInBuildSettings 在Build面板中加载的场景个数
    GetActiveScene() 获取已经加载的当前场景的信息
    GetSceneAt(index) 加载index索引的场景
     
    当加载新的场景的时候会触发下面的事件：
    activeSceneChanged 当有新场景被加载的时候就会调用这个事件
    sceneLoaded 当有新场景加载完成的时候就会触发这个事件
     
     
    扩充：事件的注册时通过加方法来进行注册的：
    SceneManger.activeSceneChanged+=OnAcitiveScenenChanged;
```

## 17.射线检测 
```
    一般射线检测要在射线检测的范围内，并且被检测物体要有Collider
    Ray ray=new Ray(起点，方向);
    PaycastHit hit; //hit中存放的是射线检测的碰撞信息
    bool temp=Physics.Raycast(ray,out hit); //具体的重载方法边用边查
     
    Ray ray = new Ray(this.transform.position + transform.forward, transform.forward); //创建射线
    RaycastHit hit; //存储射线检测到的游戏物体信息
    if(Physics.Raycast(ray,out hit)) //通过返回值来判断射线是否检测到相关的物体了
    {
    Debug.Log(hit.collider.gameObject.name);
    }
    扩充：
        Raycast；检测的是射线碰撞到的第一个物体，不具有穿透性
        RaycastAll：返回的是RaycastHit数组，具有穿透性，可以返回检测到的多个游戏物体
```
 
## 18.代码监听触发事件
```
    <Button>().onClick.AddListener(方法名)； //当触发button组件，则会触发指定的方法名的方法
     
    通过实现接口来注册监听事件： using UnityEgine.EventSystems; 导入命名空间
     
     
    IPointerDownHandler 鼠标按下的事件，具体的接口参考手册
    Raycast Target: 如果取消勾选则不做事件监听了，则无法实现检测了
```
## 19.www类 下载 是用来在网络中下载资源的
```
    public string url = "http://img.taopic.com/uploads/allimg/120727/201995-120HG1030762.jpg";
    IEnumerator Start()
    {
    WWW www = new WWW(url);
    yield return www;
    Renderer renderer = this.GetComponent<Renderer>();
    renderer.material.mainTexture = www.texture;
    }
```  
 
## 20.Touches触摸事件
```
    Input.touches: 返回放在屏幕上的手指信息，返回数组
    Touch touch1=Input.touches[0];
    touch1.position;
    TouchPhase pahse=touch1.phase phase 是用来返回手指的状态的
```
## 21.Debug.DrawRay(ray.oridin,ray.direction)
```
    绘制射线，第一个参数是原点，第二个是方向。
``` 
 
## 22.CharacterController角色控制器
```
    .SimpleMove(【vector3】) 简单移动
    .isGrounded 判断是否到地面上，bool值
    .Move() 与simpleMove的区别是要*Time.deltatime、而且simpleMove会使用自带的重力
    OnCOntrollerColliderHit(ControllerCollidrHit hit) 当有碰撞到其他的碰撞器的时候会触发这个事件函数【hit保存碰撞到的物体信息】
```
 
## 23.Mesh的设置
``` 
    material mesh指定人是什么样子的，material指定人的肤色是什么样子的
```
 
## 24.API变更
```
    弃用：Application.LoadLevel();
    新的：SceneManager.LoadScene(); 加载新的场景
    弃用
    新的：Scene scene=SceneManager.GetActiveScene(); //获得当前活动场景的信息
        SceneManger.LoadScene(scene.buildIndex) //重新加载当前场景
         
    OnLevelWasLoaded() 当场景被加载的时候调用，被弃用了
    改成事件了：sceenLoaded
```
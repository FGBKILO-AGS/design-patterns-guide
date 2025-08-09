# UML统一建模语言

## 概述

UML (Unified Modeling Language) 是一种标准化的建模语言，用于软件系统的可视化、规约、构造和文档化。在设计模式的学习和应用中，UML图是理解和表达模式结构的重要工具。

## 1. UML图的分类

UML图主要分为两大类：**结构图 (Structure Diagrams)** 和 **行为图 (Behavior Diagrams)**。

- **结构图**: 描述系统的静态结构，包括类、对象、接口、组件以及它们之间的关系。
  - **核心图**: 类图、组件图、部署图、对象图、包图。
- **行为图**: 描述系统的动态行为，即系统在时间维度上的变化和交互。
  - **核心图**: 用例图、时序图、状态机图、活动图。

---

## 2. 类图 (Class Diagram)

类图是最常用的UML图，用于描述系统中类的静态结构和它们之间的关系。

### 1.1 类的表示

#### 基本结构
```
┌─────────────────┐
│    ClassName    │  ← 类名
├─────────────────┤
│   - attribute   │  ← 属性
│   + method()    │  ← 方法
└─────────────────┘
```

#### 三层结构详解
- **第一层：类名**
  - 普通类：正常字体
  - 抽象类：斜体或 `<<abstract>>`
  - 接口：`<<interface>>` 或斜体

- **第二层：属性**
  - 格式：`访问修饰符 属性名: 类型 = 默认值`
  - 示例：`- name: String = ""`

- **第三层：方法**
  - 格式：`访问修饰符 方法名(参数): 返回类型`
  - 示例：`+ getName(): String`

### 1.2 访问修饰符

| 符号 | 访问级别 | 说明 |
|------|----------|------|
| `+` | public | 公有，任何地方都可访问 |
| `-` | private | 私有，只有本类可访问 |
| `#` | protected | 受保护，本类和子类可访问 |
| `~` | package | 包级别，同包内可访问 |

### 1.3 特殊标记

- **静态成员**：下划线表示
  - `+ getInstance(): Singleton` (静态方法)
- **抽象成员**：斜体表示
  - `+ abstractMethod()` (抽象方法)
- **常量**：全大写
  - `+ MAX_SIZE: int = 100`

### 1.4 类间关系

#### 继承/泛化 (Generalization)
```
子类 ────────▷ 父类
```
- **UML官方定义**：泛化是一般化元素和特殊化元素之间的分类关系
- **箭头表示**：空心三角形 + 实线
- **关系含义**：is-a关系，表示子类是父类的一种特殊类型
- **特征**：
  - 子类继承父类的所有属性和方法
  - 子类可以重写父类的方法
  - 子类可以添加自己特有的属性和方法
  - 支持多态性
- **代码体现**：通过extends关键字实现

#### 基础例子：简单的继承关系
```java
// 基础继承示例
public class Animal {
    protected String name;
    protected int age;
    
    public void eat() {
        System.out.println(name + " is eating");
    }
    
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
}

public class Dog extends Animal {
    private String breed;
    
    // 重写父类方法
    @Override
    public void eat() {
        System.out.println(name + " the dog is eating dog food");
    }
    
    // 添加特有方法
    public void bark() {
        System.out.println(name + " is barking");
    }
}

public class Cat extends Animal {
    // 重写父类方法
    @Override
    public void eat() {
        System.out.println(name + " the cat is eating fish");
    }
    
    // 添加特有方法
    public void meow() {
        System.out.println(name + " is meowing");
    }
}
```

#### 进阶例子：GUI组件继承体系
```java
// 进阶继承示例 - GUI组件
public abstract class Component {
    protected int x, y, width, height;
    protected boolean visible = true;
    
    public abstract void paint(Graphics g);
    public abstract void handleEvent(Event e);
    
    public void setVisible(boolean visible) {
        this.visible = visible;
    }
    
    public void setBounds(int x, int y, int width, int height) {
        this.x = x; this.y = y;
        this.width = width; this.height = height;
    }
}

public class Button extends Component {
    private String text;
    private ActionListener listener;
    
    @Override
    public void paint(Graphics g) {
        // 绘制按钮
        g.drawRect(x, y, width, height);
        g.drawString(text, x + 10, y + height/2);
    }
    
    @Override
    public void handleEvent(Event e) {
        if (e.getType() == Event.MOUSE_CLICK && listener != null) {
            listener.actionPerformed(new ActionEvent(this));
        }
    }
    
    public void addActionListener(ActionListener listener) {
        this.listener = listener;
    }
}

public class TextField extends Component {
    private String text = "";
    private boolean editable = true;
    
    @Override
    public void paint(Graphics g) {
        // 绘制文本框
        g.drawRect(x, y, width, height);
        g.drawString(text, x + 5, y + height/2);
    }
    
    @Override
    public void handleEvent(Event e) {
        if (e.getType() == Event.KEY_TYPED && editable) {
            text += e.getKeyChar();
        }
    }
}
```

- **实际应用场景**：
  - **基础场景**：动物分类、几何图形、车辆类型
  - **进阶场景**：GUI组件体系、异常处理体系、设计模式中的模板方法

#### 实现 (Realization)
```
实现类 ┄┄┄┄┄▷ 接口
```
- **UML官方定义**：实现是规约和实现之间的语义关系
- **箭头表示**：空心三角形 + 虚线
- **关系含义**：类实现接口定义的契约
- **特征**：
  - 类必须实现接口中声明的所有方法
  - 一个类可以实现多个接口
  - 接口定义了类的行为规范
  - 支持多重继承的效果
- **代码体现**：通过implements关键字实现

#### 基础例子：线程实现
一个类实现`Runnable`接口来定义一个可以在线程中运行的任务。
```java
// 基础实现示例 - 实现Runnable接口
public class Task implements Runnable {
    private String taskName;

    public Task(String name) {
        this.taskName = name;
    }

    @Override
    public void run() {
        System.out.println("Executing task: " + taskName);
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("Task " + taskName + " finished.");
    }
}

// 使用
public class Main {
    public static void main(String[] args) {
        Thread thread1 = new Thread(new Task("Download File"));
        Thread thread2 = new Thread(new Task("Process Data"));
        thread1.start();
        thread2.start();
    }
}
```

#### 进阶例子：自定义数据源切换
通过实现数据访问接口，可以灵活地切换数据源，例如从数据库切换到文件系统或云存储。
```java
// 进阶实现示例 - 数据访问策略
// 1. 定义数据访问接口 (契约)
public interface DataRepository {
    String getData(String id);
    void saveData(String id, String data);
}

// 2. 实现数据库版本
public class DatabaseRepository implements DataRepository {
    @Override
    public String getData(String id) {
        System.out.println("Fetching data for id '" + id + "' from database.");
        // ... 数据库查询逻辑 ...
        return "Data from DB";
    }

    @Override
    public void saveData(String id, String data) {
        System.out.println("Saving data for id '" + id + "' to database.");
        // ... 数据库写入逻辑 ...
    }
}

// 3. 实现文件系统版本
public class FileRepository implements DataRepository {
    @Override
    public String getData(String id) {
        System.out.println("Reading data for id '" + id + "' from file.");
        // ... 文件读取逻辑 ...
        return "Data from File";
    }

    @Override
    public void saveData(String id, String data) {
        System.out.println("Writing data for id '" + id + "' to file.");
        // ... 文件写入逻辑 ...
    }
}

// 4. 使用服务的类，它不关心具体实现
public class DataService {
    private DataRepository repository;

    // 通过构造函数注入具体实现
    public DataService(DataRepository repository) {
        this.repository = repository;
    }

    public String fetchData(String id) {
        return repository.getData(id);
    }

    public void storeData(String id, String data) {
        repository.saveData(id, data);
    }
}
```

- **实际示例**：
  - ArrayList实现List接口
  - FileWriter实现Writer接口
  - Thread实现Runnable接口

#### 依赖 (Dependency)
```
客户类 ┄┄┄┄┄> 供应类
```
- **UML官方定义**：依赖是两个模型元素间的语义关系，其中一个元素的变化会影响另一个元素
- **箭头表示**：普通箭头 + 虚线
- **关系含义**：uses-a关系，表示临时性的使用关系
- **特征**：
  - 是最弱的关联关系
  - 通常是临时性的
  - 一个类的改变可能影响另一个类
  - 不涉及属性层面的关联
- **常见形式**：
  - 方法参数：`public void method(ParameterClass param)`
  - 局部变量：`LocalClass local = new LocalClass()`
  - 方法返回值：`public ReturnClass getObject()`
  - 静态方法调用：`UtilClass.staticMethod()`
#### 基础例子：司机驾驶汽车
司机(Driver)类的方法`drive`依赖于汽车(Car)类。这种依赖是临时的，因为司机只有在“驾驶”这个动作发生时才需要汽车。
```java
// 基础依赖示例
public class Car {
    public void move() {
        System.out.println("Car is moving.");
    }
}

public class Driver {
    // drive方法依赖于Car类作为参数
    public void drive(Car car) {
        System.out.println("Driver starts driving.");
        car.move();
    }
}

// 使用
public class Main {
    public static void main(String[] args) {
        Driver driver = new Driver();
        Car car = new Car();
        driver.drive(car); // 依赖关系在此方法调用期间存在
    }
}
```

#### 进阶例子：报告生成器
一个报告生成器(ReportGenerator)依赖多个其他类来完成其工作，但这些依赖都体现在方法内部或作为方法参数，而不是类的属性。
```java
// 进阶依赖示例
public class ReportGenerator {
    // generate方法依赖多个类
    public Report generate(ReportData data) {
        // 1. 依赖格式化器 (局部变量)
        Formatter formatter = new Formatter();
        String formattedContent = formatter.format(data);

        // 2. 依赖PDF导出工具 (静态方法调用)
        PdfExporter.export(formattedContent);

        System.out.println("Report generated and exported.");
        return new Report(); // 3. 依赖Report类作为返回值
    }
}

// 被依赖的类
class ReportData { /* ... */ }
class Formatter { public String format(ReportData data) { return "Formatted data"; } }
class PdfExporter { public static void export(String content) { /* ... */ } }
class Report { /* ... */ }
```

- **实际示例**：
  - Controller依赖Service（通过方法参数）
  - 工具类的使用（通过静态方法调用）

#### 关联 (Association)
```
类A ────────> 类B
```
- **UML官方定义**：关联是类之间的结构关系，描述了连接的对象集合
- **箭头表示**：普通箭头 + 实线（可以是单向或双向）
- **关系含义**：has-a关系，表示长期性的结构关系
- **特征**：
  - 比依赖关系更强，比聚合关系更弱
  - 通常表现为类的属性
  - 可以是单向或双向关联
  - 具有持久性
- **关联类型**：
  - **单向关联**：只有一个方向的导航
  - **双向关联**：两个方向都可以导航
  - **自关联**：类与自身的关联
- **多重性约束**：
  - 1对1：一个对象关联一个对象
  - 1对多：一个对象关联多个对象
  - 多对多：多个对象关联多个对象
#### 基础例子：学生和课程 (多对多)
一个学生可以选择多门课程，一门课程也可以被多个学生选择。这是典型的多对多关联。
```java
// 基础关联示例 - 多对多
import java.util.ArrayList;
import java.util.List;

public class Student {
    private String name;
    private List<Course> courses = new ArrayList<>();

    public void addCourse(Course course) {
        courses.add(course);
    }
}

public class Course {
    private String courseName;
    private List<Student> students = new ArrayList<>();

    public void addStudent(Student student) {
        students.add(student);
    }
}
```

#### 进阶例子：订单和客户 (一对多)
一个客户可以有多个订单，但一个订单只属于一个客户。这是一种一对多关联，并且是单向的（从Order到Customer）。
```java
// 进阶关联示例 - 一对多
import java.util.List;

public class Customer {
    private String customerId;
    private String name;
    // Customer类不一定需要知道它的所有订单，所以可以没有List<Order>
}

public class Order {
    private String orderId;
    private List<OrderLine> orderLines;
    
    // Order类持有对Customer的引用，表示关联关系
    private Customer customer; 

    public Order(Customer customer) {
        this.customer = customer;
    }
}

// OrderLine和Product之间也是关联
public class OrderLine {
    private int quantity;
    private Product product; // 关联到Product
}

public class Product {
    private String productId;
    private double price;
}
```

- **实际示例**：
  - Person有Address（人有地址）
  - Order有Customer（订单有客户）
  - Teacher教Student（老师教学生）

- **关联类 (Association Class)**：
  - 当一个关联关系本身也需要具有属性和方法时，可以使用关联类。
  - 关联类通过一条虚线连接到关联关系上。
  - **示例**：学生(Student)和课程(Course)之间的关联可以有一个关联类“注册(Enrollment)”，该类包含成绩(grade)等属性。
  ```
  Student ───────── Course
      │
      ┊
  ┌───────────┐
  │Enrollment │
  ├───────────┤
  │- grade    │
  └───────────┘
  ```

- **导航性 (Navigability)**：
  - 关联关系末端的箭头表示导航性，即可以从一个类访问另一个类。
  - **单向关联**：`ClassA ───> ClassB` (A知道B，但B不知道A)
  - **双向关联**：`ClassA <───> ClassB` 或 `ClassA ────── ClassB` (A和B互相知道)
  - 无箭头通常表示双向关联或未指定导航性。

- **角色 (Roles)**：
  - 在关联关系的两端，可以为类指定扮演的角色。
  - **示例**：在公司(Company)和人(Person)的关联中，人扮演的角色是“员工(employee)”。
  ```
  Company ─────────── Person
          employee *
  ```

#### 聚合 (Aggregation)
```
整体 ◇────────> 部分
```
- **UML官方定义**：聚合是一种特殊的关联，表示整体和部分的关系
- **箭头表示**：空心菱形 + 实线
- **关系含义**：has-a关系，表示弱的"整体-部分"关系
- **特征**：
  - 部分可以独立于整体存在
  - 整体和部分的生命周期不同步
  - 部分可以被多个整体共享
  - 是一种较弱的包含关系
- **生命周期**：
  - 整体消失时，部分可以继续存在
  - 部分可以脱离整体独立存在
  - 部分可以转移到其他整体中
- **代码特征**：
  - 通常通过构造函数参数或setter方法设置
  - 部分对象通常在整体外部创建
#### 基础例子：团队和球员
一个球队(Team)由多个球员(Player)组成。球员是球队的一部分，但球员可以独立存在，比如成为自由球员或转会到其他球队。球队解散，球员依然存在。
```java
// 基础聚合示例
import java.util.ArrayList;
import java.util.List;

public class Player {
    private String name;
    public Player(String name) { this.name = name; }
    public String getName() { return name; }
}

public class Team {
    private String teamName;
    private List<Player> players = new ArrayList<>();

    // 球员对象从外部传入，而不是在Team内部创建
    public void addPlayer(Player player) {
        players.add(player);
    }
}

// 使用
public class Main {
    public static void main(String[] args) {
        Player player1 = new Player("Alice");
        Player player2 = new Player("Bob");

        Team team = new Team();
        team.addPlayer(player1);
        team.addPlayer(player2);

        // team对象被销毁后，player1和player2对象依然存在
    }
}
```

#### 进阶例子：音乐播放列表和歌曲
一个音乐播放列表(Playlist)包含多首歌曲(Song)。同一首歌曲可以被添加到多个不同的播放列表。删除一个播放列表不会删除歌曲本身。
```java
// 进阶聚合示例
import java.util.List;
import java.util.ArrayList;

public class Song {
    private String title;
    private String artist;
    public Song(String title, String artist) { /* ... */ }
}

public class Playlist {
    private String name;
    private List<Song> songs = new ArrayList<>();

    public void addSong(Song song) {
        // 只是添加引用，不负责Song的生命周期
        songs.add(song);
    }
}

public class MusicLibrary {
    private List<Song> allSongs = new ArrayList<>();
    private List<Playlist> allPlaylists = new ArrayList<>();

    public static void main(String[] args) {
        MusicLibrary library = new MusicLibrary();
        Song song1 = new Song("Bohemian Rhapsody", "Queen");
        Song song2 = new Song("Stairway to Heaven", "Led Zeppelin");
        library.allSongs.add(song1);
        library.allSongs.add(song2);

        Playlist rockClassics = new Playlist();
        rockClassics.addSong(song1);
        rockClassics.addSong(song2);

        Playlist myFavorites = new Playlist();
        myFavorites.addSong(song1); // 同一首歌song1在多个播放列表中

        library.allPlaylists.add(rockClassics);
        library.allPlaylists.add(myFavorites);

        // 如果删除rockClassics播放列表，song1和song2依然存在于library和myFavorites中
    }
}
```

- **实际示例**：
  - Department聚合Employee（部门包含员工，员工可以转部门）
  - Team聚合Player（团队包含球员，球员可以转队）
  - Library聚合Book（图书馆包含图书，图书可以转移）

#### 组合 (Composition)
```
整体 ◆────────> 部分
```
- **UML官方定义**：组合是一种强聚合形式，表示整体负责部分的生命周期
- **箭头表示**：实心菱形 + 实线
- **关系含义**：contains-a关系，表示强的"整体-部分"关系
- **特征**：
  - 部分不能独立于整体存在
  - 整体和部分的生命周期同步
  - 部分不能被多个整体共享
  - 是一种强的包含关系
- **生命周期**：
  - 整体创建时创建部分
  - 整体消失时部分也消失
  - 部分不能脱离整体独立存在
- **代码特征**：
  - 部分对象通常在整体内部创建
  - 整体负责部分的创建和销毁
#### 基础例子：汽车和引擎
一辆汽车(Car)由一个引擎(Engine)组成。引擎是汽车的核心部分，它不能独立于汽车存在。如果汽车报废，引擎也随之报废。
```java
// 基础组合示例
public class Engine {
    public void start() {
        System.out.println("Engine started.");
    }
}

public class Car {
    // Engine对象在Car的构造函数中创建，由Car负责其生命周期
    private Engine engine = new Engine();

    public void start() {
        System.out.println("Car is starting.");
        engine.start();
    }
}

// 使用
public class Main {
    public static void main(String[] args) {
        Car myCar = new Car();
        myCar.start();
        // 当myCar对象被垃圾回收时，它内部的engine对象也会被回收
    }
}
```

#### 进阶例子：订单和订单项
一个订单(Order)由多个订单项(OrderLine)组成。每个订单项代表购买的某个商品及其数量。订单项不能脱离订单存在，如果订单被取消或删除，其所有的订单项也随之失效。
```java
// 进阶组合示例
import java.util.ArrayList;
import java.util.List;

public class OrderLine {
    private String productId;
    private int quantity;
    private double price;

    public OrderLine(String productId, int quantity, double price) {
        this.productId = productId;
        this.quantity = quantity;
        this.price = price;
    }

    public double getSubtotal() {
        return quantity * price;
    }
}

public class Order {
    private String orderId;
    // OrderLine对象在Order内部管理，外部无法直接创建或删除
    private List<OrderLine> lines = new ArrayList<>();

    public Order(String orderId) {
        this.orderId = orderId;
    }

    // OrderLine的创建由Order控制
    public void addProduct(String productId, int quantity, double price) {
        lines.add(new OrderLine(productId, quantity, price));
    }

    public double getTotal() {
        return lines.stream().mapToDouble(OrderLine::getSubtotal).sum();
    }
}

// 当一个Order对象被销毁时，它包含的OrderLine列表及所有OrderLine对象都将一并被销毁
```

- **实际示例**：
  - House组合Room（房子包含房间，房子拆除房间也消失）
  - Car组合Engine（汽车包含引擎，汽车报废引擎也报废）
  - Document组合Paragraph（文档包含段落，文档删除段落也删除）

#### 关系强度对比
从弱到强的关系强度排序：
1. **依赖** < 2. **关联** < 3. **聚合** < 4. **组合** < 5. **继承**

#### 选择关系类型的判断标准
- **依赖**：临时使用，方法级别的关系
- **关联**：长期关系，属性级别的关系
- **聚合**：整体-部分关系，部分可独立存在
- **组合**：整体-部分关系，部分不可独立存在
- **继承**：is-a关系，类型层次关系

### 1.5 多重性 (Multiplicity)

| 表示 | 含义 |
|------|------|
| `1` | 恰好一个 |
| `0..1` | 零个或一个 |
| `1..*` | 一个或多个 |
| `*` | 零个或多个 |
| `n..m` | n到m个 |

## 3. 用例图 (Use Case Diagram)

用例图从用户角度描述系统的功能需求，展示了系统外部的参与者（Actor）与系统提供的用例（Use Case）之间的关系。

### 2.1 核心元素

- **参与者 (Actor)**:
  - **表示**: `웃` (人形图标)
  - **定义**: 与系统交互的外部实体，可以是人、其他系统或设备。
  - **分类**:
    - **主参与者**: 主动发起用例的参与者。
    - **次参与者**: 协助用例完成的参与者。

- **用例 (Use Case)**:
  - **表示**: `( )` (椭圆形)
  - **定义**: 系统为参与者提供的一系列功能或服务。通常用动宾短语命名，如“登录系统”、“下订单”。

- **系统边界 (System Boundary)**:
  - **表示**: `┌────────┐` (矩形框)
  - **定义**: 将系统内部与外部世界（参与者）分隔开来，用例在边界内部，参与者在外部。

### 2.2 用例关系

- **关联 (Association)**:
  - **表示**: `───` (实线)
  - **定义**: 连接参与者和用例，表示参与者与该用例进行交互。

- **包含 (Include)**:
  - **表示**: `<<include>>` + 虚线箭头
  - **定义**: 表示一个用例（基础用例）的行为包含了另一个用例（被包含用例）的行为。被包含用例是必须执行的。
  - **箭头方向**: 基础用例 → 被包含用例。
  - **示例**: “下订单”用例包含“用户身份验证”用例。

- **扩展 (Extend)**:
  - **表示**: `<<extend>>` + 虚线箭头
  - **定义**: 表示一个用例（扩展用例）在特定条件下可以扩展另一个用例（基础用例）的功能。扩展用例是可选的。
  - **箭头方向**: 扩展用例 → 基础用例。
  - **扩展点 (Extension Point)**: 在基础用例中定义，指明可以被扩展的位置。
  - **示例**: “登录”用例可以被“通过手机验证登录”用例扩展。

- **泛化 (Generalization)**:
  - **表示**: `───▷` (空心三角箭头 + 实线)
  - **定义**:
    - **参与者之间**: 表示一种更通用的参与者和一种更特殊的参与者之间的关系（如“用户”和“管理员”）。
    - **用例之间**: 表示一种更通用的用例和一种更特殊的用例之间的关系（如“支付”和“信用卡支付”）。

### 2.3 示例

#### 基础例子：ATM机
- **参与者**: 顾客 (Customer)
- **用例**:
  - 取款 (Withdraw Cash)
  - 查询余额 (Check Balance)
  - 存款 (Deposit Funds)
- **关系**: 顾客与这三个用例都有关联关系。

```
      ┌──────────────────┐
      │       ATM系统      │
      │                  │
      │    (取款)        │
      │    (查询余额)      │
      │    (存款)        │
      │                  │
      └──────────────────┘
         ^    ^    ^
         |    |    |
         └─── | ───┘
              |
             웃
           (顾客)
```

#### 进阶例子：在线购物系统
- **参与者**:
  - 顾客 (Customer)
  - 管理员 (Admin) (泛化自`系统用户`)
  - 支付网关 (Payment Gateway) (外部系统)
- **用例**:
  - **基础用例**: 登录 (Login), 下订单 (Place Order), 管理商品 (Manage Products)
  - **包含用例**: 验证用户 (Authenticate User) - 被`下订单`和`管理商品`用例包含。
  - **扩展用例**: 使用优惠券 (Apply Coupon) - 扩展`下订单`用例。
- **关系**:
  - 顾客关联到`登录`和`下订单`。
  - 管理员关联到`登录`和`管理商品`。
  - `下订单`用例包含`验证用户`用例。
  - `使用优惠券`用例扩展`下订单`用例。
  - `下订单`用例与`支付网关`参与者交互。

```
      ┌──────────────────────────────────────────┐
      │                  在线购物系统                │
      │                                          │
      │ (验证用户) <.. <<include>> .. (下订单)        │
      │      ^                           |         │
      │      | <<include>>               |         │
      │ (管理商品)                       | <<extend>>.. (使用优惠券) │
      │                                          │
      └──────────────────────────────────────────┘
         ^   ^                               ^
         |   |                               |
  웃      |   └───────────────────────────────┤      웃
(管理员) ───┘                                   └──── (顾客)
   △                                               │
   |                                               │
  웃                                                │
(系统用户)                                           │
                                                   │
                                                   V
                                                  [支付网关]
```

## 4. 时序图 (Sequence Diagram)

时序图用于描述对象间的交互过程，强调消息传递的时间顺序。

### 2.1 基本元素

#### 参与者 (Actor)
```
┌─────────┐
│ :Client │
└─────────┘
    │
    │ (生命线)
    ▼
```

#### 对象 (Object)
```
┌──────────────┐
│ obj:ClassName │
└──────────────┘
        │
        │ (生命线)
        ▼
```

#### 激活框 (Activation Box)
```
    │
    ■ ← 激活框，表示对象处于活动状态
    ■
    │
```

### 2.2 消息类型

#### 同步消息
```
对象A ────────> 对象B
      方法调用
```

#### 异步消息
```
对象A ┄┄┄┄┄> 对象B
      异步调用
```

#### 返回消息
```
对象A <┄┄┄┄┄ 对象B
      返回值
```

#### 自调用
```
对象A ┐
      │ 自调用
      └─>
```

### 2.3 创建和销毁

#### 对象创建
```
创建者 ────────> <<create>> :新对象
```

#### 对象销毁
```
对象 ────────> X
     destroy
```

### 2.4 交互片段 (Interaction Fragments)

交互片段用于在时序图中表示复杂的控制流。

- **`alt` (Alternative)**: 表示多选一的逻辑，类似于 `if-else`。
  ```
  ┌─── alt ───────────────┐
  │ [condition1]           │
  │   ... message flow ...   │
  ├────────────────────────┤
  │ [else]                 │
  │   ... message flow ...   │
  └─── end alt ────────────┘
  ```

- **`opt` (Optional)**: 表示可选的片段，类似于 `if`。
  ```
  ┌─── opt ───────────────┐
  │ [condition]            │
  │   ... message flow ...   │
  └─── end opt ────────────┘
  ```

- **`loop` (Loop)**: 表示循环执行的片段。
  ```
  ┌─── loop [min..max] ────┐
  │                        │
  │   ... message flow ...   │
  └─── end loop ───────────┘
  ```

- **`par` (Parallel)**: 表示并行执行的片段。
  ```
  ┌─── par ───────────────┐
  │                        │
  │   ... message flow ...   │
  ├────────────────────────┤
  │                        │
  │   ... message flow ...   │
  └─── end par ────────────┘
  ```

### 2.5 示例

#### 基础例子：用户登录
展示用户输入用户名和密码后，系统进行验证的简单交互过程。
```
:User   :LoginPage   :AuthService   :Database
  │        │              │             │
  │ enterCredentials()   │              │
  ├───────>│              │             │
  │        │ login(user, pass) │             │
  │        ├─────────────>│             │
  │        │              │ findUser(user) │
  │        │              ├────────────>│
  │        │              │             │ user data
  │        │              │<────────────┤
  │        │              │             │
  │        │              │ validatePassword() │
  │        │              ├─┐           │
  │        │              │ │           │
  │        │              │<┘           │
  │        │              │             │
  │        │              │ login success │
  │        │<─────────────┤             │
  │        │              │             │
  │ showSuccess() │              │             │
  │<───────┤              │             │
  │        │              │             │
```

#### 进阶例子：在线下单
展示一个更复杂的交互，包括库存检查、支付和通知，并使用`alt`和`par`交互片段。
```
:Customer   :OrderService   :InventorySvc   :PaymentGateway   :NotificationSvc
    │            │                │                │                 │
    │ placeOrder() │                │                │                 │
    ├───────────>│                │                │                 │
    │            │                │                │                 │
    ┌─── par ─────────────────────────────────────────────────────────┐
    │   │ checkStock()     │                │                 │       │
    │   ├───────────────>│                │                 │       │
    │   │                │ stock available│                 │       │
    │   │<───────────────┤                │                 │       │
    ├─────────────────────────────────────────────────────────────────┤
    │   │ processPayment() │                │                 │       │
    │   ├────────────────────────────────>│                 │       │
    │   │                │                │ payment success │       │
    │   │<────────────────────────────────┤                 │       │
    └─── end par ─────────────────────────────────────────────────────┘
    │            │                │                │                 │
    ┌─── alt ─────────────────────────────────────────────────────────┐
    │ [payment successful and stock available]                        │
    │   │ createOrder()    │                │                 │       │
    │   ├─┐              │                │                 │       │
    │   │ │              │                │                 │       │
    │   │<┘              │                │                 │       │
    │   │                │                │                 │       │
    │   │ sendConfirmation() │                 │       │
    │   ├──────────────────────────────────────────────────>│       │
    │   │                │                │                 │       │
    │   │ order success  │                │                 │       │
    │<──┤                │                │                 │       │
    ├─────────────────────────────────────────────────────────────────┤
    │ [else]                                                          │
    │   │ cancelOrder()    │                │                 │       │
    │   ├─┐              │                │                 │       │
    │   │ │              │                │                 │       │
    │   │<┘              │                │                 │       │
    │   │                │                │                 │       │
    │   │ order failed   │                │                 │       │
    │<──┤                │                │                 │       │
    └─── end alt ────────────────────────────────────────────────────┘
    │            │                │                │                 │
```

## 5. 通信图 (Communication Diagram)

在UML 2.x中，协作图被重新命名为**通信图**。它与时序图等价，可以相互转换，但它更侧重于对象之间的静态连接关系，而不是消息的时间顺序。

### 3.1 基本元素

#### 对象 (Object) 和 链接 (Link)
- **对象**: 与时序图中的对象表示法相同。
- **链接**: 连接两个对象的实线，表示它们之间可以传递消息。

```
┌───────┐         ┌───────┐
│:ClassA│─────────│:ClassB│
└───────┘         └───────┘
     (链接)
```

#### 消息 (Message)
- 消息沿着链接传递，并带有箭头指示方向。
- 消息使用**序列号**来表示顺序，而不是像时序图那样通过垂直位置表示。

```
:ClassA ── 1: doSomething() ──> :ClassB
```

### 3.2 消息编号详解

- **简单序列**: `1, 2, 3, ...` 表示顺序执行。
- **嵌套调用**: `1, 1.1, 1.2, 2, ...` 表示嵌套关系。消息 `1.1` 是在消息 `1` 的执行过程中调用的。
- **条件执行**: `1a, 1b` 表示分支，执行 `1a` 或 `1b`。
- **循环执行**: `1*` 表示消息 `1` 会被循环调用。

### 3.3 与时序图的对比

| 特性 | 时序图 | 通信图 |
|---|---|---|
| **重点** | 消息的时间顺序 | 对象间的结构关系 |
| **布局** | 垂直时间轴 | 自由布局，类似网络图 |
| **消息顺序** | 垂直位置 | 消息编号 |
| **适用场景** | 展示复杂的交互流程 | 展示简单的交互和对象关系 |

## 6. 状态机图 (State Machine Diagram)

在UML 2.x中，状态图被正式称为**状态机图**。它描述了一个对象或系统在其生命周期中响应事件所经历的状态序列。

### 4.1 基本元素详解

#### 状态 (State)
- **定义**: 对象在其生命周期中的一种状况，此时它会满足某些条件、执行某些活动或等待某些事件。
- **内部活动**:
  - `entry / action`: 进入该状态时执行的动作（一次性）。
  - `do / activity`: 处于该状态时持续进行的活动。
  - `exit / action`: 退出该状态时执行的动作（一次性）。
  - `event / action`: 在该状态下，发生指定事件时执行的动作。

#### 转换 (Transition)
- **定义**: 两个状态之间的关系，表示当特定事件发生并满足特定条件时，对象会从一个状态转移到另一个状态。
- **转换格式**: `trigger-signature [guard-condition] / activity-expression`
  - **触发器 (Trigger)**: 引起状态转换的事件。可以是信号、调用、时间事件等。
  - **守护条件 (Guard)**: 一个布尔表达式，只有当其为真时，转换才会发生。
  - **效应 (Effect/Activity)**: 转换发生时执行的动作。

#### 特殊状态
- **初始状态 (Initial State)**: `●` 状态机的起点，只能有一个。
- **终止状态 (Final State)**: `◉` 状态机的终点，可以有多个。
- **历史状态 (History State)**: `H` 或 `H*`
  - `H`: 浅历史，记住复合状态的直接子状态。
  - `H*`: 深历史，记住嵌套最深的子状态。
- **选择伪状态 (Choice Pseudostate)**: `◇` 用于基于守护条件动态选择转换路径。

### 4.2 复合状态 (Composite State)
- **定义**: 包含其他子状态的状态，用于对状态进行分层和组织。
- **优点**:
  - **简化模型**: 隐藏内部细节，使高层模型更清晰。
  - **共享转换**: 复合状态的出口转换可以被所有子状态共享。
- **示例**: "在线"状态可以包含"播放中"、"暂停"、"缓冲中"等子状态。

### 4.3 并发区域 (Concurrent Regions)
- 复合状态可以被虚线分割成多个并发区域，每个区域都有自己的子状态机，它们同时处于活动状态。
- **示例**: 电视机可以同时处于"显示画面"和"播放声音"两个并发状态。

### 4.4 示例

#### 基础例子：电灯开关
一个简单的状态机，只有两个状态：开和关。
```
      turnOn
   ● ────────> ┌──────┐
               │  On  │
   ┌──────┐ <──────── ┘
   │ Off  │    turnOff
   └──────┘
```

#### 进阶例子：文章发布流程
描述一篇文章从草稿到发布、归档的完整生命周期，包含复合状态和条件转换。
```
                                     publish [approved]
● ──> ┌────────┐ ── submit ──> ┌──────────────────┐ ───────────> ┌─────────┐ ── archive ──> ◉
      │ Draft  │              │     In Review      │               │Published│
      └────────┘ <─ reject ─── ├──────────────────┤ <─ edit ───── └─────────┘
               <─ send back ── │ entry / notify() │
                               │ do / checkGrammar  │
                               │ exit / logResult   │
                               └──────────────────┘
```
- **状态**: Draft, In Review, Published.
- **特殊状态**: 初始状态(●), 终止状态(◉).
- **事件**: submit, reject, send back, publish, edit, archive.
- **守护条件**: `[approved]` - 只有文章被批准时才能发布。
- **内部活动**: `In Review`状态有进入、执行和退出动作。

## 7. 活动图 (Activity Diagram)

活动图是一种特殊的**状态机图**，其中所有或大多数状态都是活动状态，所有或大多数转换都在源状态中的活动完成后自动触发。它非常适合用于描述业务流程或计算工作流。

### 5.1 基本元素详解

#### 动作 (Action) 和 活动 (Activity)
- **动作**: 原子性的计算，不可中断。
- **活动**: 可以分解为其他子活动或动作的非原子性计算。

#### 控制流节点 (Control Nodes)
- **初始节点 (Initial Node)**: `●` 流程的开始。
- **最终节点 (Final Node)**:
  - **活动最终节点 (Activity Final Node)**: `◉` 终止整个活动。
  - **流最终节点 (Flow Final Node)**: `⊗` 终止一个分支流，不影响其他并行流。
- **决策节点 (Decision Node)**: `◇` 根据守护条件选择一个出口流。
- **合并节点 (Merge Node)**: `◇` 将多个可选的入口流合并为一个出口流。
- **分叉节点 (Fork Node)**: `■` 将一个入口流拆分为多个并行流。
- **汇合节点 (Join Node)**: `■` 将多个并行入口流同步为一个出口流。

### 5.2 对象流 (Object Flow)

- **对象节点 (Object Node)**: `┌──────┐` 表示活动中创建、使用或传递的对象。
- **引脚 (Pin)**: `□` 表示动作的输入或输出参数。
  - **输入引脚**: `Action □──>`
  - **输出引脚**: `Action ──>□`
- **数据存储节点 (Datastore Node)**: `<<datastore>>` 表示持久化的信息。

### 5.3 泳道 (Swimlane) / 分区 (Partition)
- 用于对活动图中的动作进行分组，表示负责执行这些动作的对象或角色。
- 可以是垂直的，也可以是水平的。

### 5.4 异常处理 (Exception Handling)
- 活动图可以模拟异常处理流程。
- **异常处理器 (Exception Handler)**: `⚡` 从受保护的活动区域指向异常处理活动。

### 5.5 示例

#### 基础例子：用户登录流程
一个简单的线性活动流程，带有决策点。
```
● ──> [输入用户名密码] ──> ◇ ── [验证成功] ──> [进入系统] ──> ◉
                            │
                            └─ [验证失败] ──> [显示错误信息] ──> ●
```
- **初始节点**: ●
- **活动**: 输入用户名密码, 进入系统, 显示错误信息
- **决策节点**: ◇ (菱形)
- **最终节点**: ◉

#### 进阶例子：处理订单（带泳道和并行活动）
一个更复杂的业务流程，使用泳道来区分不同角色的职责，并使用分叉/汇合来表示并行处理。
```
┌───────────┬────────────────────────────────┬──────────────┐
│  Customer │          Order System          │   Warehouse  │
├───────────┼────────────────────────────────┼──────────────┤
│     ●     │                                │              │
│     │     │                                │              │
│     V     │                                │              │
│ [下订单]  │                                │              │
│     │     │                                │              │
│     V     │                                │              │
│    ───    │                                │              │
│     │─────> [接收订单]                       │              │
│           │     │                          │              │
│           │     V                          │              │
│           │    ─── (分叉)                    │              │
│           │   /   \                        │              │
│           │  /     \                       │              │
│           │ V       V                      │              │
│      [处理支付]  [发送订单到仓库] ───────────> [准备发货]   │
│           │       │                        │     │        │
│           │       │                        │     V        │
│           │       │                        │  [打包商品]  │
│           │       │                        │     │        │
│           │       │                        │     V        │
│           │       └──────────────────────────< [通知已发货] │
│           │       │                              │        │
│           │       V                              │        │
│           │    ─── (汇合)                          │        │
│           │     │                              │        │
│           │     V                              │        │
│      [关闭订单] ─────────────────────────────────┘        │
│           │                                │              │
│           V                                │              │
│           ◉                                │              │
└───────────┴────────────────────────────────┴──────────────┘
```
- **泳道**: Customer, Order System, Warehouse.
- **并行处理**: `处理支付`和`发送订单到仓库`是并行执行的。

## 8. 其他UML图详解

### 8.1 组件图 (Component Diagram)
- **用途**: 描述软件系统的物理组件（如JAR文件、DLL、可执行文件）以及它们之间的依赖关系。它关注系统的静态实现视图。
- **核心元素**:
  - **组件 (Component)**: `┌─┐` 表示系统中的一个模块化、可替换的部分。
  - **提供接口 (Provided Interface)**: `○` (棒棒糖表示法)，表示组件为其他组件提供的服务。
  - **需要接口 (Required Interface)**: `(─` (插座表示法)，表示组件需要其他组件提供的服务。
  - **端口 (Port)**: `□` 组件边框上的小方块，表示组件与外部环境的交互点。
  - **依赖关系 (Dependency)**: 虚线箭头，表示一个组件依赖于另一个组件。
- **示例**:
  ```
  ┌─┐             ┌─┐
  │ WebServer │ ○───────┤ OrderService │
  └─┘             └─┘
                    (─
                    │
                  ┌─┐
                  │ Database │
                  └─┘
  ```

### 8.2 部署图 (Deployment Diagram)
- **用途**: 描述系统硬件（节点）的物理拓扑结构以及软件构件（制品）在这些硬件上的部署情况。它关注系统的物理运行时配置。
- **核心元素**:
  - **节点 (Node)**: `┌───┐` 立方体表示，代表一个计算资源（如服务器、设备）。节点可以嵌套。
  - **制品 (Artifact)**: `<<artifact>>` 表示一个物理文件（如.jar, .exe, script）。
  - **部署关系 (Deployment)**: 虚线箭头，从制品指向节点，表示制品部署在节点上。
  - **通信路径 (Communication Path)**: 节点之间的实线，表示网络连接，可以标注通信协议。
- **示例**:
  ```
  ┌─────────────┐        ┌───────────────┐
  │ <<server>>  │        │ <<database>>  │
  │ WebServer   ├────────┤ DatabaseServer│
  │ ┌─────────┐ │        │ ┌───────────┐ │
  │ │app.war  │ │        │ │order_db   │ │
  │ └─────────┘ │        │ └───────────┘ │
  └─────────────┘        └───────────────┘
  ```

### 8.3 包图 (Package Diagram)
- **用途**: 将模型元素（如类、用例）组织成包，并描述包之间的依赖关系，用于管理大型系统的复杂性。
- **核心元素**:
  - **包 (Package)**: `┌────────┐` 带标签的文件夹图标。
  - **包依赖 (Package Dependency)**: 虚线箭头，表示一个包中的元素依赖于另一个包中的元素。
    - `<<import>>`: 表示一个包导入另一个包的公共成员，建立访问命名空间。
    - `<<access>>`: 表示一个包访问另一个包的私有成员（不推荐）。
    - `<<merge>>`: 表示一个包的内容合并到另一个包中。
- **示例**:
  ```
  ┌──────────┐   <<import>>   ┌──────────┐
  │  UI Layer  │┄┄┄┄┄┄┄┄┄┄┄>│ Core Logic │
  └──────────┘                └──────────┘
                                   ^
                                   ┊ <<import>>
                                   ┊
                               ┌──────────┐
                               │ Data Layer │
                               └──────────┘
  ```

### 8.4 对象图 (Object Diagram)
- **用途**: 类图的实例，显示系统在某个特定时间点的对象快照及其关系。常用于验证类图设计的正确性和展示复杂数据结构。
- **核心元素**:
  - **对象 (Object)**: `objectName:ClassName`，名称有下划线，可以省略对象名（匿名对象）或类名。
  - **链接 (Link)**: 对象之间的实线，是关联关系的实例。
  - **属性值**: 可以显示对象的具体属性值。
- **示例**:
  ```
  ┌──────────────────┐       ┌──────────────────┐
  │<u>order123:Order</u>   │       │<u>cust456:Customer</u> │
  ├──────────────────┤       ├──────────────────┤
  │- date = "..."   ├───────┤- name = "John"  │
  └──────────────────┘       └──────────────────┘
  ```

### 8.5 交互概览图 (Interaction Overview Diagram)
- **用途**: 活动图和时序图的结合，用活动图的框架来组织多个时序图（交互片段），提供对复杂交互的高层视角。
- **核心元素**:
  - **活动图节点**: 决策、合并、分叉、汇合等。
  - **交互片段 (Interaction Use)**: `ref`框，引用一个具体的时序图。

### 8.6 定时图 (Timing Diagram)
- **用途**: 强调消息交互中精确的时间约束，展示对象状态随时间的变化。
- **核心元素**:
  - **生命线**: 水平或垂直的线，表示参与者。
  - **状态时间轴**: 在生命线上显示不同状态的持续时间。
  - **时间约束**: `{t1..t2}` 表示时间范围。
  - **事件**: 引起状态变化的事件。

## 9. UML在设计模式中的应用

### 7.1 模式结构图

每个设计模式都有标准的UML类图表示：

#### 单例模式示例
```
┌─────────────────┐
│   Singleton     │
├─────────────────┤
│ - instance      │
├─────────────────┤
│ - Singleton()   │
│ + getInstance() │
└─────────────────┘
```

#### 观察者模式示例
```
┌─────────────┐         ┌─────────────┐
│   Subject   │◇────────│  Observer   │
├─────────────┤         ├─────────────┤
│ + attach()  │         │ + update()  │
│ + detach()  │         └─────────────┘
│ + notify()  │                △
└─────────────┘                │
       △                       │
       │                       │
┌─────────────┐         ┌─────────────┐
│ConcreteSubj │         │ConcreteObs  │
├─────────────┤         ├─────────────┤
│ + getState()│         │ + update()  │
│ + setState()│         └─────────────┘
└─────────────┘
```

### 7.2 行为描述图

使用时序图描述模式的动态行为：

#### 观察者模式时序图
```
Client  ConcreteSubject  ConcreteObserver
  │           │               │
  │ setState()│               │
  ├──────────>│               │
  │           │ notify()      │
  │           ├──────────────>│
  │           │               │ update()
  │           │<──────────────┤
  │           │ getState()    │
  │           ├──────────────>│
  │           │               │
```

## 10. UML工具推荐

### 在线工具
- **PlantUML**：文本驱动的UML工具
- **Draw.io**：免费的在线绘图工具
- **Lucidchart**：专业的图表工具

### 桌面工具
- **Enterprise Architect**：专业UML建模工具
- **Visual Paradigm**：功能全面的建模工具
- **StarUML**：开源UML工具

### IDE集成
- **IntelliJ IDEA**：内置UML图生成
- **Eclipse**：UML插件支持
- **Visual Studio**：类图生成功能

## 11. 总结

UML是理解和表达设计模式的重要工具。掌握UML的基本图形和符号，能够帮助我们：

1. **更好地理解设计模式的结构**
2. **清晰地表达设计思想**
3. **有效地进行团队沟通**
4. **准确地记录系统设计**

在学习设计模式时，建议：
- 重点掌握类图和时序图
- 理解各种关系的含义和使用场景
- 练习绘制常见模式的UML图
- 学会从UML图理解代码结构

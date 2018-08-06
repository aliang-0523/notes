# *java学习笔记*

## **强制类型转换**

- 只能在继承层次内进行类型转换

- 在将超类转换成子类之前应该使用**instanceof**进行检查

<code>

    if(staff[1] instanceof Manager)
    {
        boss=(Manager) staff[1];
            ... 
    }
</code>
- 不能将超类的引用赋给子类变量
  
## **抽象类**

- 当一个方法被声明为**抽象方法**时，这个类必须被声明为**抽象类**
- 当一个方法无法为子类提供任何内容时，可以将父类的方法定义为抽象方法，这样就不需要实现这个方法。
- 抽象类**可以包含**具体数据和具体方法。
- 如果在子类中没有实现抽象父类的**全部方法**，则该子类也应该定义为抽象的，如果实现了父类的所有抽象方法，则可以不定义为抽象类。
- 抽象类**不能被实例化**
- 可以定义一个**抽象类的数据对象**，但是它**只能引用非抽象子类的对象**
<code> Person person=new Student("Sword sun","1");</code>
- 在C++中有一种在**尾部用=0标记**的抽象方法，称为纯虚函数，例如下：

> class Person
> {
> 
> public:
> 
> virtual public void getDescription()=0;
> 
> }

- 可以**使用父类去引用子类对象**

### **泛型数组列表**

- 声明方式

> ArrayList<class> arrayList=new ArrayList<>();

### **对象包装器与自动装箱**

- 当想要定义一个基本数据类型列表时，**ArrayList尖括号内的类型参数不允许是基本类型**，就需要用到各种基本类型包装好的对象，java中包括了**Integer、Long、Float、Double、Short、Byte、Character、Void、Boolean**。**对象包装器类还是final的**，因此不能定义他们的子类。
- **ArrayList<Integer>的效率远远低于int[]数组**。所以应该**用它构造小型集合**，原因是此时程序员操作的方便性要比执行效率更加重要。
- **Integer对象是不可变的:包含在包装器中的内容不会改变**。不能使用这些包装器类创建修改数值参数的方法。

> public static void triple(Integer x)
> 
> {
> 
> }
- 如果想编写一个修改数值参数值的方法,就**需要使用在org.omg.CORBA包中定义的持有者(Holder)类型**，包括**intHolder、BooleanHolder**.
> public static void triple(IntHolder x)
> 
> {
> 
> x.value=3*x.value;
> 
> }

## **枚举类**

- **所有的枚举类型都是Enum类的子类**。他们继承了这个类的许多方法，其中最有用的是**toString**,这个方法能够返回枚举常量名.Size.Small.toString()将返回字符串"Small";
- **toString的逆方法是静态的values方法**，他将返回一个包含全部枚举值的数组。

> Size[] values=Size.value();
- **ordinal方法返回enum声明中枚举常量的位置**,位置从0开始计数，Size.Medium.ordinal()返回1.

## **java.lang.Class**
- Field[] getFields()
- Field[] getDeclaredFields()

*getFields方法将返回一个**包含Field对象的数组**，这些对象记录了这个类或其超类的**共有域***。

*getDeclaredFields方法也将返回包含Field对象的数组，这些对象记录了这个类的**全部域**.如果类中没有域，或者Class对象描述的是基本数据类型或数组类型，这些方法将返回一个长度为0的数组。*

- Method[] getMethods()
- Method[] getDeclaredMethods()

*返回包含Method对象的数组：getMethods将返回所有的**公有方法**，包括从超类继承来的公有方法；*

*getDeclaredMethods返回**这个类或接口的全部方法**，但不包括由超累继承了的方法*

- Constructor[] getConstructors()
- Constructor[] getDeclaredConstructors()

*返回包含Constructor对象的数组，其中包含了Class对象所描述的类的**所有公有构造器(getConstructors）**或**所有构造器(getDeclaredConstructors）***

- 在使用get方法时应该注意在访问私有域的时候,**除非拥有访问权限**，否则java安全机制**只允许查看任意对象有哪些域,而不允许读取他们的值**。
- 反射机制的默认行为受限于java的访问控制，然而，一个Java程序没有受到安全管理器的控制，就可以覆盖访问控制.为了达到这个目的，**需要调用Field、Method、或Constructor对象的setAccessible方法**。
- setAccessible方法是AccessibleObject类中的一个方法，**他是Field、Method、和Constructor类的公公超类**。


>public static Object goodCopyof(Object a,int newLength) 
> {
> 
> 		Class class1=a.getClass();
>		if(!class1.isArray())return null;
>		Class componentType=class1.getComponentType();
>		int length=Array.getLength(a);
>		Object newArray=Array.newInstanc(componentType, newLength);
>		System.arraycopy(a, 0, newArray, 0, Math.min(length, newLength));
>		return newArray;
>	}
- **这个方法可以用来扩展任意类型的数组,上面的方法不应该被声明为Object[]类型。**

### **继承设计的技巧**
- **将公共操作和域放在超类**
- **不要使用受保护的域**
- **使用继承实现"is-a"关系**
- **除非所有集成的方法都有意义，否则不要使用继承.**
- **在覆盖方法时,不要改变预期的行为**
- **使用多态而非类型信息**
- **不要过多的使用反射**

## **接口与内部类**

### **接口**

- 当调用一个方法时，比如**Array类的sort方法**，在对对象进行排序的过程中，需要该对象**实现Compareable接口中的compareTo方法**,可以通过**泛型**的方式进行类的定义。在做Employee x与Manager y比较时，应该考虑x与y的类型是否一致，即考虑对称性的问题，

> public class Employee implements Comparable《Employee》

- 接口不是类，尤其不能使用new运算符实例化一个接口,然而，尽管不能构造接口的对象,却能声明接口的变量.**接口变量必须引用实现了接口的类对象**。

> Comparable x;
> 
> x=new Employee(...);

- **虽然在接口中不能包含实例域或静态方法，但却可以包含常量**。
- 与接口中的方法都自动地被设置为public一样，**接口中的域将被自动设为public static final**;

### **对象克隆**
- 当拷贝一个变量时,**原始变量与引用变量引用同一个对象**，也就是说，**改变一个变量所引用的对象将会对另一个变量产生影响。**

> Employee ordinal=new Employee("John Public",50000);
> 
> Employee copy=original;
> 
> copy.raiseSalary(10);
- 如果创建一个对象的新的copy,它的最初状态与original一样，但以后将可以各自改变各自的状态,那就需要**使用clone方法**。

> Employee copy=original.clone();
> 
> copy.raiseSalary(10);
#### **浅拷贝**
- 在使用clone方法的过程中，由于clone方法是Object类的一个protected方法，所以**只有Object的子类才能调用clone方法**，由于这个类对具体的类对象一无所知，所以**只能将各个域进行对应的拷贝**。如果对象中的所有数据域都属于数值或基本类型，这样拷贝没有任何问题。但是，如果在对象中**包含了子对象的引用**，**拷贝的结果会使得两个域引用同一个对象**，因此原始对象与克隆对象**共享**这部分信息。
- 如果子对象是不可变的，将不会产生任何问题，不过大多数情况是**子对象可变的，因此必须重新定义clone方法**，以便实现克隆子对象的深拷贝。
- 对于每一个类，都需要做出下列判断：
1. **默认的clone方法是否满足要求**。
2. **默认的clone方法是否能够通过调用可变子对象的clone方法进行弥补**。
3. **是否不应该使用clone**。
- 实际上（3）是默认的，如果要选择（1）或（2），类必须：
1. **实现Cloneable接口**。
2. **使用public访问修饰符重新定义clone方法**。

- Cloneable接口是Java提供的几个**标记接口之一**，标记接口没有方法，使用它的唯一的目的就是**可以用instanceof进行类型检查**。
> if(obj instanceof Cloneable)
- 即使clone的默认实现（浅拷贝）能够满足需求，也应该**实现Cloneable接口**，**将clone重定义为public,并调用super.clone().**
> class Employee implements Cloneable
> 
> {
> public Employee clone() throws CloneNotSupportException
> 
> {
> 
> return (Employee) super.clone();
> 
> }
> 
> }

- 为了实现深拷贝，必须**将Employee中的所有可变域都进行拷贝**。

> class Employee implements Cloneable
> {
> 
> public Employee clone() throws CloneNotSupportException
> 
> {
> 
> Employee cloned=(Employee)super.clone();
> 
> cloned.hireday=(Date)hireday.clone();
> 
> }
> 
> }

### **接口与回调**

> import java.awt.Toolkit;
> 
> import java.awt.event.*;
> 
> import java.awt.event.ActionEvent;
> 
> import java.awt.event.ActionListener;
> 
> import java.util.*;
> 
> import javax.swing.*;
> 
> import javax.swing.Timer;
> 
> class TimePrinter implements ActionListener{
> 
> 	public void actionPerformed(ActionEvent event) {
> 
> 		Date now=new Date();
> 
> 		System.out.println("the time is"+now);
> 
> 		Toolkit.getDefaultToolkit().beep();
> 
> 	}
> 
> }
> 
> public class Main{
> 
> public static void main(String[] args) {
> 
> 		System.out.println("1");
> 
> 		ActionListener listener=new TimePrinter();
> 
> 		Timer timer=new Timer(1000,listener);
> 
> 		timer.start();
> 
> 		JOptionPane.showMessageDialog(null, "quit?");
> 
> 		System.exit(0);
> 
> 	}
> 
> }

### **内部类**
- 使用内部类的原因有以下三点：
1. **内部类方法可以访问该类定义所在的作用域中的数据，包括私有的数据**。
2. 内部类可以对同一个包中的其他类**隐藏起来**。
3. 当想要**定义一个回调函数且不想编写大量代码时，使用匿名内部类比较便捷**。

- **内部类的对象有一个隐式引用，它引用了实例化该内部对象的外围类对象。通过这个指针，可以访问外围类对象的全部状态**。
- **内部类既可以访问自身的数据域，也可以访问创建它的外围类对象的数据域**。

> public class TalkingClock{
> 
> private int interval;
> 
> 	private boolean beep;
> 
> 	public TalkingClock(int interval,boolean beep) {}
> 
> 	public void start() {}
> 
> 	public class TimePrinter implements ActionListener{
> 
> 		@Override
> 
> 		public void actionPerformed(ActionEvent e) {
> 
> 			// TODO 自动生成的方法存根
> 
> 			Date now=new Date();
> 
> 			System.out.println("the time is"+now);
> 
> 			if(beep)Toolkit.getDefaultToolkit().beep();
> 
> 		}
> 
> 	}
> 
> }

- 为了便于理解，我们将**外围类对象的引用称为outer**。
- 于是actionPerformed()方法中的**beep可以改用outer.beep书写**。
- **外围类的引用在构造器中设置。编译器修改了所有的内部类的构造器，添加一个外围类引用的参数。因为TimePrinter类没有定义构造器，所以编译器为这个类生成了一个默认的构造器**
> public TimePrinter(TalkingClock clock) {
> 
> 		outer=clock;
> 
> 	}

- **当在start方法中创建了TimePrinter对象后，编译器就会将this引用传递给当前的语音时钟的构造器**：

> ActionListener listener=new TimePrinter(this);

- 由于**内部类可以访问外围类的私有数据**，但是如果是分开写的类则不行，可见**由于内部类具有访问特权**，所以与常规类比较起来功能更加强大.
#### **局部内部类**
- TimePrinter这个类名字只在start方法中**创建这个类型的对象时使用了一次**。当遇到这类情况时，可以**在一个方法中定义局部类**。

> public void start() {
> 
> 		class TimePrinter implements ActionListener{
> 
> 			@Override
> 
> 			public void actionPerformed(ActionEvent e) 
> 
> {
> 				// TODO 自动生成的方法存根
> 
> 				Date now=new Date();
> 
> 				System.out.println("the time is"+now);
> 
> 				if(beep)
> 
>                     Toolkit.getDefaultToolkit().beep();
> 
> 			}
> 
> 		}
> 
> 		ActionListener actionListener=new TimePrinter();
> 
> 		Timer timer=new Timer(interval,actionListener);
> 
> 		timer.start();
> 
> 	}

- **局部类的方法只可以引用定义为final的变量**。
- **匿名内部类**的书写方法：

> public void start(int interval,final boolean beep)
> 
> {
> 
> ActionListener listener=new ActionListener()
> 
> {
> public void actionPerformed(ActionEvent event)
> 
> {
> Date now=new Date();
> 
> System.out.println("At the tone,the time is"+now);
> if(beep)Toolkit.getDefaultToolkit.beep();
> }
> 
> };
> 
> Timer t=new Timer(interval,listener); 
> 
> t.start();
> 
> }

#### **匿名内部类**
- 匿名内部类的通常格式为：
> new SuerType(construction parameter)
> 
> {
> 
> inner class methods and data
> 
> }

- 其中，**SuperType可以是ActionListener这样的接口**，于是内部类就**要实现这个接口**。SuperType也可以是一个类，于是**内部类就要扩展它**。
- 由于**构造器的名字必须与类名相同**，而**匿名类没有类名**，所以，**匿名类不能有构造器**。取而代之的是，**将构造器参数传递给超类（superclass）构造器**。尤其是**在内部类实现接口的时候，不能有任何构造参数**。

#### **静态内部类**

- 有时候，**使用内部类只是为了把一个类隐藏在另外一个类的内部**，并**不需要内部类引用外围类对象**。为此，可以**将内部类声明为static，以便取消产生的引用**。
- 在**内部类不需要访问外围类对象的时候**，应该使用静态内部类。
- 声明在接口中的内部类自动成为static和public类。
## **二叉树**
### 二叉树的递归方式实现：
> public static TreeNode makeBinaryTreeByArray(int[] 
> 
>  array,int index){  
> 
>       if(index<array.length){  
> 
>           int value=array[index];  
> 
>           if(value!=0){  
> 
>               TreeNode t=new TreeNode(value);  
> 
>               array[index]=0;  
>               t.left=makeBinaryTreeByArray(array,index*2);  
> 
>               t.right=makeBinaryTreeByArray(array,index*2+1);  
> 
>               return t;  
> 
>             }  
> 
>         }  
> 
>       return null;  
> 
>}  
- 主要的思想是**由于二叉树每一层的最多能容纳的子节点数为2^n-1,n为层数**，所以考虑左子树都位于数组的偶数位上（根节点如果在1号位上的话），那么就可以递归的将树的左节点设为数组的偶数位。
- 运用栈模拟二叉树的先序遍历步骤：**首先将根入栈**，当栈不为空时（采用while控制），**先弹出栈顶元素，若左右子节点都不为空，则按照先右节点入栈再左节点入栈的方式进行遍历**。
- 运用**队列**模拟二叉树的广度优先搜索算法，同样将根节点先加入到队列中，**检查队列是否为空（while检查）**，若不为空，则将队列首的先弹出，由于队列采用先进先出的方式，所以在左右子节点添加的过程中，**先添加左节点，再添加右节点**，以此进行循环。
- 二叉排序树（Binary Sort Treee）又称二叉查找树。它或者是一颗空树；或者是具有下列性质的二叉树：（1）**若左子树不空，则左子树上所有结点的值均小于它的根节点的值**；（2）**若右子树不空，则右子树上所有结点的值均大于它的根节点的值**;（3）**左、右子树也分别为二叉排序树**.

## 
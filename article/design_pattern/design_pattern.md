[设计模式（原文请点此链接）](https://www.zhihu.com/question/308850392)

设计模式在面试中会被考到，通常是介绍其原理并说出优缺点。或者对比几个比较相似的模式的异同点。在笔试中可能会出现画出某个设计模式的 UML 图这样的题。虽说面试中占的比重不大，但并不代表它不重要。
设计模式于程序员而言相当重要，能保障我们写出优秀的代码。设计模式与程序员的架构能力与阅读源码的能力息息相关，非常值得我们深入学习。
让面向对象保持结构良好的秘诀就是 设计模式。

**设计模式的六大原则**

面向对象结合设计模式，才能真正体会到程序变得可维护、可复用、可扩展、灵活性好。设计模式对于程序员而言并不陌生，每个程序员在编程时都会或多或少地接触到设计模式。无论是在大型程序的架构中，亦或是在源码的学习中，设计模式都扮演着非常重要的角色。

设计模式基于六大原则：

* 开闭原则：一个软件实体如类、模块和函数应该对修改封闭，对扩展开放。
* 单一职责原则：一个类只做一件事，一个类应该只有一个引起它修改的原因。
* 里氏替换原则：子类应该可以完全替换父类。也就是说在使用继承时，只扩展新功能，而不要破坏父类原有的功能。
* 依赖倒置原则：细节应该依赖于抽象，抽象不应依赖于细节。把抽象层放在程序设计的高层，并保持稳定，程序的细节变化由低层的实现层来完成。
* 迪米特法则：又名「最少知道原则」，一个类不应知道自己操作的类的细节，换言之，只和朋友谈话，不和朋友的朋友谈话。
* 接口隔离原则：客户端不应依赖它不需要的接口。如果一个接口在实现时，部分方法由于冗余被客户端空实现，则应该将接口拆分，让实现类只需依赖自己需要的接口方法。

所有的设计模式都是为了程序能更好的满足这六大原则。设计模式一共有 23 种，下面先介绍构建型模式，一共五种，分别是：
* 工厂方法模式
* 抽象工厂模式
* 单例模式
* 建造型模式
* 原型模式

**工厂模式** 

在平时编程中，构建对象最常用的方式是 new 一个对象。乍一看这种做法没什么不好，而实际上这也属于一种硬编码。每 new 一个对象，相当于调用者多知道了一个类，增加了类与类之间的联系，不利于程序的松耦合。其实构建过程可以被封装起来，工厂模式便是用于封装对象的设计模式。

* 简单工厂模式

  举个例子，直接 new 对象的方式相当于当我们需要一个苹果时，我们需要知道苹果的构造方法，需要一个梨子时，需要知道梨子的构造方法。更好的实现方式是有一个水果工厂，我们告诉工厂需要什么种类的水果，水果工厂将我们需要的水果制造出来给我们就可以了。这样我们就无需知道苹果、梨子是怎么种出来的，只用和水果工厂打交道即可。

  水果工厂：

  ```java
  public class FruitFactory { 
      public Fruit create(String type) {
          switch (type) {
            case "苹果": return new Apple();
            case "梨子": return new Pear();
            default: throw new IllegalArgumentException("暂时没有这种水果");
          }
      }
  }
  ```

  调用者：

  ```java
  public class User {
      private void eat(){
          FruitFactory fruitFactory = new FruitFactory();
          Fruit apple = fruitFactory.create("苹果");
          Fruit pear = fruitFactory.create("梨子");
          apple.eat();
          pear.eat();
      }
  }
  ```

  事实上，将构建过程封装的好处不仅可以降低耦合，如果某个产品构造方法相当复杂，使用工厂模式可以大大减少代码重复。比如，如果生产一个苹果需要苹果种子、阳光、水分，将工厂修改如下：

  ```java
  public class FruitFactory { 
      public Fruit create(String type) {
          switch (type) {
              case "苹果":
                  AppleSeed appleSeed = new AppleSeed();
                  Sunlight sunlight = new Sunlight();
                  Water water = new Water();
                  return new Apple(appleSeed, sunlight, water);
              case "梨子":
                  return new Pear();
              default : 
                  throw new IllegalArgumentException("暂时没有这种水果");
          }
      }
  }
  ```
  
  调用者的代码则完全不需要变化，而且调用者不需要在每次需要苹果时，自己去构建苹果种子、阳光、水分以获得苹果。苹果的生产过程再复杂，也只是工厂的事。这就是封装的好处，假如某天科学家发明了让苹果更香甜的肥料，要加入苹果的生产过程中的话，也只需要在工厂中修改，调用者完全不用关心。
  
  不知不觉中，我们就写出了简单工厂模式的代码。工厂模式一共有三种：

  1. 简单工厂模式
  2. 工厂方法模式
  3. 抽象工厂模式

  总而言之，简单工厂模式就是让一个工厂类承担构建所有对象的职责。调用者需要什么产品，让工厂生产出来即可。它的弊端也显而易见：

  * 一是如果需要生产的产品过多，此模式会导致工厂类过于庞大，承担过多的职责，变成超级类。当苹果生产过程需要修改时，要来修改此工厂。梨子生产过程需要修改时，也要来修改此工厂。也就是说这个类不止一个引起修改的原因。违背了单一职责原则。
  
  * 二是当要生产新的产品时，必须在工厂类中添加新的分支。而开闭原则告诉我们：类应该对修改封闭。我们希望在添加新功能时，只需增加新的类，而不是修改既有的类，所以这就违背了开闭原则。

* 工厂方法模式

  为了解决简单工厂模式的这两个弊端，工厂方法模式应运而生，它规定每个产品都有一个专属工厂。比如苹果有专属的苹果工厂，梨子有专属的梨子工厂，Java 代码如下：

  苹果工厂：
  
  ```java
  public class AppleFactory {
      public Fruit create() {
          return new Apple();
      }
  }
  ```

  梨子工厂：

  ```java
  public class PearFactory {
      public Fruit create(){
          return new Pear();
      }
  }
  ```
  
  调用者：

  ```java
  public class User {
      private void eat() {
          AppleFactory appleFactory = new AppleFactory();
          Fruit apple = appleFactory.create();
          PearFactory pearFactory = new PearFactory();
          Fruit pear = pearFactory.create();
          apple.eat();
          pear.eat();
      }
  }
  ```

  有读者可能会开喷了，这样和直接 new 出苹果和梨子有什么区别？上文说工厂是为了减少类与类之间的耦合，让调用者尽可能少的和其他类打交道。用简单工厂模式，我们只需要知道 FruitFactory，无需知道 Apple 、Pear 类，很容易看出耦合度降低了。但用工厂方法模式，调用者虽然不需要和 Apple 、Pear 类打交道了，但却需要和 AppleFactory、PearFactory 类打交道。有几种水果就需要知道几个工厂类，耦合度完全没有下降啊，甚至还增加了代码量！

  这位读者请先放下手中的大刀，仔细想一想，工厂模式的第二个优点在工厂方法模式中还是存在的。当构建过程相当复杂时，工厂将构建过程封装起来，调用者可以很方便的直接使用，同样以苹果生产为例：

  ```java
  public class AppleFactory {
      public Fruit create(){
          AppleSeed appleSeed = new AppleSeed();
          Sunlight sunlight = new Sunlight();
          Water water = new Water();
          return new Apple(appleSeed, sunlight, water);
      }
  }
  ```

  调用者无需知道苹果的生产细节，当生产过程需要修改时也无需更改调用端。同时，工厂方法模式解决了简单工厂模式的两个弊端。

  当生产的产品种类越来越多时，工厂类不会变成超级类。工厂类会越来越多，保持灵活。不会越来越大、变得臃肿。如果苹果的生产过程需要修改时，只需修改苹果工厂。梨子的生产过程需要修改时，只需修改梨子工厂。符合单一职责原则。
  当需要生产新的产品时，无需更改既有的工厂，只需要添加新的工厂即可。保持了面向对象的可扩展性，符合开闭原则。

* 抽象工厂模式

  工厂方法模式可以进一步优化，提取出工厂接口：

  ```java
  public interface IFactory {
      Fruit create();
  }
  ```

  然后苹果工厂和梨子工厂都实现此接口：
  
  ```java
  public class AppleFactory implements IFactory {
      @Override
      public Fruit create() {
          return new Apple();
      }
  }
  ```
  
  ```java
  public class PearFactory implements IFactory {
      @Override
      public Fruit create() {
          return new Pear();
      }
  }
  ```

  此时，调用者可以将 AppleFactory 和 PearFactory 统一作为 IFactory 对象使用，调用者 Java 代码如下：

  ```java
  public class User {
      private void eat(){
          IFactory appleFactory = new AppleFactory();
          Fruit apple = appleFactory.create();
          IFactory pearFactory = new PearFactory();
          Fruit pear = pearFactory.create();
          apple.eat();
          pear.eat();
      }
  }
  ```

  可以看到，我们在创建时指定了具体的工厂类后，在使用时就无需再关心是哪个工厂类，只需要将此工厂当作抽象的 IFactory 接口使用即可。这种经过抽象的工厂方法模式被称作抽象工厂模式。
  由于客户端只和 IFactory 打交道了，调用的是接口中的方法，使用时根本不需要知道是在哪个具体工厂中实现的这些方法，这就使得替换工厂变得非常容易。
  例如：
  
  ```java
  public class User {
      private void eat() {
          IFactory factory = new AppleFactory();
          Fruit fruit = factory.create();
          fruit.eat();
      }
  }
  ```

  如果需要替换为吃梨子，只需要更改一行代码即可：

  ```java
  public class User {
      private void eat() {
          IFactory factory = new PearFactory();
          Fruit fruit = factory.create();
          fruit.eat();
      }
  }
  ```

  IFactory 中只有一个抽象方法时，或许还看不出抽象工厂模式的威力。实际上抽象工厂模式主要用于替换一系列方法。例如将程序中的 SQL Server 数据库整个替换为 Access 数据库，使用抽象方法模式的话，只需在 IFactory 接口中定义好增删改查四个方法，让 SQLFactory 和 AccessFactory 实现此接口，调用时直接使用 IFactory 中的抽象方法即可，调用者无需知道使用的什么数据库，我们就可以非常方便的整个替换程序的数据库，并且让客户端毫不知情。
  抽象工厂模式很好的发挥了开闭原则、依赖倒置原则，但缺点是抽象工厂模式太重了，如果 IFactory 接口需要新增功能，则会影响到所有的具体工厂类。使用抽象工厂模式，替换具体工厂时只需更改一行代码，但要新增抽象方法则需要修改所有的具体工厂类。所以抽象工厂模式适用于增加同类工厂这样的横向扩展需求，不适合新增功能这样的纵向扩展。

（未完待续）

文章来源：https://www.zhihu.com/question/308850392
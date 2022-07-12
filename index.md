 ###Java操作运算符
 <!-- Aliasing During Method Calls -->
### 方法调用中的别名现象

当我们把对象传递给方法时，会发生别名现象。

```java
// operators/PassObject.java
// 正在传递的对象可能不是你之前使用的
class Letter {
    char c;
}

public class PassObject {
    static void f(Letter y) {
        y.c = 'z';
    }
    
    public static void main(String[] args) {
        Letter x = new Letter();
        x.c = 'a';
        System.out.println("1: x.c: " + x.c);
        f(x);
        System.out.println("2: x.c: " + x.c);
     }
}
```

输出结果：

```
1: x.c: a
2: x.c: z
```

在许多编程语言中，方法 `f()` 似乎会在内部复制其参数 **Letter y**。但是一旦传递了一个引用，那么实际上 `y.c ='z';` 是在方法 `f()` 之外改变对象。别名现象以及其解决方案是个复杂的问题，在附录中有包含：[对象传递和返回](./Appendix-Passing-and-Returning-Objects.md)。意识到这一点，我们可以警惕类似的陷阱。

<!-- Mathematical Operators -->
## 算术运算符

Java 的基本算术运算符与其他大多编程语言是相同的。其中包括加号 `+`、减号 `-`、除号 `/`、乘号 `*` 以及取模 `%`（从整数除法中获得余数）。整数除法会直接砍掉小数，而不是进位。

Java 也用一种与 C++ 相同的简写形式同时进行运算和赋值操作，由运算符后跟等号表示，并且与语言中的所有运算符一致（只要有意义）。  可用 x += 4 来表示：将 x 的值加上4的结果再赋值给 x。更多代码示例：

```java
// operators/MathOps.java
// The mathematical operators
import java.util.*;

public class MathOps {
    public static void main(String[] args) {
        // Create a seeded random number generator:
        Random rand = new Random(47);
        int i, j, k;
        // Choose value from 1 to 100:
        j = rand.nextInt(100) + 1;
        System.out.println("j : " + j);
        k = rand.nextInt(100) + 1;
        System.out.println("k : " + k);
        i = j + k;
        System.out.println("j + k : " + i);
        i = j - k;
        System.out.println("j - k : " + i);
        i = k / j;
        System.out.println("k / j : " + i);
        i = k * j;
        System.out.println("k * j : " + i);
        i = k % j;
        System.out.println("k % j : " + i);
        j %= k;
        System.out.println("j %= k : " + j);
        // 浮点运算测试
        float u, v, w; // Applies to doubles, too
        v = rand.nextFloat();
        System.out.println("v : " + v);
        w = rand.nextFloat();
        System.out.println("w : " + w);
        u = v + w;
        System.out.println("v + w : " + u);
        u = v - w;
        System.out.println("v - w : " + u);
        u = v * w;
        System.out.println("v * w : " + u);
        u = v / w;
        System.out.println("v / w : " + u);
        // 下面的操作同样适用于 char, 
        // byte, short, int, long, and double:
        u += v;
        System.out.println("u += v : " + u);
        u -= v;
        System.out.println("u -= v : " + u);
        u *= v;
        System.out.println("u *= v : " + u);
        u /= v;
        System.out.println("u /= v : " + u);    
    }
}

```

输出结果：

```
j : 59
k : 56
j + k : 115
j - k : 3
k / j : 0
k * j : 3304
k % j : 56
j %= k : 3
v : 0.5309454
w : 0.0534122
v + w : 0.5843576
v - w : 0.47753322
v * w : 0.028358962
v / w : 9.940527
u += v : 10.471473
u -= v : 9.940527
u *= v : 5.2778773
u /= v : 9.940527
```

为了生成随机数字，程序首先创建一个 **Random** 对象。不带参数的 **Random** 对象会利用当前的时间用作随机数生成器的“种子”（seed），从而为程序的每次执行生成不同的输出。在本书的示例中，重要的是每个示例末尾的输出尽可能一致，以便可以使用外部工具进行验证。所以我们通过在创建 **Random** 对象时提供种子（随机数生成器的初始化值，其始终为特定种子值产生相同的序列），让程序每次执行都生成相同的随机数，如此以来输出结果就是可验证的 [^1]。 若需要生成随机值，可删除代码示例中的种子参数。该对象通过调用方法 `nextInt()` 和 `nextFloat()`（还可以调用 `nextLong()` 或 `nextDouble()`），使用 **Random** 对象生成许多不同类型的随机数。`nextInt()` 的参数设置生成的数字的上限，下限为零，为了避免零除的可能性，结果偏移1。

<!-- Unary Minus and Plus Operators -->
### 一元加减运算符

一元加 `+` 减 `-` 运算符的操作和二元是相同的。编译器可自动识别使用何种方式解析运算：

```java
x = -a;
```

上例的代码表意清晰，编译器可正确识别。下面再看一个示例：

```java
x = a * -b;
```

虽然编译器可以正确的识别，但是程序员可能会迷惑。为了避免混淆，推荐下面的写法：

```java
x = a * (-b);
```

一元减号可以得到数据的负值。一元加号的作用相反，不过它唯一能影响的就是把较小的数值类型自动转换为 **int** 类型。

<!-- Auto-Increment-and-Decrement -->
## 递增和递减

和 C 语言类似，Java 提供了许多快捷运算方式。快捷运算可使代码可读性，可写性都更强。其中包括递增 `++` 和递减 `--`，意为“增加或减少一个单位”。举个例子来说，假设 a 是一个 **int** 类型的值，则表达式 `++a` 就等价于 `a = a + 1`。 递增和递减运算符不仅可以修改变量，还可以生成变量的值。

每种类型的运算符，都有两个版本可供选用；通常将其称为“前缀”和“后缀”。“前递增”表示 `++` 运算符位于变量或表达式的前面；而“后递增”表示 `++` 运算符位于变量的后面。类似地，“前递减”意味着 `--` 运算符位于变量的前面；而“后递减”意味着 `--` 运算符位于变量的后面。对于前递增和前递减（如 `++a` 或 `--a`），会先执行递增/减运算，再返回值。而对于后递增和后递减（如 `a++` 或 `a--`），会先返回值，再执行递增/减运算。代码示例：

```java
// operators/AutoInc.java
// 演示 ++ 和 -- 运算符
public class AutoInc {
    public static void main(String[] args) {
        int i = 1;
        System.out.println("i: " + i);
        System.out.println("++i: " + ++i); // 前递增
        System.out.println("i++: " + i++); // 后递增
        System.out.println("i: " + i);
        System.out.println("--i: " + --i); // 前递减
        System.out.println("i--: " + i--); // 后递减
        System.out.println("i: " + i);
    }
}
```

输出结果：

```
i: 1
++i: 2
i++: 2
i: 3
--i: 2
i--: 2
i: 1
```

对于前缀形式，我们将在执行递增/减操作后获取值；使用后缀形式，我们将在执行递增/减操作之前获取值。它们是唯一具有“副作用”的运算符（除那些涉及赋值的以外） —— 它们修改了操作数的值。

C++ 名称来自于递增运算符，暗示着“比 C 更进一步”。在早期的 Java 演讲中，*Bill Joy*（Java 作者之一）说“**Java = C++ --**”（C++ 减减），意味着 Java 在 C++  的基础上减少了许多不必要的东西，因此语言更简单。随着进一步地学习，我们会发现 Java 的确有许多地方相对 C++ 来说更简便，但是在其他方面，难度并不会比 C++ 小多少。

<!-- Relational-Operators -->
## 关系运算符

关系运算符会通过产生一个布尔（**boolean**）结果来表示操作数之间的关系。如果关系为真，则结果为 **true**，如果关系为假，则结果为 **false**。关系运算符包括小于 `<`，大于 `>`，小于或等于 `<=`，大于或等于 `>=`，等于 `==` 和不等于 `！=`。`==` 和 `!=` 可用于所有基本类型，但其他运算符不能用于基本类型 **boolean**，因为布尔值只能表示 **true** 或 **false**，所以比较它们之间的“大于”或“小于”没有意义。

<!-- Testing Object Equivalence -->
### 测试对象等价

关系运算符 `==` 和 `!=` 同样适用于所有对象之间的比较运算，但它们比较的内容却经常困扰 Java 的初学者。下面是代码示例：

```java
// operators/Equivalence.java
public class Equivalence {
    public static void main(String[] args) {
        Integer n1 = 47;
        Integer n2 = 47;
        System.out.println(n1 == n2);
        System.out.println(n1 != n2);
    }
}
```

输出结果：

```
true
false
```

表达式 `System.out.println(n1 == n2)` 将会输出比较的结果。因为两个 **Integer** 对象相同，所以先输出 **true**，再输出 **false**。但是，尽管对象的内容一样，对象的引用却不一样。`==` 和 `!=` 比较的是对象引用，所以输出实际上应该是先输出 **false**，再输出 **true**（译者注：如果你把 47 改成 128，那么打印的结果就是这样，因为 Integer 内部维护着一个 IntegerCache 的缓存，默认缓存范围是 [-128, 127]，所以 [-128, 127] 之间的值用 `==` 和 `!=` 比较也能能到正确的结果，但是不推荐用关系运算符比较，具体见 JDK 中的 Integer 类源码）。

那么怎么比较两个对象的内容是否相同呢？你必须使用所有对象（不包括基本类型）中都存在的 `equals()` 方法，下面是如何使用 `equals()` 方法的示例：

```java
// operators/EqualsMethod.java
public class EqualsMethod {
    public static void main(String[] args) {
        Integer n1 = 47;
        Integer n2 = 47;
        System.out.println(n1.equals(n2));
    }
}
```

输出结果:

```java
true
```

上例的结果看起来是我们所期望的。但其实事情并非那么简单。下面我们来创建自己的类：

```java
// operators/EqualsMethod2.java
// 默认的 equals() 方法没有比较内容
class Value {
    int i;
}

public class EqualsMethod2 {
    public static void main(String[] args) {
        Value v1 = new Value();
        Value v2 = new Value();
        v1.i = v2.i = 100;
        System.out.println(v1.equals(v2));
    }
}
```

输出结果:

```java
false
```

上例的结果再次令人困惑：结果是 **false**。原因： `equals()` 的默认行为是比较对象的引用而非具体内容。因此，除非你在新类中覆写 `equals()` 方法，否则我们将获取不到想要的结果。不幸的是，在学习 [复用](./08-Reuse.md)（**Reuse**） 章节后我们才能接触到“覆写”（**Override**），并且直到 [附录:集合主题](./Appendix-Collection-Topics.md)，才能知道定义 `equals()` 方法的正确方式，但是现在明白 `equals()` 行为方式也可能为你节省一些时间。
大多数 Java 库类通过覆写 `equals()` 方法比较对象的内容而不是其引用。
<!-- Literals -->
## 字面值常量

通常，当我们向程序中插入一个字面值常量（**Literal**）时，编译器会确切地识别它的类型。当类型不明确时，必须辅以字面值常量关联来帮助编译器识别。代码示例：

```java
// operators/Literals.java
public class Literals {
    public static void main(String[] args) {
        int i1 = 0x2f; // 16进制 (小写)
        System.out.println(
        "i1: " + Integer.toBinaryString(i1));
        int i2 = 0X2F; // 16进制 (大写)
        System.out.println(
        "i2: " + Integer.toBinaryString(i2));
        int i3 = 0177; // 8进制 (前导0)
        System.out.println(
        "i3: " + Integer.toBinaryString(i3));
        char c = 0xffff; // 最大 char 型16进制值
        System.out.println(
        "c: " + Integer.toBinaryString(c));
        byte b = 0x7f; // 最大 byte 型16进制值  01111111;
        System.out.println(
        "b: " + Integer.toBinaryString(b));
        short s = 0x7fff; // 最大 short 型16进制值
        System.out.println(
        "s: " + Integer.toBinaryString(s));
        long n1 = 200L; // long 型后缀
        long n2 = 200l; // long 型后缀 (容易与数值1混淆)
        long n3 = 200;
    
        // Java 7 二进制字面值常量:
        byte blb = (byte)0b00110101;
        System.out.println(
        "blb: " + Integer.toBinaryString(blb));
        short bls = (short)0B0010111110101111;
        System.out.println(
        "bls: " + Integer.toBinaryString(bls));
        int bli = 0b00101111101011111010111110101111;
        System.out.println(
        "bli: " + Integer.toBinaryString(bli));
        long bll = 0b00101111101011111010111110101111;
        System.out.println(
        "bll: " + Long.toBinaryString(bll));
        float f1 = 1;
        float f2 = 1F; // float 型后缀
        float f3 = 1f; // float 型后缀
        double d1 = 1d; // double 型后缀
        double d2 = 1D; // double 型后缀
        // (long 型的字面值同样适用于十六进制和8进制 )
    }
}
```

输出结果:

```
i1: 101111
i2: 101111
i3: 1111111
c: 1111111111111111
b: 1111111
s: 111111111111111
blb: 110101
bls: 10111110101111
bli: 101111101011111010111110101111
bll: 101111101011111010111110101111
```

在文本值的后面添加字符可以让编译器识别该文本值的类型。对于 **Long** 型数值，结尾使用大写 `L` 或小写 `l` 皆可（不推荐使用 `l`，因为容易与阿拉伯数值 1 混淆）。大写 `F` 或小写 `f` 表示 **float** 浮点数。大写 `D` 或小写 `d` 表示 **double** 双精度。

十六进制（以 16 为基数），适用于所有整型数据类型，由前导 `0x` 或 `0X` 表示，后跟 0-9 或 a-f （大写或小写）。如果我们在初始化某个类型的数值时，赋值超出其范围，那么编译器会报错（不管值的数字形式如何）。在上例的代码中，**char**、**byte** 和 **short** 的值已经是最大了。如果超过这些值，编译器将自动转型为 **int**，并且提示我们需要声明强制转换（强制转换将在本章后面定义），意味着我们已越过该类型的范围界限。

八进制（以 8 为基数）由 0~7 之间的数字和前导零 `0` 表示。

Java 7 引入了二进制的字面值常量，由前导 `0b` 或 `0B` 表示，它可以初始化所有的整数类型。

使用整型数值类型时，显示其二进制形式会很有用。在 Long 型和 Integer 型中这很容易实现，调用其静态的 `toBinaryString()` 方法即可。 但是请注意，若将较小的类型传递给 **Integer.**`toBinaryString()` 时，类型将自动转换为 **int**。

<!-- Underscores in Literals -->
### 下划线

Java 7 中有一个深思熟虑的补充：我们可以在数字字面量中包含下划线 `_`，以使结果更清晰。这对于大数值的分组特别有用。代码示例：

```java
// operators/Underscores.java
public class Underscores {
    public static void main(String[] args) {
        double d = 341_435_936.445_667;
        System.out.println(d);
        int bin = 0b0010_1111_1010_1111_1010_1111_1010_1111;
        System.out.println(Integer.toBinaryString(bin));
        System.out.printf("%x%n", bin); // [1]
        long hex = 0x7f_e9_b7_aa;
        System.out.printf("%x%n", hex);
    }
}
```

输出结果:

```
3.41435936445667E8
101111101011111010111110101111
2fafafaf
7fe9b7aa
```

下面是合理使用的规则：

1. 仅限单 `_`，不能多条相连。
2. 数值开头和结尾不允许出现 `_`。
3. `F`、`D` 和 `L`的前后禁止出现 `_`。
4. 二进制前导 `b` 和 十六进制 `x` 前后禁止出现 `_`。

[1] 注意 `%n`的使用。熟悉 C 风格的程序员可能习惯于看到 `\n` 来表示换行符。问题在于它给你的是一个“Unix风格”的换行符。此外，如果我们使用的是 Windows，则必须指定 `\r\n`。这种差异的包袱应该由编程语言来解决。这就是 Java 用 `%n` 实现的可以忽略平台间差异而生成适当的换行符，但只有当你使用 `System.out.printf()` 或 `System.out.format()` 时。对于 `System.out.println()`，我们仍然必须使用 `\n`；如果你使用 `%n`，`println()` 只会输出 `%n` 而不是换行符。

<!-- Exponential Notation -->
### 指数计数法

指数总是采用一种我认为很不直观的记号方法:

```java
// operators/Exponents.java
// "e" 表示 10 的几次幂
public class Exponents {
    public static void main(String[] args) {
        // 大写 E 和小写 e 的效果相同:
        float expFloat = 1.39e-43f;
        expFloat = 1.39E-43f;
        System.out.println(expFloat);
        double expDouble = 47e47d; // 'd' 是可选的
        double expDouble2 = 47e47; // 自动转换为 double
        System.out.println(expDouble);
    }
}
```

输出结果:

```
1.39E-43
4.7E48
```

在科学与工程学领域，**e** 代表自然对数的基数，约等于 2.718 （Java 里用一种更精确的 **double** 值 **Math.E** 来表示自然对数）。指数表达式 "1.39 x e-43"，意味着 “1.39 × 2.718 的 -43 次方”。然而，自 FORTRAN 语言发明后，人们自然而然地觉得e 代表 “10 的几次幂”。这种做法显得颇为古怪，因为 FORTRAN 最初是为科学与工程领域设计的。

理所当然，它的设计者应对这样的混淆概念持谨慎态度 [^2]。但不管怎样，这种特别的表达方法在 C，C++ 以及现在的 Java 中顽固地保留下来了。所以倘若习惯 e 作为自然对数的基数使用，那么在 Java 中看到类似“1.39e-43f”这样的表达式时，请转换你的思维，从程序设计的角度思考它；它真正的含义是 “1.39 × 10 的 -43 次方”。

注意如果编译器能够正确地识别类型，就不必使用后缀字符。对于下述语句：

```java
long n3 = 200;
```

它并不存在含糊不清的地方，所以 200 后面的 L 大可省去。然而，对于下述语句：

```java
float f4 = 1e-43f; //10 的幂数
```

编译器通常会将指数作为 **double** 类型来处理，所以假若没有这个后缀字符 `f`，编译器就会报错，提示我们应该将 **double** 型转换成 **float** 型。

<!-- Bitwise-Operators -->
## 位运算符

位运算符允许我们操作一个整型数字中的单个二进制位。位运算符会对两个整数对应的位执行布尔代数，从而产生结果。

位运算源自 C 语言的底层操作。我们经常要直接操纵硬件，频繁设置硬件寄存器内的二进制位。Java 的设计初衷是电视机顶盒嵌入式开发，所以这种底层的操作仍被保留了下来。但是，你可能不会使用太多位运算。

若两个输入位都是 1，则按位“与运算符” `&` 运算后结果是 1，否则结果是 0。若两个输入位里至少有一个是 1，则按位“或运算符” `|` 运算后结果是 1；只有在两个输入位都是 0 的情况下，运算结果才是 0。若两个输入位的某一个是 1，另一个不是 1，那么按位“异或运算符” `^` 运算后结果才是 1。按位“非运算符” `~` 属于一元运算符；它只对一个自变量进行操作（其他所有运算符都是二元运算符）。按位非运算后结果与输入位相反。例如输入 0，则输出 1；输入 1，则输出 0。

位运算符和逻辑运算符都使用了同样的字符，只不过数量不同。位短，所以位运算符只有一个字符。位运算符可与等号 `=` 联合使用以接收结果及赋值：`&=`，`|=` 和 `^=` 都是合法的（由于 `~` 是一元运算符，所以不可与 `=` 联合使用）。

我们将 **Boolean** 类型被视为“单位值”（one-bit value），所以它多少有些独特的地方。我们可以对 boolean 型变量执行与、或、异或运算，但不能执行非运算（大概是为了避免与逻辑“非”混淆）。对于布尔值，位运算符具有与逻辑运算符相同的效果，只是它们不会中途“短路”。此外，针对布尔值进行的位运算为我们新增了一个“异或”逻辑运算符，它并未包括在逻辑运算符的列表中。在移位表达式中，禁止使用布尔值，原因将在下面解释。

<!-- Shift Operators -->
## 移位运算符

移位运算符面向的运算对象也是二进制的“位”。它们只能用于处理整数类型（基本类型的一种）。左移位运算符 `<<` 能将其左边的运算对象向左移动右侧指定的位数（在低位补 0）。右移位运算符 `>>` 则相反。右移位运算符有“正”、“负”值：若值为正，则在高位插入 0；若值为负，则在高位插入 1。Java 也添加了一种“不分正负”的右移位运算符（>>>），它使用了“零扩展”（zero extension）：无论正负，都在高位插入 0。这一运算符是 C/C++ 没有的。

如果移动 **char**、**byte** 或 **short**，则会在移动发生之前将其提升为 **int**，结果为 **int**。仅使用右值（rvalue）的 5 个低阶位。这可以防止我们移动超过 **int** 范围的位数。若对一个 **long** 值进行处理，最后得到的结果也是 **long**。

移位可以与等号 `<<=` 或 `>>=` 或 `>>>=` 组合使用。左值被替换为其移位运算后的值。但是，问题来了，当无符号右移与赋值相结合时，若将其与 **byte** 或 **short** 一起使用的话，则结果错误。取而代之的是，它们被提升为 **int** 型并右移，但在重新赋值时被截断。在这种情况下，结果为 -1。下面是代码示例：

```java
// operators/URShift.java
// 测试无符号右移
public class URShift {
    public static void main(String[] args) {
        int i = -1;
        System.out.println(Integer.toBinaryString(i));
        i >>>= 10;
        System.out.println(Integer.toBinaryString(i));
        long l = -1;
        System.out.println(Long.toBinaryString(l));
        l >>>= 10;
        System.out.println(Long.toBinaryString(l));
        short s = -1;
        System.out.println(Integer.toBinaryString(s));
        s >>>= 10;
        System.out.println(Integer.toBinaryString(s));
        byte b = -1;
        System.out.println(Integer.toBinaryString(b));
        b >>>= 10;
        System.out.println(Integer.toBinaryString(b));
        b = -1;
        System.out.println(Integer.toBinaryString(b));
        System.out.println(Integer.toBinaryString(b>>>10));
    }
}
```

输出结果：

```
11111111111111111111111111111111
1111111111111111111111
1111111111111111111111111111111111111111111111111111111111111111
111111111111111111111111111111111111111111111111111111
11111111111111111111111111111111
11111111111111111111111111111111
11111111111111111111111111111111
11111111111111111111111111111111
11111111111111111111111111111111
1111111111111111111111
```


在上例中，结果并未重新赋值给变量 **b** ，而是直接打印出来，因此一切正常。下面是一个涉及所有位运算符的代码示例：

```java
// operators/BitManipulation.java
// 使用位运算符
import java.util.*;
public class BitManipulation {
    public static void main(String[] args) {
        Random rand = new Random(47);
        int i = rand.nextInt();
        int j = rand.nextInt();
        printBinaryInt("-1", -1);
        printBinaryInt("+1", +1);
        int maxpos = 2147483647;
        printBinaryInt("maxpos", maxpos);
        int maxneg = -2147483648;
        printBinaryInt("maxneg", maxneg);
        printBinaryInt("i", i);
        printBinaryInt("~i", ~i);
        printBinaryInt("-i", -i);
        printBinaryInt("j", j);
        printBinaryInt("i & j", i & j);
        printBinaryInt("i | j", i | j);
        printBinaryInt("i ^ j", i ^ j);
        printBinaryInt("i << 5", i << 5);
        printBinaryInt("i >> 5", i >> 5);
        printBinaryInt("(~i) >> 5", (~i) >> 5);
        printBinaryInt("i >>> 5", i >>> 5);
        printBinaryInt("(~i) >>> 5", (~i) >>> 5);
        long l = rand.nextLong();
        long m = rand.nextLong();
        printBinaryLong("-1L", -1L);
        printBinaryLong("+1L", +1L);
        long ll = 9223372036854775807L;
        printBinaryLong("maxpos", ll);
        long lln = -9223372036854775808L;
        printBinaryLong("maxneg", lln);
        printBinaryLong("l", l);
        printBinaryLong("~l", ~l);
        printBinaryLong("-l", -l);
        printBinaryLong("m", m);
        printBinaryLong("l & m", l & m);
        printBinaryLong("l | m", l | m);
        printBinaryLong("l ^ m", l ^ m);
        printBinaryLong("l << 5", l << 5);
        printBinaryLong("l >> 5", l >> 5);
        printBinaryLong("(~l) >> 5", (~l) >> 5);
        printBinaryLong("l >>> 5", l >>> 5);
        printBinaryLong("(~l) >>> 5", (~l) >>> 5);
    }

    static void printBinaryInt(String s, int i) {
        System.out.println(
        s + ", int: " + i + ", binary:\n " +
        Integer.toBinaryString(i));
    }

    static void printBinaryLong(String s, long l) {
        System.out.println(
        s + ", long: " + l + ", binary:\n " +
        Long.toBinaryString(l));
    }
}
```

输出结果（前 32 行）：

```
-1, int: -1, binary:
11111111111111111111111111111111
+1, int: 1, binary:
1
maxpos, int: 2147483647, binary:
1111111111111111111111111111111
maxneg, int: -2147483648, binary:
10000000000000000000000000000000
i, int: -1172028779, binary:
10111010001001000100001010010101
~i, int: 1172028778, binary:
 1000101110110111011110101101010
-i, int: 1172028779, binary:
1000101110110111011110101101011
j, int: 1717241110, binary:
1100110010110110000010100010110
i & j, int: 570425364, binary:
100010000000000000000000010100
i | j, int: -25213033, binary:
11111110011111110100011110010111
i ^ j, int: -595638397, binary:
11011100011111110100011110000011
i << 5, int: 1149784736, binary:
1000100100010000101001010100000
i >> 5, int: -36625900, binary:
11111101110100010010001000010100
(~i) >> 5, int: 36625899, binary:
10001011101101110111101011
i >>> 5, int: 97591828, binary:
101110100010010001000010100
(~i) >>> 5, int: 36625899, binary:
10001011101101110111101011
    ...
```

结尾的两个方法 `printBinaryInt()` 和 `printBinaryLong()` 分别操作一个 **int** 和 **long** 值，并转换为二进制格式输出，同时附有简要的文字说明。除了演示 **int** 和 **long** 的所有位运算符的效果之外，本示例还显示 **int** 和 **long** 的最小值、最大值、+1 和 -1 值，以便我们了解它们的形式。注意高位代表符号：0 表示正，1 表示负。上面显示了 **int** 部分的输出。以上数字的二进制表示形式是带符号的补码（2's complement）。


###||引自《On Java 8》


Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/yuqi-wang-forever/yuqi-wang-forever.github.io/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.

---
title: Chisel
date: 2023/11/27
categories:
  - [计算机科学, 计算机组成原理, YatCPU]
  - [计算机科学, 编程语言]
tag:
  - 计组
  - Scala
  - Chisel
## valine:
##   placeholder:
---

## Scala

### import

在 Scala 中，import 语句用于引入其他类、对象或特质，以便在当前作用域中可以直接访问它们的成员。下面是一些关于 Scala 中 import 的常见用法：

1. 基本用法

```scala
// 导入单个类
import scala.collection.mutable.ListBuffer

// 导入整个包
import scala.collection.mutable._

// 重命名导入的类/对象/特质
import java.util.{ArrayList => JArrayList, HashMap => JHashMap}

// 隐藏某个成员
import java.util.{Random => _, _} // 隐藏java.util.Random
```

2. 导入对象的成员

```scala
import java.awt.Color._
val c = RED // 直接访问Color对象的成员RED
```

3. 静态导入

```scala
import java.lang.Math._
val maxNum = max(5, 10) // 直接使用Math类的静态方法max
```

### 测试

#### ScalaTest

要使用 ScalaTest 进行测试可以按照以下基本步骤进行操作：

导入必要的包和类：

```scala
import org.scalatest.flatspec.AnyFlatSpec
import org.scalatest.matchers.should.Matchers
```

创建测试类并扩展相应的测试风格特质：

```scala
class MyTest extends AnyFlatSpec with Matchers {
  // 在这里编写测试代码
}
```

在这个例子中，我们创建了一个名为"MyTest"的测试类，并将其扩展为 AnyFlatSpec 和 Matchers。AnyFlatSpec 是 ScalaTest 提供的一种测试规范风格，而 Matchers 允许你使用自然语言风格编写断言。

编写测试代码：
在测试类中，你可以编写各种测试方法来验证你的代码行为是否符合预期，例如：

```scala
it should "return the correct result" in {
  // 编写测试代码来验证某个函数或者方法的行为
  val result = myFunction()
  result should equal (expectedResult)
}
```

在这个例子中，我们使用"it should"来描述测试的行为，然后使用"in"关键字定义测试代码块。在测试代码块中，我们调用了一个名为"myFunction"的函数，并使用"should equal"断言来验证其返回值是否等于"expectedResult"。

运行测试：\
使用**构建工具**（如 sbt）来运行测试。在 sbt 中，你可以执行以下命令来运行测试：

```bash
sbt test
```

这将会执行所有的测试，并输出测试结果。

## Chisel

在 Scala 中，Chisel 是一个常用于硬件设计的领域特定语言（DSL），它提供了许多有用的库和工具，一些常用的库包括：

1. chisel3：这是 Chisel 的核心库，它定义了 Chisel 的语法和 API。
2. firrtl：这是 Chisel 的前端，用于将 Chisel 代码转换为可执行的 RTL 代码。
3. chisel-iotesters：这是一个用于测试 Chisel 模块的库，它提供了一些常见的测试工具和模拟器。
4. treadle：这是一个开源的 Verilog 仿真器，可以与 Chisel 代码一起使用进行仿真和调试。
5. chisel-testers2：这是一个测试框架，用于编写和运行 Chisel 测试。
6. chisel-formal：这是一个用于形式验证的库，可用于对 Chisel 模块进行形式化验证。
7. chisel-fluent：这是一个用于创建可重用组件库的库，使得在不同的项目中可以轻松地重用 Chisel 模块。

在 chisel3 库中，常用的一些子库包括：

1. chisel3：Chisel3 核心库，定义了 Chisel 的语法和 API。
2. chisel3.util：提供了一些常见的工具和实用函数，如位宽转换、数据选择器、优先级编码器等。
3. chisel3.experimental：包含一些实验性质的功能和模块，可能还处于开发或测试阶段。
4. chisel3.testers：提供了对 Chisel 模块进行测试的相关工具和框架，用于验证设计的正确性。
5. chisel3.tester：一个用于编写和运行 Chisel 测试的测试框架，支持生成测试向量、执行仿真等操作。
6. chisel3.stage：用于将 Chisel 生成的硬件描述转换为 Verilog 或 FIRRTL 格式的工具。
7. chisel3.formal：用于形式验证的库，可以对 Chisel 模块进行形式化验证。

有的时候不知道为什么`import chisel._`了还是有些函数或运算符编译反馈`not found`或者 warnning，解决办法是“更具体地 import”，比如:\
遇到编译报错：`not found value Cat`
可以在文件开头加上`import chisel.util.Cat`来解决

### 数据类型

#### Seq

Seq (Sequence) 是一个表示序列的数据类型。它用于定义一系列连续的元素，并提供了一些方法来操作和访问这些元素。

Seq 可以包含任意类型的元素，例如整数、布尔值或自定义的 Chisel 数据类型。通过使用**Seq.apply**方法或使用**逗号分隔的元素列表**，可以**创建一个 Seq 实例**。

以下是一个使用 Seq 的示例：

```chisel
import chisel3._

class MyModule extends Module {
  val io = IO(new Bundle {
    val data = Output(Vec(4, UInt(8.W)))
  })

  val mySeq = Seq(1.U, 2.U, 3.U, 4.U) // 创建一个包含4个UInt元素的Seq

  io.data := mySeq
}
```

在上面的示例中，我们创建了一个名为 mySeq 的 Seq，其中包含了 4 个 UInt 类型的元素。然后，我们将这个 mySeq 赋值给模块的输出端口 io.data，以便在仿真或生成的电路中使用。

除了基本的创建和赋值操作之外，Seq 还提供了许多方法来操作和查询序列，例如 **length（获取序列的长度）、apply（按索引获取元素）、map（对序列中的每个元素进行映射操作）** 等。可以根据具体的需求选择适当的方法来操作 Seq。

下面是三个示例，分别演示了如何使用 Seqlength、Seq.apply 和 Seq.map 方法：

```chisel
import chisel3._

class MyModule extends Module {
  val io = IO(new Bundle {
    val dataIn = Input(Vec(4, UInt(8.W)))
    val dataOut = Output(UInt(8.W))
  })

  val mySeq = Seq(1.U, 2.U, 3.U, 4.U) // 创建一个包含4个UInt元素的Seq

  // 使用length方法获取序列的长度，并将结果输出到io.dataOut端口
  io.dataOut := mySeq.length.U

  // 使用apply方法按索引获取序列中的元素，并将结果输出到io.dataOut端口
  io.dataOut := mySeq(2)

  // 使用map方法对序列中的每个元素进行加1操作，并将结果输出到io.dataOut端口
  io.dataOut := mySeq.map(_ + 1.U)(2)

  // 将输入端口io.dataIn中的数据存入Seq中，并使用foreach方法打印Seq中的所有元素
  val inputSeq = Seq(io.dataIn(0), io.dataIn(1), io.dataIn(2), io.dataIn(3))
  inputSeq.foreach(x => println(s"Element: $x"))
}
```

在上面的示例中，我们首先创建了一个名为 mySeq 的 Seq，其中包含了 4 个 UInt 类型的元素。然后，我们分别使用 length、apply 和 map 方法来操作这个序列，并将结果输出到模块的输出端口 io.dataOut。最后，我们还展示了如何将输入端口 io.dataIn 中的数据存入 Seq 中，并使用 foreach 方法打印序列中的所有元素。

#### Vec

Vec （Vector） 是一个用于表示固定大小的向量的类型。它可以存储多个**相同类型**的元素，并提供一些便捷的操作方法。

以下是一个使用 Vec 的简单例子：

```chisel
import chisel3._

class ExampleModule extends Module {
  val io = IO(new Bundle {
    val dataIn = Input(Vec(4, UInt(8.W)))
    val dataOut = Output(UInt(32.W))
  })

  // 创建一个包含4个8位无符号整数的Vec
  val myVec = VecInit(Seq(1.U, 2.U, 3.U, 4.U))

  // 从Vec中读取数据，地址为io.dataIn(2)，输出到io.dataOut
  io.dataOut := myVec(io.dataIn(2))
}
```

在上面的例子中，我们首先使用 VecInit 函数创建了一个包含 4 个 8 位无符号整数的 Vec，并将其命名为 myVec。VecInit 函数接受一个序列作为参数，用于指定初始值。在这个例子中，我们使用 Seq 声明了一个包含四个元素的序列，每个元素都是一个 8 位的无符号整数。

然后，我们通过 io.dataIn(2) 获取输入端口 io.dataIn 中索引为 2 的元素作为读取的地址，使用 myVec 进行索引操作，并将结果输出到 io.dataOut 端口。

需要注意的是，Vec 的索引从 0 开始，因此在上述例子中，io.dataIn(2) 表示 io.dataIn 中的第三个元素。同时，需要确保索引操作的范围在 Vec 的大小范围之内，否则会导致运行时错误。

#### Array

在 Chisel 中，您可以使用 Scala 的 Array 类型来表示和操作固定大小的存储器或缓冲区等数据结构。下面是一个使用 Array 的简单示例：

```chisel
import chisel3._

class MyModule extends Module {
  val io = IO(new Bundle {
    val in = Input(UInt(8.W))
    val out = Output(UInt(8.W))
  })

  val myArray = Mem(4, UInt(8.W)) // 创建一个包含4个8位元素的Array

  when(io.in < 4.U) {
    myArray(io.in) := io.in * 2.U // 访问并更新Array中的元素
  }

  io.out := myArray(2.U) // 从Array中获取元素并将其输出
}
```

在这个例子中，我们使用 Mem 函数创建了一个包含 4 个 8 位元素的 Array。使用()操作符来访问和更新 Array 中的元素。最后，我们从 Array 中获取第三个元素并将其作为输出。

需要注意的是，Chisel 中的 Array 类型与 Scala 中的 Array 类型有所不同。在 Chisel 中，Array 类型通常用于表示硬件电路的存储器或缓冲区，而不是通用的运行时数组。因此，对 Array 的操作会生成与之对应的硬件电路。

#### IndexedSeq

IndexedSeq 是 Scala 标准库中的一种数据类型，用于表示一组具有固定顺序的元素，可以通过索引访问。在 Chisel 中，您可以使用 IndexedSeq 来表示固定大小的存储器或缓冲区等数据结构。

以下是一个使用 IndexedSeq 的简单示例：

```chisel
import chisel3._

class MyModule extends Module {
  val io = IO(new Bundle {
    val in = Input(UInt(8.W))
    val out = Output(UInt(8.W))
  })

  val myMem = IndexedSeq.fill(4)(0.U(8.W)) // 创建一个包含4个8位元素的IndexedSeq

  when(io.in < 4.U) {
    myMem(io.in) := io.in * 2.U // 访问并更新IndexedSeq中的元素
  }

  io.out := myMem(2.U) // 从IndexedSeq中获取元素并将其输出
}
```

在这个例子中，我们创建了一个包含 4 个 8 位元素的 IndexedSeq，并使用 io.in 信号作为索引来访问和更新该索引处的元素。最后，我们从 IndexedSeq 中获取第三个元素并将其作为输出。

### 信号转换的实现

#### Mux

在 Chisel 中，mux 函数用于选择两个输入中的一个，并将其作为输出。它的语法如下：

```chisel
mux(condition: Bool, trueOutput: T, falseOutput: T): T
```

其中，condition 是一个**布尔**类型的条件，如果为 true，则选择 trueOutput 作为输出，否则选择 falseOutput 作为输出。

以下是一个简单的例子，展示了如何使用 mux 函数：

```chisel
import chisel3._

class MuxExample extends Module {
  val io = IO(new Bundle {
    val sel = Input(Bool())
    val in0 = Input(UInt(4.W))
    val in1 = Input(UInt(4.W))
    val out = Output(UInt(4.W))
  })

  io.out := mux(io.sel, io.in1, io.in0)
}
```

在这个例子中，当输入 sel 为 true 时，输出 in1，否则输出 in0。

#### MuxLookup

它可以根据输入的索引值来选择对应的数据，并返回所选数据。

在 MuxLookup 的输入的三个参数中:\
第一个参数是选择的**索引值**，\
第二个参数是**默认值**（如果索引值**不匹配时**的默认返回值），\
第三个参数是一个**数组**，其中包含索引值与对应数据的映射关系。

下面是一个简单的示例，演示了如何在 Chisel 中使用 MuxLookup：

```chisel
import chisel3._

class MyModule extends Module {
  val io = IO(new Bundle {
    val index = Input(UInt(2.W))
    val data0 = Input(UInt(8.W))
    val data1 = Input(UInt(8.W))
    val data2 = Input(UInt(8.W))
    val result = Output(UInt(8.W))
  })

  // 使用 MuxLookup 进行多路选择
  io.result := MuxLookup(io.index, 0.U,
    Array(
      0.U -> io.data0,
      1.U -> io.data1,
      2.U -> io.data2
    )
  )
}
```

在这个例子中，我们定义了一个名为 MyModule 的模块，该模块有一个名为 index 的输入，以及三个数据输入 data0、data1 和 data2。我们使用 MuxLookup 来根据 index 的值选择相应的数据，并将结果输出到 result。

#### Cat

在 Chisel 中，Cat 函数用于将多个信号或者数字连接成一个宽信号，其语法为：

```chisel
Cat(signals: Bits*)
```

其中 signals 是需要拼接的信号，可以是 Bits 或者 UInt 类型。

例如，假设有两个 UInt 类型的信号 a 和 b，我们可以使用 Cat 函数将它们拼接成一个宽度为 2\*W 的 UInt 类型的信号 c，其中 W 是 a 和 b 的宽度，代码如下：

```chisel
val a = UInt(3.W)
val b = UInt(5.W)

val c = Cat(a, b)
```

这里 c 的宽度为 2\*W，即 8 位。Cat 函数会先将 a 的 3 位和 b 的 5 位分别拼接起来，得到一个 8 位的信号。如果需要改变信号的顺序，可以在 signals 中按照需要的顺序传入信号。

除了 UInt 类型的信号，Cat 函数也可以用于拼接 Bits 类型的信号，例如：

```chisel
val a = "b1101".U(4.W)
val b = "b1010".U(4.W)

val c = Cat(a, b)
```

这里 a 和 b 都是 4 位的二进制信号，并且使用 U 方法进行初始化。Cat 函数将 a 和 b 拼接起来得到一个 8 位的二进制信号。

需要注意的是，在使用 Cat 函数时，传入的信号宽度必须匹配，否则会出现编译错误。

Cat 函数可以连接三个及以上信号。在 Chisel 中，Cat 函数可以接受任意数量的参数，每个参数都是一个需要拼接的信号。下面是一个使用 Cat 函数拼接三个信号的例子：

```chisel
val a = UInt(3.W)
val b = UInt(5.W)
val c = UInt(2.W)

val d = Cat(a, b, c)
```

这里我们定义了三个无符号整数信号 a、b 和 c，分别为 3 位、5 位和 2 位。然后我们使用 Cat 函数将它们拼接起来，得到一个 10 位的无符号整数信号 d。

需要注意的是，使用 Cat 函数拼接多个信号时，每个信号的位宽必须相同，否则会出现编译错误。如果需要拼接不同位宽的信号，可以使用 ZeroExt 或者 SignExt 函数将其扩展到相同的位宽再进行拼接。

##### ##运算符

在 Chisel 中，除了字符串之外，Cat 函数和##运算符是等效的，它们都用于连接不同的信号或值。

Cat 函数用于连接多个信号或值，例如：

```chisel
val result = Cat(sig1, sig2, sig3)
```

这将把 sig1、sig2 和 sig3 连接成一个更长的信号。

\##运算符也可以用于连接信号或值，例如：

```chisel
val result = sig1 ### sig2 ### sig3
```

这将以与 Cat 函数相同的方式连接 sig1、sig2 和 sig3。上面两种实现方式是等价的。

#### log2Up

log2Up 是一个用于计算以 2 为底的对数的函数。它返回一个整数，表示将给定的输入值取整后最接近且大于等于其对数的结果。

具体而言，log2Up 函数用于确定将 x 个元素分为若干组时，所需的组数。例如，如果有 9 个元素需要分为若干组，则可以使用 log2Up(9)来计算所需的组数，结果为 4，因为 2^4=16，其中 16 是大于等于 9 的最小的 2 的幂。

以下是一个使用 log2Up 的示例代码片段：

```chisel
import chisel3._

class ExampleModule extends Module {
  val io = IO(new Bundle {
    val input = Input(UInt(8.W))
    val output = Output(UInt(log2Up(9).W))
  })

  io.output := io.input
}
```

在上面的示例中，log2Up(9)用于指定输出信号的宽度，以确保可以容纳最多 9 个元素的索引。

### Module

Module（模块）：
在 Chisel 中，Module 是用于描述数字电路中的模块或组件的关键概念。它类似于 Verilog 中的 module，用于定义数字电路中的各种组件，比如寄存器、ALU 等。Module 中可以包含其他 Module 或 Bundle，并且可以实例化其他 Module。

#### Bundle

Bundle（捆绑包）：
在 Chisel 中，Bundle 是用于描述数字电路中信号线集合的概念。它类似于 Verilog 中的 wire 或者 reg，用于定义一组信号线或接口。通常情况下，我们会将相关的信号线打包成一个 Bundle，以便更好地组织和管理这些信号线。

#### Mem

在 Chisel 中，Mem 是一个表示内存的抽象数据类型。它可以用来创建和操作存储器。你可以使用 Mem 来定义不同类型和大小的存储器，并对其进行读写操作。这使得在硬件描述中对存储器进行建模变得更加方便和灵活。

当使用 Chisel 中的 Mem 时，你可以像下面这样定义一个简单的存储器：

```chisel
import chisel3._

class MyModule extends Module {
  val io = IO(new Bundle {
    val address = Input(UInt(5.W))
    val dataIn = Input(UInt(8.W))
    val dataOut = Output(UInt(8.W))
  })

  val memory = Mem(32, UInt(8.W)) // 创建一个包含32个8位元素的存储器

  when (io.dataIn **= 1.U) {
    memory.write(io.address, 42.U) // 将42写入指定地址
  }

  io.dataOut := memory.read(io.address) // 从指定地址读取数据并输出
}
```

在这个例子中，我们创建了一个名为 memory 的 Mem 实例，它有 32 个 8 位元素。然后根据输入的 address 和 dataIn 执行写操作，根据输入的 address 执行读操作，并将读取的数据输出到 dataOut。这个例子展示了如何在 Chisel 中使用 Mem 来创建、写入和读取存储器。

#### SyncReadMem

SyncReadMem 是 Chisel 中的一个内存模块，可以用于存储和读取数据。它与标准的 Mem 模块相比，具有更高的时序保证和更低的资源消耗。

以下是一个使用 SyncReadMem 的简单例子：

```chisel
import chisel3._

class ExampleModule extends Module {
  val io = IO(new Bundle {
    val address = Input(UInt(8.W))
    val dataOut = Output(UInt(32.W))
  })

  // 创建一个地址宽度为8位，数据宽度为32位的 SyncReadMem
  val myMemory = SyncReadMem(256, UInt(32.W))

  // 向内存中写入数据，地址从io.address获取，数据为0xdeadbeef
  myMemory.write(io.address, 0xdeadbeef.U)

  // 从内存中读取数据，地址从io.address获取，输出到io.dataOut
  io.dataOut := myMemory.read(io.address)
}
```

在上面的例子中，我们首先创建了一个宽度为 8 位、深度为 256 的 SyncReadMem，并将其命名为 myMemory。然后，我们使用 write 函数向内存中写入数据。write 函数的第一个参数是要写入的地址，第二个参数是要写入的数据。在这个例子中，我们使用输入端口 io.address 作为要写入的地址，写入的数据为 0xdeadbeef。最后，我们使用 read 函数从内存中读取数据，并将读取的数据输出到 io.dataOut 端口。

需要注意的是，SyncReadMem 中写入和读取的地址必须是 UInt 类型，且宽度必须与创建 SyncReadMem 时指定的地址宽度相同。而要写入的数据和从内存中读取的数据必须是与创建 SyncReadMem 时指定的数据宽度相同的 UInt 类型。

#### Enumeration

### 测试

#### org.scalatest

org.scalatest 是 Scala 的一个功能强大的测试框架，它提供了多种测试风格和断言方法，可以针对各种类型的测试场景来编写测试套件。该框架支持多种测试运行器，例如 JUnit、TestNG 等，并允许你以多种方式运行测试，例如通过 sbt、Maven、Gradle 或者独立运行测试类或套件。它支持多种测试风格，包括 FlatSpec、FunSpec、WordSpec、FeatureSpec 等。

#### chiseltest

ChiselTest 则是 Chisel 的一个测试框架，它专门针对硬件设计而设计，提供了一组工具和库，用于编写和运行测试套件以验证硬件设计的功能和正确性。它与 Chisel 硬件描述语言紧密绑定，可以方便地使用 Chisel 模块来建模和测试硬件电路。同时，它还提供了仿真运行器和硬件设备运行器，以便在不同的平台上运行测试。

#### ChiselScalatestTester

ChiselScalatestTester 是一个用于进行 Chisel 设计的 Scala 测试的工具包。它可以帮助你编写测试代码并运行 Chisel 设计的单元测试。

以下是使用 ChiselScalatestTester 的基本步骤：

1. 在你的项目中添加 ChiselScalatestTester 的依赖。你可以在项目的构建文件（如 build.sbt）中添加以下代码：

```scala
libraryDependencies += "edu.berkeley.cs" %% "chiseltest" % "0.3.2"
```

2. 创建一个继承自 ChiselFlatSpec 或 ChiselScalatestTester 的测试类。例如：

```chisel
import chisel3._
import chiseltest._
import org.scalatest._

class MyModuleSpec extends FlatSpec with ChiselScalatestTester {
  // 测试代码将写在这里
}
```

测试代码有以下几种实现：

##### test 方法

在测试类中定义你的测试。你可以使用 test 方法来定义一个测试。例如：

```chisel
test("MyModule should do something") {
  // 这里是测试代码
  // 创建一个测试模块的实例
  val dut = Module(new MyModule())

  // 在时钟上升沿之前设置输入信号
  dut.io.input.poke(0.U)

  // 在时钟上升沿时触发输入信号
  dut.clock.step()
  dut.io.input.poke(1.U)

  // 在时钟上升沿之后检查输出信号
  dut.clock.step()
  dut.io.output.expect(1.U)
}
```

在测试中，你可以使用 poke 方法来设置输入信号的值，使用 expect 方法来检查输出信号的值。使用 clock.step() 方法来推进时钟。

运行测试。你可以使用 ScalaTest 提供的运行器来执行测试。例如，你可以使用 sbt 来运行测试：
sbt test
以上是使用 ChiselScalatestTester 的基本步骤。你可以根据自己的需求编写更复杂的测试代码。还可以参考 Chisel 官方文档和 ChiselScalatestTester 的文档获取更多详细信息。

##### 描述语言

behavior of 和 it should 是 ScalaTest 中的 DSL（领域特定语言），用于组织和描述测试用例。

behavior of 用于定义一个测试套件，例如：

```chisel
behavior of "MyModule"
```

这里定义了一个名为 "MyModule" 的测试套件。你可以在这个测试套件中定义若干个测试用例。

it should 用于定义一个测试用例，例如：

```chisel
it should "perform some operation correctly" in {
  // Test code goes here
}
```

这里定义了一个名为 "perform some operation correctly" 的测试用例。在 in 关键字之后，你可以编写测试代码，包括调用 Chisel 模块的方法并验证其输出是否符合预期。

需要注意的是，`behavior of` 和 `it should` 都是 ScalaTest 中的 DSL，实际上它们只是一些方法调用的语法糖，它们并没有直接与 ChiselScalatestTester 或者 Chisel 本身相关联。因此，你可以根据需要进行灵活使用，例如可以自定义测试用例的描述语言或者不使用 behavior of 而是将测试用例作为独立的函数来定义。

##### 自定义描述语言

如果你想自定义测试用例的描述语言，可以使用 ScalaTest 中的 describe 方法和 should 方法。例如：

```chisel
import org.scalatest._

class MyModuleSpec extends FlatSpec with Matchers {
  "MyModule" should "perform some operation correctly" in {
    // Test code goes here
  }
}
```

在这个例子中，我们**使用 FlatSpec 样式的测试框架，而不是 ChiselScalatestTeste**r。在测试代码中，我们使用了 should 方法来描述测试用例的预期行为。这个描述语言与 behavior of 和 it should 大致相同，只是使用了不同的关键字。

##### 函数定义

如果你想将测试用例定义为独立的函数，可以像下面这样编写代码：

```chisel
import org.scalatest._

class MyModuleSpec extends FlatSpec with Matchers {
  "MyModule" should "perform some operation correctly" in {
    performSomeOperation() shouldBe true
  }

  def performSomeOperation(): Boolean = {
    // Test code goes here
    true
  }
}
```

在这个例子中，我们定义了一个名为 performSomeOperation 的独立函数，并在测试用例中调用它。这种方式可以使测试代码更加清晰，同时也方便测试用例的复用。

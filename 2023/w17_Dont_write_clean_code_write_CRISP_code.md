# 不要写 clean code, 要写 CRISP code

- 原文地址：https://bitfieldconsulting.com/golang/crisp-code
- 原文作者：John Arundel
- 本文永久链接：https://github.com/gocn/translator/blob/master/2023/w17_Dont_write_clean_code_write_CRISP_code.md
- 译者：[Cluas](https://github.com/Cluas)
- 校对：[zxmfke](https://github.com/zxmfke)

我相信我们都赞成“clean code”，但这是一种没有人可以合理反对的母爱和苹果派式（它是一种显而易见的东西，没有人会不同意）的东西。除非是为色情网站编写代码，否则谁想写“肮脏”的代码呢？

当然，问题在于我们中很少有人能够就“clean code”*意味*着什么以及如何达到共识。例如，“方法应该只做一件事”这样的规则在 T 恤上看起来很棒，但在实践中并不容易应用。这里的问题在于，究竟什么算是“一件事”呢？

在我一生中的编程经历中，从使用 ZX81（1981 年发布）开始，一直到现在，我发现了一些持久有用的原则。相比规则，原则更加灵活，可能更广泛适用。


我认为好的程序应该是 Correct（正确的）、Readable（可读的）、Idiomatic（惯用的）、Simple（简单的）和 Performant（高效的）。因此，这里是我在 Go 语言中写 CRISP 代码的五个规则：它们不一定按重要性排序，除了第一个规则，但它是一个不错的缩略词。

## Correct(正确的)


你的代码可能有很多其他优点，但如果它不是*正确的*，那么这些优点实际上并不重要。而且，如果我们愿意放弃正确性，我们可以相对容易地获得任何其他属性。在编写代码时，正确性是最重要的因素之一，因为一个程序只有在它能够正确地完成它的任务时才有意义。

>*正确性是首要的品质。如果一个系统不能够完成它应该完成的任务，那么它的其他方面——无论它有多快，有多好的用户界面——都不重要。但这说起来容易做起来难*
>*——Bertrand Meyer，《面向对象软件构造》*

虽然这个观点似乎很显然，不值得一提，但如果存在不正确的代码，而我认为确实存在，那么这个观点肯定值得强调，因为显然有人没有注意到这个重要性。

实际上，代码正确意味着什么？直截了当的答案是“它做了编写者想要的事情”，但即使如此也不完全正确，因为我可能有错误的*意图*！例如，我可能在错误的观念下编写了一个素数生成器，认为所有的奇数都是素数。在这种情况下，我的代码可能会产生奇数整数，如我所要求的，但仍然是不正确的。因此，正确的代码必须符合编写者的意图，并且这些意图必须是正确的。

一个好的[测试](https://bitfieldconsulting.com/golang/tdd)套件，就像苹果派一样，几乎每个人都希望拥有，即使他们更喜欢别人做艰苦的工作来制作它。而且测试是任何好程序的必要但不充分的部分。毫无疑问，有些正确的程序没有测试，但谁知道它们是哪些呢？因此，测试对于确保代码正确性和质量至关重要。

一个写得好的测试能够表达程序员对在特定情况下代码应该做什么的想法，运行测试应该能够给我们一定程度的信心，判断代码在实际情况下是否确实做到了这一点。然而，测试本身也可能是不正确的。因此，在编写测试时，需要仔细考虑各种情况，并确保测试能够覆盖到所有可能的情况，以尽可能地减少测试本身的错误。

这是我们需要对任何给定的测试非常怀疑和持怀疑态度的原因之一。它看起来是在说什么？它实际上是否表达了这个意思？这是正确的话吗？如果它验证程序的结果与某些预期的结果相比，那么这个“预期”是正确的吗？如果测试声称覆盖某个特定的代码段，它是否实际测试了该代码在所有重要的方式下的行为，还是仅仅使其被执行而已？因此，在评估测试时，需要对测试的正确性进行仔细的评估和分析，以确保测试本身是正确的，并能够有效地检测代码的行为。

没有测试是一个可怕的情况，但实际上比有不起作用的测试稍微好一些。一个不正常或不充分的测试可能会让我们对代码产生虚假的信心，而这个代码可能也是不正确的。因此，测试的质量和有效性对于确保代码质量至关重要，不仅要编写足够的测试，还要确保这些测试的正确性和完整性，以便在检测到错误时能够及时发现并修复它们。

因此，一旦编写了测试，就应该非常仔细地再次仔细阅读它们，以对抗的眼光来审视它们。程序员是不可救药的乐观主义者：尽管有很多反对证据，我们始终认为自己的代码会起作用。相反，我们应该假设如果可能存在错误，那么就一定存在错误。[谦逊](https://bitfieldconsulting.com/golang/tao-of-go)并不是软件工程师通常所关联的美德，但对于优秀的测试编写者来说，这肯定是他们所了解的。

即使是通过最彻底的测试的代码也不应假定为正确的，而应该假设它是不正确的，因为它很可能确实存在错误。这种假设能够帮助我们更加谨慎地对待代码，更加仔细地审查和测试它，并且不断地改进它，以确保代码的质量和稳定性。这也是一种谦虚的态度，能够帮助我们在软件开发过程中保持谨慎和谦虚的心态。

## Readable(可读的)

再次强调，这听起来可能像是不需要说出来的事情：谁会支持*难以阅读*的代码？我不会，但似乎这种代码仍然存在很多。当然，并不是有人故意去写难以阅读的代码；而是因为我们错误地将其他美德放在了阅读性之上，所以才会导致代码变得难以阅读。

[性能](https://bitfieldconsulting.com/golang/slower)是其中一种美德，有些情况下性能确实很重要。但并不像你想象的那么多，因为现代计算机的速度相当快，而且由于它们大多数工作都是为了服务于我们人类，所以通常可以承担一些时间成本。

因此，虽然可读性并不像正确性那么重要，但它比其他任何东西都更加重要。正如丘吉尔所说的勇气一样，可读性被正确地视为第一品质，因为它是保证其他所有品质的关键。

是什么造就了代码的可读性或不可读性？我认为“可读性”不是你可以向代码中添加的东西，而是当你消除了所有使其难以理解的因素时所剩下的东西。

例如，选择不当的变量名会让读者感到困惑。对于同一物品使用不同的名称，或者对于不同的物品使用相同的名称都会带来混淆。不必要的函数调用，仅仅是为了满足某些规则，比如“方法应该少于 10 行”，会打乱读者的阅读节奏。这就像阅读一篇文章时遇到一个不熟悉的单词：你是应该停下来查找它的含义，冒着失去思路的风险，还是努力推断上下文中的含义？

让任何代码更易读的最好方法，说起来有些奇怪，是从“阅读”它开始。但是我指的是一种特殊的阅读方式。我不是指匆忙地滚动浏览长长的代码页面，瞥一眼和略读一下以了解正在发生的事情。那种方式固然是一项有用的技能，但是只针对其他任务。

相反，我们需要以一种仔细、按顺序、有意识的方式阅读代码。我们需要从头开始，一直读到结尾。如果不清楚程序从哪里开始，那么这就是我们需要解决的第一件事。

随着我们跟随程序的执行线索，我们需要仔细阅读每一行，然后尝试回答两个问题：

1. 它在说什么？
2. 它的意思是什么？
例如，考虑以下这行代码：

```plain
requests++
```
它所表达的意思很清楚：它将某个数字变量`requests`的值增加 1。不过不那么容易理解的是它所意味着的东西。`requests`变量中正在计数什么？为什么要进行递增？这有什么意义？`requests`变量的当前值是从哪里获取的？它当前的值是什么？该值将在何时何地进行检查？是否存在某个最大值？当达到最大值时会发生什么？等等。

这些问题可能都有完全正确的答案，但这不是关键。关键是，读者能否仅仅通过查看代码来回答它们？如果不能，我们能做什么来提供答案或使答案更容易找到？通过将自己的代码阅读为新手一样，我们会以新的视角看待它，并发现需要更多认知努力才能跟随的部分。


以这种仔细、分析的方式阅读代码是[真正学习](https://bitfieldconsulting.com/golang/how) 编写代码的最佳途径之一。

## Idiomatic(惯用的)

我不认为这是完全正确的词：我更喜欢“传统”的说法，但这与 *CRISP* 的反向缩略语不符。不过，当人们说“某某是惯用的（idiomatic）”时，他们真正的意思是“这是常规的做法。”。

常规做法是有用的：例如，有很多可能的方式来布置汽车的驾驶控制装置，我们习惯的那种方式并没有明显的最优解。它只是我们习惯的方式。没有法律阻止汽车制造商以不同的方式安排踏板，但实际上他们并没有这样做。即使存在某些*人体工程学*上的好处，这也不值得给用户带来的*认知*成本。

类似地，我认为使用传统的*命名*来表示事物非常有价值：在 HTTP 处理程序中，请求指针总是称为`r`，响应写入器称为`w`。如果存在普遍惯例，遵循它是值得的。你也可以有本地和个人的惯例。在我的代码中，任意的`bytes.Buffer`总是命名为`buf`，我的测试中比较的值总是命名为`want`和`got`，等等。

`err`是一个很好的普遍惯例例子：Go 程序员总是使用这个名称来表示任意错误值。虽然我们通常不会在同一个函数中重复使用变量名，但我们会在整个函数中重复使用`err`以表示可能出现的所有不同错误。试图通过创建变体名称如`err2`、`err3`等来避免这种情况是错误的。

为什么呢？因为这会让读者付出更少的认知努力。如果你看到`err`，你的思维会轻松地理解它。如果你看到其他名称，你的思维就必须停下来解决这个谜题。我称这些微小的障碍为*认知微攻击*。每个微攻击可能是如此微小，以至于其单独的效果是可以忽略不计的，但是它们很快堆积起来，如果它们足够多，就会带来一堆麻烦。

在编写每行代码时，你应该思考：“理解这需要多少努力？我能否通过某种方式减少这个努力？”优秀软件工程的秘密在于把许多小事做好。选择正确的名称，以逻辑的方式组织代码，每行代码只表达一个思想：所有这些都有助于使你的代码可读性更强、更愉悦地使用。 

如何学习什么是惯用和传统的？通过以与阅读我们自己代码相同的仔细和有意识的方式阅读其他人的程序。如果你从未阅读过小说，就不能写出好的小说，程序也是如此。（但是阅读一两本精心选择的[编程书籍](https://bitfieldconsulting.com/books)也是个好主意，尽管这不太重要。）

在野外找到的代码质量参差不齐，你应该尽可能广泛地取样。阅读坏代码和好代码同样有用，尽管原因不同。即使在好程序中也会发现错误，当你发现错误时，你会学到一些东西。但最有用的是学习*每个人都在做什么*。

## Simple(简单的)

啊哈，[简单](https://bitfieldconsulting.com/golang/tao-of-go)。还有比这更棘手和麻烦的概念吗？每个人都认为一看就知道简单，但奇怪的是，在实践中，没有两个人能够就“简单”达成一致意见。

正如 Rich Hickey 指出的那样，[简单并不等同于易用](https://www.youtube.com/watch?v=LKtk3HCgTa8)。“易用”是熟悉、低努力、我们毫不费力地拿起的东西。这通常会导致“复杂”，因此从“易用”到“简单”需要付出很多努力和思考。

简单的一个方面是*直接性*：它做了就像它想表达的一样。它没有奇怪和意外的副作用，也没有混淆几个不相关的事情。直接性与简短相反：不要写一个短而复杂的函数，而是写三个简单但相似的函数。


这就是人们发现很难编写简单代码的原因之一：我们都害怕重复自己。*不要重复自己* （DRY）原则已经根深蒂固，我们甚至将其用作动词：“我们需要让这个函数更干净”（正如 Calvin 提醒我们的那样，[将名词动词化会使语言变得奇怪](https://www.gocomics.com/calvinandhobbes/1993/01/25)）。

但是，重复本身并没有什么问题。我再说一遍，重复本身并没有什么问题：我们反复执行的任务可能非常重要。如果我们发现自己创建新的抽象只是为了避免重复，而没有任何其他目的，那么我们就出了问题。我们正在使程序变得更加复杂，而不是更加简单。

这引出了简洁的另一个方面：*节俭性*。用少量的东西完成大量的工作。一个只做一件事的包比做十件事的包更简单。函数越少，调用栈越浅，程序就越简单。这可能会导致一些*长*函数，但这没关系。我们要尊重 Uncle Bob，但函数应该尽可能长。仅仅为了缩短函数的长度而添加不必要的复杂性，这是一种盲目应用规则打败常识的好例子。

正如 Alan Perlis 观察到的那样，简单并不是复杂性的先导，而是随后出现的。换句话说，不要试图*编写*简单的程序：先编写程序，然后使其简单化。阅读代码并询问它的含义，然后问问自己是否可以找到一种更简单的编写相同内容的方法。

你还可以在*自然性*中找到简洁。任何一种语言都有自己的[道](https://bitfieldconsulting.com/golang/tao-of-go)，自己的自然形式和结构，与它们配合工作通常比反对它们更好。例如，你可以尝试将 Go 当作 Java、Python 或 Clojure 来使用，这些语言都没有问题，但在 Go 中编写 Go 程序更简单，结果更好。

## Performant(高效的)

你可能认为我把这个方面放在最后很奇怪。几乎所有你听到或读到的编程内容都与性能有关，是吗？是的。但我认为这不一定是因为性能很*重要*，而是因为它很容易谈论。可以衡量的东西会被最大化，而性能很容易衡量：你可以使用秒表。要量化简洁或甚至正确性等方面的东西要困难得多。

但是，如果代码*不正确*，有谁在乎它运行得有多快呢？换句话说，如果不需要正确性，我们可以使给定的函数任意高效。同样地，如果不简单，我们将浪费更多的*程序员*时间来理解它，而我们节省的 CPU 时间永远不可能比这个多。而且每小时程序员的运行成本比任何 CPU 都要高。优化限制资源难道不是有意义的吗？

正如我之前所说，对于绝大多数程序来说，[性能并不重要](https://bitfieldconsulting.com/golang/slower)。当它确实很重要时，最好的解决方案通常不是使你的代码更难读。这里适用于“慢就是快，快就是顺畅”的说法。如果你为了节省几微秒的时间，使你的程序陷入了无望的复杂性，那么很好，但这将是任何人所做的最后一个优化。试图加速复杂代码通常是适得其反的，因为如果你无法理解它，就无法优化它。另一方面，当你必须加速一个简单的程序时，很容易做到。

然而，我们应该意识到我们所做选择的性能影响，即使我们不会因此而被推动。如果我们不需要相对快速地完成这个任务，我们就不会将它交给计算机。另一种思考方式是，一个高效的程序可以在给定的时间内_完成_更多的工作，即使实际花费的时间是无关紧要的。

公平地说，即使是一个低效的程序也会运行得相当快：正如 Richard Feynman 观察到的那样，计算机内部非常愚蠢，但运行速度非常快。这并不意味着我们可以承受浪费时间，因为计算乘以时间等于能量，而我们已经以一个荒谬得不可持续的速度加热了地球。如果我们因为选择了一个笨拙的数据结构而最终排放出额外的数百万吨碳，那将是一件令人遗憾的事情。

当你编程时，牢记“机械同理心”的概念是有帮助的。这意味着你对计算机的基本工作原理有一定的了解，并且注意不要滥用它或阻碍它的工作。例如，如果你不知道*内存*是什么，你怎么能编写高效的程序呢？

我经常看到的代码是毫不在意地将整个数据文件读入内存，然后只处理其中的几个字节。我是在一台只有 1K 内存的机器上学习编程的，大约只能存放这篇文章的一千分之一。

现在，几年后，我正在一台具有约一千六百万个字的内存的机器上写作，而这些字本身的大小增加了八倍，那么这是否意味着我们现在可以放松并使用尽可能多的内存呢？不，因为任务的规模也变得更大了。

首先，你在系统中传输的数据越多，程序执行所需的时间就越长。其次，无论你的 Kubernetes 集群有多大，它仍然由具有固定、有限内存的物理机器组成，每个容器使用的内存不能超过单个节点的总内存。所以要注意你的每一个字节，这样千兆字节就会自动得到保障。

## 要点

- 没有银弹，没有万能的软件工程方法论，一些所谓的“干货”往往只是忽悠。
- 始终要定义术语以避免不必要的误解：像“简洁”、“简单”和“可读性”这样的词听起来很不错，但是，就像“自由”、“正义”或“平等”一样，它们对于不同的人意味着不同的东西。
- 不要轻信像“整洁代码”、“函数应该只做一件事”或“不要重复自己”这样的简洁口号：如果你能够准确地确定它们的含义，它们往往会被证明是错误的。
- 正确性比其他一切都更为重要，甚至比可读性更为重要；这一点是如此显而易见，以至于我们需要不断提醒自己。
- 测试只能证明代码符合测试人员的预期，很多时候甚至无法证明。
- 一个有错误或不充分的测试要比没有测试更糟糕，因为它会让人误以为你已经进行了测试。
- 所有软件都有 Bug，分为已经发现和尚未发现的两类。
- “可读性”是所有人都赞成的，但是对于什么是“可读”的意见很不一致。
- 编写简洁易读的程序需要不断发现和消除代码中的阅读障碍。
- 你可能知道代码在说什么，但你能告诉它的实际含义吗？
- 原创性在其他领域很有趣，但在编程中除外。
- 不断寻找并消除代码中微不足道的认知问题，直到你找不到为止，然后再更加努力地寻找。
- 要成为一名有能力的程序员，需要多写代码；要成为一名大师，需要多读代码。
- 简单并不等同于容易。
- 当重复代码有助于避免不必要的抽象并提高代码清晰度时，就请毫不犹豫地重复代码。
- 写程序要先实现功能，然后再考虑简化。
- 遵循 Go 语言的特性和风格可以更好地编写高质量的代码。
- 通常情况下，优化是不必要的，甚至是有害的：不要浪费时间来节省时间。
- 你的程序很可能已经足够快了：虽然计算机内部很愚蠢，但它跑得飞快。
- 时间是无限的，但空间是有限的，应该在速度和内存占用之间做出权衡。
- 提高程序速度很容易（在多台计算机上运行），但提高内存占用却很难（一个盒子里只能放下那么多内存条）。
- 精益求精，注重每个字节细节，而大量数据处理的问题将迎刃而解。
- 如果无法做到简洁易读，那就做到“CRISP”（正确的、可读的、惯用的、简单的、高效的）。

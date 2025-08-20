---
title: 【Python编程：从入门到实践】-基础知识-读书笔记
date: 2025-08-20 17:21:06
tags:
  - python
  - note
---
# 【Python编程：从入门到实践】-基础知识-读书笔记

> 根据 [遗忘曲线](https://baike.baidu.com/item/%E9%81%97%E5%BF%98%E6%9B%B2%E7%BA%BF/7278665?fr=aladdin)：如果没有记录和回顾，6天后便会忘记75%的内容
>
>   读书笔记正是帮助你记录和回顾的工具，不必拘泥于形式，其核心是：记录、翻看、思考

| **书名** | Python编程：从入门到实践（第3版） |
| --- | --- |
| **原版书名** | Python Crash Course, 3rd Edition |
| **作者** | <font style="color:rgb(26, 28, 30);background-color:rgb(252, 252, 252);">[美] 埃里克·马瑟斯 (Eric Matthes)</font> |
| **译者** | 袁国忠 |
| **状态** | <font style="background:#DBF1B7;color:#2A4200">已读完</font>  |
| **简介** | **Python入门经典，用三大项目实战，带你从零快速上手。** |


### **第一章：起步**
#### **核心概要**
本章是进入Python编程世界的“启动仪式”。它并不涉及复杂的编程理论，而是专注于**搭建一个稳定、可靠的编程环境**。作者认为，一个顺畅的开端至关重要，如果在环境配置上遇到过多挫折，很容易打击学习热情。因此，本章的目标非常明确：确保我们能够在自己的计算机上成功安装Python、配置好编辑器，并顺利运行第一个简单的程序——`hello_world.py`。

---

#### **关键知识点与学习心得**
1. **编程环境的重要性**
    - **知识点**：本章强调，不同的操作系统（Windows, macOS, Linux）在Python的安装和使用上存在细微差别。一个配置正确的环境是后续所有学习和实践的基础。
    - **心得**：这部分打消了我对环境配置的恐惧。作者没有直接堆砌命令，而是先解释“为什么要做”，再告诉我们“怎么做”。这让我明白，磨刀不误砍柴工，花时间把环境弄好，是为了以后能更专注于编程逻辑本身。
2. **Python版本选择**
    - **知识点**：书中推荐使用Python 3.9或更高的版本。作者明确指出，虽然旧系统可能预装了Python 2，但现在应该始终使用Python 3。
    - **心得**：版本选择是初学者常遇到的第一个坑。书里给出了明确的指引，避免了我在版本兼容性问题上走弯路。
3. **文本编辑器VS Code简介**
    - **知识点**：本章推荐使用VS Code（Visual Studio Code）作为代码编辑器。它功能强大、免费，并且对初学者友好。书中提到了它的一些优点，如语法高亮、跨平台支持，并建议安装Python扩展来增强功能。
    - **心得**：之前我以为随便一个记事本就能写代码。读完这部分才明白，一个好的编辑器能极大地提升效率和代码可读性。语法高亮功能尤其有用，它能让我一眼就看出哪里是函数，哪里是字符串，有效减少拼写错误。
4. **各操作系统下的安装流程**
    - **知识点**：书中为Windows、macOS和Linux三大主流操作系统提供了详细、分步骤的安装指南。
        * **Windows**：通常需要从Python官网下载安装包，并在安装时务必勾选 "Add Python to PATH" 选项。
        * **macOS**：同样建议从官网安装，并注意使用 `python3` 命令来区分系统可能自带的旧版本。
        * **Linux**：大多自带Python，但需要检查版本，并可能需要通过包管理器（如apt）安装最新版。
    - **心得**：这部分内容非常实用，就像一份手把手的操作手册。特别是 "Add Python to PATH" 这个提醒，解决了我在网上看其他教程时遇到的“命令无法识别”的经典问题。
5. **第一个程序：**`hello_world.py`
    - **知识点**：通过编写并运行一个只包含一行代码 `print("Hello Python world!")` 的程序，来验证整个环境是否配置成功。这个简单的程序如果能跑通，就意味着更复杂的程序也能正常运行。
    - **心得**：这不仅仅是打印一句话那么简单，它更像是一个“点火测试”。当我看到终端成功输出 "Hello Python world!" 时，心中充满了成就感。这个正向反馈极大地鼓舞了我继续学习的信心。
6. **从终端运行程序**
    - **知识点**：除了在VS Code中直接运行，本章还教学了如何在终端（或命令行）中通过 `cd` 命令切换目录，并使用 `python hello_world.py` (或 `python3`) 的方式来执行程序。
    - **心得**：这让我了解到运行Python程序的两种主要方式。虽然在开发初期，编辑器的一键运行更方便，但学会使用终端是成为一名合格程序员的必经之路，尤其是在未来部署和管理项目时。
7. **排除安装问题**
    - **知识点**：作者非常贴心地提供了一个故障排除清单，比如检查拼写、重新来过、寻求他人或在线社区的帮助等。
    - **心得**：这部分让我感到很温暖。作者预见到了初学者可能遇到的困难，并提供了解决问题的思路，而不是简单地假设一切顺利。这让我明白，遇到问题是正常的，关键在于如何解决它。

---

#### **总结与反思**
第一章虽然没有教授高深的编程技巧，但它的价值在于**建立自信和扫清障碍**。它像一位经验丰富的向导，带领我走出了学习编程的第一里路。通过亲手搭建环境并成功运行第一个程序，我不仅验证了工具的可用性，更重要的是，我在心理上完成了从“旁观者”到“实践者”的角色转变。

本章的学习为我后续的所有章节打下了坚实的基础。现在，我的“实验室”已经准备就绪，可以开始进行真正的“化学实验”了。

### **第二章：变量和简单的数据类型**
#### **核心概要**
本章是Python编程的正式起点，深入讲解了程序处理信息的基本方式。核心内容围绕**变量（Variables）****的创建与使用，以及两种最基础的****数据类型（Data Types）**：**字符串（String）****和****数字（Number）**。本章详细阐述了如何命名变量、避免常见错误、操作文本数据、执行数学运算，并引入了注释和Python的设计哲学“Python之禅”，旨在为编写清晰、规范的代码打下坚实基础。

---

#### **2.1 & 2.2 变量（Variables）**
**1. 变量的定义与使用**

+ **核心概念**：变量是赋予某个值的“标签”。通过变量，我们可以存储和引用程序中的信息。
+ **代码示例**：

```python
message = "Hello Python world!"
print(message)

message = "Hello Python Crash Course world!"
print(message)
```

+ **关键点**：可以随时修改变量的值，Python会记录最新的赋值。

**2. 变量的命名规则与规范**

+ **硬性规则（必须遵守，否则报错）**：
    - 变量名只能包含字母、数字和下划线 (`_`)。
    - 不能以数字开头。
    - 不能包含空格，应使用下划线分隔单词。
    - 不能使用Python的**关键字**（如 `print`, `if`, `for`）和函数名作为变量名。
+ **编程规范（建议遵守，提高代码可读性）**：
    - 变量名应使用小写，单词间用下划线连接（**蛇形命名法**），如 `greeting_message`。
    - 变量名应简短且具有描述性，如 `name` 好于 `n`。
    - 慎用小写字母 `l` 和大写字母 `O`，因为它们容易和数字 `1` 和 `0` 混淆。

**3. 避免命名错误 (**`NameError`**)**

+ **知识点**：当使用一个未定义或拼写错误的变量时，Python会引发 `NameError`。
+ **调试技巧**：Python的错误追踪信息（traceback）非常有用。它会明确指出：
    1. 错误发生的文件名和行号。
    2. 出错的代码行。
    3. 具体的错误类型和描述，有时甚至会**智能地提示可能的正确拼写**。
    - **示例**：如果将 `message` 错拼为 `mesage`，错误提示会是：`NameError: name 'mesage' is not defined. Did you mean: 'message'?`
+ **心得**：学会阅读traceback是编程中最重要的技能之一。它不是在“骂你”，而是在“帮你”。

**4. 变量是标签（重要概念）**

+ **知识点**：书中特别强调，将变量理解为指向特定值的**标签**，比理解为装东西的**盒子**更准确。这个概念有助于未来理解更复杂的编程行为。

---

#### **2.3 字符串（String）**
字符串是一系列字符，用于表示文本。

**1. 定义字符串**

+ **语法**：可以使用单引号 (`'`) 或双引号 (`"`)。这种灵活性使得在字符串中可以方便地包含另一种引号。

```python
'I told my friend, "Python is my favorite language!"'
"The language 'Python' is named after Monty Python."
```

**2. 修改字符串大小写（方法 Method）**

+ **概念**：方法是Python可对数据执行的操作，通过 `变量名.方法名()` 的形式调用。
+ **常用方法**：
    - `.title()`：将每个单词的首字母改为大写。
    - `.upper()`：将整个字符串改为大写。
    - `.lower()`：将整个字符串改为小写。这在存储用户数据前进行统一格式化时非常有用。

**3. f-字符串 (f-strings)**

+ **功能**：一种强大且简洁的字符串格式化方法，用于在字符串中嵌入变量的值。
+ **语法**：在字符串的起始引号前加上字母 `f`，然后将变量名放在花括号 `{}` 中。

```python
first_name = "ada"
last_name = "lovelace"
full_name = f"{first_name} {last_name}"
print(f"Hello, {full_name.title()}!") # 输出: Hello, Ada Lovelace!
```

**4. 处理空白**

+ **添加空白**：
    - `\t`：添加制表符（一个Tab缩进）。
    - `\n`：添加换行符。
+ **删除空白**：这些方法**不改变原始字符串**，而是返回一个修改后的新字符串。
    - `.rstrip()`：删除字符串末尾的空白。
    - `.lstrip()`：删除字符串开头的空白。
    - `.strip()`：删除字符串两端的空白。
    - **重要**：要永久删除空白，必须将结果重新赋给原变量：`favorite_language = favorite_language.strip()`。

**5. 删除前缀/后缀**

+ `.removeprefix('prefix')`：如果字符串以前缀开头，则删除此前缀并返回新字符串。
+ `.removesuffix('suffix')`：同理，删除后缀。

**6. 避免语法错误 (**`SyntaxError`**)**

+ **常见原因**：字符串中的撇号与用于定义字符串的单引号冲突。
+ **解决方法**：如果字符串内包含单引号，则使用双引号来定义整个字符串。

---

#### **2.4 数字（Number）**
**1. 整数 (Integer)**

+ **运算**：支持加 (`+`)、减 (`-`)、乘 (`*`)、除 (`/`)、乘方 (`**`)。
+ **注意**：除法运算 (`/`) 的结果**始终是浮点数**，即使两个数能整除（如 `4 / 2` 结果是 `2.0`）。

**2. 浮点数 (Float)**

+ **定义**：任何带小数点的数。
+ **重要提醒**：浮点数运算的结果可能存在不确定的小数位数（如 `0.2 + 0.1` 得到 `0.30000000000000004`），这是由计算机内部存储数字的方式决定的，需要注意。

**3. 数字中的下划线**

+ **功能**：为了让长数字更易读，可以在数字中使用下划线。Python在处理时会自动忽略它们。

```python
universe_age = 14_000_000_000 
```

**4. 多重赋值**

+ **语法**：可以在一行代码中给多个变量赋值。

```python
x, y, z = 0, 1, 2
```

**5. 常量 (Constant)**

+ **约定**：Python没有内置的常量类型，但程序员有一个普遍的约定：**用全大写的变量名来表示常量**，意在提醒任何阅读代码的人这个值不应被修改。

```python
MAX_CONNECTIONS = 5000
```

---

#### **2.5 & 2.6 代码风格与哲学**
**1. 注释 (Comment)**

+ **语法**：以井号 (`#`) 开头，Python会忽略该行井号之后的所有内容。
+ **目的**：解释代码的逻辑和意图，提高代码的可读性。

**2. Python之禅 (The Zen of Python)**

+ **调用方式**：在Python终端中输入 `import this`。
+ **核心理念**：这是Python语言的设计哲学，指导我们编写出优雅、简洁、易读的代码。
    - **优美优于丑陋** (Beautiful is better than ugly.)
    - **简单优于复杂** (Simple is better than complex.)
    - **可读性很重要** (Readability counts.)



#### **本章总结**
第二章是Python编程的基石。通过对变量、字符串和数字的详细学习，我掌握了处理基本数据的核心技能。本章不仅教会了我具体的语法和函数，如f-字符串和.strip()方法，更重要的是，它从一开始就强调了**代码的可读性、规范性和调试思维**。理解变量是“标签”、注意浮点数的精度问题、遵循命名规范、善用错误提示，这些细节将对我未来的编程之路产生深远的影响。“Python之禅”更是点睛之笔，让我认识到编程不仅仅是实现功能，更是一种追求简洁与优雅的艺术。

### **第三章：列表简介**
#### **核心概要**
本章我们开始学习如何组织和管理**一组数据**。在此之前，我们使用的变量一次只能存储一个值。本章引入了Python中一个极其强大且常用的数据结构——**列表（List）**。列表可以让你在一个变量中存储任意数量的信息项，无论是几个还是数百万个。本章的核心目标是让你掌握列表的**创建、访问、修改、添加和删除**元素这五大基本操作，并学习如何对列表进行**组织和管理**，同时注意避免一种非常常见的错误——索引错误。

---

#### **3.1 列表是什么（What is a List?）**
**1. 列表的定义**

+ **概念**：列表是由一系列按特定顺序排列的元素组成的集合。你可以把任何东西放入列表中，元素之间可以没有任何关系。
+ **语法**：用方括号 `[]` 来表示列表，并用逗号 `,` 来分隔其中的元素。

```python
bicycles = ['trek', 'cannondale', 'redline', 'specialized']
print(bicycles)
# 输出: ['trek', 'cannondale', 'redline', 'specialized']
```

**2. 访问列表元素**

+ **核心概念**：列表中的元素是有序的，每个元素都有一个对应的位置或**索引（Index）**。我们可以通过索引来精确地获取列表中的任何一个元素。
+ **语法**：在列表名后面跟一个方括号，里面写上元素的索引。

```python
print(bicycles[0]) # 输出: trek
```

+ **重要细节**：
    - **索引从0开始**：这是编程中一个非常普遍且重要的规则。列表的第一个元素的索引是 `0`，第二个是 `1`，以此类推。初学者很容易在这里犯“差一错误”（off-by-one error）。
    - **使用列表中的值**：访问到的元素可以像普通变量一样使用。例如，可以对它调用字符串方法：

```python
print(bicycles[0].title()) # 输出: Trek
```

    - **f-字符串中的应用**：可以方便地在f-字符串中使用列表元素来创建动态消息。

```python
message = f"My first bicycle was a {bicycles[0].title()}."
print(message) # 输出: My first bicycle was a Trek.
```

**3. 使用负数索引**

+ **功能**：Python提供了一种特殊的语法来访问列表末尾的元素。
+ **语法**：
    - 索引 `-1` 表示访问**最后一个**元素。
    - 索引 `-2` 表示访问**倒数第二个**元素，以此类推。
+ **优势**：当你不知道列表具体有多长时，这个功能非常有用。

```python
motorcycles = ['honda', 'yamaha', 'suzuki']
print(motorcycles[-1]) # 输出: suzuki
```

---

#### **3.2 修改、添加和删除元素**
列表通常是**动态的**，意味着我们可以在程序运行过程中随时改变其内容。

**1. 修改列表元素**

+ **语法**：指定列表名和要修改元素的索引，然后赋予新值。

```python
motorcycles = ['honda', 'yamaha', 'suzuki']
motorcycles[0] = 'ducati'
print(motorcycles) # 输出: ['ducati', 'yamaha', 'suzuki']
```

**2. 在列表中添加元素**

+ **在列表末尾添加元素 (**`.append()`**)**：这是最简单、最常用的添加方式。

```python
motorcycles.append('ducati')
print(motorcycles) # 输出: ['honda', 'yamaha', 'suzuki', 'ducati']
```

    - **应用场景**：可以从一个空列表开始，通过循环和 `.append()` 动态地构建整个列表。
+ **在列表中间插入元素 (**`.insert()`**)**：如果你想在列表的任意位置添加新元素。
    - **语法**：`.insert(index, element)`，需要提供新元素的索引和值。
    - **注意**：插入位置之后的所有元素都会自动向右移动一个位置。

```python
motorcycles.insert(0, 'ducati')
print(motorcycles) # 输出: ['ducati', 'honda', 'yamaha', 'suzuki']
```

**3. 从列表中删除元素**

+ **使用 **`del`** 语句删除（知道索引）**：如果你知道要删除元素的索引，可以使用 `del`。
    - **注意**：使用 `del` 删除后，这个元素就**彻底消失了**，你无法再访问它。

```python
del motorcycles[0]
print(motorcycles) # 输出: ['yamaha', 'suzuki']
```

+ **使用 **`.pop()`** 方法删除（“弹出”元素）**：`.pop()` 可以删除列表中的一个元素，并且让你能够**继续使用**这个被删除的元素的值。
    - **默认行为**：如果不提供索引，`.pop()` 会删除并返回列表**末尾**的元素（像从栈顶弹出一个元素）。

```python
popped_motorcycle = motorcycles.pop()
print(motorcycles)       # 输出: ['honda', 'yamaha']
print(popped_motorcycle) # 输出: suzuki
```

    - **指定索引**：你也可以提供索引来删除列表中任意位置的元素：`motorcycles.pop(0)`。
    - **如何选择**：当你想删除一个元素并且不再使用它时，用 `del`；如果你想在删除后仍然需要使用那个元素的值，就用 `.pop()`。
+ **根据值删除元素 (**`.remove()`**)**：如果你不知道元素的索引，只知道它的值。
    - **语法**：`.remove(value)`

```python
motorcycles.remove('ducati')
```

    - **重要细节**：`.remove()` 方法**只删除第一个**匹配到的值。如果列表中有多个相同的值，你需要使用循环来确保它们都被删除（这将在后续章节介绍）。

---

#### **3.3 组织列表**
**1. 永久性排序 (**`.sort()`**)**

+ **功能**：直接修改列表元素的排列顺序。
+ **默认行为**：按字母顺序排序。

```python
cars = ['bmw', 'audi', 'toyota', 'subaru']
cars.sort()
print(cars) # 输出: ['audi', 'bmw', 'subaru', 'toyota']
```

+ **反向排序**：传递参数 `reverse=True`。

```python
cars.sort(reverse=True)
```

+ **注意**：`.sort()` 的修改是**永久性的**，列表的原始顺序会丢失。

**2. 临时性排序 (**`sorted()`**)**

+ **功能**：返回一个排序后的列表**副本**，而不改变原始列表的顺序。

```python
cars = ['bmw', 'audi', 'toyota', 'subaru']
print(sorted(cars)) # 输出: ['audi', 'bmw', 'subaru', 'toyota']
print(cars)         # 输出: ['bmw', 'audi', 'toyota', 'subaru']
```

+ **反向排序**：同样可以传递 `reverse=True` 参数。

**3. 反转列表顺序 (**`.reverse()`**)**

+ **功能**：反转列表元素的排列顺序，不是按字母顺序，而是简单地将列表倒置。
+ **注意**：`.reverse()` 的修改也是**永久性的**，但可以再次调用它来恢复原始顺序。

**4. 确定列表长度 (**`len()`**)**

+ **功能**：获取列表中元素的数量。

```python
len(cars) # 返回 4
```

---

#### **3.4 使用列表时避免索引错误 (**`IndexError`**)**
+ **错误原因**：当你试图访问一个不存在的索引时，Python会引发 `IndexError`。最常见的原因是忘记了索引从0开始，或者错误地估计了列表的长度。
+ **示例**：对于 `motorcycles = ['honda', 'yamaha', 'suzuki']`，`motorcycles[3]` 就会引发 `IndexError`，因为有效的索引是0, 1, 2。
+ **解决方法**：
    - 时刻记住索引是从0开始的。
    - 使用 `-1` 来访问最后一个元素，这总能正确工作，除非列表为空。
    - 如果列表为空，任何索引访问都会出错。
+ **调试技巧**：如果遇到 `IndexError` 却找不到原因，可以尝试打印列表本身或其长度 (`len()`)，这有助于你了解列表的当前状态。

#### **本章总结**
第三章为我们打开了数据集合的大门。通过学习列表，我掌握了存储和操作一组数据的基本方法。关键操作包括：使用从0开始的索引访问元素、修改、添加 (.append(), .insert()) 和删除 (del, .pop(), .remove()) 元素。我还学会了如何通过 .sort()（永久）和 sorted()（临时）来组织列表，以及使用 .reverse() 和 len()。最重要的是，我了解了 IndexError 这种常见错误的原因和避免方法。这些都是使用Python处理数据的核心技能，为后续学习更复杂的循环和数据结构奠定了坚实的基础。

### **第四章：操作列表**
#### **核心概要**
在第三章，我们学会了如何处理列表中的单个元素。但如果列表包含成百上千个元素，逐个处理显然是不现实的。本章的核心就是解决这个问题，引入了**循环（Loop）****的概念，特别是**`for`**循环。通过循环，我们可以****遍历（Traverse）****整个列表，对每个元素执行相同的操作。本章还将深入讲解****代码缩进**的重要性，并介绍如何创建**数值列表**、使用列表的**一部分（切片）**，以及学习一种不可变的列表——**元组（Tuple）**。最后，本章提供了关于代码格式的指导，帮助我们写出更专业、更易读的代码。

---

#### **4.1 遍历整个列表**
**1. **`for`** 循环**

+ **目的**：当你需要对列表中的每个元素都执行相同的操作时，使用`for`循环可以自动化这个过程。
+ **语法**：

```python
magicians = ['alice', 'david', 'carolina']
for magician in magicians:
    print(magician)
```

+ **语法解析**：
    1. `for magician in magicians:`：这行代码告诉Python从列表`magicians`中取出一个元素，并将其存储在临时变量`magician`中。
    2. `:`：冒号告诉Python，接下来的一行是循环体的开始。
    3. **循环体**：冒号后面的所有**缩进**的代码行都是循环的一部分。
+ **执行过程**：Python会重复执行循环体，每次循环都将列表中的下一个元素赋给临时变量`magician`，直到列表中的所有元素都被处理完毕。
+ **命名规范**：临时变量的命名最好使用有意义的单数形式，而列表名使用复数形式，如 `for cat in cats:` 或 `for dog in dogs:`。这让代码的意图一目了然。

**2. 在**`for`**循环中执行更多操作**

+ **知识点**：循环体内可以包含任意多行代码，它们都会对列表中的每个元素执行一次。

```python
for magician in magicians:
    print(f"{magician.title()}, that was a great trick!")
    print(f"I can't wait to see your next trick, {magician.title()}.\n")
```

+ **注意**：所有希望在循环内重复执行的代码都**必须缩进**。

**3. 在**`for`**循环结束后执行操作**

+ **知识点**：没有缩进的代码行位于循环之外，它们只会在整个循环结束后执行一次。

```python
print("Thank you, everyone. That was a great magic show!")
```

+ **应用**：这常用于在处理完所有列表项后进行总结或执行后续步骤。

---

#### **4.2 避免缩进错误**
**1. 缩进的重要性**

+ **核心概念**：Python**通过缩进来判断代码行之间的关系**。缩进决定了哪些代码属于哪个代码块（如`for`循环体）。这是Python语法的一个强制性要求，也是其代码清晰易读的关键原因之一。
+ **规范**：PEP 8 编码规范建议使用**4个空格**作为一级缩进。

**2. 常见的缩进错误**

+ **忘记缩进 (**`IndentationError`**)**：本应缩进的代码（如`for`循环后的第一行）没有缩进，Python会报告`IndentationError: expected an indented block`。
+ **不必要的缩进 (**`IndentationError`**)**：不属于任何代码块的代码行被错误地缩进，Python会报告`IndentationError: unexpected indent`。
+ **忘记缩进额外的代码行（逻辑错误）**：这是最隐蔽的错误。代码在语法上没有问题，可以运行，但结果不符合预期。比如，循环体中本应有两行缩进的代码，但你只缩进了第一行，导致第二行代码只在循环结束后执行了一次。
+ **循环后不必要的缩进（逻辑错误）**：本应在循环结束后执行的代码被错误地缩进，导致它在每次循环中都被执行。
+ **遗漏冒号 (**`SyntaxError`**)**：`for`语句（以及后续将学到的`if`, `def`等）末尾的冒号非常重要，如果遗漏，会导致`SyntaxError: expected ':'`。

---

#### **4.3 创建数值列表**
**1. 使用 **`range()`** 函数**

+ **功能**：轻松生成一系列连续的整数。
+ **语法**：`range(start, stop)`。它会生成从`start`开始，到`stop - 1`结束的数。这是一个“**包前不包后**”的规则。

```python
for value in range(1, 5):
    print(value)  # 输出 1, 2, 3, 4
```

**2. 使用 **`range()`** 创建数字列表**

+ **功能**：可以将`range()`的结果直接转换为列表。
+ **语法**：使用`list()`函数。

```python
numbers = list(range(1, 6))
print(numbers)  # 输出: [1, 2, 3, 4, 5]
```

+ **指定步长**：`range()`可以接受第三个参数，表示步长。

```python
even_numbers = list(range(2, 11, 2))
print(even_numbers) # 输出: [2, 4, 6, 8, 10]
```

**3. 对数字列表执行简单统计**

+ **内置函数**：Python提供了一些简单的函数来处理数字列表。
    - `min()`：找出最小值。
    - `max()`：找出最大值。
    - `sum()`：计算总和。

**4. 列表推导式 (List Comprehension)**

+ **功能**：一种将`for`循环和创建新元素的代码合并成一行的简洁语法。
+ **语法**：`[expression for item in iterable]`
+ **示例**：创建一个包含前10个整数平方的列表。
    - **常规方法**：

```python
squares = []
for value in range(1, 11):
    square = value ** 2
    squares.append(square)
```

    - **列表推导式**：

```python
squares = [value**2 for value in range(1, 11)]
```

+ **心得**：列表推导式非常强大和高效，是Python的一大特色。虽然初学时可能觉得有些复杂，但一旦掌握，就能写出非常简洁的代码。

---

#### **4.4 使用列表的一部分（切片 Slice）**
**1. 创建切片**

+ **功能**：处理列表中的一部分元素。
+ **语法**：`list_name[start:stop]`。同样是“**包前不包后**”。

```python
players = ['charles', 'martina', 'michael', 'florence', 'eli']
print(players[0:3]) # 输出: ['charles', 'martina', 'michael']
```

+ **语法简写**：
    - `players[:4]`：如果省略起始索引，切片从列表开头开始。
    - `players[2:]`：如果省略终止索引，切片到列表末尾结束。
    - `players[-3:]`：可以使用负数索引，表示从倒数第三个元素切到末尾。

**2. 遍历切片**

+ **功能**：可以在`for`循环中使用切片来遍历列表的一部分。

```python
for player in players[:3]:
    print(player.title())
```

**3. 复制列表**

+ **错误的方式**：直接赋值 `friend_foods = my_foods` 并没有创建新列表，而是让两个变量指向了**同一个列表**。修改其中一个会影响另一个。
+ **正确的方式**：使用不带任何索引的切片 `[:]` 来创建整个列表的**副本**。

```python
my_foods = ['pizza', 'falafel', 'carrot cake']
friend_foods = my_foods[:] # 创建副本
```

---

#### **4.5 元组（Tuple）**
**1. 元组的定义**

+ **核心概念**：元组是一种**不可变**的列表。一旦创建，就不能修改其中的元素。
+ **语法**：使用圆括号 `()` 来定义。

```python
dimensions = (200, 50)
```

+ **注意**：尝试修改元组中的元素会导致`TypeError`。

**2. 遍历元组**

+ **方法**：和列表一样，可以使用`for`循环来遍历元组中的所有值。

**3. 修改元组变量**

+ **知识点**：虽然不能修改元组的**元素**，但可以给存储元组的**变量**重新赋值一个新的元组。

```python
dimensions = (200, 50)
dimensions = (400, 100) # 这是合法的
```

**4. 适用场景**

+ 当你需要存储一组不应被修改的值时，使用元组比列表更安全。

---

#### **4.6 设置代码格式**
+ **PEP 8**：Python官方的代码风格指南。
+ **核心建议**：
    - **缩进**：使用4个空格。
    - **行长**：建议每行代码不超过80个字符，注释不超过72个字符。
    - **空行**：用空行来组织代码，分隔不同的逻辑部分，但不要滥用。

---

#### **本章总结**
第四章是质的飞跃。`for`循环让我具备了处理大规模数据的能力，不再局限于手动操作单个元素。对**缩进**的深入理解让我明白了Python代码结构的本质。**列表推导式**展示了Python的简洁之美。**切片**操作提供了处理列表子集的灵活性，尤其是`[:]`在复制列表时的重要作用。最后，**元组**的引入让我知道了在需要保护数据不被修改时，还有列表之外的选择。本章不仅是关于“如何做”，更是关于“如何做得更好、更规范”，为编写更复杂、更专业的程序打下了坚实的基础。



### **第五章：if语句**
#### **核心概要**
本章我们开始学习如何让程序具备**决策能力**。在此之前，我们编写的程序都是按顺序执行每一行代码。本章引入了`if`语句，它允许程序**检查当前的某个条件是否满足，并根据检查结果执行相应的操作**。这使得程序能够响应不同的情况，变得更加智能和灵活。本章的核心内容包括**条件测试**的各种形式、简单的**`if`语句**、`if-else`**结构**、`if-elif-else`**链**，以及如何将这些逻辑判断应用到列表中。

---

#### **5.1 & 5.2 条件测试（Conditional Tests）**
条件测试是每一条`if`语句的核心，其结果要么是`True`（真），要么是`False`（假）。

**1. 检查是否相等 (**`==`**)**

+ **核心概念**：这是最基本的条件测试，用于判断两个值是否相等。
+ **语法**：使用**两个等号 **`==`。

```python
car = 'bmw'
print(car == 'bmw') # 输出: True

car = 'audi'
print(car == 'bmw') # 输出: False
```

+ **重要区别**：
    - **一个等号 (**`=`**)** 是**赋值**，意为“将右边的值赋给左边的变量”。
    - **两个等号 (**`==`**)** 是**比较**，意为“左边的值是否等于右边的值？”

**2. 检查是否相等时忽略大小写**

+ **问题**：直接使用 `==` 进行比较时，是**区分大小写**的。`'Audi'` 和 `'audi'` 会被认为是不同的。
+ **解决方法**：在比较前，先将变量的值统一转换为小写（或大写），通常使用 `.lower()` 方法。

```python
car = 'Audi'
print(car.lower() == 'audi') # 输出: True
```

+ **注意**：`.lower()` 方法**不改变**原始变量 `car` 的值，只是返回一个新的小写版本的字符串用于比较。

**3. 检查是否不相等 (**`!=`**)**

+ **语法**：使用感叹号和等号 `!=`。

```python
requested_topping = 'mushrooms'
if requested_topping != 'anchovies':
    print("Hold the anchovies!")
```

**4. 数值比较**

+ **操作符**：除了相等和不相等，还支持标准的数学比较。

```python
age = 18
print(age == 18) # True
print(age < 21)  # True
print(age >= 18) # True
```

    - `>` (大于)
    - `<` (小于)
    - `>=` (大于等于)
    - `<=` (小于等于)

**5. 检查多个条件**

+ **使用 **`and`：当需要**所有条件都为`True`**时，整个表达式才为`True`。

```python
age_0 = 22
age_1 = 18
print(age_0 >= 21 and age_1 >= 21) # False, 因为 age_1 不满足条件
```

+ **使用 **`or`：当**至少有一个条件为`True`**时，整个表达式就为`True`。

```python
age_0 = 22
age_1 = 18
print(age_0 >= 21 or age_1 >= 21) # True, 因为 age_0 满足条件
```

**6. 检查特定值是否在列表中 (**`in`**)**

+ **功能**：判断一个值是否存在于一个列表中。
+ **语法**：使用关键字 `in`。

```python
requested_toppings = ['mushrooms', 'onions', 'pineapple']
print('mushrooms' in requested_toppings) # True
print('pepperoni' in requested_toppings) # False
```

**7. 检查特定值是否不在列表中 (**`not in`**)**

+ **功能**：与`in`相反，判断一个值是否**不**存在于一个列表中。
+ **语法**：使用关键字 `not in`。

```python
banned_users = ['andrew', 'carolina', 'david']
user = 'marie'
if user not in banned_users:
    print(f"{user.title()}, you can post a response.")
```

**8. 布尔表达式 (Boolean Expression)**

+ **概念**：就是条件测试的另一个名字。布尔值只有两个：`True` 和 `False`。可以直接将布尔值赋给变量，用于跟踪程序的状态。

```python
game_active = True
can_edit = False
```

---

#### **5.3 **`if`** 语句**
**1. 简单的**`if`**语句**

+ **结构**：由一个条件测试和在测试通过时要执行的一个或多个操作组成。
+ **语法**：

```python
if conditional_test:
    do_something()
```

+ **示例**：

```python
age = 19
if age >= 18:
    print("You are old enough to vote!")
    print("Have you registered to vote yet?")
```

+ **注意**：和`for`循环一样，`if`语句后面的所有**缩进**代码块都是在条件为`True`时才执行。

**2. **`if-else`** 语句**

+ **功能**：当条件测试通过时执行一个代码块，未通过时执行另一个代码块。
+ **语法**：

```python
if conditional_test:
    do_something()
else:
    do_something_else()
```

+ **适用场景**：非常适合处理只有两种可能性的情况。

**3. **`if-elif-else`** 结构**

+ **功能**：用于检查两个以上的情况。Python会按顺序检查每个条件，一旦找到一个为`True`的条件，就会执行其后的代码块，并**跳过**余下的所有测试。
+ **语法**：

```python
if age < 4:
    price = 0
elif age < 18:
    price = 25
else:
    price = 40
```

+ **重要细节**：
    - `elif` 是 `else if` 的缩写。
    - 可以根据需要使用任意数量的`elif`代码块。
    - `else`代码块是可选的。如果省略它，那么在所有`if`和`elif`条件都不满足时，程序将不执行任何操作。

**4. 测试多个条件**

+ **问题**：`if-elif-else`结构只执行一个代码块。如果你需要检查多个条件，并且**每个满足的条件都要触发相应的操作**，该怎么办？
+ **解决方法**：使用一系列**独立的**`if`**语句**。

```python
requested_toppings = ['mushrooms', 'extra cheese']

if 'mushrooms' in requested_toppings:
    print("Adding mushrooms.")
if 'pepperoni' in requested_toppings:
    print("Adding pepperoni.")
if 'extra cheese' in requested_toppings:
    print("Adding extra cheese.")
```*   **总结**：如果只需要一个分支被执行，就用`if-elif-else`；如果需要所有满足条件的分支都被执行，就用多个独立的`if`语句。
```

---

#### **5.4 使用**`if`**语句处理列表**
**1. 检查特殊元素**

+ **功能**：在遍历列表时，可以对特定的元素进行特殊处理。

```python
for topping in requested_toppings:
    if topping == 'green peppers':
        print("Sorry, we are out of green peppers right now.")
    else:
        print(f"Adding {topping}.")
```

**2. 确定列表是否为空**

+ **核心概念**：在`if`语句中，当列表不为空时，它被评估为`True`；当列表为空时，它被评估为`False`。
+ **应用**：在执行`for`循环之前，先检查列表是否为空，可以避免对空列表执行循环而产生不期望的结果或错误。

```python
requested_toppings = []

if requested_toppings: # 列表不为空时为True
    for requested_topping in requested_toppings:
        # ...
else:
    print("Are you sure you want a plain pizza?")
```

**3. 使用多个列表**

+ **场景**：当需要将一个列表（如顾客点的配料）与另一个列表（如餐厅供应的配料）进行核对时。

```python
available_toppings = ['mushrooms', 'olives', 'green peppers', ...]
requested_toppings = ['mushrooms', 'french fries', 'extra cheese']

for requested_topping in requested_toppings:
    if requested_topping in available_toppings:
        print(f"Adding {requested_topping}.")
    else:
        print(f"Sorry, we don't have {requested_topping}.")
```

---

#### **5.5 设置**`if`**语句的格式**
+ **PEP 8 建议**：在比较运算符（如 `==`, `>=`, `<=`）的两边各添加一个空格，以提高可读性。
    - **推荐**：`if age < 4:`
    - **不推荐**：`if age<4:`

---

#### **本章总结**
第五章是编程逻辑的基石。通过`if`语句，我学会了如何让程序根据不同的条件执行不同的代码路径。本章从最基础的**条件测试**（相等、不等、大小比较、`and`/`or`、`in`/`not in`）讲起，逐步深入到**`if`**、**`if-else`** 和 `if-elif-else` 这三种核心的控制流结构。我明白了它们各自的适用场景，特别是何时用`if-elif-else`链，何时用一系列独立的`if`语句。将这些逻辑判断与列表相结合，让我能够编写出更加实用和智能的程序，比如处理特殊项、检查空列表、以及在多个列表之间进行数据验证。本章的学习让我真正开始“与程序对话”，告诉它在不同情况下该如何思考和行动。

### **第六章：字典**
#### **核心概要**
在前面的章节中，我们学习了列表，它通过**位置索引**（0, 1, 2...）来存储和访问数据。本章引入了一个更为灵活的数据结构——**字典（Dictionary）**。字典存储的是**键-值对（key-value pair）****的集合，它不依赖于位置，而是通过唯一的****键（key）****来访问与之关联的****值（value）**。这使得字典非常适合用来模拟现实世界中各种事物的属性。本章将详细介绍如何**创建、访问、添加、修改和删除**字典中的键-值对，以及如何**遍历**字典，并探讨了**嵌套**这一强大的组织数据的方式，即将列表和字典互相嵌套。

---

#### **6.1 & 6.2 使用字典**
**1. 一个简单的字典**

+ **核心概念**：字典存储的是一系列的**键-值对**。每个键都与一个值相关联。
+ **语法**：使用花括号 `{}` 来定义字典，键和值之间用冒号 `:` 分隔，键-值对之间用逗号 `,` 分隔。

```python
alien_0 = {'color': 'green', 'points': 5}
```

+ **键（Key）**：是唯一的标识符，通常是字符串（但也可以是数字等不可变类型）。
+ **值（Value）**：与键相关联的数据，可以是任何Python对象：数字、字符串、列表，甚至是另一个字典。

**2. 访问字典中的值**

+ **语法**：在字典名后面跟一个方括号，里面写上要访问的键。

```python
print(alien_0['color'])  # 输出: green
print(alien_0['points']) # 输出: 5
```*   **应用**：可以像使用普通变量一样使用访问到的值。
```python
new_points = alien_0['points']
print(f"You just earned {new_points} points!")
```

**3. 添加键-值对**

+ **功能**：字典是动态的，可以随时添加新的键-值对。
+ **语法**：`dictionary_name['new_key'] = new_value`

```python
alien_0['x_position'] = 0
alien_0['y_position'] = 25
print(alien_0) 
# 输出: {'color': 'green', 'points': 5, 'x_position': 0, 'y_position': 25}
```

**4. 从空字典开始构建**

+ **语法**：先创建一个空字典 `{}`，然后逐个添加键-值对。
+ **适用场景**：当需要由用户输入或程序动态生成数据来填充字典时，这种方式非常常用。

```python
alien_0 = {}
alien_0['color'] = 'green'
alien_0['points'] = 5
```

**5. 修改字典中的值**

+ **语法**：与添加键-值对的语法相同。如果键已存在，则会更新其对应的值。

```python
alien_0 = {'color': 'green'}
alien_0['color'] = 'yellow'
print(alien_0['color']) # 输出: yellow
```

**6. 删除键-值对**

+ **语法**：使用 `del` 语句，指定字典名和要删除的键。
+ **注意**：`del` 会将键和与之关联的值**彻底删除**。

```python
alien_0 = {'color': 'green', 'points': 5}
del alien_0['points']
print(alien_0) # 输出: {'color': 'green'}
```

**7. 使用 **`get()`** 方法访问值**

+ **问题**：如果使用方括号 `[]` 访问一个不存在的键，程序会引发**`KeyError`**并崩溃。
+ `get()`** 的优势**：提供了一种更安全的方式来访问值。
+ **语法**：`dictionary.get('key_name', default_value)`

```python
alien_0 = {'color': 'green', 'speed': 'slow'}
point_value = alien_0.get('points', 'No point value assigned.')
print(point_value) # 输出: No point value assigned.
```

    - **第一个参数**：要获取的键。
    - **第二个参数（可选）**：如果该键不存在时，要返回的默认值。
    - 如果省略第二个参数且键不存在，`get()` 会返回 `None` (一个表示“无”的特殊值)，但不会报错。
+ **心得**：当你不确定一个键是否一定存在于字典中时，使用`.get()`是更好的编程习惯。

---

#### **6.3 遍历字典**
**1. 遍历所有的键-值对 (**`.items()`**)**

+ **功能**：可以同时获取字典中的键和值。
+ **语法**：使用`.items()`方法，并在`for`循环中声明两个变量来分别接收键和值。

```python
user_0 = {'username': 'efermi', 'first': 'enrico', 'last': 'fermi'}

for key, value in user_0.items():
    print(f"\nKey: {key}")
    print(f"Value: {value}")
```

+ **命名规范**：为了提高可读性，最好使用有意义的变量名，如 `for username, user_info in users.items():`。

**2. 遍历所有的键 (**`.keys()`**)**

+ **功能**：只获取字典中的键。
+ **语法**：使用`.keys()`方法。

```python
favorite_languages = {'jen': 'python', 'sarah': 'c', 'edward': 'rust'}

for name in favorite_languages.keys():
    print(name.title())
```

+ **默认行为**：遍历字典时，默认就是遍历它的键。因此，`for name in favorite_languages:` 和 `for name in favorite_languages.keys():` 的效果是**完全相同**的。显式使用`.keys()`可以让代码意图更清晰。

**3. 按特定顺序遍历字典的键**

+ **功能**：如果你希望按排序后的顺序处理键。
+ **方法**：在`for`循环中，用`sorted()`函数包裹`.keys()`的结果。

```python
for name in sorted(favorite_languages.keys()):
    print(f"{name.title()}, thank you for taking the poll.")
```

**4. 遍历所有的值 (**`.values()`**)**

+ **功能**：只获取字典中的值。
+ **语法**：使用`.values()`方法。

```python
for language in favorite_languages.values():
    print(language.title())
```

+ **问题**：`.values()`可能会返回重复的值。
+ **解决方法**：要获取不重复的值，可以先用`set()`函数将其转换为**集合**（一种自动去重的数据结构）。

```python
for language in set(favorite_languages.values()):
    print(language.title())
```

---

#### **6.4 嵌套（Nesting）**
嵌套是一种强大的数据组织方式，允许你在列表和字典中存储更复杂的数据结构。

**1. 字典列表（A List of Dictionaries）**

+ **结构**：一个列表中包含多个字典，每个字典都代表一个独立的对象。
+ **适用场景**：当你有多个结构相似的对象时，比如游戏中的一群外星人或网站上的一系列用户。

```python
aliens = [] # 创建一个空列表来存储外星人
for alien_number in range(30):
    new_alien = {'color': 'green', 'points': 5, 'speed': 'slow'}
    aliens.append(new_alien)
```

**2. 在字典中存储列表（A List in a Dictionary）**

+ **结构**：字典中的某个值本身就是一个列表。
+ **适用场景**：当一个键需要关联多个值时，比如一个披萨有多种配料，或一个人喜欢多种语言。

```python
pizza = {
    'crust': 'thick',
    'toppings': ['mushrooms', 'extra cheese'],
}

# 访问列表
for topping in pizza['toppings']:
    print(f"\t{topping}")
```

**3. 在字典中存储字典（A Dictionary in a Dictionary）**

+ **结构**：字典中的值是另一个字典。
+ **适用场景**：当一个键关联的值本身也包含复杂的结构化信息时，比如一个用户名关联着该用户的详细信息（名、姓、地址等）。

```python
users = {
    'aeinstein': {
        'first': 'albert',
        'last': 'einstein',
        'location': 'princeton',
    },
    'mcurie': {
        'first': 'marie',
        'last': 'curie',
        'location': 'paris',
    },
}
```

+ **访问嵌套字典**：

```python
for username, user_info in users.items():
    full_name = f"{user_info['first']} {user_info['last']}"
    print(f"\tFull name: {full_name.title()}")
```

+ **注意**：虽然可以嵌套很多层，但过度嵌套会让代码变得难以理解。如果结构过于复杂，可能需要考虑使用类（后续章节内容）。

---

#### **本章总结**
第六章为我的编程工具箱增添了一件极其强大的工具——字典。与列表的有序索引不同，字典通过**键-值对**的方式存储数据，这让我能够更直观、更灵活地模拟现实世界中的事物。我详细学习了字典的**增、删、改、查**操作，特别掌握了使用`.get()`方法安全地访问值以避免`KeyError`。本章的重点之一是**遍历字典**的多种方式：使用`.items()`遍历键值对，`.keys()`遍历键，以及`.values()`遍历值（并结合`set()`去重）。最后，**嵌套**的概念极大地扩展了我组织数据的能力，让我能够构建出如“字典列表”、“字典中包含列表”等复杂而强大的数据结构。掌握字典后，我处理结构化数据的能力有了质的飞跃。

### **第七章：用户输入和while循环**
#### **核心概要**
到目前为止，我们编写的程序都是“自说自话”，从头到尾执行完毕，无法与用户进行交互。本章引入了两个核心概念，彻底改变了这一点：

1. `input()`** 函数**：让程序能够暂停运行，接收用户从键盘输入的信息。
2. `while`** 循环**：让程序能够根据特定条件**持续运行**，而不是只执行一次就结束。

通过结合使用 `input()` 和 `while` 循环，我们将能够编写出真正的**交互式程序**，比如不断询问用户问题、根据用户选择决定何时退出、以及使用循环来高效地处理列表和字典中的数据。

---

#### **7.1 **`input()`** 函数的工作原理**
**1. 获取用户输入**

+ **功能**：`input()` 函数会暂停程序，等待用户输入文本，并在用户按下回车键后，获取输入的内容。
+ **语法**：`variable = input("Your prompt message: ")`
    - **提示（Prompt）**：`input()` 函数可以接受一个字符串参数，这个字符串会显示给用户，告诉用户应该输入什么信息。

```python
message = input("Tell me something, and I will repeat it back to you: ")
print(message)
```

+ **重要特性**：`input()` 函数接收到的所有输入，**无论用户输入的是数字还是文本，都会被Python当作字符串来处理**。

**2. 编写清晰的提示**

+ **规范**：为了让用户体验更好，提示信息应该清晰、简洁。
+ **技巧**：
    - 在提示信息的末尾通常会加上一个冒号和空格 (`: `)，这样用户的输入就会紧跟在提示符后面，看起来更整洁。
    - 如果提示信息很长，可以先将其存储在一个变量中，再把这个变量传递给 `input()` 函数，这样可以提高代码的可读性。

```python
prompt = "If you share your name, we can personalize the messages you see."
prompt += "\nWhat is your first name? " # 使用 += 附加字符串

name = input(prompt)
print(f"\nHello, {name}!")
```

**3. 获取数值输入**

+ **问题**：因为 `input()` 返回的是字符串，所以不能直接用它来进行数学比较或运算。

```python
age = input("How old are you? ")
# if age >= 18:  <-- 这里会引发 TypeError，因为不能比较字符串和整数
```*   **解决方法**：使用 `int()` 函数将字符串转换为整数。
```python
age = input("How old are you? ")
age = int(age) # 转换类型

if age >= 18:
    print("You are tall enough to ride!")
```

+ **注意**：`int()` 函数只能转换纯数字组成的字符串。如果用户输入了非数字字符，程序会引发`ValueError`。

**4. 求模运算符 (**`%`**)**

+ **功能**：这是一个非常有用的数学运算符，它将两个数相除并返回**余数**。
+ **语法**：`number % divisor`

```python
print(4 % 3)  # 输出: 1
print(6 % 3)  # 输出: 0
```

+ **应用**：一个常见的用途是判断一个数是奇数还是偶数。如果一个数能被2整除，那么它对2求模的余数就是0。

```python
number = input("Enter a number: ")
number = int(number)

if number % 2 == 0:
    print("The number is even.")
else:
    print("The number is odd.")
```

---

#### **7.2 **`while`** 循环简介**
**1. **`for`** 循环 vs. **`while`** 循环**

+ `for`** 循环**：用于**遍历一个集合**（如列表或字典）中的每个元素，循环次数是确定的。
+ `while`** 循环**：在**指定的条件为**`True`**时**不断地执行一个代码块，循环次数通常是不确定的，取决于条件的改变。

**2. 使用**`while`**循环**

+ **语法**：

```python
while conditional_test:
    do_something()
```

+ **示例**：从1数到5

```python
current_number = 1
while current_number <= 5:
    print(current_number)
    current_number += 1 # 关键一步：更新条件，否则会无限循环
```

+ **注意**：`+= 1` 是 `current_number = current_number + 1` 的简写。

**3. 让用户选择何时退出**

+ **方法**：设置一个**退出值（quit value）**，当用户输入这个值时，`while`循环的条件变为`False`，循环结束。

```python
prompt = "\nTell me something... (Enter 'quit' to end the program.) "
message = ""
while message != 'quit':
    message = input(prompt)
    if message != 'quit': # 避免打印出 'quit'
        print(message)
```

**4. 使用标志（Flag）**

+ **概念**：标志是一个布尔变量（`True`/`False`），用于控制程序的活动状态。
+ **优势**：当程序有多个可能导致退出的事件时（比如游戏中的多种结束条件），使用标志可以让`while`语句的条件保持简洁，而将复杂的逻辑判断放在循环体内处理。

```python
active = True # 这是一个标志
while active:
    message = input(prompt)
    
    if message == 'quit':
        active = False
    else:
        print(message)
```

**5. 使用**`break`**退出循环**

+ **功能**：`break`语句可以立即退出任何循环（`for`或`while`），不再执行循环中余下的代码。
+ **应用**：可以用来创建一个无限循环 `while True:`，然后在循环体内通过`if`语句判断退出条件，并用`break`来终止循环。

```python
while True:
    city = input(prompt)
    
    if city == 'quit':
        break # 立即退出循环
    else:
        print(f"I'd love to go to {city.title()}!")
```

**6. 在循环中使用**`continue`

+ **功能**：`continue`语句会忽略本次循环中余下的代码，直接返回到循环的开头，进行下一次条件判断。
+ **示例**：只打印奇数

```python
current_number = 0
while current_number < 10:
    current_number += 1
    if current_number % 2 == 0:
        continue # 如果是偶数，跳过print语句，直接开始下一次循环
    
    print(current_number)
```

**7. 避免无限循环**

+ **重要提醒**：每个`while`循环都必须有可靠的退出路径。如果循环条件永远为`True`，程序就会陷入**无限循环**。
+ **调试**：如果程序卡住不动，很可能就是陷入了无限循环。可以通过按 `Ctrl + C` 来终止它。

---

#### **7.3 使用**`while`**循环处理列表和字典**
**1. 在列表之间移动元素**

+ **场景**：将一个列表中的元素处理后，移到另一个列表中。例如，将待验证用户移到已验证用户列表。
+ **方法**：使用`while`循环和`.pop()`方法。`while`循环比`for`循环更适合这种场景，因为在`for`循环中修改列表的长度是不安全的。

```python
unconfirmed_users = ['alice', 'brian', 'candace']
confirmed_users = []

while unconfirmed_users:
    current_user = unconfirmed_users.pop()
    print(f"Verifying user: {current_user.title()}")
    confirmed_users.append(current_user)
```

**2. 删除列表中所有为特定值的元素**

+ **问题**：`for`循环不适合在遍历时删除元素，而`.remove()`方法一次只删除一个。
+ **解决方法**：使用`while`循环。

```python
pets = ['dog', 'cat', 'dog', 'goldfish', 'cat', 'rabbit', 'cat']

while 'cat' in pets:
    pets.remove('cat')
```

**3. 使用用户输入填充字典**

+ **场景**：进行一项调查，不断收集用户的回答，并将结果存储在字典中。

```python
responses = {}
polling_active = True # 使用标志

while polling_active:
    name = input("\nWhat is your name? ")
    response = input("Which mountain would you like to climb someday? ")
    
    responses[name] = response # 存储到字典
    
    repeat = input("Would you like to let another person respond? (yes/ no) ")
    if repeat == 'no':
        polling_active = False
```

---

#### **本章总结**
第七章是实现程序交互性的关键。我学会了使用`input()`函数来获取用户的文本和数值输入，并理解了进行类型转换（`int()`）的重要性。更核心的是，我掌握了`while`循环，它让程序可以根据条件持续运行。我学会了三种控制`while`循环流程的方式：**直接检查条件**、使用**标志（Flag）**、以及在循环体内使用**`break`**语句。我还了解了`continue`语句如何跳过当前循环的剩余部分。最后，本章展示了`while`循环在处理列表和字典时的强大能力，特别是在需要修改集合内容（如移动或删除元素）的场景下，`while`循环比`for`循环更加安全和适用。现在，我终于可以编写一个能与人“对话”的程序了。

### **第八章：函数**
#### **核心概要**
本章介绍了一个至关重要的编程概念——**函数（Function）**。函数是**为了完成特定任务而组织起来的、可重复使用的代码块**。在此之前，我们写的代码都是从上到下执行，如果想重复某个任务，就必须复制粘贴代码。函数彻底改变了这一点，它让我们能够给一段代码命名，然后在程序的任何地方通过这个名字来**调用（Call）**它。

本章的核心目标是让你掌握：

1. **如何定义和调用函数**。
2. **如何向函数传递信息**（通过**实参**和**形参**）。
3. **如何让函数返回值**，以便程序可以使用函数处理后的结果。
4. **如何将函数与列表等数据结构结合使用**。
5. **如何将函数存储在独立的文件（模块）中**，以保持主程序的整洁。

学习函数，是为了编写更高效、更易读、更易于维护和调试的代码。

---

#### **8.1 定义函数**
**1. 最简单的函数**

+ **概念**：一个不接收信息、也不返回信息的简单函数。
+ **语法**：
    - 使用关键字 `def` (define的缩写)来声明函数。
    - 后面跟着函数名和一对圆括号 `()`。
    - 函数定义的第一行以冒号 `:` 结尾。
    - 函数体内的所有代码都必须**缩进**。
+ **文档字符串 (Docstring)**：函数定义后的第一行，用三引号 `"""..."""` 括起来的字符串，用于解释函数的功能。这是一个非常好的编程习惯。

```python
def greet_user():
    """显示简单的问候语"""
    print("Hello!")

# 调用函数
greet_user()
```

**2. 向函数传递信息**

+ **概念**：可以让函数接收信息，使其功能更加灵活。
+ **形参 (Parameter)**：在**函数定义**的圆括号中声明的变量，用于接收信息。
+ **实参 (Argument)**：在**调用函数**时，传递给函数的具体值。

```python
def greet_user(username): # username 是形参
    """显示个性化的问告语"""
    print(f"Hello, {username.title()}!")

greet_user('jesse') # 'jesse' 是实参
```*   **工作流程**：调用函数时，实参的值会传递并赋给对应的形参。
```

---

#### **8.2 传递实参**
有多种方式可以向函数传递实参。

**1. 位置实参 (Positional Arguments)**

+ **概念**：这是最常见的传参方式。实参的传递顺序与形参的定义顺序一一对应。
+ **示例**：

```python
def describe_pet(animal_type, pet_name):
    # ...

describe_pet('hamster', 'harry') # 'hamster' 对应 animal_type, 'harry' 对应 pet_name
```

+ **注意**：**顺序至关重要**，如果顺序搞错，会导致逻辑错误。

**2. 关键字实参 (Keyword Arguments)**

+ **概念**：在函数调用时，明确指定每个实参要传递给哪个形参。
+ **语法**：`parameter_name = value`
+ **优势**：**顺序不再重要**，并且让函数调用的意图更加清晰。

```python
describe_pet(animal_type='hamster', pet_name='harry')
describe_pet(pet_name='harry', animal_type='hamster') # 效果完全相同
```

**3. 默认值 (Default Values)**

+ **功能**：在定义函数时，可以为形参指定一个默认值。如果在调用函数时没有为该形参提供实参，Python就会使用这个默认值。
+ **语法**：在形参列表中，使用等号为形参赋值。

```python
def describe_pet(pet_name, animal_type='dog'): # animal_type 默认值为 'dog'
    # ...

describe_pet(pet_name='willie') # 只需提供 pet_name，animal_type 会自动使用 'dog'
describe_pet(pet_name='harry', animal_type='hamster') # 也可以提供新值来覆盖默认值
```

+ **重要规则**：在函数定义中，**必须先列出没有默认值的形参，再列出有默认值的形参**。

**4. 避免实参错误**

+ **常见错误**：`TypeError`，通常是因为提供的实参数量与函数定义的形参数量不匹配（太多或太少）。
+ **调试**：Python的错误信息会明确告诉你函数需要哪些参数，以及你提供了多少。

---

#### **8.3 返回值**
函数不一定非要直接打印输出，它们可以处理数据后**返回（Return）**一个或一组值。

**1. 返回简单的值**

+ **语法**：使用 `return` 语句。
+ **功能**：函数执行到`return`语句时会结束，并将`return`后面的值返回到函数被调用的地方。你需要用一个变量来接收这个返回值。

```python
def get_formatted_name(first_name, last_name):
    """返回标准格式的姓名"""
    full_name = f"{first_name} {last_name}"
    return full_name.title()

musician = get_formatted_name('jimi', 'hendrix') # 将返回值存储在变量 musician 中
print(musician) # 输出: Jimi Hendrix
```

**2. 让实参变成可选的**

+ **场景**：有些信息（如中间名）不是必需的。
+ **方法**：为可选的形参提供一个默认值（通常是空字符串 `''` 或 `None`）。然后在函数体内使用 `if` 语句检查该形参是否有值。

```python
def get_formatted_name(first_name, last_name, middle_name=''):
    if middle_name: # 如果 middle_name 不是空字符串
        full_name = f"{first_name} {middle_name} {last_name}"
    else:
        full_name = f"{first_name} {last_name}"
    return full_name.title()
```

**3. 返回字典**

+ **功能**：函数可以返回任何数据类型，包括字典，这对于构建结构化的数据非常有用。

```python
def build_person(first_name, last_name):
    person = {'first': first_name, 'last': last_name}
    return person
```

**4. 结合使用函数和**`while`**循环**

+ **应用**：可以创建一个交互式程序，在`while`循环中调用函数来处理用户的输入。

```python
while True:
    # ...获取用户输入 f_name 和 l_name
    formatted_name = get_formatted_name(f_name, l_name)
    print(f"\nHello, {formatted_name}!")
```

---

#### **8.4 & 8.5 & 8.6 传递和存储函数**
**1. 传递列表**

+ **功能**：可以将整个列表作为实参传递给函数。函数可以直接访问和操作列表的内容。

```python
def greet_users(names):
    for name in names:
        print(f"Hello, {name.title()}!")

usernames = ['hannah', 'ty', 'margot']
greet_users(usernames)
```

+ **在函数中修改列表**：函数对列表所做的任何修改都是**永久性的**，因为函数操作的是原始列表本身。
+ **禁止函数修改列表**：如果你不希望函数修改原始列表，可以向函数传递列表的**副本**。
    - **语法**：使用切片 `[:]` 来创建副本：`function_name(list_name[:])`。

**2. 传递任意数量的实参**

+ **使用 **`*args`**（收集位置实参）**：
    - **语法**：在形参名前加一个星号 `*`。
    - **功能**：这会告诉Python创建一个**元组（Tuple）**，并将所有传递给该形参的位置实参都收集到这个元组中。形参名通常约定为 `*args`。

```python
def make_pizza(*toppings): # toppings 会是一个元组
    print(toppings)

make_pizza('pepperoni') # 输出: ('pepperoni',)
make_pizza('mushrooms', 'green peppers') # 输出: ('mushrooms', 'green peppers')
```*   **使用 `**kwargs`（收集关键字实参）**：
```

    - **语法**：在形参名前加两个星号 `**`。
    - **功能**：这会告诉Python创建一个**字典（Dictionary）**，并将所有传递给该形参的关键字实参都收集到这个字典中。形参名通常约定为 `**kwargs`。

```python
def build_profile(first, last, **user_info): # user_info 会是一个字典
    user_info['first_name'] = first
    user_info['last_name'] = last
    return user_info

user_profile = build_profile('albert', 'einstein', location='princeton', field='physics')
```

+ **形参顺序**：在定义函数时，`*args` 必须在位置形参之后，`**kwargs` 必须在最后。

**3. 将函数存储在模块中**

+ **模块 (Module)**：一个`.py`文件就是一个模块。
+ **优势**：可以将函数存储在独立的文件中，然后在主程序中**导入（import）**它们。这有助于：
    - 保持主程序代码的简洁和逻辑清晰。
    - 实现代码的复用。
+ **导入方式**：
    1. **导入整个模块**：`import module_name`，调用时需使用 `module_name.function_name()`。
    2. **导入特定函数**：`from module_name import function_name`，调用时直接使用 `function_name()`。
    3. **使用**`as`**给函数指定别名**：`from module_name import function_name as fn`。
    4. **使用**`as`**给模块指定别名**：`import module_name as mn`。
    5. **导入模块中所有函数（不推荐）**：`from module_name import *`，这可能导致命名冲突。

---

#### **8.7 函数编写指南**
+ **命名**：函数名和模块名应使用描述性的小写名称，单词间用下划线分隔。
+ **注释**：每个函数都应有一个简洁的文档字符串（docstring），解释其功能。
+ **参数格式**：在指定默认值时，等号两边不加空格，如 `def function(param=5):`。
+ **空行**：使用两个空行来分隔模块中的不同函数，以提高可读性。
+ **import语句**：应放在文件的开头。

---

#### **本章总结**
第八章是提升编程能力的关键一跃。通过学习函数，我掌握了将代码模块化、组织化的核心方法。我不仅学会了如何定义和调用函数，还深入理解了**位置实参、关键字实参、默认值**以及如何接收**任意数量的实参**（`*args`和`**kwargs`）。`return`语句让我能够让函数处理数据并返回结果，极大地增强了程序的灵活性。将函数存储在**模块**中并进行导入，更是让我理解了现代软件工程中代码组织和复用的基本思想。现在，我已经不再满足于编写简单的脚本，而是有能力开始构建结构更清晰、逻辑更复杂的程序了。

### **第九章：类**
#### **核心概要**
在之前的章节中，我们学习了如何使用函数来组织代码，以及如何使用列表和字典来存储数据。本章将这两者结合起来，引入了**类（Class）**的概念。类是一种将**数据（属性）**和**操作这些数据的方法（函数）**封装在一起的方式。

简单来说，**类是创建对象的蓝图**。比如，我们可以创建一个`Dog`类，这个类描述了所有狗共有的特征（如名字、年龄）和行为（如坐下、打滚）。然后，我们可以根据这个`Dog`类，创建出具体的、独一-无二的狗的**对象（Object）****或****实例（Instance）**，比如一只叫“威利”的6岁小狗，或者一只叫“露西”的3岁小狗。

本章的核心目标是让你掌握：

1. **如何创建和使用类**。
2. **如何创建类的实例，并访问其属性和调用其方法**。
3. **如何通过继承（Inheritance）让一个类获得另一个类的所有功能**，并在此基础上进行扩展。
4. **如何将类存储在模块中**，以实现代码的组织和复用。

---

#### **9.1 创建和使用类**
**1. 创建 **`Dog`** 类**

+ **概念**：创建一个简单的`Dog`类，用于模拟小狗。
+ **语法**：

```python
class Dog:
    """一次模拟小狗的简单尝试"""
    # ... 类的内容 ...
```

    - 使用关键字 `class` 来定义一个类。
    - 类名通常采用**驼峰命名法（CamelCase）**，即每个单词的首字母都大写，如 `ElectricCar`。
    - 类定义后紧跟一个描述该类功能的**文档字符串**。

**2. **`__init__()`** 方法**

+ **核心概念**：这是一个**特殊的方法**，在每次根据类创建新实例时，Python都会**自动运行**它。它的名字前后都有两个下划线，用于区分普通方法。
+ **功能**：初始化实例的属性。可以想象成是新生对象的“出生设置”。
+ `self`** 形参**：
    - `__init__()` 方法的第一个形参**必须是 **`self`。
    - `self` 是一个指向**实例本身**的引用。它让实例能够访问类中的属性和方法。
    - 在创建实例时，Python会自动传递`self`实参，我们**不需要手动提供**。
+ **属性 (Attributes)**：

```python
def __init__(self, name, age):
    """初始化属性name和age"""
    self.name = name  # 将形参name的值赋给实例的属性self.name
    self.age = age    # 将形参age的值赋给实例的属性self.age
```

    - 属性是与实例相关联的变量。
    - 在`__init__()`方法中，通过`self.属性名 = 值`的方式来定义属性。

**3. 定义方法 (Methods)**

+ **概念**：在类中定义的函数称为方法。它描述了实例能够执行的操作。
+ **语法**：与定义普通函数类似，但第一个形参**必须是 **`self`，以便方法能够访问实例的属性和其他方法。

```python
def sit(self):
    """模拟小狗收到命令时坐下"""
    print(f"{self.name} is now sitting.")

def roll_over(self):
    """模拟小狗收到命令时打滚"""
    print(f"{self.name} rolled over!")
```

**4. 根据类创建实例**

+ **概念**：创建类的具体对象的过程称为**实例化**。
+ **语法**：`instance_name = ClassName(arg1, arg2, ...)`

```python
my_dog = Dog('Willie', 6)
```

+ **执行过程**：Python会调用`Dog`类中的`__init__()`方法，并将 `'Willie'` 和 `6` 这两个实参分别传递给形参`name`和`age`。`__init__()`方法创建一个实例，设置其属性，然后将这个实例返回并赋给变量`my_dog`。

**5. 访问属性**

+ **语法**：使用**点号 **`.` 来访问实例的属性。

```python
print(f"My dog's name is {my_dog.name}.") # 输出: My dog's name is Willie.
print(f"My dog is {my_dog.age} years old.") # 输出: My dog is 6 years old.
```

**6. 调用方法**

+ **语法**：同样使用**点号 **`.` 来调用实例的方法。

```python
my_dog.sit()       # 输出: Willie is now sitting.
my_dog.roll_over() # 输出: Willie rolled over!
```

**7. 创建多个实例**

+ **概念**：可以根据一个类创建任意数量的实例。每个实例都是独立的，拥有自己的一套属性，但共享相同的方法。

```python
your_dog = Dog('Lucy', 3)
```

---

#### **9.2 使用类和实例**
**1. **`Car`** 类示例**

+ 本节通过创建一个更实际的`Car`类来进一步讲解类的使用。这个类有`make`（制造商）、`model`（型号）、`year`（年份）等属性。

**2. 给属性指定默认值**

+ **功能**：可以让某个属性在创建实例时拥有一个固定的初始值，而无需通过形参提供。
+ **方法**：直接在`__init__()`方法中定义这个属性即可。

```python
class Car:
    def __init__(self, make, model, year):
        # ...
        self.odometer_reading = 0 # 里程表读数，默认值为0
```

**3. 修改属性的值**

+ **方法一：直接修改**：通过实例直接访问并修改属性的值。

```python
my_new_car.odometer_reading = 23
```

+ **方法二：通过方法修改**：在类中编写一个专门的方法来更新属性的值。这样做更规范，并且可以在方法中加入逻辑检查（如禁止将里程回调）。

```python
def update_odometer(self, mileage):
    """将里程表读数设置为指定的值"""
    if mileage >= self.odometer_reading:
        self.odometer_reading = mileage
    else:
        print("You can't roll back an odometer!")
```

+ **方法三：通过方法递增**：编写一个方法，对属性的值进行增加，而不是设置一个全新的值。

```python
def increment_odometer(self, miles):
    """让里程表读数增加指定的量"""
    self.odometer_reading += miles
```

---

#### **9.3 继承（Inheritance）**
**1. 核心概念**

+ **功能**：当你创建的一个类是另一个已存在类的特殊版本时，可以使用继承。
+ **父类 (Parent Class)**：被继承的类。
+ **子类 (Child Class)**：继承父类的类。
+ **优势**：子类会自动获得父类的所有属性和方法，同时还可以定义自己特有的属性和方法。这极大地促进了代码的复用。

**2. 子类的**`__init__()`**方法**

+ **语法**：

```python
class ElectricCar(Car):
    def __init__(self, make, model, year):
        """初始化父类的属性"""
        super().__init__(make, model, year)
```

    - 定义子类时，在类名后的圆括号中指定父类的名称：`class ElectricCar(Car):`。
    - 在子类的`__init__()`方法中，必须调用父类的`__init__()`方法，以初始化父类中定义的属性。
    - 使用`super()`函数来实现这一点。`super()`是一个特殊函数，它让你能够调用父类的方法。

**3. 给子类定义属性和方法**

+ **功能**：在继承了父类的基础上，可以添加子类独有的新属性和新方法。

```python
class ElectricCar(Car):
    def __init__(self, make, model, year):
        super().__init__(make, model, year)
        self.battery_size = 40 # 添加子类特有属性
    
    def describe_battery(self): # 添加子类特有方法
        print(f"This car has a {self.battery_size}-kWh battery.")
```

**4. 重写父类中的方法 (Overriding)**

+ **功能**：如果父类中的某个方法不适用于子类，可以在子类中定义一个**同名**的方法来覆盖它。
+ **执行过程**：当子类的实例调用这个方法时，Python会忽略父类中的版本，只执行子类中定义的版本。

```python
def fill_gas_tank(self):
    """电动汽车没有油箱"""
    print("This car doesn't have a gas tank!")
```

**5. 将实例用作属性（组合 Composition）**

+ **概念**：当你发现一个类越来越复杂，包含了很多针对某个特定部分的属性和方法时，可以把这部分提取出来，创建一个全新的类。然后，将这个新类的实例用作原来那个类的属性。
+ **示例**：将`ElectricCar`中所有关于电池的细节（如容量、续航里程等）都提取到一个独立的`Battery`类中。

```python
class Battery:
    # ... 关于电池的属性和方法 ...

class ElectricCar(Car):
    def __init__(self, make, model, year):
        super().__init__(make, model, year)
        self.battery = Battery() # 将Battery实例作为ElectricCar的一个属性
```*   **优势**：这让主类（`ElectricCar`）的结构更清晰，代码更有条理。
```

---

#### **9.4 & 9.5 导入类与Python标准库**
**1. 导入类**

+ **功能**：和函数一样，可以将类存储在模块中，然后在需要时导入。
+ **方法**：
    - **导入单个类**：`from car import Car`
    - **从一个模块导入多个类**：`from car import Car, ElectricCar`
    - **导入整个模块**：`import car`，使用时需要 `car.Car()`。
    - **使用别名**：`from electric_car import ElectricCar as EC`。

**2. Python标准库**

+ **概念**：Python自带的一组非常有用的模块，无需额外安装。
+ **示例**：`random`模块，提供了生成随机数的工具，如`randint()`和`choice()`。

---

#### **9.6 类的编程风格**
+ **类名**：使用驼峰命名法（`MyClassName`）。
+ **实例名和模块名**：使用小写字母和下划线（`my_instance`, `my_module.py`）。
+ **空行**：在类中，使用一个空行来分隔方法；在模块中，使用两个空行来分隔类。

---

#### **本章总结**
第九章是思维方式上的一次重大转变。通过学习**类**，我不再是将数据和操作分离开来思考，而是将它们**封装**成一个紧密联系的整体——**对象**。我掌握了定义类、创建实例、使用属性和方法的核心技能。**继承**的概念让我明白了如何构建有层次、可复用的代码结构，而**组合**（将实例用作属性）则教会了我如何将复杂的大类拆解成更小、更易于管理的小类。最后，学习导入类让我能够像组织函数一样，有条理地组织我的项目文件。现在，我已经具备了用**面向对象**的思维来模拟和解决现实世界问题的基本能力。



### **第十章：文件和异常**
#### **核心概要**
到目前为止，我们程序中创建的数据（如列表、字典）都存储在内存中，一旦程序结束，这些数据就会丢失。本章将解决这个问题，主要围绕两个核心主题展开：

1. **文件（Files）**：学习如何**读取文件**中的数据，以及如何将程序中的数据**写入文件**。这使得我们的程序能够处理持久化的数据，即程序关闭后数据依然存在。
2. **异常（Exceptions）**：学习如何处理程序在运行时可能发生的错误。通过使用**`try-except`代码块**，我们可以预见并捕获潜在的错误（如文件不存在、除以零等），从而避免程序崩溃，并给用户提供友好的提示。

此外，本章还会介绍使用`json`模块来存储复杂的Python数据结构，这是一种在程序之间共享数据和保存用户设置的常用方法。

---

#### **10.1 读取文件**
**1. 读取整个文件**

+ **核心模块**：`pathlib` 模块中的 `Path` 类，这是现代Python中处理文件路径的首选方式。
+ **步骤**：
    1. **导入**`Path`**类**：`from pathlib import Path`
    2. **创建路径对象**：`path = Path('pi_digits.txt')`。这会创建一个指向当前目录下`pi_digits.txt`文件的对象。
    3. **读取文件内容**：使用 `.read_text()` 方法读取文件的全部内容，并将其作为一个**长字符串**返回。
    4. **清理空白**：`.read_text()` 读取的内容末尾可能包含一个多余的空行，可以使用 `.rstrip()` 方法将其删除。
+ **代码示例**：

```python
from pathlib import Path

path = Path('pi_digits.txt')
contents = path.read_text()
print(contents.rstrip())
```

**2. 文件路径**

+ **相对路径 (Relative Path)**：相对于**当前运行的程序文件**所在的位置。
    - 如果文件在同一个目录下：`Path('pi_digits.txt')`
    - 如果文件在子目录`text_files`中：`Path('text_files/pi_digits.txt')`
+ **绝对路径 (Absolute Path)**：从计算机的根目录开始的完整路径。
    - **优点**：无论你的程序文件在哪里，都能准确找到文件。
    - **注意**：在Windows系统中，路径分隔符是反斜杠 `\`，但在Python代码中，**建议始终使用正斜杠 **`/`，`pathlib` 会自动处理不同操作系统间的差异。

**3. 逐行读取**

+ **方法**：使用字符串的 `.splitlines()` 方法，它可以将一个包含多行文本的长字符串分割成一个**列表**，列表中的每个元素是文件中的一行。
+ **应用**：可以结合`for`循环来处理文件中的每一行。

```python
contents = path.read_text()
lines = contents.splitlines() # lines 是一个列表

for line in lines:
    print(line)
```

**4. 使用文件内容**

+ **核心概念**：从文件中读取的内容都是**字符串**。
+ **处理技巧**：
    - **拼接字符串**：如果想将文件的所有行合并成一个没有换行符的字符串，可以遍历行列表并逐行附加到一个空字符串上。
    - **类型转换**：如果文件中存储的是数字，并且你想进行数学运算，必须使用 `int()` 或 `float()` 将其从字符串转换为数字类型。

```python
pi_string = ''
for line in lines:
    pi_string += line.lstrip() # lstrip()删除每行开头的空格

pi_float = float(pi_string)
print(pi_float + 1)
```

**5. 处理大型文件**

+ **知识点**：Python处理数据的能力没有上限，只要计算机内存足够，上述方法同样适用于包含数百万行内容的文件。
+ **示例**：书中演示了如何读取一个包含一百万位圆周率的文件，并检查其中是否包含某个人的生日。这展示了将文件内容读入内存后进行数据分析的强大能力。

---

#### **10.2 写入文件**
**1. 写入一行**

+ **方法**：使用路径对象的 `.write_text()` 方法。
+ **语法**：`path.write_text("Some content to write.")`
+ **重要行为**：
    - 如果文件**不存在**，`.write_text()` 会**自动创建**它。
    - 如果文件**已存在**，`.write_text()` 会**覆盖（删除）**文件的原有内容，然后写入新内容。**这是一个需要特别小心的操作！**
+ **数值写入**：如果要写入数字，必须先用`str()`函数将其转换为字符串。

**2. 写入多行**

+ **方法**：在要写入的字符串中包含换行符 `\n`。

```python
contents = "I love programming.\n"
contents += "I love creating new games.\n"

path = Path('programming.txt')
path.write_text(contents)
```

---

#### **10.3 异常 (Exceptions)**
异常是Python用来管理程序运行时错误的特殊对象。

**1. **`try-except`** 代码块**

+ **目的**：处理可能引发异常的代码，避免程序因此崩溃。
+ **语法**：

```python
try:
    # 可能会出错的代码
except ExceptionName:
    # 出错了怎么办
```

+ **执行流程**：
    - Python首先尝试执行`try`代码块中的代码。
    - 如果**没有错误**发生，`except`代码块会被忽略。
    - 如果**发生错误**，并且错误类型与`except`后面指定的`ExceptionName`匹配，Python就会执行`except`代码块中的代码。
+ **示例：处理**`ZeroDivisionError`

```python
try:
    print(5 / 0)
except ZeroDivisionError:
    print("You can't divide by zero!")
```

这样，程序不会崩溃，而是会打印出一条友好的提示信息。

**2. **`else`** 代码块**

+ **功能**：`else`代码块包含的是仅在`try`代码块**成功执行**（即没有引发任何异常）时才需要运行的代码。
+ **优势**：将成功逻辑与`try`块中的风险代码分离，使代码结构更清晰。

```python
try:
    answer = int(first_number) / int(second_number)
except ZeroDivisionError:
    print("You can't divide by zero!")
else:
    print(answer)
```

**3. 处理**`FileNotFoundError`

+ **场景**：当尝试读取一个不存在的文件时，会引发`FileNotFoundError`。
+ **解决方法**：将读取文件的代码放在`try`块中。

```python
path = Path('alice.txt')
try:
    contents = path.read_text(encoding='utf-8')
except FileNotFoundError:
    print(f"Sorry, the file {path} does not exist.")
else:
    # ...成功读取文件后，在这里分析文本...
```

+ `encoding='utf-8'`：当读取的文件编码与系统默认编码不同时（尤其是在不同操作系统间分享文件时），指定编码非常重要，`utf-8`是一种通用的编码格式。

**4. 静默失败 (Failing Silently)**

+ **概念**：有时候，我们不希望在发生错误时通知用户，而是让程序静默地继续运行。
+ **方法**：在`except`代码块中使用`pass`语句。`pass`语句是一个占位符，告诉Python什么也不要做。

```python
except FileNotFoundError:
    pass # 静默地忽略错误
```

**5. 决定报告哪些错误**

+ **原则**：这是一个设计决策。如果用户需要知道某个文件未被处理，就应该给出提示。如果程序能够正常工作而无需该文件，那么静默失败可能更合适。

---

#### **10.4 存储数据 (**`json`**模块)**
**1. **`json`**模块简介**

+ **功能**：允许你将Python的复杂数据结构（如列表、字典）转换为**JSON（JavaScript Object Notation）格式**的字符串，以便存储到文件中。之后还可以从文件中读取JSON字符串，并将其恢复为原始的Python数据结构。
+ **优势**：JSON是一种跨语言的通用数据格式，非常适合在不同程序之间共享数据。

**2. **`json.dumps()`** 和 **`json.loads()`

+ `json.dumps(data)` (dump string)：将Python对象**转换为**JSON格式的字符串。

```python
import json
numbers = [2, 3, 5, 7, 11, 13]
contents = json.dumps(numbers) # contents 是一个字符串 ' [2, 3, 5, 7, 11, 13]'
path.write_text(contents)
```

+ `json.loads(json_string)` (load string)：将JSON格式的字符串**解析为**Python对象。

```python
contents = path.read_text()
numbers = json.loads(contents) # numbers 又变回了列表 [2, 3, 5, 7, 11, 13]
```

**3. 保存和读取用户生成的数据**

+ **应用**：这是一个非常常见的模式，用于记住用户的设置或信息。
    1. 程序启动时，尝试从文件中加载用户信息。
    2. 如果文件不存在（`FileNotFoundError`），则提示用户输入信息，并使用`json.dumps()`将其保存到文件中。
    3. 如果文件存在，则使用`json.loads()`加载信息，并使用这些信息。

**4. 重构 (Refactoring)**

+ **概念**：在不改变代码外部行为的前提下，改进代码的内部结构。
+ **目的**：让代码更清晰、更易于理解和扩展。
+ **方法**：将紧密相关的代码块组织成函数。例如，可以将“获取已存储的用户名”和“获取新用户名”的逻辑分别封装在不同的函数中。

---

#### **本章总结**
第十章极大地扩展了我的程序的能力。通过**文件操作**，我的程序终于可以和外部世界进行数据交换，实现了数据的**持久化**。我学会了使用`pathlib.Path`来读取和写入文本文件。更重要的是，我掌握了**异常处理**这一关键技能。使用**`try-except-else`代码块**，我能够编写出更健壮（Robust）的程序，它们在面对预料之外的情况（如文件丢失或无效输入）时，不会轻易崩溃，而是能给出友好的反馈。最后，学习使用`json`模块，让我能够方便地保存和加载用户的复杂数据，为开发更具实用性的应用程序铺平了道路。



### **第十一章：测试代码**
#### **核心概要**
本章的核心主题是**软件测试**。在此之前，我们验证代码是否正确的方式主要是手动运行程序并观察输出。这种方式在程序简单时还行得通，但随着项目变得越来越复杂，手动测试会变得非常繁琐且容易出错。

本章引入了**自动化测试**的概念，即编写一段代码（测试代码）来自动检查另一段代码（被测试代码）的行为是否符合预期。我们将学习使用一个非常流行且强大的第三方测试框架——`pytest`。

本章的目标是让你掌握：

1. **为什么需要测试**，以及测试的基本概念（单元测试、测试用例）。
2. **如何安装和使用 **`pytest` 来测试你编写的函数和类。
3. **如何编写一个测试**，包括创建测试文件、测试函数，以及使用**断言（assert）**来验证结果。
4. **如何解读测试结果**，包括测试通过和测试失败时的输出信息。
5. **如何处理测试失败**，以及如何通过添加新测试来确保代码的健壮性。
6. 学习使用**夹具（fixture）**来简化测试代码，避免重复。

---

#### **11.1 使用 pip 安装 pytest**
**1. 第三方包**

+ **概念**：除了Python自带的标准库，还有大量由社区开发的、功能强大的库，称为第三方包。`pytest`就是其中之一。
+ **安装工具 **`pip`：`pip`是Python的包管理器，用于安装和管理第三方包。

**2. 更新 **`pip`

+ **原因**：`pip`本身也在不断更新，为了安全和稳定，在使用前最好先将它升级到最新版本。
+ **命令**：

```bash
python -m pip install --upgrade pip
```

    - `python -m pip`：确保我们使用的是当前Python环境关联的`pip`。

**3. 安装 **`pytest`

+ **命令**：

```bash
python -m pip install --user pytest
```

    - `--user`：这个标志告诉`pip`将包安装在用户目录下，而不是系统目录下，这可以避免权限问题，是一种好的实践。

---

#### **11.2 测试函数**
**1. 单元测试与测试用例**

+ **单元测试 (Unit Test)**：针对程序中最小的可测试单元（通常是一个函数或一个方法）进行的测试。它的目标是验证这个单元的行为是否正确。
+ **测试用例 (Test Case)**：一组相关的单元测试，它们共同验证一个函数或类在各种不同情况下的行为是否都符合预期。

**2. 一个待测试的函数**

+ 我们首先需要一个函数来进行测试。书中以一个格式化姓名的函数为例：
    - `name_function.py`:

```python
def get_formatted_name(first, last):
    """生成格式规范的姓名"""
    full_name = f"{first} {last}"
    return full_name.title()
```

**3. 编写一个可通过的测试**

+ `pytest`** 的基本规则**：
    1. 测试文件必须以 `test_` 开头，或者以 `_test.py` 结尾。通常使用前者，如 `test_name_function.py`。
    2. 测试文件中的测试函数必须以 `test_` 开头。
+ **断言 (Assertion)**：
    - **概念**：断言是你对程序行为的一种“声明”或“断定”。`assert`语句会检查其后的条件是否为`True`。
    - **行为**：如果条件为`True`，测试继续进行。如果条件为`False`，测试**失败**，`pytest`会报告错误。
+ **编写测试代码**：
    - `test_name_function.py`:

```python
from name_function import get_formatted_name

def test_first_last_name():
    """能够正确地处理像Janis Joplin这样的姓名吗？"""
    formatted_name = get_formatted_name('janis', 'joplin')
    assert formatted_name == 'Janis Joplin'
```

**4. 运行测试**

+ **方法**：在终端中，切换到包含测试文件的目录，然后简单地运行 `pytest` 命令。

```bash
pytest
```

+ **解读测试通过的输出**：
    - `pytest`会自动发现并运行所有符合命名规则的测试文件和测试函数。
    - 输出中会显示收集到了多少个测试项（`collected 1 item`）。
    - 每个通过的测试都会显示一个**绿色的点 **`.`。
    - 最后会有一个总结，告诉你多少个测试通过了（`1 passed`）。

**5. 编写一个未通过的测试**

+ **目的**：观察当代码不符合预期时，测试是如何失败的。这有助于我们学习如何根据测试失败的报告来修复代码。
+ **场景**：我们故意修改`get_formatted_name`函数，使其要求传入中间名，这会导致原来的测试失败。
    - `name_function.py` (修改后):

```python
def get_formatted_name(first, middle, last):
    # ...
```*   **运行测试并解读失败的输出**：
```

    - 测试失败会显示一个**红色的 **`F`。
    - 会有一个详细的`FAILURES`部分。
    - **详细报告**：
        1. 指明哪个测试函数失败了（`test_first_last_name`）。
        2. 显示导致失败的代码行（`formatted_name = get_formatted_name('janis', 'joplin')`）。
        3. 给出具体的错误类型和信息（`TypeError: get_formatted_name() missing 1 required positional argument: 'last'`），告诉我们函数调用时缺少了参数。

**6. 在测试未通过时怎么办？**

+ **核心原则**：当测试失败时，**不要修改测试代码**去适应错误的代码。测试是标尺，代码是待测量的物品。尺子是准的，错的是物品。
+ **正确做法**：**修复被测试的代码**，让它的行为重新符合测试的预期。
+ **修复示例**：将`get_formatted_name`的`middle`参数设为可选的默认值，使其能同时处理带中间名和不带中间名的两种情况。

```python
def get_formatted_name(first, last, middle=''):
    # ...
```

**7. 添加新测试**

+ **目的**：在修复代码并确保它能通过旧测试后，我们还需要为新添加的功能（处理中间名）编写新的测试。
+ **方法**：在同一个测试文件中添加一个新的测试函数。

```python
def test_first_last_middle_name():
    """能够正确处理像Wolfgang Amadeus Mozart这样的姓名吗？"""
    formatted_name = get_formatted_name('wolfgang', 'mozart', 'amadeus')
    assert formatted_name == 'Wolfgang Amadeus Mozart'
```*   再次运行`pytest`，现在会看到两个测试都通过了（输出中有两个绿色的点 `..`）。
```

---

#### **11.3 测试类**
测试类的思路与测试函数类似，主要目标是测试类中**方法的行为**。

**1. 一个要测试的类**

+ 书中以一个管理匿名调查的`AnonymousSurvey`类为例。
    - `survey.py`:

```python
class AnonymousSurvey:
    def __init__(self, question):
        self.question = question
        self.responses = []

    def store_response(self, new_response):
        self.responses.append(new_response)
```

**2. 测试**`AnonymousSurvey`**类**

+ **步骤**：
    1. 在测试函数中，创建一个类的**实例**。
    2. 调用实例的方法来模拟使用场景。
    3. 使用`assert`来检查实例的属性（如存储答案的列表）是否变成了我们预期的状态。
+ **代码示例**：
    - `test_survey.py`:

```python
from survey import AnonymousSurvey

def test_store_single_response():
    """测试单个答案会被妥善地存储"""
    question = "What language did you first learn to speak?"
    language_survey = AnonymousSurvey(question)
    language_survey.store_response('English')
    assert 'English' in language_survey.responses
```

**3. 使用夹具 (Fixtures)**

+ **问题**：当有多个测试函数时，我们可能会在每个函数中都重复创建同一个类的实例，这会导致代码冗余。
+ **夹具的功能**：夹具是一种特殊的函数，用于**搭建测试环境**。它可以创建一个供多个测试函数使用的资源（如一个类的实例）。
+ **如何创建夹具**：
    1. 导入 `pytest`。
    2. 创建一个函数，并在其上方使用装饰器 `@pytest.fixture`。
    3. 这个函数创建并`return`一个资源。
+ **如何使用夹具**：
    1. 将夹具函数的**函数名**作为一个**参数**传递给你需要使用该资源的测试函数。
    2. `pytest`会自动运行夹具，并将其返回值传递给测试函数。
+ **代码示例**（使用夹具重构 `test_survey.py`）：

```python
import pytest
from survey import AnonymousSurvey

@pytest.fixture
def language_survey():
    """一个可供所有测试函数使用的AnonymousSurvey实例"""
    question = "What language did you first learn to speak?"
    survey = AnonymousSurvey(question)
    return survey

def test_store_single_response(language_survey): # 使用夹具
    language_survey.store_response('English')
    assert 'English' in language_survey.responses

def test_store_three_responses(language_survey): # 使用同一个夹具
    responses = ['English', 'Spanish', 'Mandarin']
    for response in responses:
        language_survey.store_response(response)
    for response in responses:
        assert response in language_survey.responses
```

+ **优势**：极大地减少了测试代码的重复，让测试更简洁、更易于维护。

---

#### **本章总结**
第十一章让我认识到了**自动化测试**的巨大价值。编写测试虽然会增加一些初期工作量，但它能带来长期的回报：

+ **信心**：让我确信我的代码在各种情况下都能正常工作。
+ **安全网**：当我对代码进行修改或添加新功能时，运行测试可以立即发现是否无意中破坏了原有功能。
+ **更好的设计**：为了让代码更容易被测试，我需要思考如何将功能拆分成更小、更独立的单元（函数和方法），这本身就会促使我写出更模块化、更高质量的代码。

我掌握了使用`pytest`编写和运行**单元测试**的基本流程，理解了**断言**的核心作用，并学会了通过**夹具**来高效地组织我的测试用例。现在，我不仅是一个代码的“创造者”，也开始成为自己代码的“质检员”。


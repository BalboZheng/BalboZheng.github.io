<<<<<<< HEAD
<<<<<<< HEAD:_posts/2019-03-18-shortcut-keys.md
---
layout:     post
title:      "IntelliJ 快捷键"
subtitle:   "IntelliJ IDEA常用快捷键汇总"
date:       2019-03-18 12:30:11
author:     "Balbo"
header-img: "img/post-bg-2019.jpg"
tags:
    - idea
    - pycharm
---
## IDEA
在使用IntelliJ Idea的时候，使用快捷键是必不可少的。掌握一些常用的快捷键能大大提高我们的开发效率。有些快捷键可以熟练的使用，但是还有另外一些快捷键虽然很好用，但是由于因为没有形成使用习惯或者没有理解快捷键的用法，甚至之前对一些快捷键根本没有概念，导致不会去使用。对于这些快捷键，如果能够用好，编辑代码的效率必能提高一个水平。所以在此梳理出来，加强自己的使用，形成习惯。
（注：有些操作的快捷键做了更改，和IntelliJ Idea默认的快捷键不一样）


| 动作                         | 快捷键        | 说明                                                         |
| ------------------------------ | ---------------- | -------------------------------------------------------------- |
| Move Caret to Code Block End   | Ctrl+]           | 诸如{}围起来的代码块，使用该快捷键可以快速跳转至代码块的结尾处 |
| Move Caret to Code Block Start | Ctrl+[           | 同上，快速跳至代码块的开始出                     |
| Complete Current Statement     | Ctrl+Shift+Enter | 将输入的if、for、函数等等补上{}或者;使代码语句完整 |
| Start New Line                 | Shift+Enter      | 在当前行的下方开始新行                              |
| Start New Line Before Current  | Ctrl+Delete      | 删除光标所在至单词结尾处的所有字符            |
| Delete to Word Start           | Ctrl+BackSpace   | 删除光标所在至单词开头的所有字符               |
| Move Caret to Previous Word    | Ctrl+向左箭头 | 将光标移至前一个单词                                 |
| Move Caret to Next Word        | Ctrl+向右箭头 | 将光标移至后一个单词                                 |
| Scroll Up                      | Ctrl+向上箭头 | 向上滚动一行                                             |
| Scroll Down                    | Ctrl+向下箭头 | 向下滚动一行                                             |
| Extend Selection               | Ctrl+W           | 选中整个单词                                             |
| Toggle Case                    | Ctrl+Shift+U     | 切换大小写                                                |

### Edit 

| 动作     | 快捷键    | 说明               |
| ---------- | ------------ | -------------------- |
| Undo       | Ctrl+Z       | 撤销               |
| Redo       | Ctrl+Shift+Z | 重做               |
| Cut        | Ctrl+X       | 剪切               |
| Copy       | Ctrl+C       | 复制               |
| Paste      | Ctrl+V       | 粘贴               |
| Join Lines | Ctrl+Shift+J | 将选中的行合并成一行 |

### Find

| 动作                   | 快捷键     | 说明                               |
| ------------------------ | ------------- | ------------------------------------ |
| Find                     | Ctrl+F        | 在当前文件中查找             |
| Replace                  | Ctrl+R        | 替换字符串                      |
| Find in Path             | Ctrl+Shift+F  | 在全局文件中查找字符串    |
| Replace in Path          | Ctrl+Shift+R  | 在全局中替换字符串          |
| Find Usages              | Alt+F7        | 查找当前变量的使用，并列表显示 |
| Show Usages              | Ctrl+Alt+F7   | 查找当前变量的使用，并直接对话框显示 |
| Find Usages in File      | Ctrl+F7       | 在文件中查找符号的使用    |
| Highlight Usages in File | Ctrl+Shift+F7 | 在文件中高亮显示变量的使用 |

这里的快捷键用的频率还是很高的，但是之前用的最多的是Ctrl+F和Ctrl+Shift+F，后面相关的Find Usages基本上没有用过，后面应该多使用，有的时候相对Ctrl+F在文件内按字符串查找，还是更好用一些
### Navigate

| 动作                     | 快捷键            | 说明                                               |
| -------------------------- | -------------------- | ---------------------------------------------------- |
| Class…                   | Ctrl+N               | 查找类文件                                      |
| File…                    | Ctrl+Shift+N         | 查找文件                                         |
| Line…                    | Ctrl+G               | 定位到文件某一行                             |
| Back                       | Alt+向左箭头     | 返回至上次光标位置                          |
| Forward                    | Alt+向右箭头     | 返回至后一次光标位置                       |
| Last Edit Location         | Ctrl+Shift+Backspace | 返回上次编辑位置                             |
| Next Edit Location         | Ctrl+Shift+反斜杠 | 返回后一次编辑位置                          |
| Declaration                | Ctrl+B               | 定位至变量定义的位置                       |
| Implementation(s)          | Ctrl+Alt+B           | 定位至选中类或方法的具体实现           |
| Type Declaration           | Ctrl+Shift+B         | 直接定位至光标所在变量的类型定义     |
| Super Method               | Ctrl+U               | 直接定位至当前方法override或者implements的方法定义处 |
| File Structure             | Ctrl+F12             | 显示当前文件的文件结构                    |
| File Path                  | Ctrl+Alt+F12         | 显示当前文件的路径，并可以方便的将相关父路径打开 |
| Type Hierarchy             | Ctrl+H               | 显示当前类的继承层次                       |
| Method Hierarchy           | Ctrl+Shift+H         | 显示当前方法的继承层次                    |
| Call Hierarchy             | Ctrl+Alt+H           | 显示当前方法的调用层次                    |
| Next Highlighted Error     | F2                   | 定位至下一个错误处                          |
| Previous Highlighted Error | Shift+F2             | 定位至前一个错误处                          |
| Previous Occurrence        | Ctrl+Alt+向上箭头 | 查找前一个变量共现的地方                 |
| Next Occurrence            | Ctrl+Alt+向下箭头 | 查找下一个变量共现的地方                 |

目前还不知道Previous Occurrence 和 Next Occurrence是怎么用的，在变量上使用没有反应。不过在Edit–Find菜单下有几个菜单项：Find Next \/ Move to Next Occurrence、Find Previous \/ Move to Previous Occurrence等。当选中变量的时候，需要首先点击“Find Word at Caret”，然后再点击上述选项才有用
### Code

| 动作             | 快捷键         | 说明                                     |
| ------------------ | ----------------- | ------------------------------------------ |
| Override Methods… | Ctrl+O            | 重写基类的方法                      |
| Implement Methods… | Ctrl+I            | 实现基类或接口中的方法          |
| Generate…        | Alt+Insert        | 产生构造方法、getter/setter等方法 |
| Surround With…   | Ctrl+Alt+T        | 将选中的代码使用if、while、try/catch等包装 |
| Unwrap/Remove…   | Ctrl+Shift+Delete | 去除相关的包装代码                |

### Completion

| 动作    | 快捷键 | 说明       |
| --------- | --------- | ------------ |
| Basic     | Alt+/     | 自动完成 |
| SmartType | Alt+Enter | 自动提示完成 |

### Folding

| 动作               | 快捷键    | 说明       |
| -------------------- | ------------ | ------------ |
| Expand               | Ctrl+=       | 展开代码 |
| Collapse             | Ctrl+-       | 展开代码 |
| Expand Recursively   | Ctrl+Alt+=   | 递归展开代码 |
| Collapse Recursively | Ctrl+Alt+-   | 递归收缩代码 |
| Expand All           | Ctrl+Shift+= | 展开所有代码 |
| Collapse All         | Ctrl+Shift+- | 收缩所有代码 |

### Refactor

| 动作           | 快捷键     | 说明       |
| ---------------- | ------------- | ------------ |
| Rename           | Shift+F6      | 重命名    |
| Change Signature | Ctrl+F6       | 更改函数签名 |
| Change Signature | Ctrl+Shift+F6 | 更改函数签名 |

### Other

| 动作                      | 快捷键    | 说明                |
| --------------------------- | ------------ | --------------------- |
| Insert Live Template        | Ctrl+J       | 插入Live Template   |
| Surround with Live Template | Ctrl+Alt+J   | 使用Live Template包装 |
| Comment with Line Comment   | Ctrl+/       | 使用//进行注释  |
| Comment with Line Comment   | Ctrl+Shift+/ | 使用/**/进行注释 |
| Reformat Code               | Ctrl+Alt+L   | 格式化代码       |
| Auto-Indent Lines           | Ctrl+Alt+I   | 自动缩进行       |
| Optimize Imports            | Ctrl+Alt+O   | 优化import          |

| 动作              | 快捷键               | 说明                         |
| ------------------- | ----------------------- | ------------------------------ |
| Move Statement Down | Ctrl+Shift+向下箭头 | 将光标所在的代码块向下整体移动 |
| Move Statement Up   | Ctrl+Shift+向上箭头 | 将光标所在的代码块向上移动 |
| Move Element Left   | Ctrl+Alt+Shift+向左箭头 | 将元素向左移动          |
| Move Element Right  | Ctrl+Alt+Shift+向右箭头 | 将元素向右移动          |
| Move Line Down      | Alt+Shift+向下箭头  | 将行向下移动             |
| Move Line Up        | Alt+Shift+向上箭头  | 将行向上移动             |
---
## PYCHARM
### 编辑（Editing）
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| Ctrl + Space | 基本的代码完成（类、方法、属性） | Ctrl + Alt + I | 自动缩进 |
| Ctrl + Alt + Space | 快速导入任意类 | Tab / Shift + Tab | 缩进、不缩进当前行 ||
| Ctrl + Shift + Enter | 语句完成 | Ctrl+X/Shift+Delete | 剪切当前行或选定的代码块到剪贴板 |
| Ctrl + P | 参数信息（在方法中调用参数） | Ctrl+C/Ctrl+Insert | 复制当前行或选定的代码块到剪贴板 |
| Ctrl + Q | 快速查看文档 | Ctrl+V/Shift+Insert | 从剪贴板粘贴 |
| Shift + F1 | 外部文档 | Ctrl + Shift + V | 从最近的缓冲区粘贴 |
| Ctrl + 鼠标 | 简介 | Ctrl + D | 复制选定的区域或行 |
| Ctrl + F1 | 显示错误描述或警告信息 | Ctrl + Y | 删除选定的行 |
| Alt + Insert | 自动生成代码 | Ctrl + Shift + J | 添加智能线 |
| Ctrl + O | 重新方法 | Ctrl + Enter | 智能线切割 |
| Ctrl + Alt + T | 选中 | Shift + Enter | 另起一行 |
| Ctrl + / | 行注释 | Ctrl + Shift + U | 在选定的区域或代码块间切换 |
| Ctrl + Shift + / | 块注释 | Ctrl + Delete | 删除到字符结束 |
| Ctrl + W | 选中增加的代码块 | Ctrl + Backspace | 删除到字符开始 |
| Ctrl + Shift + W | 回到之前状态 | Ctrl + Numpad+/- | 展开折叠代码块 |
| Ctrl + Shift + ]/[ | 选定代码块结束、开始 | Ctrl + Numpad+ | 全部展开 |
| Alt + Enter | 快速修正 | Ctrl + Numpad- | 全部折叠 |
| Ctrl + Alt + L | 代码格式化 | Ctrl + F4 | 关闭运行的选项卡 |
| Ctrl + Alt + O | 优化导入 |

### 查找/替换(Search/Replace)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| F3 | 下一个 | Ctrl + Shift + F | 全局查找 |
| Shift + F3 | 前一个 | Ctrl + Shift + R | 全局替换 |
| Ctrl + R | 替换 |

### 运行(Running)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| Alt + Shift + F10 | 运行模式配置 | Shift + F9 | 调试 |
| Alt + Shift + F9 | 调试模式配置 | Ctrl + Shift + F10 | 运行编辑器配置 |
| Shift + F10 | 运行 | Ctrl + Alt + R | 运行manage.py任务 |

### 调试(Debugging)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| F8 | 跳过 | Ctrl + Alt + F8 | 快速验证表达式 |
| F7 | 进入 | F9 | 恢复程序 |
| Shift + F8 | 退出 | Ctrl + F8 | 断点开关 |
| Alt + F9 | 运行游标 | Ctrl + Shift + F8 | 查看断点 |
| Alt + F8 | 验证表达式 |

### 导航(Navigation)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| Ctrl + N | 跳转到类 | Ctrl + Shift + B | 跳转到类型声明 |
| Ctrl + Shift + N | 跳转到符号 | Ctrl + U | 跳转到父方法、父类 |
| Alt + Right/Left | 跳转到下一个、前一个编辑的选项卡 | Alt + Up/Down | 跳转到上一个、下一个方法 |
| F12 | 回到先前的工具窗口 | Ctrl + ]/[ | 跳转到代码块结束、开始 |
| Esc | 从工具窗口回到编辑窗口 | Ctrl + F12 | 弹出文件结构 |
| Shift + Esc | 隐藏运行的、最近运行的窗口 | Ctrl + H | 类型层次结构 |
| Ctrl + Shift + F4 | 关闭主动运行的选项卡 | Ctrl + Shift + H | 方法层次结构 |
| Ctrl + G | 查看当前行号、字符号 | Ctrl + Alt + H | 调用层次结构 |
| Ctrl + E | 当前文件弹出 | F2 / Shift + F2 | 下一条、前一条高亮的错误 |
| Ctrl+Alt+Left/Right | 后退、前进 | F4 / Ctrl + Enter | 编辑资源、查看资源 |
| Ctrl+Shift+Backspace | 导航到最近编辑区域 | Alt + Home | 显示导航条F11书签开关 |
| Alt + F1 | 查找当前文件或标识 | Ctrl + Shift + F11 | 书签助记开关 |
| Ctrl+B / Ctrl+Click | 跳转到声明 | Ctrl + #[0-9] | 跳转到标识的书签 |
| Ctrl + Alt + B | 跳转到实现 | Shift + F11 | 显示书签 |
| Ctrl + Shift + I | 查看快速定义 |  |  |

### 搜索相关(Usage Search)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| Alt + F7/Ctrl + F7 | 文件中查询用法 | Ctrl + Alt + F7 | 显示用法 |
| Ctrl + Shift + F7 | 文件中用法高亮显示 |

### 重构(Refactoring)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| F5 | 复制 | Ctrl + Alt + M | 提取方法 |
| F6 | 剪切 | Ctrl + Alt + V | 提取属性 |
| Alt + Delete | 安全删除 | Ctrl + Alt + F | 提取字段 |
| Shift + F6 | 重命名 | Ctrl + Alt + C | 提取常量 |
| Ctrl + F6 | 更改签名 | Ctrl + Alt + P | 提取参数 |
| Ctrl + Alt + N | 内联 |  |  |

### 控制VCS/Local History
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| Ctrl + K | 提交项目 | Alt + Shift + C | 查看最近的变化 |
| Ctrl + T | 更新项目 | Alt + BackQuote(’)VCS | 快速弹出 |

### 模版(Live Templates)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| Ctrl + Alt + J | 当前行使用模版 | Ctrl +Ｊ | 插入模版 |

### 基本(General)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| Alt + #[0-9] | 打开相应的工具窗口 | Ctrl + BackQuote(’) | 快速切换当前计划 |
| Ctrl + Alt + Y | 同步 | Ctrl + Alt + S | 打开设置页 |
| Ctrl + Shift + F12 | 最大化编辑开关 | Ctrl + Shift + A | 查找编辑器里所有的动作 |
| Alt + Shift + F | 添加到最喜欢 | Ctrl + Tab | 在窗口间进行切换 |
| Alt + Shift + I | 根据配置检查当前文件 |

### 一些常用设置：
1. pycharm默认是自动保存的，习惯自己按ctrl + s 的可以进行如下设置：

    1. file -> Setting -> General -> Synchronization -> Save files on frame deactivation 和 Save files automatically if application is idle for .. sec 的勾去掉
    2. file ->Setting -> Editor -> Editor Tabs -> Mark modified tabs with asterisk 打上勾
    
2. Alt + Enter: 自动添加包
3. 对于常用的快捷键，可以设置为visual studio(eclipse...)一样的：file -> Setting -> Keymap -> Keymaps -> vuisual studio -> Apply
4. Pycharm中默认是不能用Ctrl+滚轮改变字体大小的，可以在file -> Setting ->Editor-〉Mouse中设置
5. 要设置Pycharm的字体，要先在file -> Setting ->Editor-〉Editor中选择一种风格并保存，然后才可以改变
6. 在setting中搜索theme可以改变主题，所有配色统一改变
=======
---
layout:     post
title:      "IntelliJ 快捷键"
subtitle:   "IntelliJ IDEA常用快捷键汇总"
date:       2019-03-18 12:30:11
author:     "Balbo"
header-img: "img/post-bg-2019.jpg"
catalog: true
tags:
    - idea
    - pycharm
---
=======
---
layout: post
title: IDE 快捷键
subtitle: IntelliJ IDEA常用快捷键汇总
author: Balbo Cheng
categories: 
tags: [idea, pycharm, webstorm]
---


>>>>>>> e5c77ca (新页面)
## IDEA
在使用IntelliJ Idea的时候，使用快捷键是必不可少的。掌握一些常用的快捷键能大大提高我们的开发效率。有些快捷键可以熟练的使用，但是还有另外一些快捷键虽然很好用，但是由于因为没有形成使用习惯或者没有理解快捷键的用法，甚至之前对一些快捷键根本没有概念，导致不会去使用。对于这些快捷键，如果能够用好，编辑代码的效率必能提高一个水平。所以在此梳理出来，加强自己的使用，形成习惯。
（注：有些操作的快捷键做了更改，和IntelliJ Idea默认的快捷键不一样）


| 动作                         | 快捷键        | 说明                                                         |
| ------------------------------ | ---------------- | -------------------------------------------------------------- |
| Move Caret to Code Block End   | Ctrl+]           | 诸如{}围起来的代码块，使用该快捷键可以快速跳转至代码块的结尾处 |
| Move Caret to Code Block Start | Ctrl+[           | 同上，快速跳至代码块的开始出                     |
| Complete Current Statement     | Ctrl+Shift+Enter | 将输入的if、for、函数等等补上{}或者;使代码语句完整 |
| Start New Line                 | Shift+Enter      | 在当前行的下方开始新行                              |
| Start New Line Before Current  | Ctrl+Delete      | 删除光标所在至单词结尾处的所有字符            |
| Delete to Word Start           | Ctrl+BackSpace   | 删除光标所在至单词开头的所有字符               |
| Move Caret to Previous Word    | Ctrl+向左箭头 | 将光标移至前一个单词                                 |
| Move Caret to Next Word        | Ctrl+向右箭头 | 将光标移至后一个单词                                 |
| Scroll Up                      | Ctrl+向上箭头 | 向上滚动一行                                             |
| Scroll Down                    | Ctrl+向下箭头 | 向下滚动一行                                             |
| Extend Selection               | Ctrl+W           | 选中整个单词                                             |
| Toggle Case                    | Ctrl+Shift+U     | 切换大小写                                                |

### Edit 

| 动作     | 快捷键    | 说明               |
| ---------- | ------------ | -------------------- |
| Undo       | Ctrl+Z       | 撤销               |
| Redo       | Ctrl+Shift+Z | 重做               |
| Cut        | Ctrl+X       | 剪切               |
| Copy       | Ctrl+C       | 复制               |
| Paste      | Ctrl+V       | 粘贴               |
| Join Lines | Ctrl+Shift+J | 将选中的行合并成一行 |

### Find

| 动作                   | 快捷键     | 说明                               |
| ------------------------ | ------------- | ------------------------------------ |
| Find                     | Ctrl+F        | 在当前文件中查找             |
| Replace                  | Ctrl+R        | 替换字符串                      |
| Find in Path             | Ctrl+Shift+F  | 在全局文件中查找字符串    |
| Replace in Path          | Ctrl+Shift+R  | 在全局中替换字符串          |
| Find Usages              | Alt+F7        | 查找当前变量的使用，并列表显示 |
| Show Usages              | Ctrl+Alt+F7   | 查找当前变量的使用，并直接对话框显示 |
| Find Usages in File      | Ctrl+F7       | 在文件中查找符号的使用    |
| Highlight Usages in File | Ctrl+Shift+F7 | 在文件中高亮显示变量的使用 |

这里的快捷键用的频率还是很高的，但是之前用的最多的是Ctrl+F和Ctrl+Shift+F，后面相关的Find Usages基本上没有用过，后面应该多使用，有的时候相对Ctrl+F在文件内按字符串查找，还是更好用一些
### Navigate

| 动作                     | 快捷键            | 说明                                               |
| -------------------------- | -------------------- | ---------------------------------------------------- |
| Class…                   | Ctrl+N               | 查找类文件                                      |
| File…                    | Ctrl+Shift+N         | 查找文件                                         |
| Line…                    | Ctrl+G               | 定位到文件某一行                             |
| Back                       | Alt+向左箭头     | 返回至上次光标位置                          |
| Forward                    | Alt+向右箭头     | 返回至后一次光标位置                       |
| Last Edit Location         | Ctrl+Shift+Backspace | 返回上次编辑位置                             |
| Next Edit Location         | Ctrl+Shift+反斜杠 | 返回后一次编辑位置                          |
| Declaration                | Ctrl+B               | 定位至变量定义的位置                       |
| Implementation(s)          | Ctrl+Alt+B           | 定位至选中类或方法的具体实现           |
| Type Declaration           | Ctrl+Shift+B         | 直接定位至光标所在变量的类型定义     |
| Super Method               | Ctrl+U               | 直接定位至当前方法override或者implements的方法定义处 |
| File Structure             | Ctrl+F12             | 显示当前文件的文件结构                    |
| File Path                  | Ctrl+Alt+F12         | 显示当前文件的路径，并可以方便的将相关父路径打开 |
| Type Hierarchy             | Ctrl+H               | 显示当前类的继承层次                       |
| Method Hierarchy           | Ctrl+Shift+H         | 显示当前方法的继承层次                    |
| Call Hierarchy             | Ctrl+Alt+H           | 显示当前方法的调用层次                    |
| Next Highlighted Error     | F2                   | 定位至下一个错误处                          |
| Previous Highlighted Error | Shift+F2             | 定位至前一个错误处                          |
| Previous Occurrence        | Ctrl+Alt+向上箭头 | 查找前一个变量共现的地方                 |
| Next Occurrence            | Ctrl+Alt+向下箭头 | 查找下一个变量共现的地方                 |

目前还不知道Previous Occurrence 和 Next Occurrence是怎么用的，在变量上使用没有反应。不过在Edit–Find菜单下有几个菜单项：Find Next \/ Move to Next Occurrence、Find Previous \/ Move to Previous Occurrence等。当选中变量的时候，需要首先点击“Find Word at Caret”，然后再点击上述选项才有用
### Code

| 动作             | 快捷键         | 说明                                     |
| ------------------ | ----------------- | ------------------------------------------ |
| Override Methods… | Ctrl+O            | 重写基类的方法                      |
| Implement Methods… | Ctrl+I            | 实现基类或接口中的方法          |
| Generate…        | Alt+Insert        | 产生构造方法、getter/setter等方法 |
| Surround With…   | Ctrl+Alt+T        | 将选中的代码使用if、while、try/catch等包装 |
| Unwrap/Remove…   | Ctrl+Shift+Delete | 去除相关的包装代码                |

### Completion

| 动作    | 快捷键 | 说明       |
| --------- | --------- | ------------ |
| Basic     | Alt+/     | 自动完成 |
| SmartType | Alt+Enter | 自动提示完成 |

### Folding

| 动作               | 快捷键    | 说明       |
| -------------------- | ------------ | ------------ |
| Expand               | Ctrl+=       | 展开代码 |
| Collapse             | Ctrl+-       | 展开代码 |
| Expand Recursively   | Ctrl+Alt+=   | 递归展开代码 |
| Collapse Recursively | Ctrl+Alt+-   | 递归收缩代码 |
| Expand All           | Ctrl+Shift+= | 展开所有代码 |
| Collapse All         | Ctrl+Shift+- | 收缩所有代码 |

### Refactor

| 动作           | 快捷键     | 说明       |
| ---------------- | ------------- | ------------ |
| Rename           | Shift+F6      | 重命名    |
| Change Signature | Ctrl+F6       | 更改函数签名 |
| Change Signature | Ctrl+Shift+F6 | 更改函数签名 |

### Other

| 动作                      | 快捷键    | 说明                |
| --------------------------- | ------------ | --------------------- |
| Insert Live Template        | Ctrl+J       | 插入Live Template   |
| Surround with Live Template | Ctrl+Alt+J   | 使用Live Template包装 |
| Comment with Line Comment   | Ctrl+/       | 使用//进行注释  |
| Comment with Line Comment   | Ctrl+Shift+/ | 使用/**/进行注释 |
| Reformat Code               | Ctrl+Alt+L   | 格式化代码       |
| Auto-Indent Lines           | Ctrl+Alt+I   | 自动缩进行       |
| Optimize Imports            | Ctrl+Alt+O   | 优化import          |

| 动作              | 快捷键               | 说明                         |
| ------------------- | ----------------------- | ------------------------------ |
| Move Statement Down | Ctrl+Shift+向下箭头 | 将光标所在的代码块向下整体移动 |
| Move Statement Up   | Ctrl+Shift+向上箭头 | 将光标所在的代码块向上移动 |
| Move Element Left   | Ctrl+Alt+Shift+向左箭头 | 将元素向左移动          |
| Move Element Right  | Ctrl+Alt+Shift+向右箭头 | 将元素向右移动          |
| Move Line Down      | Alt+Shift+向下箭头  | 将行向下移动             |
| Move Line Up        | Alt+Shift+向上箭头  | 将行向上移动             |
<<<<<<< HEAD
---
## PYCHARM
### 编辑（Editing）
=======

## PYCHARM
### 编辑（Editing）

>>>>>>> e5c77ca (新页面)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| Ctrl + Space | 基本的代码完成（类、方法、属性） | Ctrl + Alt + I | 自动缩进 |
| Ctrl + Alt + Space | 快速导入任意类 | Tab / Shift + Tab | 缩进、不缩进当前行 ||
| Ctrl + Shift + Enter | 语句完成 | Ctrl+X/Shift+Delete | 剪切当前行或选定的代码块到剪贴板 |
| Ctrl + P | 参数信息（在方法中调用参数） | Ctrl+C/Ctrl+Insert | 复制当前行或选定的代码块到剪贴板 |
| Ctrl + Q | 快速查看文档 | Ctrl+V/Shift+Insert | 从剪贴板粘贴 |
| Shift + F1 | 外部文档 | Ctrl + Shift + V | 从最近的缓冲区粘贴 |
| Ctrl + 鼠标 | 简介 | Ctrl + D | 复制选定的区域或行 |
| Ctrl + F1 | 显示错误描述或警告信息 | Ctrl + Y | 删除选定的行 |
| Alt + Insert | 自动生成代码 | Ctrl + Shift + J | 添加智能线 |
| Ctrl + O | 重新方法 | Ctrl + Enter | 智能线切割 |
| Ctrl + Alt + T | 选中 | Shift + Enter | 另起一行 |
| Ctrl + / | 行注释 | Ctrl + Shift + U | 在选定的区域或代码块间切换 |
| Ctrl + Shift + / | 块注释 | Ctrl + Delete | 删除到字符结束 |
| Ctrl + W | 选中增加的代码块 | Ctrl + Backspace | 删除到字符开始 |
| Ctrl + Shift + W | 回到之前状态 | Ctrl + Numpad+/- | 展开折叠代码块 |
| Ctrl + Shift + ]/[ | 选定代码块结束、开始 | Ctrl + Numpad+ | 全部展开 |
| Alt + Enter | 快速修正 | Ctrl + Numpad- | 全部折叠 |
| Ctrl + Alt + L | 代码格式化 | Ctrl + F4 | 关闭运行的选项卡 |
| Ctrl + Alt + O | 优化导入 |

### 查找/替换(Search/Replace)
<<<<<<< HEAD
=======

>>>>>>> e5c77ca (新页面)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| F3 | 下一个 | Ctrl + Shift + F | 全局查找 |
| Shift + F3 | 前一个 | Ctrl + Shift + R | 全局替换 |
| Ctrl + R | 替换 |

### 运行(Running)
<<<<<<< HEAD
=======

>>>>>>> e5c77ca (新页面)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| Alt + Shift + F10 | 运行模式配置 | Shift + F9 | 调试 |
| Alt + Shift + F9 | 调试模式配置 | Ctrl + Shift + F10 | 运行编辑器配置 |
| Shift + F10 | 运行 | Ctrl + Alt + R | 运行manage.py任务 |

### 调试(Debugging)
<<<<<<< HEAD
=======

>>>>>>> e5c77ca (新页面)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| F8 | 跳过 | Ctrl + Alt + F8 | 快速验证表达式 |
| F7 | 进入 | F9 | 恢复程序 |
| Shift + F8 | 退出 | Ctrl + F8 | 断点开关 |
| Alt + F9 | 运行游标 | Ctrl + Shift + F8 | 查看断点 |
| Alt + F8 | 验证表达式 |

### 导航(Navigation)
<<<<<<< HEAD
=======

>>>>>>> e5c77ca (新页面)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| Ctrl + N | 跳转到类 | Ctrl + Shift + B | 跳转到类型声明 |
| Ctrl + Shift + N | 跳转到符号 | Ctrl + U | 跳转到父方法、父类 |
| Alt + Right/Left | 跳转到下一个、前一个编辑的选项卡 | Alt + Up/Down | 跳转到上一个、下一个方法 |
| F12 | 回到先前的工具窗口 | Ctrl + ]/[ | 跳转到代码块结束、开始 |
| Esc | 从工具窗口回到编辑窗口 | Ctrl + F12 | 弹出文件结构 |
| Shift + Esc | 隐藏运行的、最近运行的窗口 | Ctrl + H | 类型层次结构 |
| Ctrl + Shift + F4 | 关闭主动运行的选项卡 | Ctrl + Shift + H | 方法层次结构 |
| Ctrl + G | 查看当前行号、字符号 | Ctrl + Alt + H | 调用层次结构 |
| Ctrl + E | 当前文件弹出 | F2 / Shift + F2 | 下一条、前一条高亮的错误 |
| Ctrl+Alt+Left/Right | 后退、前进 | F4 / Ctrl + Enter | 编辑资源、查看资源 |
| Ctrl+Shift+Backspace | 导航到最近编辑区域 | Alt + Home | 显示导航条F11书签开关 |
| Alt + F1 | 查找当前文件或标识 | Ctrl + Shift + F11 | 书签助记开关 |
| Ctrl+B / Ctrl+Click | 跳转到声明 | Ctrl + #[0-9] | 跳转到标识的书签 |
| Ctrl + Alt + B | 跳转到实现 | Shift + F11 | 显示书签 |
| Ctrl + Shift + I | 查看快速定义 |  |  |

### 搜索相关(Usage Search)
<<<<<<< HEAD
=======

>>>>>>> e5c77ca (新页面)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| Alt + F7/Ctrl + F7 | 文件中查询用法 | Ctrl + Alt + F7 | 显示用法 |
| Ctrl + Shift + F7 | 文件中用法高亮显示 |

### 重构(Refactoring)
<<<<<<< HEAD
=======

>>>>>>> e5c77ca (新页面)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| F5 | 复制 | Ctrl + Alt + M | 提取方法 |
| F6 | 剪切 | Ctrl + Alt + V | 提取属性 |
| Alt + Delete | 安全删除 | Ctrl + Alt + F | 提取字段 |
| Shift + F6 | 重命名 | Ctrl + Alt + C | 提取常量 |
| Ctrl + F6 | 更改签名 | Ctrl + Alt + P | 提取参数 |
| Ctrl + Alt + N | 内联 |  |  |

### 控制VCS/Local History
<<<<<<< HEAD
=======

>>>>>>> e5c77ca (新页面)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| Ctrl + K | 提交项目 | Alt + Shift + C | 查看最近的变化 |
| Ctrl + T | 更新项目 | Alt + BackQuote(’)VCS | 快速弹出 |

### 模版(Live Templates)
<<<<<<< HEAD
=======

>>>>>>> e5c77ca (新页面)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| Ctrl + Alt + J | 当前行使用模版 | Ctrl +Ｊ | 插入模版 |

### 基本(General)
<<<<<<< HEAD
=======

>>>>>>> e5c77ca (新页面)
| 命令 | 表述 | 命令 | 表述 |
| ---- | ---- | ---- | ---- |
| Alt + #[0-9] | 打开相应的工具窗口 | Ctrl + BackQuote(’) | 快速切换当前计划 |
| Ctrl + Alt + Y | 同步 | Ctrl + Alt + S | 打开设置页 |
| Ctrl + Shift + F12 | 最大化编辑开关 | Ctrl + Shift + A | 查找编辑器里所有的动作 |
| Alt + Shift + F | 添加到最喜欢 | Ctrl + Tab | 在窗口间进行切换 |
| Alt + Shift + I | 根据配置检查当前文件 |

### 一些常用设置：
<<<<<<< HEAD
=======

>>>>>>> e5c77ca (新页面)
1. pycharm默认是自动保存的，习惯自己按ctrl + s 的可以进行如下设置：

    1. file -> Setting -> General -> Synchronization -> Save files on frame deactivation 和 Save files automatically if application is idle for .. sec 的勾去掉
    2. file ->Setting -> Editor -> Editor Tabs -> Mark modified tabs with asterisk 打上勾
    
2. Alt + Enter: 自动添加包
3. 对于常用的快捷键，可以设置为visual studio(eclipse...)一样的：file -> Setting -> Keymap -> Keymaps -> vuisual studio -> Apply
4. Pycharm中默认是不能用Ctrl+滚轮改变字体大小的，可以在file -> Setting ->Editor-〉Mouse中设置
5. 要设置Pycharm的字体，要先在file -> Setting ->Editor-〉Editor中选择一种风格并保存，然后才可以改变
<<<<<<< HEAD
6. 在setting中搜索theme可以改变主题，所有配色统一改变
>>>>>>> f0bfe13 (new update):_posts/知识点/2019-03-18-shortcut-keys.md
=======
6. 在setting中搜索theme可以改变主题，所有配色统一改变
>>>>>>> e5c77ca (新页面)

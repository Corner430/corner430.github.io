---
title: if __name__ == '__main__'
date: 2023-07-14 20:15:28
tags:
    - Python
declare: true
---
在Python中，`if __name__ == "__main__"`的作用是判断当前模块是否作为主程序直接运行，而不是作为被导入的模块。<!--more-->

当一个Python文件被直接执行时，Python解释器会将该文件的`__name__`属性设置为`"__main__"`。而当一个Python文件被作为模块导入时，`__name__`属性会被设置为该模块的名称。

因此，通过使用`if __name__ == "__main__"`条件语句，我们可以将一些代码块限定为仅在该文件作为主程序运行时才执行，而在被导入时不执行。这样可以实现一些特定的逻辑，例如在模块被导入时只定义函数和类，而在作为主程序运行时执行一些测试或执行主要逻辑。

下面是一个示例代码，演示了`if __name__ == "__main__"`的使用：

```python
# 模块代码

def foo():
    print("函数 foo() 被调用")

class MyClass:
    def __init__(self, name):
        self.name = name

    def greet(self):
        print(f"Hello, {self.name}!")

if __name__ == "__main__":
    # 仅在作为主程序运行时执行的代码
    print("这是主程序")
    foo()
    obj = MyClass("Alice")
    obj.greet()
```

当该模块作为主程序运行时，输出为：

```
这是主程序
函数 foo() 被调用
Hello, Alice!
```

但如果该模块被其他模块导入时，`if __name__ == "__main__"`之后的代码块不会执行。这可以避免一些不必要的代码执行，同时保持模块的可导入性和可重用性。

综上所述，`if __name__ == "__main__"`的作用是提供一个保护块，使得部分代码仅在模块作为主程序运行时执行，而在被导入时不执行。这样可以增强模块的灵活性和可维护性。
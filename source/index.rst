Pocoo Style Guide 翻译
========================

Pocoo代码指导是面向所有的Pocoo项目的. 通常, Pocoo代码指导是紧密的遵循PEP8规范的, 只有一些小的概念和扩展.

----------
通常布局
----------

* 缩进:

    4个空格. 没有缩进(tab), 没有例外.

* 最大的行长度:

    79个字符, 如果确实需要的话, 84个也可以. 尝试避免过度的嵌套, 明智地选择\ ``break``\ , \ ``continue``\ 和\ ``return``\ .

* 很长的代码段:

    为了能够让你使用更长的语句, 你需要使用反斜杠(\\), 这个时候, 你应该在下一行的\ ``.``\ 或者\ ``=``\ 处对齐, 或者缩进4个空格::

        this_is_a_very_long(function_call, 'with many parameters') \
            .that_returns_an_object_with_an_attribute

        MyModel.query.filter(MyModel.scalar > 120) \
                     .order_by(MyModel.name.desc()) \
                     .limit(10)

    如果你使用括号或者逗号来换行, 把它们对齐到括号::

        this_is_a_very_long(function_call, 'with many parameters',
                            23, 42, 'and even more')

    对于有多个元素的列表和元组, 在括号处换行::

        items = [
            'this is the first', 'set of items', 'with more items',
            'to come in this line', 'like this'
        ]

* 空行:

    顶级的函数和类用两行来分隔开, 其它的都用一行. 不要使用很多空行来隔离逻辑区块. 例子::

        def hello(name):
            print 'Hello %s!' % name


        def goodbye(name):
            print 'See you %s.' % name


        class MyClass(object):
            """This is a simple docstring."""

            def __init__(self, name):
                self.name = name

            def get_annoying_name(self):
                return self.name.upper() + '!!!!111'

----------------
表达式和语句
----------------

* 通常的空格规则:

    * 一元运算符不需要空格.
    * 二元运算符前后都放一个空格.

    好的::

        exp = -1.05
        value = (item_value / item_count) * offset / exp
        value = my_list[index]
        value = my_dict['key']

    坏的::

        exp = - 1.05
        value = ( item_value / item_count ) * offset / exp
        value = (item_value/item_count)*offset/exp
        value=( item_value/item_count ) * offset/exp
        value = my_list[ index ]
        value = my_dict ['key']

* Yoda语句:

    永远不要用变量来和常量对比, 永远要用常量来和变量对比.

    好的::

        if method == 'md5':
            pass

    坏的::

        if 'md5' == method:
            pass

* 比较:

    * 对于任意类型: \ ``==``\ 和\ ``!=``\ 
    * 对于实例, 使用\ ``is``\ 和\ ``is not``\ (eg: \ ``foo is not None``\ )
    * 永远不要用任何东西和\ ``True``\ 和\ ``False``\ 来比较(比如, 永远不要这样做\ ``foo == False``\ , 用\ ``not foo``\ )

* 否定检查:

    使用\ ``foo not in bar``\ , 而不是\ ``not foo in bar``\ .

* 实例检查:

    使用\ ``isinstance(a, C)``\ , 而不是\ ``type(A) is C``\ , 但是尝试避免检查实例. 检查特点.

## 命名规范 ##

    * 类命名: \ ``CamelCase``\ , 首字母和缩略词保持大写字母(\ ``HTTPWriter``\ 不是\ ``HttpWriter``\ ).
    * 变量名: \ ``lowercase_with_underscores``\ , 小写字母+下划线.
    * 方法和函数名: `lowercase_with_underscores``\ , 小写字母+下划线.
    * 常量: \ ``UPPERCASE_WITH_UNDERSCORES``\ , 全部大写+下划线.
    * 预编译的正则表达式: \ ``name_re``\ .

保护的成员用一个下划线开头. 两个下划线保留给组件类.

On classes with keywords, trailing underscores are appended. Clashes with builtins are allowed and must not be resolved by appending an underline to the variable name. If the function needs to access a shadowed builtin, rebind the builtin to a different name instead.

函数和方法的参数:

    * 类方法: \ ``cls``\ 是第一个参数.
    * 对象方法: \ ``self``\ 是第一个参数.
    * Lambdas的参数必须被\ ``x``\ 代替, 比如\ ``display_name = property(lambda x: x.real_name or x.username)``\ .

-----------------
文档字符串
-----------------

* 注释的约定:

    所有的注释都需要用能够被Sphinx支持的格式来书写. 依赖于注释的函数的不同, 它们的布局有些不同. 如果只有1行, 前后2个注释的双引号在同一行. 否则, 文字和开始的注释符号在一起, 关闭的注释符号单独一行::

        def foo():
            """This is a simple docstring."""


        def bar():
            """This is a longer docstring with so much information in there
            that it spans three lines.  In this case, the closing triple quote
            is on its own line.
            """

    通常来说, 注释应该被分成很小的简介和详细信息, 如果需要的话, 用一个空行空开.

* 模组的头:

    模组的头由utf-8的编码声明(如果使用了非ASCII字母, 强烈建议使用声明)和一个标准的注释组成::

        # -*- coding: utf-8 -*-
        """
            package.module
            ~~~~~~~~~~~~~~

            A brief description goes here.

            :copyright: (c) YEAR by AUTHOR.
            :license: LICENSE_NAME, see LICENSE_FILE for more details.
        """

    请时刻记着合适的版权和License信息是一个合格的Flask插件的一部分.

-------------
注释
-------------

注释的规则和文档字符串很相似. 它们都需要用reStructruedText个规范来很好的格式化. 如果一个注释用户来标明一个属性, 在#后加一个冒号::

    class User(object):
        #: the name of the user as unicode string
        name = Column(String)
        #: the sha1 hash of the password + inline salt
        pw_hash = Column(String)

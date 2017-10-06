第十四章 面向对象
一、基本概念
1、类

2、对象

3、接口

4、抽象类
	抽象类，又称父类、超类，是对类的进一步抽象。抽象类只能被类继承，不能被实例化。
	抽象类、类和对象构成了程序描述的三级分类体系。

5、封装

6、继承

7、多态
	父类的通用行为可以被子类用更特殊的行为重写。


二、继承

三、混合

function Composition (target, source) {
    var desc  = Object.getOwnPropertyDescriptor;
    var prop  = Object.getOwnPropertyNames;
    var def_prop = Object.defineProperty;
 
    prop(source).forEach(
        function(key) {
            def_prop(target, key, desc(source, key))
        }
    )
    return target;
}


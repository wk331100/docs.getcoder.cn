@(Coder1.0)[简单|高性能|开发效率高|学习成本低|不依赖其他程序]

**现状：** 目前市面上PHP的框架数不胜数，FPM类的框架主要分为两大类：C编写的扩展，PHP编写的框架。 C编写的扩展目前比较流行的有： 鸟哥的Yaf，国外的Phalcon，和360 MISC的 ASF，这类框架主要特点就是运行速度快，比较适合用于逻辑简单的、并发要求比较高的业务场景。PHP编写的框架，目前Laravel系列半壁江山，还有其他的Yii、Symfony、TP、CI等。这类的框架有一个特点：大而全。或许这些的框架开发者们觉得，从速度上比不过C编写的框架，那么在功能上一定要胜出。所以，这类框架都有着非常丰富的组件，依赖注入等设计模式。但是随着版本的迭代，这类的框架发展的像是一个航母一样，越来越像Zend Framework。

**C编写的扩展：** 虽然运行速度比较快，但是安装繁琐，既要根据操作系统选择版本，也要编译安装PHP的扩展，操作起来麻烦。而且一旦业务系统大量操作数据库、缓存、API、等操作。这类框架的提升的那点性能，几乎就可以忽略不计。

**Composer：** 目前PHP最火的包管理器，目前大多数新框架都依赖于Composer。 

**所以：**  能否有一个PHP框架，除了PHP不依赖于任何扩展和程序，像很多年以前的框架那样，下载下来就能运行？ 
能否：没有那么多大多数场景都用不到的组件和功能，简单、快捷、开发效率高、学习成本低？

> Coder  就是这样的框架：没有其他依赖下载即用，简单学习成本低，开发效率高，运行性能强，服务化，API专用

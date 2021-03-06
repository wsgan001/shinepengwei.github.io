---
layout: post
title: C++中static和extern全局变量详解
category: "编程"
tag: "C++"
---
<p>本文介绍了C++中的全局变量问题，同时介绍了static和extern修饰全局变量的区别。
</p><p>Static主要是为了将变量声明为静态存储类别，以及限定变量的作用域，extern关键字是为了声明变量或者函数的作用域。
</p><h1>编译单元（模块）
</h1><p>将代码生成exe文件需要使用两步：
</p><p>1编译是把.cpp文件和对应的.h文件变异成obj文件。2连接是把obj文件连接成exe文件。
</p><p>一般来说，编译单元是指编译阶段生成的obj文件，一个CPP和.h文件共同组成了一个编译单元，一个工程由多个编译单元构成，每个obj文件包含了变量存储的相对地址。
</p><h1>声明和定义
</h1><p>声明只是给某一个变量或者函数起一个名字，告诉编译器有这么个东西，并没有实际的内存空间。但当函数或变量定义的时候，他在内存便产生了实际的物理空间。
</p><h1>Static关键字
</h1><p>Static关键字在对变量进行修饰的时候，主要有两个作用，1、表示静态存储，将变量存在静态数据区，生命周期为定义处至程序结束。2、控制变量或者函数只在文件作用域或者类作用域可以被使用。
</p><p>《程序猿面试宝典》中总结static关键词有以下作用，其中1、2、3对应了刚才提到的1和2：
</p><ol><li>函数体内static变量内存只被分配一次，也只被初始化一次，在以后的使用中，static变量维持上次的值。
</li><li>模块内（文件、函数体）的static变量（如某一文件中的全局变量）可以被模块内的所有函数访问，但是不能被模块外的函数访问，即限定作用域。
</li><li>模块内的Static函数也只能被模块内的函数调用。
</li><li>类中static成员变量属于整个类而不是某个对象。
</li><li>类中static成员函数属于整个类，不接受this指针，也只能访问类中的static成员变量。通过CLASS::Function()调用。
</li></ol><p><span style="color:red">在全局变量中，static主要是为了限定作用域。
</span></p><h2>全局变量/函数
</h2><p>全局变量定义在函数体外，可以被整个程序访问。但是如果仅仅是声明和定义了一个全局变量，其他变异模块并不知道这个变量，所以无法使用。方法就是在需要使用该全局变量的模块使用extern声明。
</p><p>全局变量的使用标准：
</p><p>一直以来，有一个有关全局变量定义的标准：
</p><p>1 )在这个文件中定义的全局变量不打算给别人用，那么你就将它定义为static全局变量吧！因为这样你不必担心其他文件也定义了一个同名变量，在连接的时候出现重定义。
</p><p>2） 如果你的全局变量是打算给其他文件使用的，那么就不要加上static，因为这样在其他文件中可以使用extern对该定义进行引用。
</p><p>3） static 和extern是不能同时用来修饰一个变量的，extern修饰表示该变量只是声明，声明它使用了其他文件的变量定义，static的修饰表示我这个变量（自己定义的），只能被当前文件访问。两者完全冲突，所以编译器会报错——'n'的声明中有相互冲突的限定符。
</p><p><span style="color:red">不加static修饰的变量和加了static的变量的区别是：没有static的全局变量可以通过extern被其他变异模块使用。
</span></p><p>
?</p><h1>Extern全局变量
</h1><p>Extern是声明变量可以在这个模块被访问，该变量在其他模块定义。<span style="color:red">Extern仅仅是一个声明，告知编译器此变量在可能其他变异模块定义。</span>
	</p><p>如果在两个不同的文件都需要使用某一全局变量，就可以使用extern关键字。
</p><p>首先在文件A中对全局变量进行定义和声明。
</p><p>在文件B中如果需要访问这个全局变量，则使用extern声明这个全局变量即可。在连接阶段，从A的目标代码中找到这个变量。
</p><p>使用extern变量的方法：
</p><p>在cpp文件中定义。
</p><p>在.h文件中用extern声明。
</p><p>所有需要使用该变量的文件中通过include"**.h"文件使用全局变量。
</p><p>
?</p><h1>Static全局变量
</h1><p>用static修饰全局变量需要注意以下几点：
</p><ol><li>Static和extern冲突，不可同时出现。
</li><li>Static变量的声明和定义同时进行。
</li><li>Static变量如果定义在.H文件中，并且在不同的.cpp文件include，编译器不会报错，但将生成不同的变量（不同的内存地址），只不过这些变量具有相同的名字和相同的内容。
</li><li>一般定义static全局变量时都将其定义在.cpp文件里面，原因见上一条。
</li></ol><p>
?</p><p>
?</p><p>
?</p>

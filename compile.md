## 编译原理

### 预处理	
负责头文件展开，宏替换，条件编译的选择，删除注释等工作
### 编译	
负载将预处理生成的文件，经过词法分析，语法分析，语义分析及优化后生成汇编文件
### 汇编	
是将汇编代码转换为机器可执行指令的过程
### 链接	
载根据目标文件及所需的库文件产生最终的可执行文件。链接主要解决了模块间的相互引用的问题，分为地址和空间分配，符号解析和重定位几个步骤。实际上在编译阶段生成目标文件时，会暂时搁置那些外部引用，而这些外部引用就是在链接时进行确定的。链接器在链接时，会根据符号名称去相应模块中寻找对应符号。待符号确定之后，链接器会重写之前那些未确定的符号的地址，这个过程就是重定位。

### 静态链接	

静态链接（Static Linking）是指在编译程序时，将程序所需的库文件直接链接到可执行文件中，使程序成为一个独立的、完整的可执行文件。这样，程序在运行时就不再需要动态链接库文件，可以直接运行。

静态链接有以下几个优缺点：

优点：

独立性：程序运行时不依赖外部库文件，可直接运行，便于部署和分发。
稳定性：程序与库文件的版本不会因为系统更新而发生变化，避免了版本不兼容的问题。
缺点：

体积较大：将所有需要的库文件都链接到可执行文件中，会导致文件体积变大。
内存占用较高：静态链接的程序在运行时，每个程序都会占用一份库文件的内存空间，不同程序之间无法共享库文件，导致内存占用较高。
更新不便：当库文件需要更新时，需要重新编译程序，无法做到实时更新。

### 动态链接	

动态链接（Dynamic Linking）是指在编译程序时，不将程序所需的库文件链接到可执行文件中，而是在程序运行时，由操作系统动态加载所需的库文件。这样，程序在运行时会依赖外部的动态链接库文件（如 DLL、SO 等）。

动态链接有以下几个优缺点：

优点：

体积较小：程序可执行文件中不包含库文件，体积较小，便于分发。
内存占用较低：动态链接的程序在运行时，可以共享同一个库文件的内存空间，节省内存资源。
更新便捷：当库文件需要更新时，只需替换库文件即可，无需重新编译程序，可以实现实时更新。
缺点：

依赖性：程序运行时需要依赖外部库文件，如果缺少相应的库文件，程序无法运行。
稳定性较低：程序与库文件的版本可能因为系统更新而发生变化，可能导致版本不兼容的问题。
总的来说，动态链接和静态链接各有优缺点，具体使用哪种链接方式需要根据实际需求和场景进行选择。

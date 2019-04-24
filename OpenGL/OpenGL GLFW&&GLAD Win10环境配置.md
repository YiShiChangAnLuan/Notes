GLFW
---

- 从[GLFW](https://www.glfw.org/download.html)中下载预编译二进制文件，因为尝试过编译源代码后报错，所以放弃编译源代码。  
![picture 1](https://github.com/YiShiChangAnLuan/images/blob/master/OpenGL%20GLFW%26%26GLAD%20Win10%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE/1.png)  

- 之前我配置的是32位的，现在尝试64位的。不过建议使用32位，据说64位会有莫名其妙的错误  
![picture 2](https://github.com/YiShiChangAnLuan/images/blob/master/OpenGL%20GLFW%26%26GLAD%20Win10%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE/2.png)  

- 复制目录中的`include`和`lib`，放在固定的地方可以为以后用的时候提供方便。（我用的[Visual Studio 2019](https://visualstudio.microsoft.com/vs2019-launch/)）  
![picture 3](https://github.com/YiShiChangAnLuan/images/blob/master/OpenGL%20GLFW%26%26GLAD%20Win10%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE/3.png)  

- 做完这些之后，使用vs创建一个`C++`的空项目  
![picture 4](https://github.com/YiShiChangAnLuan/images/blob/master/OpenGL%20GLFW%26%26GLAD%20Win10%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE/4.png)  

- 设置一下项目属性  
![picture 5](https://github.com/YiShiChangAnLuan/images/blob/master/OpenGL%20GLFW%26%26GLAD%20Win10%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE/5.png)  

- 把GLFW库链接(Link)进工程  
![picture 6](https://github.com/YiShiChangAnLuan/images/blob/master/OpenGL%20GLFW%26%26GLAD%20Win10%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE/6.png) 

- 在链接器里面添加这个文件`glfw3.lib`和`opengl32.lib`  
![picture 7](https://github.com/YiShiChangAnLuan/images/blob/master/OpenGL%20GLFW%26%26GLAD%20Win10%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE/7.png) 
    - Windows上的OpenGL库：`opengl32.lib`已经包含在`Microsoft SDK`里了，它在Visual Studio安装的时候就默认安装了。

    - Linux上的OpenGL库：在Linux下需要链接`libGL.so`库文件，这需要添加`-lGL`到链接器设置中。如果找不到这个库可能需要安装Mesa，NVidia或AMD的开发包，这部分因平台而异。  

GLAD
---
- 打开GLAD的[在线服务](https://glad.dav1d.de/)，将语言(Language)设置为C/C++，在API选项中，选择3.3以上的OpenGL(gl)版本。之后将模式(Profile)设置为Core，并且保证生成加载器(Generate a loader)的选项是选中的。现在可以先（暂时）忽略拓展(Extensions)中的内容。都选择完之后，点击生成(Generate)按钮来生成库文件。  
![picture 8](https://github.com/YiShiChangAnLuan/images/blob/master/OpenGL%20GLFW%26%26GLAD%20Win10%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE/8.png)
- GLAD现在应该提供给你了一个zip压缩文件  
![picture 9](https://github.com/YiShiChangAnLuan/images/blob/master/OpenGL%20GLFW%26%26GLAD%20Win10%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE/9.png)  
- 包含两个头文件目录，和一个glad.c文件。将两个头文件目录（glad和KHR）复制到刚才准备的环境文件夹里  
![picture 10](https://github.com/YiShiChangAnLuan/images/blob/master/OpenGL%20GLFW%26%26GLAD%20Win10%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE/10.png)  
- 并添加glad.c文件到工程中  
![picture 11](https://github.com/YiShiChangAnLuan/images/blob/master/OpenGL%20GLFW%26%26GLAD%20Win10%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE/11.png) 

随便找个[代码]()试试看不能跑  
![picture 12](https://github.com/YiShiChangAnLuan/images/blob/master/OpenGL%20GLFW%26%26GLAD%20Win10%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE/1.png)




教程来自于这里[LearnOpenGL CN](https://learnopengl-cn.github.io/)
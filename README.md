# Leader
Provide an interface to open something what usually needed,such as software,file
 
工作中经常要打开一些软件和目录。即使使用了windows的快捷键以及bat文件，但仍然觉得繁琐以及不耐烦。因此特意编写了这个工具Leader。

Leader是使用wxpython编写,通过pyinstaller打包的。由于水平有限，上百行代码最终却打包成了10M的文件。但一时半会没找到减少体积的姿势，通过upx压缩打包又报错，最终决定先将工具分享出来，等以后添加功能时再一并处理文件体积太大的问题。


### 工具运行原理
Leader正确运行需要三个文件：Leader.exe（主程序）、conf.json（配置文件）、logo.ico（图标）

每次打开Leader,程序将读取conf.json文件的内容（ 通过eval(open('conf.json','r').read() ) 方式将配置内容转换为字典类型数据）动态生成小图标菜单栏目，获取logo.ico作为托盘小图标。

### 关于配置文件conf.json
配置文件conf.json中的内容需符合字典格式。

字典Key与value的格式：
* key

  菜单栏的名字
  
* dict[key][Attr]

  * 等于Menu时，将该栏目定义为菜单栏，其子菜单内容为`dict[key][Menu]`中的内容。
  * 等于App时，将该栏目定义为普通菜单，`dict[key][Path]`指定的文件、文件夹、可执行程序的路径，`dict[key][Action]`指定的执行的动作，如当Action为“start”，Path为“F:/test”时，即为打开F盘中的test文件夹；当Action为“typora”，Path为“F:/test/test.md”时，即为通过typora打开F盘中的test文件夹下test.md文档（typora程序路径需添加到环境变量path中）;当Action为“”（空），Path为“F:/test/test.exe”时，即为打开F盘中的test文件夹下的test.exe程序（jar可执行文件等同）
  
### 配置文件实例
```
{
'Jar':{
        'Path':r'F:\test.jar',
        'action':'',
        'Attr':'APP'
        },
'Dir':{
        'Menu':{
                'test':{
                            'Path':r'F:\test',
                            'action':'start', 
                            'Attr':'APP'
                            }
                },
        'Attr':'Menu'},
'MarkDown':{
        'Menu':{
                'test.md':{
                            'Path':r'F:\test\test.md',
                            'action':'Typora', 
                            'Attr':'APP'
                            }
                },
        'Attr':'Menu'}
}
```

此时，Leader将依次生成以下菜单栏：
* Jar  (一个普通菜单,点击打开test.jar程序)

* Dir  (一个子菜单栏)
  * test  (Dir的子菜单，一个普通菜单，点击打开F盘下的test文件夹)
  
* MarkDown  (一个子菜单栏)
  * test.md   (MarkDown的子菜单，一个普通菜单，点击通过typora打开test.md)
  
  
### 如有建议
作者QQ:1026378409

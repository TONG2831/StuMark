

## Node+npm的安装及配置

**Node简介**:

> Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。  
>
> Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效。  
>
> Node.js 的包管理器 npm，是全球最大的开源库生态系统。 

---

**npm简介:**

> npm：是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题。比如常用的有：
>
> 　　1）允许用户从NPM服务器下载别人编写的第三方包到本地使用。
>
> 　　2）允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
>
> 　　3）允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

#### 一.Node的安装

1. node.js安装时，会自动在环境变量 path 中配置

2. 安装好的项目结构中存在 `node_modules` 文件夹

3. 在node.js安装路径下，创建两个文件夹`node_global` 、`node_cache` 作为全局路径，npm下载的模块都放在`global` 中，使用cmd命令修改

   ```cmd
   npm config set prefix "D:\nodejs\node_global"
   
   npm config set cache "D:\nodejs\node_cache"
   ```

   如果不修改，则默认为`C:\Users\用户名\AppData\Roaming\npm ` 

   4. `node -v` 用于显示版本号

#### 二.NPM命令

> npm 表示节点程序包管理器。npm 提供以下两个主要功能：
>
> - Node.js包/模块的在线软件仓库，可通过搜索 [search.nodejs.org](http://search.nodejs.org/)
> - 命令行实用程序安装包，作为Node.js版本管理和依赖包管理

1. npm随node.js安装

2. 我们要先配置npm的全局模块的存放路径以及cache的路径，例如我希望将以上两个文件夹放在NodeJS的主目录下，便在NodeJs下建立"node_global"及"node_cache"两个文件夹，输入以下命令改变npm配置 

   ```
   npm config set prefix "D:\Program Files\nodejs\node_global"
   npm config set cache "D:\Program Files\nodejs\node_cache"
   ```

   

3. 使用淘宝的cnpm:

   - `npm install -g cnpm --registry=https://registry.npm.taobao.org` 
   - 使用该镜像，下载速度快，在之后的使用过程中 `cnpm` 命令

4. 使用npm安装新模块命令:`npm install 模块名 -g` ，例：安装vue `npm install vue -g` 

5. 因为cnpm会被安装到D:\Program Files\nodejs\node_global下，而系统变量path并未包含该路径。在系统变量path下添加该路径即可正常使用cnpm。

    








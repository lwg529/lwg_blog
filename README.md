### lwg 的博客

* 地址: <https://lwg529.github.io/>

### 如何写博客

* 安装 Hexo: `npm install hexo -g`
* 工程：Clone 此项目并安装模块 `git clone *****.git && cd **** && npm install`
* 编写：在 `source/_posts/` 目录下编写博客，格式为 `markdown`
* 本地预览：可以用 `hexo serve` 启动本地服务

### 发布

hexo clean 清除缓存文件 (db.json) 和已生成的静态文件 (public), 在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。

hexo generate 生成静态文件

hexo deploy 将.deploy目录部署到GitHub

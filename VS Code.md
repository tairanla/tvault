# VS Code

## 修改软件字体

Windows 下 VS Code 默认字体在低分辨率显示器下显示效果太差了，使用下面的方法修改字体。

进入 VS Code 安装目录：`安装目录\bash\resources\app\out\vs\workbench\`，使用编辑器打开目录下的两个文件：
- workbench.desktop.main.css
- workbench.desktop.main.js
全局替换文件中的字体：Segoe WPC，例如，可以使用 Cascadia Mono 字体全局替换 Segoe WPC

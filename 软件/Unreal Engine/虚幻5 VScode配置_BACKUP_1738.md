# 配置 VScode
**unreal vscode官方文档： https://docs.unrealengine.com/5.1/zh-CN/setting-up-visual-studio-code-for-unreal-engine/**

1. 如果你是在Mac或Linux上调试，请下载并安装[LLDB扩展](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb)。
2. 如果你需要将VS Code设置为默认IDE，请打开 **虚幻编辑器（Unreal Editor）** 并转至 **编辑（Edit）** > **编辑器偏好设置（Editor Preferences）** > **通用（General）** > **源代码（Source Code）** ，然后将你的 **源代码编辑器（Source Code Editor）** 设置为 **Visual Studio Code** 。重启编辑器，使更改生效。这不是生成VS Code解决方案（参阅步骤5c）所必需的，但它会成为默认值，取代Visual Studio。
![image](./images/default_IDE.PNG)

3. 生成你的VS Code工作区。（两种做法）
	+ 打开 **虚幻编辑器（Unreal Editor）** 并点击 **工具（Tools）** > **刷新Visual Studio Code项目（Refresh Visual Studio Code Project）** 。
			![image](./images/RefreshVSCode.jpg)
	+ 在Windows和Mac上，右键点击项目的 `.uproject` 文件并点击 **生成项目文件（Generate Project Files）** 。完成后，你应该会在项目的文件夹中看到 `.code-workspace` 文件。
		![image](./images/code-workspace.jpg)

# 常见问题
## vscode c/c++扩展 找不到头文件问题
我个人使用的编辑器是 vscode 编辑器， unreal vscode官方文档有给出具体的配置方案，但是有些问题文档中并没有说，比如新建的c++类会提示找不到头文件这类问题导致没法使用代码提示，官方给的解决方案是在`.vscode/c_cpp_properties.json`这个文件中加入`includePath`这个参数用来包含头文件，但是我试了这个方便并行不通（可能是我对json不够了解）；所以我找到了另一种解决方法，直接在资源管理器中右键点击项目的 `.uproject` 文件并点击 **生成项目文件（Generate Project Files）** 后就能搜索的到类的头文件了。

## could not be compiled try rebuilding from source manually

在新的电脑打开工程文件可能会出现此类错误提示，我出现这个错误之前还有一个 visual studio tools的插件报错，可能是因为没有安装插件而导致的工程文件无法生成。
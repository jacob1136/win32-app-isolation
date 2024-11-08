# Welcome to the Win32 app isolation repo
Win32 应用程序隔离是 Windows 上的一项新安全功能，有助于在应用程序受到攻击时控制损害并保护用户的隐私选择。
Win32 应用程序隔离建立在 [AppContainers](https://learn.microsoft.com/en-us/windows/win32/secauthz/implementing-an-appcontainer) 的基础上，它提供安全边界以及虚拟化资源并提供对其他资源的中介访问。
此存储库包含帮助您隔离应用程序的文档和工具。

## 开始 
* 隔离应用程序的第一步是将其打包以便隔离运行，具体步骤如下
	* [使用 MSIX packaing tool](docs/packaging/msix-packaging-tool.md)
	* 或 [使用 Visual Studio](docs/packaging/packaging-with-visual-studio.md)
	
* 打包应用程序后，使用 [Application Capability Profiler](docs/profiler/application-capability-profiler.md)更新应用程序，允许它访问更多资源。
* 我们还有更多关于 [fundamentals](docs/fundamentals) 的文档，包括文件访问同意。
* 现在，您已准备好在 Windows 上部署和运行应用程序。

*用于打包应用程序以隔离运行的工具的二进制文件在 repo 的 [releases](https://github.com/microsoft/win32-app-isolation/releases) 部分共享。

*支持的 Windows 版本和工具的发布说明可在 [此处](relnotes/windows-release-notes.md) 找到。

## Resources
您可以使用以下资源查找有关 Win32 应用程序隔离的更多信息： 
* [Win32 app isolation Build session](https://www.youtube.com/watch?v=w6VwHGPz12w&pp=ygUTd2luMzIgYXBwIGlzb2xhdGlvbg%3D%3D&ab_channel=MicrosoftDeveloper)
* [Win32 app isolation blog](https://blogs.windows.com/windowsdeveloper/2023/06/14/public-preview-improve-win32-app-security-via-app-isolation/)

## Trademarks

本项目可能包含项目、产品或服务的商标或徽标。授权使用 Microsoft 
商标或徽标的授权使用必须遵循 
[微软商标和品牌指南](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general)。
在本项目的修改版本中使用 Microsoft 商标或徽标不得造成混淆或暗示 Microsoft 赞助。
对第三方商标或徽标的任何使用均须遵守第三方的政策。

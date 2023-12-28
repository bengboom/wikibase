每种语言都有很多扩展包、lib库
需要一个工具来管理这些库的依赖性和兼容性
这就是包管理器

**软件包管理系统**是在电脑中自动安装、配制、卸载和升级[软件包](https://zh.wikipedia.org/wiki/%E8%BD%AF%E4%BB%B6%E5%8C%85 "软件包")的工具组合，在各种[系统软件](https://zh.wikipedia.org/wiki/%E7%B3%BB%E7%BB%9F%E8%BD%AF%E4%BB%B6 "系统软件")和[应用软件](https://zh.wikipedia.org/wiki/%E5%BA%94%E7%94%A8%E8%BD%AF%E4%BB%B6 "应用软件")的安装管理中均有广泛应用。

**包管理器基本概念：**
1. **包（Packages）**：包是一种可以被共享、发布和重复使用的代码和资源的集合。它们可以包括库、工具、框架或其他类型的软件组件。
2. **依赖关系（Dependencies）**：包管理器用于跟踪和管理库之间的依赖关系。当一个库依赖于另一个库时，包管理器负责确保所需的库及其正确版本被安装和维护。
3. **版本管理（Versioning）**：包管理器允许指定所依赖的库的版本。这是为了确保不同版本的库之间的兼容性，以避免潜在的冲突和问题。

**常见包管理工具：**
1. **Pip**：用于 Python 的包管理器，可以安装、升级和卸载 Python 包。它使用`requirements.txt`文件来指定依赖关系。
2. **npm**：JavaScript 和 Node.js 的包管理器，用于安装、升级和卸载 JavaScript 库。它使用`package.json`文件来管理依赖项。
3. **Conda**：通用的包管理工具，支持多种编程语言，用于创建和管理环境，以及安装和管理库。
4. **Composer**：用于 PHP 的依赖管理工具，用于安装和管理 PHP 包。它使用`composer.json`文件来定义依赖项。
5. **Maven**：用于 Java 的包管理工具，用于管理 Java 项目的依赖项。它使用`pom.xml`文件来指定依赖项。
6. **NuGet**：.NET 平台的包管理工具，用于管理.NET库的依赖关系。它使用`.csproj`文件或`.nuspec`文件来管理包。 

**常见操作和命令：**
1. 安装包：使用包管理器安装特定的库或工具，可以通过命令（如`npm install`、`pip install`）来完成。
2. 卸载包：卸载不再需要的库或工具，可以通过命令（如`npm uninstall`、`pip uninstall`）来完成。
3. 更新包：更新已安装的库的版本，以解决安全漏洞或获得新功能，可以通过命令（如`npm update`、`pip install --upgrade`）来完成。
4. 创建环境：在某些包管理工具中，如 Conda，你可以创建隔离的环境，以确保不同项目之间的依赖关系不冲突。
5. 管理依赖关系：通过配置文件（如`package.json`、`requirements.txt`）来指定项目的依赖关系，以确保正确的库被安装。

## Javascript-Yarn






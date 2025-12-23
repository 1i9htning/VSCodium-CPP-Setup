# VSCodium CPP Setup

## 基础流程

1. 在 Windows 主机上安装 VSCodium Ver1.106.37943

2. 分别在 Windows 与 WSL 中安装扩展

3. 在 WSL 中安装 clang clangd：`sudo apt update \ sudo apt install clang \ sudo apt install clangd`

4. 导入 VSCodium 设置（`settings.json`）

6. 配置 WSL 中的 clangd。见[配置 clangd](#配置-clangd)

## 配置 clangd

在我的环境中，WSL 分发版本为 Ubuntu-22.04，自带的开发环境为 GCC 11。而在 VSCodium 的 clangd 插件日志中，它默认去查找 GCC 12 头文件（原因暂时未知）。此时有两个解决办法：

- 手动指定 clangd 的头文件查找路径

  1. 执行`g++-11 -E -v -x c++ /dev/null`获取标准头文件路径

  2. 在`~/.config/clangd/config.yaml`中手动指定标准头文件路径

      ~~~yaml
        CompileFlags:
        Add:
          - -isystem
          - /usr/include/c++/11
          - -isystem
          - /usr/include/x86_64-linux-gnu/c++/11
          - -isystem
          - /usr/include/c++/11/backward
          - -isystem
          - /usr/lib/gcc/x86_64-linux-gnu/11/include
          - -isystem
          - /usr/local/include
          - -isystem
          - /usr/include/x86_64-linux-gnu
          - -isystem
          - /usr/include
      ~~~

- 安装 GCC 12 的标准库头文件

  ~~~bash
  sudo apt install libstdc++-12-dev
  ~~~
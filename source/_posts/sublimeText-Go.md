title: Go语言环境配置
date: 2015-01-07 15:36:33
tags: Go
category: Tool
toc: true
---

使用Sublime Text配置Go语言环境。

## Windows下配置GO语言环境

### 1.下载安装go

### 2.设置GOPATH和GOROOT，

如go安装目录为D:/Program/Go，那么GOROOT="D:/Program/Go"，GOPATH="%GOROOT%/bin"

### 3.准备工作完成之后，开始搭建Sublime Text开发go语言的环境。

#### a.下载安装sublime text，安装Package Control

#### b.安装gocode和MarGo

　　win+r打开终端运行如下命令安装gocode和MarGo
    #安装gocode
    go get github.com/nsf/gocode 　　
    # 安装margo
    go get github.com/slene/margo

#### c.安装sublimetext插件

完成后，重启sublimetext，安装插件
GoSublime、SidebarEnhancements和Go Buid

修改GoSublime配置：在 Preferences->Package Settings->GoSublime，找到Settings - Default。打开的文件中，添加如下配置:
    
    "env": {"path":"D:/Program/go/bin;" },
好了，然后就可以写go代码进行验证了。
如果使用ctrl+B快捷键编译go不能使用，可以新建一个build system。执行：Tools->Build system->new build system，替换内容为： 

    {
        "cmd": [
            "go",
            "run",
            "$file_name"
        ],
        "file_regex": "^[ ]*File \"(…*?)\", line ([0-9]*)",
        "working_dir": "$file_path",
        "selector": "source.go"
    }

执行编译时选择这个build system就可以了。
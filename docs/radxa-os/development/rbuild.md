# 生成 RadxaOS 系统镜像

## 使用 `rbuild`

[`rbuild`](https://github.com/radxa-repo/rbuild) 是目前 RadxaOS 的生成环境，其有以下几个特点：

1. 生成环境容器化，无需额外配置生成依赖
2. 模块化的生成代码，修改方便

受限于所使用的软件依赖，`rbuild` 目前只支持在 x64 平台上运行。

可参考此项目自带的 [Getting Started](https://radxa-repo.github.io/rbuild/) 页面来完成项目安装和系统生成测试。

如需要在 `rbuild` 基础上进行二次开发，则请继续阅读文档其余部分，以及[系统生成代码](https://github.com/radxa-repo/rbuild/tree/main/common)。

## 参考

- [debos](https://github.com/go-debos/debos): 所使用的系统生成工具

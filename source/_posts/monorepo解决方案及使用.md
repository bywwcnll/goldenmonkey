---
title: monorepo解决方案及使用
date: 2020-07-27 15:30:00
tags:
- monorepo
- lerna
- bit
---
## monorepo 使用场景  

1. 项目代码存储库过多  

1. 代码存储库间的依赖关系杂乱

1. 版本的控制、更新、发布操作繁琐

1. 多个项目依赖包存在大量重复安装  

## lerna 使用 [官方文档](https://github.com/lerna/lerna)

``` bash
yarn global add lerna
mkdir monorepo-example && cd $_
lerna init --independent
```

> 两个主要的命令是 `lerna bootstrap` 和 `lerna publish`

- [lerna bootstrap](https://github.com/lerna/lerna/tree/master/commands/bootstrap#readme)

  使所有packages的依赖包共享，减少不必要的重复安装，吊装依赖包

  ``` bash
  lerna bootstrap --hoist --strict
  ```

- [lerna publish](https://github.com/lerna/lerna/tree/master/commands/publish#readme)

  管理所有的packages发布

## lerna + yarn 配置

### 常用 `lerna.json`

``` json
{
  "version": "0.0.1",
  "npmClient": "yarn",
  "useWorkspaces": true,
  "packages": [
    "packages/*"
  ]
}
```

### 配套 `package.json`

``` json
{
  "name": "root",
  "private": true,
  "workspaces": [
    "packages/*"
  ]
}
```

### yarn 工作区配置 [官方文档](https://classic.yarnpkg.com/zh-Hans/docs/workspaces)

``` bash
yarn config set workspaces-experimental true
```

## bit 使用 [官方文档](https://docs.bit.dev/docs/quick-start)

> ✨让你更好的管理所有的组件

``` bash
yarn global add bit-bin
cd monorepo-example
bit init
bit login
# bit a xxx -i xxx -m xxx
# bit t -a
# bit build xxx
# bit e xxx [-e]
# bit ls
# bit rm [-r] xxx
```

- 添加常用[编译器](https://bit.dev/bit/envs)

> 不添加编译器，打出的包的结构会有问题，可能会包含原组件结构的所有信息，或者会出现`bit_wrapper_dir`文件夹

``` bash
bit import bit.envs/compilers/babel -c
bit import bit.envs/compilers/vue -c
bit import bit.envs/compilers/typescript -c
```

## bit.json 配置 [官方文档](https://docs.bit.dev/docs/conf-bit-json)

``` json
{
    "env": {},
    "componentsDefaultDirectory": "components/{name}",
    "packageManager": "yarn",
    "useWorkspaces": true
}
```

## bit 私有服务器搭建

> 服务端

``` bash
yarn global add bit-bin
mkdir myscope && cd $_
bit init --bare
```

> 客户端：

``` bash
cd my-project
bit init
bit remote add ssh://bit@bitserver:22:/path/to/myscope
```

> 注意事项

- 将客户端的`id_rsa.pub`添加到服务端的`authorized_keys`中实现免密登录, `authorized_keys`文件权限设置成600

- 客户端添加scope出现`/path/to/myscope not found`时，可查看`~/Library/Caches/Bit/logs/debug.log`日志，一般是由于`bit`命令未找到，可`ln -s`创建命令软链接

  原因在服务端的.bashrc里，[解决方案](https://github.com/teambit/bit/issues/1236)

  ``` bash
  # If not running interactively, don't do anything
  case $- in
      *i*) ;;
        *) return;;
  esac
  ```

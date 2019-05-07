# 前端构建

构建：从 source 到 target 的自动化过程

构建的三个通用要点：

- 依赖管理
- 构建过程管理
- 构建效率


### 标准构建流程

拉取分支 => 安装依赖 => lint => test => build => deploy

其它功能如校验、性能评分等可作为自定义插件添加到构建流水线中


### 生产

- 资源处理
    + 路径
    + 静态扫描
    + 打包/合并
        - 模块化
        - 分块
        - 异步模块
        - hash name
        - 依赖（循环依赖）
    + 编译
        - js
        - css（autoprefixer、px2rem...）
        - html（单页、多页、模板注入）
    + 压缩
        - js
        - sourcemap
        - css
        - 图片、字体等资源
        - html
- 分环境构建
    - `DefinePlugin`
    - Multiple targets
- 交互式环境
    - 命令
    - 构建结果呈现（stats、dependency graph...）
- 构建配置


### 开发

- 开发服务器
    + 报错
    + 热更
- 代理服务器


### 构建效率

- 缓存
- 取消不必要的构建对象或构建环节
- 并行

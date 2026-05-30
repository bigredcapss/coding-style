# maven项目搭建规范速查手册

## 项目搭建原则
基于MVC分层原则，进行项目模块的搭建。

## maven项目模块定义规范

- xxx-openapi:某业务网关,负责与前端进行交互;
- xxx-api:对外暴露的某业务接口定义,包括业务公共枚举,异常码定义,pojo类,接口定义;
- xxx-provider:对外暴露的某业务接口实现;

### maven项目模块结构具体示例

```markdown
xxx-project
├── xxx-openapi
│   ├── src
│   │   ├── main
│   │   │   ├── java
│   │   │   │   └── com.xxx.xx.xxxopenapi
│   │   │   │       ├── aop                 # AOP切面（日志、权限、异常处理）
│   │   │   │       ├── config              # Spring配置类、中间件配置
│   │   │   │       ├── controller          # HTTP接口控制器
│   │   │   │       ├── enums               # 业务枚举定义
│   │   │   │       ├── pojo                # 数据对象（DTO/BO/PO等）
│   │   │   │       ├── utils               # 通用工具类
│   │   │   │       └── XxxOpenapiStart     # 服务启动类
│   │   │   └── resources
│   │   │       ├── bootstrap.properties
│   │   │       ├── logback-spring.xml      # 日志配置
│   │   │       └── rpc-services.xml        # RPC服务配置
│   │   └── test
│   │       └── java
│   └── pom.xml                             # 模块Maven配置
│
├── xxx-api
│   ├── src
│   │   ├── main
│   │   │   └── java
│   │   │       └── com.xxx.xxx.xxxapi
│   │   │           ├── enums               # 业务枚举定义
│   │   │           ├── exception           # 自定义业务异常
│   │   │           ├── pojo.dto            # RPC接口依赖的序列化传输对象定义
│   │   │           └── rpc                 # RPC服务接口定义
│   │   └── test
│   │       └── java                        # 单元测试目录
│   └── pom.xml                             # 模块Maven配置
│
├── xxx-provider
│   ├── src
│   │   ├── main
│   │   │   ├── java
│   │   │   │   └── com.xxx.xxx.xxxprovider
│   │   │   │       ├── client                      # 外部服务客户端封装
│   │   │   │       ├── component                   # 通用业务组件
│   │   │   │       ├── config                      # 配置类包
│   │   │   │       ├── constants                   # 常量定义包
│   │   │   │       ├── controller                  # HTTP接口控制器
│   │   │   │       ├── convert                     # DTO/BO/PO转换器
│   │   │   │       ├── enums                       # 业务枚举包
│   │   │   │       ├── helper
│   │   │   │       │   └── valid                   # 参数校验辅助
│   │   │   │       ├── mapper                      # MyBatis Mapper接口
│   │   │   │       ├── pojo
│   │   │   │       │   ├── bo                      # 业务对象
│   │   │   │       │   ├── dto                     # 数据传输对象
│   │   │   │       │   ├── model                   # 数据库领域模型
│   │   │   │       │   └── po                      # 数据库访问对象
│   │   │   │       ├── repository                  # 数据访问层
│   │   │   │       ├── rpc                         # 对外暴露的RPC服务实现【rpc与export包必选其一】
│   │   │   │       ├── export                      # 对外暴露的服务实现【rpc与export包必选其一】
|   |   |   |		├── facade                      # 聚合多个业务service
│   │   │   │       ├── service                     # 业务服务层
│   │   │   │       ├── factory                     # 工厂类
│   │   │   │       ├── template                    # 模版类
│   │   │   │       ├── task                        # 定时/异步任务
│   │   │   │       ├── utils                       # 通用工具类
│   │   │   │       └── XxxProviderStart            # 服务启动类
│   │   │   └── resources
│   │   │       ├── scripts
│   │   │       │   ├── dsl.sql                     # DDL,DML相关的sql语句
│   │   │       │   ├── xxx-index.dsl               # ES索引定义
│   │   │       ├── mybatis
│   │   │       │   ├── mapper                      # MyBatis XML映射文件包
│   │   │       │   ├── generatorConfig.xml         # MyBatis代码生成器配置
│   │   │       │   └── mybatis-config.xml          # MyBatis全局配置
│   │   │       ├── bootstrap.properties            # 启动配置
│   │   │       ├── logback-spring.xml              # 日志配置
│   │   │       └── rpc-services.xml                # RPC服务配置
│   │   └── test                                    # 单元测试目录
|   |       └── java                                # 单元测试目录
│   └── pom.xml                                     # 模块Maven配置
├── docs                                            # 项目相关文档
└── pom.xml                                         # 父项目Maven配置（可选）
```

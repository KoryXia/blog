---
title: SpringBoot3 整合 ShardingSphere 
tags: [笔记]
category: 编程
data: 2024-07-08
---
### **`SpringBoot3` 整合 `ShardingSphere 5.5.0` 过程记录**

#### **1. 引言**
在使用`SpringBoot3`时，如果使用`ShardingSphere 5.5.0`以前的版本，可能会遇到与`snakeyaml`和`jaxb`依赖相关的问题。通过升级到`ShardingSphere 5.5.0`版本，可以解决这些问题。本文将详细记录如何在`SpringBoot3`中整合`ShardingSphere 5.5.0`，并配置分库分表。

#### **2. Gradle 配置**

首先，在`build.gradle`文件中添加`ShardingSphere`依赖，并排除不必要的模块：

```gradle
implementation('org.apache.shardingsphere:shardingsphere-jdbc:5.5.0') {
    exclude group: 'org.apache.shardingsphere', module: 'shardingsphere-test-util'
}
```

#### **3. ShardingSphere 配置**

接下来，在`shardingsphere.yaml`中配置`ShardingSphere`，以下是一个示例配置：

```yaml
databaseName: campus
dataSources:
  ds_0:
    dataSourceClassName: com.zaxxer.hikari.HikariDataSource
    driverClassName: com.mysql.cj.jdbc.Driver
    jdbcUrl: jdbc:mysql://localhost:3306/campus?autoReconnect=true
    username: root
    password:
rules:
  - !SHARDING
    tables:
      t_student:
        actualDataNodes: ds_0.t_student_${1..2}
        tableStrategy:
          standard:
            shardingColumn: id
            shardingAlgorithmName: t_student_inline
        keyGenerateStrategy:
          column: id
          keyGeneratorName: snowflake

    shardingAlgorithms:
      t_student_inline:
        type: INLINE
        props:
          algorithm-expression: t_student_${id % 2 + 1}

    keyGenerators:
      snowflake:
        type: SNOWFLAKE

props:
  sql:
    show: true
```

在`shardingsphere.yaml`中导入配置：

```yaml
spring:
  datasource:
    driver-class-name: org.apache.shardingsphere.driver.ShardingSphereDriver
    url: jdbc:shardingsphere:classpath:shardingsphere.yaml
```


#### **4. 配置解析**

- **数据源配置**：在`dataSources`部分配置数据源，这里使用`HikariDataSource`作为连接池，并配置了`MySQL`数据库连接信息。

- **分表策略**：
    - `actualDataNodes`: 定义实际的数据节点，这里将`t_student`表分为两个节点`t_student_1`和`t_student_2`。
    - `tableStrategy`: 指定分表策略，使用`standard`策略，基于`id`列进行分表，使用名为`t_student_inline`的分表算法。
    - `keyGenerateStrategy`: 指定主键生成策略，基于`id`列，使用名为`snowflake`的主键生成器。

- **分表算法**：
    - `shardingAlgorithms`: 定义分表算法，`t_student_inline`使用`INLINE`类型，算法表达式为`t_student_${id % 2 + 1}`，即根据`id`的取模结果决定写入哪个表。

- **主键生成器**：
    - `keyGenerators`: 定义主键生成器，`snowflake`使用`SNOWFLAKE`类型。

- **调试配置**：
    - `props.sql.show`: 设置为`true`，以便在控制台显示SQL语句，便于调试。

#### **5. 结论**

通过上述配置，可以在 `SpringBoot3` 项目中成功整合 `ShardingSphere 5.5.0`，并实现简单的分表操作。分库的配置与分表类似，只需在`rules`部分增加`databaseStrategy`配置并定义多个数据源即可。

# MyScriptToPgScript
将Mysql的建表脚本转换为PostgreSQL的建表脚本。
PostgreSQL 与 Mysql 相比，有很多令人向往的东西，网上评论很多，不再赘述。
本项目主要是配合代码生成器项目使用，详见：
[PgBatis3AutoCode](https://gitee.com/tommygun/PgBatis3AutoCode)
如想从 MySql 转到 PostgreSQL ，可以试试本项目。

## 使用方法
### 1、准备MySQL数据库脚本
* 数据库脚本文件以 .sql 结尾；
* 所有数据库脚本文件请放在同一个文件夹下，不要放在子目录中；
* 程序自动遍历所有脚本文件，在另一文件夹中生成PgSQL脚本；

MySQL脚本示例：
```
CREATE TABLE `TB_Users` (
  `ID` int(11) NOT NULL COMMENT '用户ID',
  `UserName` varchar(50) NOT NULL COMMENT '用户姓名',
  `UserLevel` tinyint NOT NULL COMMENT '用户级别',
  `UserNotes` varchar(200) DEFAULT NULL COMMENT '用户注释',
  PRIMARY KEY (`ID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='用户表';
```

注意：必须有字段注释和表注释！

### 2、生成配置文件
配置文件格式如下：

```
<?xml version="1.0" encoding="UTF-8" ?>
<Config>
    <!-- MySQL数据表脚本文件所在目录 -->
    <Script_MySQL ConType="dir">E:\mytable</Script_MySQL>
    <!-- 生成的PgSQL数据表脚本文件所在目录 -->
    <Script_PostgreSQL ConType="dir">E:\pgtable</Script_PostgreSQL>
</Config>
```

注意：目录必须存在！

### 3、执行
1.生成jar包；

2.使用方法1：行执行下列命令，生成所有代码：

```
java -jar .\MyScriptToPgScript.jar config.xml
```

其中：MyScriptToPgScript.jar 为生成的 jar 包， config.xml 为配置文件路径。

## 注意
* 表脚本必须要有字段注释和表注释。
* 程序会生成 autocode.log 日志文件。
* 仅支持 UTF-8 。


## 生成的PgSQL数据表脚本示例：

```
-- DROP TABLE IF EXISTS "TB_Users";

CREATE TABLE "TB_Users"(
	"ID" integer NOT NULL,
	"UserName" character varying(50) COLLATE "default" NOT NULL,
	"UserLevel" smallint NOT NULL,
	"UserNotes" character varying(200) COLLATE "default" DEFAULT NULL::character varying,
	CONSTRAINT TB_Users_pkey PRIMARY KEY ("ID")
)
WITH (
    OIDS = FALSE
)
;

COMMENT ON TABLE "TB_Users" IS '用户表';

COMMENT ON COLUMN "TB_Users"."ID" IS '用户ID';
COMMENT ON COLUMN "TB_Users"."UserName" IS '用户姓名';
COMMENT ON COLUMN "TB_Users"."UserLevel" IS '用户级别';
COMMENT ON COLUMN "TB_Users"."UserNotes" IS '用户注释';
```
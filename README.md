# Java核心技术（卷一）

## Java 的基本程序设计结构

### 数 据 类 型

| 类型   | 存储需求 | 取值范围                                                 |
| ------ | -------- | -------------------------------------------------------- |
| int    | 4 字节   | -2 147 483 648 - 2 147 483 647 (正好超过 20 亿)          |
| short  | 2 字节   | -32 768 - 32 767                                         |
| long   | 8字节    | -9 223 372 036 854 775 B08 - 9 223 372 036 854 775 807   |
| byte   | 1 字节   | -128 - 127                                               |
| float  | 4 字节   | 大约 ± 3.402 823 47E+38F (有效位数为 6 ~ 7 位）          |
| double | 8 宇节   | 大约 ± 1.797 693 134 862 315 70E+308 (有效位数为 15 位） |

- char 类型（特 殊 字 符 的 转 义 序 列）

  | 转义序列 | 名称   | Unicode 值 |
  | -------- | ------ | ---------- |
  | \b       | 退格   | \u0008     |
  | \t       | 制表   | \u0009     |
  | \n       | 换行   | \u000a     |
  | \r       | 回车   | \u000d     |
  | \\"      | 双引号 | \u0022     |
  | \\'      | 单引号 | \u0027     |
  | \\\      | 反斜杠 | \u005c     |

  


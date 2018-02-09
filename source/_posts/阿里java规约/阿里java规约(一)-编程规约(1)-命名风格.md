---
title: 阿里java规约(一)-编程规约(1)-命名风格.md
categories: 阿里java规约
tags:
  - 阿里java规约
toc: true
date: 2018-02-09 15:02:33
scaffolds:
---

# 命名风格(16)
## 强制(10)
1. 【强制】不以`下划线或美元符号`做开头结尾
2. 【强制】禁止使用拼音与英文混合,禁止使用中文,纯拼音也尽量避免
    * 正例： alibaba / taobao / youku / hangzhou 等国际通用的名称， 可视同英文。
3. 【强制】`类名`使用 UpperCamelCase 风格，但以下情形例外： DO / BO / DTO / VO / AO / PO 等。
    * 正例： MarcoPolo / UserDO / XmlService / TcpUdpDeal / TaPromotion
    * 反例： macroPolo / UserDo / XMLService / TCPUDPDeal / TAPromotion 
4. 【强制】方法名、参数名、成员变量、局部变量都统一使用 lowerCamelCase 风格，必须遵从`驼峰`形式。
5. 【强制】`常量命名全部大写，单词间用下划线隔开`，力求语义表达完整清楚，不要嫌名字长。
6. 【强制】`抽象类`命名使用 `Abstract` 或 `Base` 开头； `异常类`命名使用 `Exception` 结尾； `测试类`
命名以它要`测试的类名`开始，以 `Test 结尾`。
7. 【强制】`类型与中括号紧挨`相连来定义数组。
    * 正例： 定义整形数组 `int[]` arrayDemo;
    * 反例： 在 main 参数中，使用 String args[]来定义。
8. 【强制】POJO 类中`布尔类型的变量`，都`不要加 is 前缀`，否则部分框架解析会引起序列化错误。
9. 【强制】`包名`统一使用`小写`，点分隔符之间有且仅有`一个自然语义`的英语单词。`包名`统一使用`单数形式`，但是`类名`如果有复数含义，类名`可以`使用`复数形式`。
    * 正例： 应用工具类包名为 com.alibaba.ai.util、类名为 MessageUtils（ 此规则参考 spring
    * 的框架结构）
10. 【强制】`杜绝完全不规范的缩写`， 避免望文不知义。
    * 反例： AbstractClass“缩写” 命名成 AbsClass； condition“ 缩写” 命名成 condi，此类随意缩写严重降低了代码的可阅读性。

## 推荐(4)
11. 【推荐】为了达到代码自解释的目标，任何自定义编程元素在命名时，使用`尽量完整`的单词组合来`表达其意`。
    * 正例： 从远程仓库拉取代码的类命名为 PullCodeFromRemoteRepository。
    * 反例： 变量 int a; 的随意命名方式。
12. 【推荐】如果模块、 接口、类、方法`使用了设计模式`，在`命名时体现出具体模式`。
    * 说明： 将设计模式体现在名字中，有利于阅读者快速理解架构设计理念。
    * 正例：    
                public class OrderFactory;  
                public class LoginProxy;  
                public class ResourceObserver;  

13. 【推荐】`接口类`中的`方法`和`属性不要加任何修饰符号`（ public 也不要加） ，保持代码的简洁性，并`加上有效的 Javadoc 注释`。尽量不要在接口里定义变量，如果一定要定义变量，肯定是与接口方法相关，并且是整个应用的基础常量。
    * 正例：   
            接口方法签名 void f();  
            接口基础常量 String COMPANY = "alibaba";  
    * 反例： 接口方法定义 public abstract void f();
    * 说明： JDK8 中接口允许有默认实现，那么这个 default 方法，是对所有实现类都有价值的默认实现。
14. `接口和实现类`的命名有两套规则：
    1. 【强制】对于 `Service` 和 `DAO` 类，基于 SOA 的理念，暴露出来的服务一定是接口，内部的实现类用 `Impl` 的后缀与接口区别。
        *  正例： CacheServiceImpl 实现 CacheService 接口。
    2. 【推荐】 如果是形容能力的接口名称，取对应的形容词为接口名 （ 通常是`–able` 的形式）。
        * 正例： AbstractTranslator 实现 Translatable。

## 参考(2)
15. 【参考】`枚举类`名建议带上 `Enum 后缀`，枚举`成员名称`需要`全大写`，单词间用`下划线隔开`。
    * 说明： 枚举其实就是特殊的常量类，且构造方法被默认强制是私有。
    * 正例： 枚举名字为 ProcessStatusEnum 的成员名称： SUCCESS / UNKNOWN_REASON。
16. 【参考】各层命名规约：
    1. Service/DAO 层方法命名规约
        *  获取`单个对象`的方法用 `get` 作前缀。
        *  获取`多个对象`的方法用 `list` 作前缀。
        *  获取`统计值`的方法用 `count` 作前缀。
        *  `插入`的方法用 `save/insert` 作前缀。
        *  `删除`的方法用 `remove/delete` 作前缀。
        *  `修改`的方法用 `update` 作前缀。
    2. 领域模型命名规约
        * 数据对象： xxxDO， xxx 即为数据表名。
        * 数据传输对象： xxxDTO， xxx 为业务领域相关的名称。
        * 展示对象： xxxVO， xxx 一般为网页名称。
        * POJO 是 DO/DTO/BO/VO 的统称，禁止命名成 xxxPOJO。

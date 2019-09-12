---
title: 单元测试-easyMock
categories: 学习
tags:
  - java
  - mock
  - easyMock
toc: true
date: 2019-09-12 16:26:04
scaffolds:
---

easyMock简单使用
<!-- more -->

## 1. 引入pom.xml依赖
```xml
<!-- https://mvnrepository.com/artifact/org.easymock/easymock -->
      <dependency>
          <groupId>org.easymock</groupId>
          <artifactId>easymock</artifactId>
          <version>3.6</version>
          <scope>test</scope>
      </dependency>
      <dependency>
          <groupId>org.powermock</groupId>
          <artifactId>powermock-api-easymock</artifactId>
          <version>1.5.1</version>
          <scope>test</scope>
      </dependency>
      <dependency>
          <groupId>org.powermock</groupId>
          <artifactId>powermock-module-junit4</artifactId>
          <version>1.5.1</version>
          <scope>test</scope>
      </dependency>
      <!-- https://mvnrepository.com/artifact/cglib/cglib-nodep -->
      <dependency>
          <groupId>cglib</groupId>
          <artifactId>cglib-nodep</artifactId>
          <version>2.2</version>
          <scope>test</scope>
      </dependency>
```

## 2. 代码

```java
import cn.huibo.service.impl.ReceivevMessageParserImpl;
import cn.jkcrm.person.service.WechatPNBindService;
import org.easymock.EasyMock;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.powermock.api.easymock.PowerMock;
import org.powermock.core.classloader.annotations.PrepareForTest;
import org.powermock.modules.junit4.PowerMockRunner;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;


@RunWith(PowerMockRunner.class)
@PrepareForTest({WechatPNBindService.class})
public class TestMessage {

    @Test
    public void test()throws Exception {

        PowerMock.mockStatic(WechatPNBindService.class);

        EasyMock.expect(WechatPNBindService.getShopNickByWxId(EasyMock.isA(String.class)))
                .andReturn("wxafdsadf")
                .anyTimes();

        PowerMock.replayAll();

        ReceivevMessageParserImpl parser = new ReceivevMessageParserImpl();

        BufferedReader reader = new BufferedReader(
                new FileReader(new File("E:\\hb-workspace\\Wechart-app-consumer\\test\\message")));

        parser.doExecute(reader.readLine());
    }
}

```

## 3. 参考
[有效使用easyMock编写java单元测试](https://blog.csdn.net/chjttony/article/details/14522771)
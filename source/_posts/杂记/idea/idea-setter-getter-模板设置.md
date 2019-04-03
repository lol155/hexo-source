---
title: idea getter setter 模板设置
categories: idea
tags:
  - idea
toc: true
date: 2019-04-03 14:19:03
scaffolds:
---

设置
![idea-md-20194314812](http://blogimage.signalfire2017.com/image/work/idea-md-20194314812.png?imageMogr2/thumbnail/!100p)

getter
```
#if($field.modifierStatic)
static ##
#end
$field.type ##
#set($name = $StringUtil.capitalizeWithJavaBeanConvention($StringUtil.sanitizeJavaIdentifier($helper.getPropertyName($field, $project))))
get${name}() {
    return $field.name;
}

```
setter  
```
#set($paramName = $helper.getParamName($field, $project))
public ##
#if($field.modifierStatic)
static void ##
#else
  $classSignature ##
#end
set$StringUtil.capitalizeWithJavaBeanConvention($StringUtil.sanitizeJavaIdentifier($helper.getPropertyName($field, $project)))($field.type $paramName) {
#if ($field.name == $paramName)
  #if (!$field.modifierStatic)
  this.##
  #else
    $classname.##
  #end
#end
$field.name = $paramName;
#if(!$field.modifierStatic)
return this;
#end
}
```


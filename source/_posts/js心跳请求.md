---
title: js心跳请求
categories: 简单记录
tags:
  - javascript
toc: true
date: 2018-01-24 15:03:02
scaffolds:
---

示例代码,mark一下  
var timer = setInterval (function(){},time)  
clearInterval(timer)  
<!-- more -->

```javascript
var timer;
timer = setInterval(function(){
    Common.ajax.execute({
        'url': Common.url.getBaseURL() + '/flow/get-trusteeship.do',
        'data': {
            "programId": $("#programId").val(),
        },
        'success': function (data) {
            if (data.result == 'success' && data.trusteeship) {
                if(data.trusteeship.status == 3){
                    clearInterval(timer);
                    EmayPagination.action.skipToCurrentPage($('#trusteeshipListQueryPaginationContainer'),
                        $('#trusteeshipListQueryPaginationContainer .pagination-node.active').attr('data-current-page-number'));
                    Trusteeship.dom.setStep(3);
                }
            }
        },
        'error': function () {
            Messager.action.error('操作异常。');
        }
    });

},5000);
```
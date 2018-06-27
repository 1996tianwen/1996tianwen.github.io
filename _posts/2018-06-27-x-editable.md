---
layout: post
title: x-editable
keywords: 
category: datatable
author: 晴天
description: 
---

![](/images/18-06-27-editable.png)

<p>x-editable组件是一个用于创建可编辑弹出框的插件，它支持三种风格的样式：bootstrap、Jquery UI、Jquery。</p>

![](/images/18-06-27-实际效果.png)

```
     columns: [
    	{
        	name: 'PARAM_VALUE',
        	data: function parseData(row) {
          		if (row.PARAM_VALUE){
          		return '<a href=\"#\"  name="'+row.PARAM_NAME+'" data-pk="'+row.DEVICE_ID+'" value="' + row.PARAM_VALUE + '">' + row.PARAM_VALUE + '</a>';
          		}else {
            		return "";
          		}
        	}
      	}
      ],
      drawCallback: function () {
          $("#individualCharacterParameters a").editable({
       		 mode:"inline",
       		 url: function (params) {
          		var deviceId = params.pk;
          		var paramName = $(this).attr("name");
         		 var paramValue = params.value;
         		 var json = {
           			 TD_S_PARAM: []
          		};
         		 json.TD_S_PARAM.push({
           		 	MODIFY_TAG: 2,
           		 	PARAM_NAME: paramName,
            		PK_DEVICE_ID: deviceId,
            		PARAM_VALUE: paramValue,
            		UPDATE_USER_NAME: $('#USER_NAME').val()
          		});
          		console.log(json);
          		$.ajax({
            		url: $('#servletContextPath').val() + '/service/common/dbServiceFlow',
            		type: 'POST',
           			data: {
              			json: JSON.stringify(json),
              			operateCode: 3032
            		},
            		error: function (err) {
             			console.log(err);
            		},
            		success: function (data) {
              			alertMsg(data);
            		}
          		});
        	}
      	})
     }
```


---
layout: post
title: 初识datatable
keywords: 
category: datatable
author: 晴天
description: 
---

<p>发现项目前台用了这个datatable，所以来学一学。</p>

<p>一个简单demo</p>

```
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<!--DataTables CSS-->
	<link rel="stylesheet" type="text/css" href="http://cdn.datatables.net/1.10.15/css/jquery.dataTables.css">
	<!--JQuery-->
	<script type="text/javascript" charset="utf8" src="http://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
	<!--DataTables 本地js-->
	<script type="text/javascript" charset="utf8" src="js/jquery.dataTables.js"></script>
</head>
<body>
	<table id="table_id_example" class="display">
		<thead>
			<tr>
				<th>Column 1</th>
				<th>Column 2</th>
			</tr>
		</thead>
		<tbody>
			<tr>
				<td>Row 1 Data 1</td>
				<td>Row 1 Data 2</td>
			</tr>
			<tr>
				<td>Row 1 Data 1</td>
				<td>Row 1 Data 2</td>
			</tr>
		</tbody>
	</table>
</body>

<script type="text/javascript">
	$(document).ready(function(){
		$('#table_id_example').DataTable();
	});
</script>
</html>
```

<p>ajax，用谷歌浏览器打开网页时出现了以下的错。</p>

```
Cross origin requests are only supported for protocol schemes: http, data, chrome, chrome-extension, https.
send @ jquery-1.12.4.js:10254
```

<p>换用ie，就能正常打开。。。</p>

```
<!DOCTYPE html>
<html>
<head>
	<title>ajax</title>
	<link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.10.16/css/jquery.dataTables.min.css">
	<script type="text/javascript" src="https://code.jquery.com/jquery-1.12.4.js"></script>
	<script type="text/javascript" src="https://cdn.datatables.net/1.10.16/js/jquery.dataTables.min.js"></script>
</head>
<body>
	//hover 当鼠标经过时高亮行显示
	<table id="example" class="hover" style="width:100%">
        <thead>
            <tr>
                <th>Name</th>
                <th>Position</th>
                <th>Office</th>
                <th>Extn.</th>
                <th>Start date</th>
                <th>Salary</th>
            </tr>
        </thead>
        <tfoot>
            <tr>
                <th>Name</th>
                <th>Position</th>
                <th>Office</th>
                <th>Extn.</th>
                <th>Start date</th>
                <th>Salary</th>
            </tr>
        </tfoot>
    </table>
</body>
<script type="text/javascript">
	$(document).ready(function(){
		$('#example').DataTable({
			//分页 默认打开
			paging:false,
			"ajax":"array.txt"
		});
	});
</script>
</html>
```

```
{
  "data": [
    [
      "Tiger Nixon",
      "System Architect",
      "Edinburgh",
      "5421",
      "2011/04/25",
      "$320,800"
    ],
    [
      "Garrett Winters",
      "Accountant",
      "Tokyo",
      "8422",
      "2011/07/25",
      "$170,750"
    ],
    [
      "Ashton Cox",
      "Junior Technical Author",
      "San Francisco",
      "1562",
      "2009/01/12",
      "$86,000"
    ],
    [
      "Cedric Kelly",
      "Senior Javascript Developer",
      "Edinburgh",
      "6224",
      "2012/03/29",
      "$433,060"
    ],
    [
      "Airi Satou",
      "Accountant",
      "Tokyo",
      "5407",
      "2008/11/28",
      "$162,700"
    ],
    [
      "Brielle Williamson",
      "Integration Specialist",
      "New York",
      "4804",
      "2012/12/02",
      "$372,000"
    ],
    [
      "Herrod Chandler",
      "Sales Assistant",
      "San Francisco",
      "9608",
      "2012/08/06",
      "$137,500"
    ],
    [
      "Rhona Davidson",
      "Integration Specialist",
      "Tokyo",
      "6200",
      "2010/10/14",
      "$327,900"
    ],
    [
      "Colleen Hurst",
      "Javascript Developer",
      "San Francisco",
      "2360",
      "2009/09/15",
      "$205,500"
    ],
    [
      "Sonya Frost",
      "Software Engineer",
      "Edinburgh",
      "1667",
      "2008/12/13",
      "$103,600"
    ],
    [
      "Jena Gaines",
      "Office Manager",
      "London",
      "3814",
      "2008/12/19",
      "$90,560"
    ],
    [
      "Quinn Flynn",
      "Support Lead",
      "Edinburgh",
      "9497",
      "2013/03/03",
      "$342,000"
    ],
    [
      "Charde Marshall",
      "Regional Director",
      "San Francisco",
      "6741",
      "2008/10/16",
      "$470,600"
    ],
    [
      "Haley Kennedy",
      "Senior Marketing Designer",
      "London",
      "3597",
      "2012/12/18",
      "$313,500"
    ],
    [
      "Tatyana Fitzpatrick",
      "Regional Director",
      "London",
      "1965",
      "2010/03/17",
      "$385,750"
    ],
    [
      "Michael Silva",
      "Marketing Designer",
      "London",
      "1581",
      "2012/11/27",
      "$198,500"
    ],
    [
      "Paul Byrd",
      "Chief Financial Officer (CFO)",
      "New York",
      "3059",
      "2010/06/09",
      "$725,000"
    ],
    [
      "Gloria Little",
      "Systems Administrator",
      "New York",
      "1721",
      "2009/04/10",
      "$237,500"
    ],
    [
      "Bradley Greer",
      "Software Engineer",
      "London",
      "2558",
      "2012/10/13",
      "$132,000"
    ],
    [
      "Dai Rios",
      "Personnel Lead",
      "Edinburgh",
      "2290",
      "2012/09/26",
      "$217,500"
    ],
    [
      "Jenette Caldwell",
      "Development Lead",
      "New York",
      "1937",
      "2011/09/03",
      "$345,000"
    ],
    [
      "Yuri Berry",
      "Chief Marketing Officer (CMO)",
      "New York",
      "6154",
      "2009/06/25",
      "$675,000"
    ],
    [
      "Caesar Vance",
      "Pre-Sales Support",
      "New York",
      "8330",
      "2011/12/12",
      "$106,450"
    ],
    [
      "Doris Wilder",
      "Sales Assistant",
      "Sidney",
      "3023",
      "2010/09/20",
      "$85,600"
    ],
    [
      "Angelica Ramos",
      "Chief Executive Officer (CEO)",
      "London",
      "5797",
      "2009/10/09",
      "$1,200,000"
    ],
    [
      "Gavin Joyce",
      "Developer",
      "Edinburgh",
      "8822",
      "2010/12/22",
      "$92,575"
    ],
    [
      "Jennifer Chang",
      "Regional Director",
      "Singapore",
      "9239",
      "2010/11/14",
      "$357,650"
    ],
    [
      "Brenden Wagner",
      "Software Engineer",
      "San Francisco",
      "1314",
      "2011/06/07",
      "$206,850"
    ],
    [
      "Fiona Green",
      "Chief Operating Officer (COO)",
      "San Francisco",
      "2947",
      "2010/03/11",
      "$850,000"
    ],
    [
      "Shou Itou",
      "Regional Marketing",
      "Tokyo",
      "8899",
      "2011/08/14",
      "$163,000"
    ],
    [
      "Michelle House",
      "Integration Specialist",
      "Sidney",
      "2769",
      "2011/06/02",
      "$95,400"
    ],
    [
      "Suki Burks",
      "Developer",
      "London",
      "6832",
      "2009/10/22",
      "$114,500"
    ],
    [
      "Prescott Bartlett",
      "Technical Author",
      "London",
      "3606",
      "2011/05/07",
      "$145,000"
    ],
    [
      "Gavin Cortez",
      "Team Leader",
      "San Francisco",
      "2860",
      "2008/10/26",
      "$235,500"
    ],
    [
      "Martena Mccray",
      "Post-Sales support",
      "Edinburgh",
      "8240",
      "2011/03/09",
      "$324,050"
    ],
    [
      "Unity Butler",
      "Marketing Designer",
      "San Francisco",
      "5384",
      "2009/12/09",
      "$85,675"
    ],
    [
      "Howard Hatfield",
      "Office Manager",
      "San Francisco",
      "7031",
      "2008/12/16",
      "$164,500"
    ],
    [
      "Hope Fuentes",
      "Secretary",
      "San Francisco",
      "6318",
      "2010/02/12",
      "$109,850"
    ],
    [
      "Vivian Harrell",
      "Financial Controller",
      "San Francisco",
      "9422",
      "2009/02/14",
      "$452,500"
    ],
    [
      "Timothy Mooney",
      "Office Manager",
      "London",
      "7580",
      "2008/12/11",
      "$136,200"
    ],
    [
      "Jackson Bradshaw",
      "Director",
      "New York",
      "1042",
      "2008/09/26",
      "$645,750"
    ],
    [
      "Olivia Liang",
      "Support Engineer",
      "Singapore",
      "2120",
      "2011/02/03",
      "$234,500"
    ],
    [
      "Bruno Nash",
      "Software Engineer",
      "London",
      "6222",
      "2011/05/03",
      "$163,500"
    ],
    [
      "Sakura Yamamoto",
      "Support Engineer",
      "Tokyo",
      "9383",
      "2009/08/19",
      "$139,575"
    ],
    [
      "Thor Walton",
      "Developer",
      "New York",
      "8327",
      "2013/08/11",
      "$98,540"
    ],
    [
      "Finn Camacho",
      "Support Engineer",
      "San Francisco",
      "2927",
      "2009/07/07",
      "$87,500"
    ],
    [
      "Serge Baldwin",
      "Data Coordinator",
      "Singapore",
      "8352",
      "2012/04/09",
      "$138,575"
    ],
    [
      "Zenaida Frank",
      "Software Engineer",
      "New York",
      "7439",
      "2010/01/04",
      "$125,250"
    ],
    [
      "Zorita Serrano",
      "Software Engineer",
      "San Francisco",
      "4389",
      "2012/06/01",
      "$115,000"
    ],
    [
      "Jennifer Acosta",
      "Junior Javascript Developer",
      "Edinburgh",
      "3431",
      "2013/02/01",
      "$75,650"
    ],
    [
      "Cara Stevens",
      "Sales Assistant",
      "New York",
      "3990",
      "2011/12/06",
      "$145,600"
    ],
    [
      "Hermione Butler",
      "Regional Director",
      "London",
      "1016",
      "2011/03/21",
      "$356,250"
    ],
    [
      "Lael Greer",
      "Systems Administrator",
      "London",
      "6733",
      "2009/02/27",
      "$103,500"
    ],
    [
      "Jonas Alexander",
      "Developer",
      "San Francisco",
      "8196",
      "2010/07/14",
      "$86,500"
    ],
    [
      "Shad Decker",
      "Regional Director",
      "Edinburgh",
      "6373",
      "2008/11/13",
      "$183,000"
    ],
    [
      "Michael Bruce",
      "Javascript Developer",
      "Singapore",
      "5384",
      "2011/06/27",
      "$183,000"
    ],
    [
      "Donna Snider",
      "Customer Support",
      "New York",
      "4226",
      "2011/01/25",
      "$112,000"
    ]
  ]
}
```

<p>Data：</p>

<p>数据是复杂的，并且所有的数据是不一样的。因此DataTables中有很多的选项可用于配置如何获得表中的数据显示，以及如何处理这些复杂的数据。DataTables处理数据的三个核心概念：</p>

<li>处理模式</li>

<li>数据类型</li>

<li>数据源</li>

<p>处理模式(Processing modes)：DataTables中有两种不同的方式处理数据（排序、搜索、分页等）</p>

<p>	客户端处理(Client)：所有数据集预先加载（一次获取所有数据），数据处理都是在浏览器中完成的【逻辑分页】</p>

<p>	服务器端处理(ServerSide)：数据处理是在服务器上执行（页面只处理当前页的数据）【物理分页】</p>

<p>每种模式都有自己的有点和缺点，选择哪种模式是由你的数据量决定的。根据经验来看，数据少于10000行，可以选择客户端模式，超过10000行的使用服务器端处理。**注意：两种处理模式不能同时使用，但是可以动态更改从一个模式到另一个。**</p>

```
$('#example').DataTable({
  //开启服务器模式
  serverSide: true
});
```

<p>数据源类型(Data source types)</p>

* 数组

* 对象

* 实例


<p>Data sources</p>

* DOM
* JavaScript
* Ajax

<p>待续。。。</p>
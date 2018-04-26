---
layout: post
title: datatable ajax
keywords: 
category: datatable
author: 晴天
description: 
---

<p>报表页面js：</p>

```
$(function jQueryDocReady() {
  var select2conf = {
    childrenAcct: {
      collection: 'TF_F_ACCOUNT',
      selectId: 'ACCT_ID',
      selectText: 'ACCT_NAME',
      setText: 'ACCT_NAME',
      tableRef: {
        ref: 'CHILDREN',
        params: {
          VACCT_ID: $('#currAcctId').val()
        }
      },
      dbRoute: 'cen'
    },
    operateType: {
      collection: 'TD_S_DATA_DICT',
      selectId: 'DICT_CODE',
      selectText: 'DICT_NAME',
      conditions: {DICT_TYPE_CODE: 'report_operate_type'},
      dbRoute: 'cen',
      disseachable: true
    },
    payType: {
      collection: 'TD_S_DATA_DICT',
      selectId: 'DICT_CODE',
      selectText: 'DICT_NAME',
      conditions: {DICT_TYPE_CODE: 'report_pay_type'},
      dbRoute: 'cen',
      disseachable: true
    }
  };


  var option = {
    select2Conf: select2conf,
    minLength: 0
  };

  var table = $('#chargeReportDetailTab');
  var oTable;
  var operateType_con = $('#operateType_con').val();
  var payType_con = $('#payType_con').val();
  var month_con = $('#month_con').val();
  var opts = $.extend(true, {}, window.dataTableLanguage, {
    search: '搜索:'
  });

  $('.date-picker').datepicker({
    rtl: AIot.isRTL(),
    orientation: 'left',
    autoclose: true,
    language: 'zh-CN',
    clearBtn: true,
    todayHighlight: true,
    startView: 0,
    maxViewMode: 2,
    minViewMode: 0,
    format: 'yyyy-mm-dd'
  });


  initSearchValue($('#searchBar'), 'charge_rpt_');

  oTable = table.DataTable({
    dom: '<"row"<"col-sm-4 hidden-xs zindex"<"datatable-search dropdown">B>' +
    '<"col-sm-8"<"datatable-menu">>>rt<"row"<"col-sm-5 hidden-xs"<"col-sm-4 datatable_length"l>' +
    '<"col-sm-8 "i>><"col-sm-7 col-xs-12"p>>',
    language: opts,
    stateSave: true,
    serverSide: true,
    buttons: [{
      extend: 'excelHtml5',
      text: '<i class="icon-login" style="font-weight: bold"></i>导出报表',
      className: 'btn-sm excelExport',
      exportOptions: {
        columns: ':visible'
      }
    }],
    processing: true,
    ajax: {
      url: $('#servletContextPath').val() + '/service/common/queryDataTable',
      type: 'GET',
      contentType: "application/json; charset=utf-8",
      data: function (sendData) {
        sendData.params = 'DATATABLE';
        sendData.dbRoute = 'cen';
        sendData.conditions = {
          'a.PAY_STATUS': '1',
          'c.ACCT_ID': $('#currAcctId').val()
        };
        if (notNull(sessionStorage.getItem('charge_rpt_startMSISDN'))) {
          sendData.conditions.startMSISDN = sessionStorage.getItem('charge_rpt_startMSISDN');
        }
        if (notNull(sessionStorage.getItem('charge_rpt_endMSISDN'))) {
          sendData.conditions.endMSISDN = sessionStorage.getItem('charge_rpt_endMSISDN');
        }
        if (notNull(sessionStorage.getItem('charge_rpt_startICCID'))) {
          sendData.conditions.startICCID = sessionStorage.getItem('charge_rpt_startICCID');
        }
        if (notNull(sessionStorage.getItem('charge_rpt_endICCID'))) {
          sendData.conditions.endICCID = sessionStorage.getItem('charge_rpt_endICCID');
        }
        if (notNull(sessionStorage.getItem('charge_rpt_startDate'))) {
          sendData.conditions.startDate = sessionStorage.getItem('charge_rpt_startDate');
        }
        if (notNull(sessionStorage.getItem('charge_rpt_endDate'))) {
          sendData.conditions.endDate = sessionStorage.getItem('charge_rpt_endDate');
        }

        if (notNull(sessionStorage.getItem('charge_rpt_account'))) {
          sendData.conditions['c.DISTRIBUTION_ACCT_ID'] = sessionStorage.getItem('charge_rpt_account');
        }
        if (notNull(sessionStorage.getItem('charge_rpt_operateType'))) {
          sendData.conditions['f.API_TYPE'] = sessionStorage.getItem('charge_rpt_operateType');
        }
        if (notNull(sessionStorage.getItem('charge_rpt_payType'))) {
          sendData.conditions['a.PAY_TYPE'] = sessionStorage.getItem('charge_rpt_payType');
        }
        if (payType_con != null) {
          sendData.conditions.currmonth = "1";
        }
        if (month_con != null) {
          sendData.conditions.somemonth = month_con;
        }
        return {
          json: JSON.stringify(sendData),
          operateCode: '7005'
        };
      }
    },
    columns: [
      {
        name: "a.PAY_ID",//操作流水号
        data: function (row) {
          if (row.sumCommission) {
            $('#brokerage').text(row.sumCommission);
          }
          if (row.sumFee) {
            $('#sale').text(row.sumFee);
          }
          return row.PAY_ID;
        }
      },
      {
        name: "b.GOODS_CODE", //ICCID
        data: function (row, type) {
          if (null != row.GOODS_CODE) {
            return row.GOODS_CODE;
          }
          return '';
        }
      },
      {
        name: "b.MSISDN",//MSISDN
        data: function (row, type) {
          if (null != row.MSISDN) {
            return row.MSISDN;
          }
          return '';
        }
      },
      {
        name: "c.DISTRIBUTION_ACCT_ID",//渠道商/终端
        data: function (row, type) {
          if (null != row.ACCT_NAME) {
            return row.ACCT_NAME;
          }
          return '';
        }
      },
      {
        name: "a.OUT_TRADE_NO",//充值流水号
        data: function (row, type) {
          if (null != row.OUT_TRADE_NO) {
            return row.OUT_TRADE_NO;
          }
          return '';
        }
      },
      {
        name: "a.TOTAL_FEE",//销售额
        data: function (row, type) {
          if (null != row.TOTAL_FEE) {
            return row.TOTAL_FEE;
          }
          return '';
        }
      },
      {
        name: "d.ADJUST_MONEY",//佣金
        data: function (row, type) {
          if (null != row.COMMISSION) {
            return row.COMMISSION;
          }
          return '';
        }
      },
      {
        name: "f.API_TYPE",//操作类型
        data: function (row, type) {
          if (null != row.API_NAME) {
            return row.API_NAME;
          }
          return '';
        }
      },
      {
        name: "a.PAY_TYPE",//支付方式
        data: function (row, type) {
          if (null != row.PAY_NAME) {
            return row.PAY_NAME;
          }
          return '';
        }
      },
      {
        name: "a.PAY_TIME",//操作日期
        data: function (row, type) {
          if (null != row.PAY_TIME) {
            return AIot.formatDate(row.PAY_TIME, 'yyyy-MM-dd HH:mm:ss');
          }
          return '';
        }
      }
    ],
    columnDefs: [{
      sortable: false,
      targets: [3, 7, 8]
    }],
    lengthMenu: [
      [10, 25, 50, 100],
      [10, 25, 50, 100] // 每页显示个数
    ],
    order: [
      [9, 'desc']
    ],
    scrollX: true,
    pagingType: 'full_numbers',
    drawCallback: function (settings ) {
    },
    headerCallback: function (thead, data, start, end, display) {
      if (data.length === 0) {
        $('#brokerage').text('0');
        $('#sale').text('0');
      }
    },
    initComplete: function (settings, json) {
      if($('#pageNumber').length==0) {
        $("#chargeReportDetailTab_paginate").after(" <div id='pageNumber' class='col-sm-3' style='margin-top:5px'>到第 <input style='height:28px;line-height:28px;width:40px;' class='margin text-center' id='changePage' type='text'> 页  <a class='shiny' style='' href='javascript:void(0);' id='dataTable-btn'>跳转</a></div> ");
      }
      var oTablel = $("#chargeReportDetailTab").dataTable();
      $('#dataTable-btn').click(function(e) {
        if($("#changePage").val() && $("#changePage").val() > 0) {
          var redirectpage = $("#changePage").val() - 1;
        } else {
          var redirectpage = 0;
        }
        oTablel.fnPageChange(redirectpage);
      });
    }
  });


  moveExportBtnToPageToolsBar(oTable.buttons().container(), $('.btn-container'));
  var search_func;
  $('#search-btn').on('click', search_func = function () {
    var inputs = $('.search-item');
    inputs.each(function each(i, ele) {
      var $ele = $(ele);
      var index = $ele.attr('data-column');
      var value;
      if ($ele.val() !== '' && $ele.val() !== null) {
        value = $ele.val().trim();
      } else {
        value = ''
      }
      oTable.column(index).search(value);
    });

    saveSearchValue($('#searchBar'), 'charge_rpt_');
    oTable.draw();
  });


  $('#clear-btn').on('click', function () {
    clearSearchValue($('#searchBar'), 'charge_rpt_');
    operateType_con = null;
    payType_con = null;
    month_con = null;
    oTable.column(4).search('');
    oTable.draw();
  });

  if(operateType_con!=null||payType_con!=null||month_con!=null){
    clearSearchValue($('#searchBar'), 'charge_rpt_');
  }

  if (operateType_con != null) {
    if (operateType_con == '0') {
      $('#operateType').select2({'data': [{id: 12, text: '开通'}]});
    }else {
      $('#operateType').select2({'data': [{id: 11, text: '续费'}]});
    }
    search_func();
  }

  if (payType_con != null) {
    $('#startDate').val(AIot.formatDate(new Date().setDate(1), 'yyyy-MM-dd'));
    var date = new Date();
    var currentMonth = date.getMonth();
    var nextMonth = ++currentMonth;
    var nextMonthFirstDay = new Date(date.getFullYear(), nextMonth, 1);
    var oneDay = 1000 * 60 * 60 * 24;

    $('#endDate').val(AIot.formatDate(new Date(nextMonthFirstDay - oneDay), 'yyyy-MM-dd'));
  }

  if (month_con != null) {
    var new_year = parseInt(month_con.substr(0, 4));  //取当前的年份
    var new_month = parseInt(month_con.substr(4, 2));//取下一个月的第一天，方便计算（最后一天不固定）
    var new_date = new Date(new_year, new_month - 1, 1).setDate(1);        //取当年当月中的第一天
    $('#startDate').val(AIot.formatDate(new_date, 'yyyy-MM-dd'));


    var nextMonthFirstDay = new Date(new_year, new_month, 1);
    var oneDay = 1000 * 60 * 60 * 24;
    $('#endDate').val(AIot.formatDate(new Date(nextMonthFirstDay - oneDay), 'yyyy-MM-dd'));
  }

  AIot.initSelect($('#searchBar'), option);

});
```

<p>待补充。。。</p>

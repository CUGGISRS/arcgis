trafficeventScheduling-js.js

```js
var trafficeventScheduling = function () {
    var eventmap;
    var eventLayer;
    var dpdCondition = "#prevTime";
    var prevStartTime = "#prevStartTime";
    var prevEndTime = "#prevEndTime";
    var nextTime = "#nextTime";
    var nextStartTime = "#nextStartTime";
    var nextEndTime = "#nextEndTime";
    var searchObj;
    var roadCodeVla = "";
    var crossCodeVla = "";
    var dirCodeVla = "";
	// 定位样式
    // SimpleMarkerSymbol：标记符号用于在图形层上绘制点和多点
    var arraySymbol = new esri.symbols.SimpleMarkerSymbol(
        {
            "color": new esri.Color([255, 0, 0]),
            "style": "Square",
            "type": "esriSMS",
            "xoffset": 0,
            "yoffset": 0
        }
    );
    var reportisKill = false;
//获取当前时间 相差几天的时间
    var getMyDateByDay = function (obj, func) {
        var option = {
            days: 0,
            format: 'yyyy-MM-dd'
        };
        var settings = $.extend({}, option, obj);
        var days = $.trim(settings.days) == "" ? option.days : (isNaN(settings.days) ? 0 : parseInt(settings.days));
        var format = $.trim(settings.format) == "" ? "yyyy-MM-dd" : (/yyyy-MM-dd (hh:mm:ss)?/g.test(settings.format) == true ? settings.format : "yyyy-MM-dd");

        var date = new Date();
        var getDaySeconds = date.getTime() - (days * 24 * 60 * 60 * 1000);
        var getDate = new Date(getDaySeconds);

        var transiton = function (num) {
            return (num < 10 ? "0" : "") + num;
        };
        var getStrDate = function (date) {
            return format.replace(/yyyy|MM|dd|hh|mm|ss/g, function (str) {
                switch (str) {
                    case 'yyyy':
                        return transiton(date.getFullYear());
                        break;
                    case 'MM':
                        return transiton(date.getMonth() + 1);
                        break;
                    case 'dd':
                        return transiton(date.getDate());
                        break;
                    case 'hh':
                        return transiton(date.getHours());
                        break;
                    case 'mm':
                        return transiton(date.getMinutes());
                        break;
                    case 'ss':
                        return transiton(date.getSeconds());
                        break;
                }
            });
        };
        var startTime = getStrDate(getDate);
        var endTime = getStrDate(date);
        var allTime = startTime + " - " + endTime;

        if (typeof func == "function") {
            func(startTime, endTime, allTime, getDate, date);
        }

    };

    function initDatePicker(type) {
        var formatType;
        var initValue;
        var startView;
        var minView;
        switch (type) {
            case 1: {
                type = 'month';
                formatType = 'yyyy-mm';
                initValue = new Date().getFullYear() + "-" + (new Date().getMonth() + 1);
                startView = "year";
                minView = "year";
                break;
            }
            case 0: {
                type = 'date';
                formatType = 'yyyy-mm-dd';
                initValue = new Date().getFullYear() + "-" + (new Date().getMonth() + 1) + "-" + new Date().getDate();
                startView = "month";
                minView = "month";
                break;
            }
        }
        $("#prevStartTime,#prevEndTime,#nextStartTime,#nextEndTime").datetimepicker({
            autoclose: true,
            language: 'cn',
            startView: startView,
            // startDate : initValue,
            minView: minView,
            format: formatType,
            timePicker: true
        });
        $("#prevStartTime,#prevEndTime,#nextStartTime,#nextEndTime").datetimepicker("setDate", new Date());
    }

    var bindInit = function () {
        $("#mapBox").height(($(document.body).height() - 120) + "px");
        var width = $((window)).width() * 0.8
        var height = $((window)).height() * 0.8
        $("#eventminuteInfo").attr("data-height", height)//地图定位高
        $("#eventminuteInfo").attr("data-width", width)//
        $("a[href='#tab_808_2'],#eventSrcBtn").off().on("click", function () {
            schedulingCommand_Page.clearZaiJiInterval();
            loadEventTable();
            loadEventMap();
        });
        $("#tjsjSelect").select2({
            placeholder: "请选择",
            allowClear: true,
            multiple: false,
            data: [{id: '0', text: "按日"}, {id: '1', text: "按月"}],
            width: null
        }).val(0).trigger("change");
        initDateTimeRangeSelected("#prevTime", function (startTime, endTime) {
            $("#prevStartTime").val($.trim(startTime));
            $("#prevEndTime").val($.trim(endTime));
        }, 0, {
            showClear: true,
            format: "YYYY-MM-DD",
            // startDate: moment().subtract(180, 'days').startOf('day'),
            // endDate: moment().endOf('day'),
            dateLimit: 30,
            dateRangePickerOption: {
                timePicker: false,
                ranges: {
                    '最近7日': [moment().subtract(6, 'days').startOf('day'), moment().endOf('day')],
                    '最近30日': [moment().subtract(29, 'days').startOf('day'), moment().endOf('day')]
                },
            }
        });
        // common.showDateTimeRangePick("#dpdCondition", function (startTime, endTime) {
        //     $("#carPassStartTime").val($.trim(startTime));
        //     $("#carPassEndTime").val($.trim(endTime));
        // }, null, {});

        $("#carPassStartTime,#carPassEndTime").datetimepicker({
            format: "yyyy-mm-dd hh:ii:ss",
            // startDate: new Date(),
            autoclose: true,
            // todayBtn: true,
            language: 'zh-CN',
            minuteStep: 1,
            //minView: 1
        });
        $("#fssjModal").datetimepicker({
            format: "yyyy-mm-dd hh:ii:ss",
            autoclose: true,
            clearBtn: true,
            language: 'cn',
            minuteStep: 1,
            endDate: new Date(),
        });
        $("#carPassStartTime").datetimepicker("setDate", new Date(new Date().getTime() - 24 * 60 * 60 * 1000));
        $("#carPassEndTime").datetimepicker("setDate", new Date());
        //初始化时间 30天
        // getMyDateByDay({days:"29",format:"yyyy-MM-dd"},function (startTime,endTime,allTime,startDate,endDate) {
        //     $("#prevTime").val(allTime);
        //     $('#prevTime').data('daterangepicker').setStartDate(startDate);
        //     $('#prevTime').data('daterangepicker').setEndDate(endDate);
        //     $("#prevStartTime").val(startTime);
        //     $("#prevEndTime").val(endTime);
        // });
        //
        initDatePicker(0)
        // initDateTimeRangeSelected("#nextTime", function (startTime, endTime) {
        //     $("#nextStartTime").val($.trim(startTime));
        //     $("#nextEndTime").val($.trim(endTime));
        // },0,{
        //     showClear:true,
        //     format:"YYYY-MM-DD",
        //     // startDate: moment().subtract(180, 'days').startOf('day'),
        //     // endDate: moment().endOf('day'),
        //     dateLimit: 30,
        //     dateRangePickerOption:{
        //         timePicker: false,
        //         ranges: {
        //             '最近7日': [moment().subtract(6, 'days').startOf('day'), moment().endOf('day')],
        //             '最近30日': [moment().subtract(29, 'days').startOf('day'), moment().endOf('day')]
        //         },
        //     }
        // });
        $("#statisticsSearch").off().on("click", function () {
            statisticsSearch();
        });
        //勾选或者取消勾选
        $("#comparison").off().change(function () {
            var comparisonChecked = $(this).prop("checked");
            if (comparisonChecked) {
                $("#nextStartTime,#nextEndTime").show();
            } else {
                $("#nextStartTime,#nextEndTime").hide();
            }
        });
        //事件类型
        $.getJSON("/trafficEvent/getTrIncidentType", function (data) {
            if (data != null && data.state == "SUCCESS") {
                var bindData = [];
                for (var i = 0; i < data.rows.length; i++) {
                    bindData.push({id: data.rows[i].TYPE_NO, text: data.rows[i].TYPE_NAME});
                }
                $("#sjlxTjfx").select2({
                    placeholder: "全部",
                    allowClear: true,
                    multiple: true,
                    data: bindData,
                    width: null
                }).val(null).trigger("change");
                $("#sjlxTjfx + span").slimScroll({
                    height: 34,
                    width: 200
                });
                $("#eventSjLx").select2({
                    placeholder: "请选择",
                    allowClear: true,
                    multiple: false,
                    data: bindData,
                    width: null
                }).val(null).trigger("change").on("change", function () {
                    if ($("#eventSjLx").val() != "" || $("#eventSjLx").val() != null) {
                        $.getJSON("/trafficEvent/getTrIncidentSubType", {"typeNo": $("#eventSjLx").val()}, function (data) {
                            var bindData = [];
                            for (var i = 0; i < data.rows.length; i++) {
                                bindData.push({id: data.rows[i].SUBTYPE_NO, text: data.rows[i].SUBTYPE_NAME});
                            }
                            if (bindData.length > 0) {
                                $("#eventSjZl").select2({
                                    placeholder: "请选择",
                                    allowClear: true,
                                    multiple: false,
                                    data: bindData,
                                    width: null
                                }).val(null).trigger("change");
                            } else {
                                $("#eventSjZl").select2({
                                    placeholder: "请选择事件类型",
                                    allowClear: true,
                                    multiple: false,
                                    data: [],
                                    width: null
                                }).val(null).trigger("change");
                            }
                        })
                    } else {
                        $("#eventSjZl").select2({
                            placeholder: "请选择事件类型",
                            allowClear: true,
                            multiple: false,
                            data: [],
                            width: null
                        }).val(null).trigger("change");
                    }
                });
                $("#sjlxModal").select2({
                    placeholder: "请选择",
                    allowClear: true,
                    multiple: false,
                    data: bindData,
                    width: null
                }).val(null).trigger("change").on("change", function () {
                    if ($("#sjlxModal").val() != "" || $("#sjlxModal").val() != null) {
                        $.getJSON("/trafficEvent/getTrIncidentSubType", {"typeNo": $("#sjlxModal").val()}, function (data) {
                            var bindData = [];
                            for (var i = 0; i < data.rows.length; i++) {
                                bindData.push({id: data.rows[i].SUBTYPE_NO, text: data.rows[i].SUBTYPE_NAME});
                            }
                            if (bindData.length > 0) {
                                $("#sjzlModal").select2({
                                    placeholder: "请选择",
                                    allowClear: true,
                                    multiple: false,
                                    data: bindData,
                                    width: null
                                }).val(null).trigger("change");
                            } else {
                                $("#sjzlModal").select2({
                                    placeholder: "请选择事件类型",
                                    allowClear: true,
                                    multiple: false,
                                    data: [],
                                    width: null
                                }).val(null).trigger("change");
                            }
                        })
                    } else {
                        $("#sjzlModal").select2({
                            placeholder: "请选择事件类型",
                            allowClear: true,
                            multiple: false,
                            data: [],
                            width: null
                        }).val(null).trigger("change");
                    }
                });
                $("#reportsjlx").select2({
                    placeholder: "请选择",
                    allowClear: true,
                    multiple: false,
                    data: bindData,
                    width: null
                }).val(null).trigger("change").on("change", function () {
                    if ($("#reportsjlx").val() != "" || $("#reportsjlx").val() != null) {
                        $.getJSON("/trafficEvent/getTrIncidentSubType", {"typeNo": $("#reportsjlx").val()}, function (data) {
                            var bindData = [];
                            for (var i = 0; i < data.rows.length; i++) {
                                bindData.push({id: data.rows[i].SUBTYPE_NO, text: data.rows[i].SUBTYPE_NAME});
                            }
                            $("#reportsjzl").empty();
                            $("#reportsjzl").select2({
                                placeholder: "请选择",
                                allowClear: true,
                                multiple: false,
                                data: bindData,
                                width: null
                            }).val(null).trigger("change");
                        })
                    } else {
                        $("#reportsjzl").select2({
                            placeholder: "请选择事件类型",
                            allowClear: true,
                            multiple: false,
                            data: [],
                            width: null
                        }).val(null).trigger("change");
                    }
                });
            }
        });
        $.getJSON("/trafficEvent/getDirectionList", function (data) {
            if (data != null && data.state == "SUCCESS") {
                var bindData = [];
                for (var i = 0; i < data.rows.length; i++) {
                    bindData.push({id: data.rows[i].DIRECTION, text: data.rows[i].DIRECTION_NAME});
                }
                $("#szfxModal").select2({
                    placeholder: "请选择",
                    allowClear: true,
                    multiple: false,
                    data: bindData,
                    width: null
                }).val(null).trigger("change");
                $("#reportszfxModal").select2({
                    placeholder: "请选择",
                    allowClear: true,
                    multiple: false,
                    data: bindData,
                    width: null
                }).val(null).trigger("change").on("change", function () {
                    if ($("#reportszfxModal").val() != "" && $("#reportszfxModal").val() != null) {
                        for (var i = 0; i < data.rows.length; i++) {
                            if ($("#reportszfxModal").val() == data.rows[i].DIRECTION) {
                                dirCodeVla = data.rows[i].DIRECTION_NAME
                                break
                            }
                        }
                        $("#reportfsddModal").val(roadCodeVla + crossCodeVla + dirCodeVla)
                    } else {
                        dirCodeVla = "";
                        $("#reportfsddModal").val(roadCodeVla + crossCodeVla + dirCodeVla)
                    }
                });
            }
        });
        $.getJSON("/trafficEvent/getTrRoadCode", function (data) {
            if (data != null && data.state == "SUCCESS") {
                var bindData = [];
                for (var i = 0; i < data.rows.length; i++) {
                    bindData.push({
                        id: data.rows[i].ROAD_CODE,
                        text: data.rows[i].ROAD_CODE + "：" + data.rows[i].ROAD_NAME,
                        roadKind: data.rows[i].ROAD_KIND
                    });
                }
                $("#reportdldm").select2({
                    placeholder: "请选择",
                    allowClear: true,
                    multiple: false,
                    data: bindData,
                    width: null
                }).val(null).trigger("change").on("change", function () {
                    reportisKill = true
                    for (var i = 0; i < bindData.length; i++) {
                        if ($("#reportdldm").val() == bindData[i].id) {
                            if (bindData[i].roadKind != "4") {
                                reportisKill = false;
                            }
                            break;
                        }
                    }
                    for (var i = 0; i < data.rows.length; i++) {
                        if ($("#reportdldm").val() == data.rows[i].ROAD_CODE) {
                            roadCodeVla = data.rows[i].ROAD_NAME;
                            break
                        }
                    }
                    $("#reportfsddModal").val(roadCodeVla + crossCodeVla + dirCodeVla);
                    if (!reportisKill) {
                        if ($("#reportgls").val() == "") {
                            crossCodeVla = "";
                        } else {
                            crossCodeVla = $("#reportgls").val() + "km";
                        }
                        $("#reportfsddModal").val(roadCodeVla + crossCodeVla + dirCodeVla)
                        $("#lkldDiv").hide();
                        $("#glsDiv").show();
                        $("#reportgls").off().on("change", function () {
                            crossCodeVla = $("#reportgls").val() + "km";
                            $("#reportfsddModal").val(roadCodeVla + crossCodeVla + dirCodeVla)
                        })
                    } else {
                        $("#lkldDiv").show();
                        $("#glsDiv").hide();
                        if ($("#reportdldm").val() != "" && $("#reportdldm").val() != null) {
                            $.getJSON("/trafficEvent/getTrCrossCode", {"roadCode": $("#reportdldm").val()}, function (data) {
                                var bindData = [];
                                for (var i = 0; i < data.rows.length; i++) {
                                    bindData.push({
                                        id: data.rows[i].CROSS_CODE,
                                        text: data.rows[i].CROSS_CODE + "：" + data.rows[i].CROSS_NAME
                                    });
                                }
                                if (bindData.length > 0) {
                                    $("#reportlkld").select2({
                                        placeholder: "请选择",
                                        allowClear: true,
                                        multiple: false,
                                        data: bindData,
                                        width: null
                                    }).val(null).trigger("change").on("change", function () {
                                        if ($("#reportlkld").val() != "" && $("#reportlkld").val() != null) {
                                            for (var i = 0; i < data.rows.length; i++) {
                                                if ($("#reportlkld").val() == data.rows[i].CROSS_CODE) {
                                                    crossCodeVla = data.rows[i].CROSS_NAME;
                                                    break
                                                }
                                            }
                                            $("#reportfsddModal").val(roadCodeVla + crossCodeVla + dirCodeVla)
                                        } else {
                                            crossCodeVla = "";
                                            $("#reportfsddModal").val(roadCodeVla + crossCodeVla + dirCodeVla)
                                        }
                                    });
                                } else {
                                    crossCodeVla = "";
                                    $("#reportfsddModal").val(roadCodeVla + crossCodeVla + dirCodeVla)
                                    $("#reportlkld").select2({
                                        placeholder: "请选择",
                                        allowClear: true,
                                        multiple: false,
                                        data: [],
                                        width: null
                                    }).val(null).trigger("change");
                                }
                            })
                        } else {
                            roadCodeVla = "";
                            $("#reportfsddModal").val(roadCodeVla + crossCodeVla + dirCodeVla);
                            $("#reportlkld").select2({
                                placeholder: "请选择",
                                allowClear: true,
                                multiple: false,
                                data: [],
                                width: null
                            }).val(null).trigger("change");
                        }
                    }

                });
                // $("#dldmModal").select2({
                //     placeholder: "请选择",
                //     allowClear: true,
                //     multiple: false,
                //     data: bindData,
                //     width: null
                // }).val(null).trigger("change").on("change",function () {
                //     if($("#dldmModal").val() !=""|| $("#dldmModal").val()!=null){
                //         $.getJSON("/trafficEvent/getTrCrossCode",{"roadCode":$("#dldmModal").val()},function (data) {
                //             var bindData = [];
                //             for (var i = 0; i < data.rows.length; i++) {
                //                 bindData.push({id: data.rows[i].CROSS_CODE, text: data.rows[i].CROSS_CODE + data.rows[i].CROSS_NAME });
                //             }
                //             if(bindData.length>0){
                //                 $("#lkldModal").select2({
                //                     placeholder: "请选择",
                //                     allowClear: true,
                //                     multiple: false,
                //                     data: bindData,
                //                     width: null
                //                 }).val(null).trigger("change");
                //             }else{
                //                 $("#lkldModal").select2({
                //                     placeholder: "请选择",
                //                     allowClear: true,
                //                     multiple: false,
                //                     data: [],
                //                     width: null
                //                 }).val(null).trigger("change");
                //             }
                //         })
                //     }else{
                //         $("#lkldModal").select2({
                //             placeholder: "请选择",
                //             allowClear: true,
                //             multiple: false,
                //             data: [],
                //             width: null
                //         }).val(null).trigger("change");
                //     }
                // });;
            }
        });
        $.getJSON("/trafficEvent/getTrIncidentCloseReason", function (data) {
            if (data != null && data.state == "SUCCESS") {
                var bindData = [];
                for (var i = 0; i < data.rows.length; i++) {
                    bindData.push({id: data.rows[i].REASON_NO, text: data.rows[i].REASON_NAME});
                }
                // $("#dldmModal").select2({
                //     placeholder: "请选择",
                //     allowClear: true,
                //     multiple: false,
                //     data: bindData,
                //     width: null
                // }).val(null).trigger("change")
                // $("#reportdldm").select2({
                //     placeholder: "请选择",
                //     allowClear: true,
                //     multiple: false,
                //     data: bindData,
                //     width: null
                // }).val(null).trigger("change")
            }
        });
        //事件来源
        $("#eventSjly,#sjlyModal").select2({
            placeholder: "请选择",
            allowClear: true,
            multiple: false,
            data: [{"id": "1", "text": "人工上报"}, {"id": "2", "text": "视频检测"}, {"id": "3", "text": "视频巡逻"}],
            width: null
        }).val(null).trigger("change");
        //事件状态
        $("#eventCzzt,#sjztModal").select2({
            placeholder: "请选择",
            allowClear: true,
            multiple: false,
            data: [{"id": "0", "text": "未处置"}, {"id": "1", "text": "已关闭"}, {"id": "2", "text": "已自撤"}, {
                "id": "3",
                "text": "调度中"
            }, {"id": "4", "text": "调度完毕"}, {"id": "5", "text": "已归档"}],
            width: null
        }).val(null).trigger("change");
        //是否关注
        $("#eventSfgz,#sfgzModal").select2({
            placeholder: "请选择",
            allowClear: true,
            multiple: false,
            data: [{"id": "1", "text": "是"}, {"id": "0", "text": "否"}],
            width: null
        }).val(null).trigger("change");
        //
        $("#eventSjzt").select2({
            placeholder: "请选择",
            allowClear: true,
            multiple: false,
            data: [{"id": "0", "text": "无效数据"}, {"id": "1", "text": "有效数据"}],
            width: null
        }).val(null).trigger("change");
        $("#sjdjModal,#reportsjdjModal").select2({
            placeholder: "请选择",
            allowClear: true,
            multiple: false,
            data: [{"id": "1", "text": "一般"}, {"id": "2", "text": "重大"}, {"id": "3", "text": "特别重大"}],
            width: null
        }).val(null).trigger("change");
        var isClick = true;
        $("#prevIllegalImg").off().on("click", function (ele) {
            if (isClick) {
                isClick = false;
                var is = $(ele).attr("is");
                if (is === "no") {
                    return;
                }
                var childrens = $("#topAllIllegalTypes").children();
                var Y = $(childrens[0]).offset().left;
                var Y2 = $("#topAllIllegalTypesWrapper").offset().left;
                if (Y < Y2) {
                    var leftInt = $("#topAllIllegalTypes").css("left");
                    leftInt = parseInt(leftInt.replace("px", ""));
                    var elemetWidth = $(childrens[0]).outerWidth() + 10;//outerWidth()返回元素的宽高 + padding + border
                    $("#topAllIllegalTypes").css("left", (leftInt + elemetWidth) + "px");
                    $(ele).attr("is", "no");
                    setTimeout(function () {
                        $(ele).attr("is", "");
                        isClick = true;
                    }, 600);
                } else {
                    isClick = true
                }
            }
        });
        $("#nextIllegalImg").off().on("click", function (ele) {
            if (isClick) {
                isClick = false;

                var is = $(ele).attr("is");
                if (is === "no") {
                    isClick = true;
                    return;
                }
                var lastChildren = $("#topAllIllegalTypes>.twoC:last");
                var Y = $(lastChildren).offset().left + $(lastChildren).outerWidth();
                var Y2 = $("#topAllIllegalTypesWrapper").offset().left + $("#topAllIllegalTypesWrapper").outerWidth();
                if (Y > Y2) {
                    var leftInt = $("#topAllIllegalTypes").css("left");
                    leftInt = parseInt(leftInt.replace("px", ""));
                    var elemetWidth = $(lastChildren).outerWidth(true);
                    $("#topAllIllegalTypes").css("left", (leftInt - elemetWidth) + "px");
                    $(ele).attr("is", "no");
                    setTimeout(function () {
                        isClick = true;
                        $(ele).attr("is", "");
                    }, 600);
                } else {
                    isClick = true;
                }

            }

        });
        $("#allIllegalCodeSum").off().on('click', '.twoC', function (event) {
            $("#allIllegalCodeSum .selectedTag").removeClass("selectedTag");
            $(this).addClass("selectedTag");
            var type = $("#differentWays>li.active>a").attr("type");
            var incidentType = $(this).attr("type");
            var incidentSubType = $(this).attr("subType");
            illegalTypeName = $(this).attr("typeName");
            searchObj.incidentType = incidentType;
            searchObj.incidentSubType = incidentSubType;
            $("#pointType>li").off().on("click", function () {
                setTimeout(function () {
                    $(".selectedTag").trigger("click");
                }, 500);

            });
            if (searchObj.nextStartTime === undefined || searchObj.nextEndTime === undefined) {//没有对比
                if (type === "时间") {
                    loadNoTime()
                } else if (type === "位置") {
                    loadNoPoint();
                    $("#roadTopMore").off().on("click", function () {
                        $("#pointsMore").modal({
                            backdrop: 'static',
                            keyboard: false,
                            show: true
                        });
                        pointsMoreTable()
                    });
                    $("#crossTopMore").off().on("click", function () {
                        $("#crosssMore").modal({
                            backdrop: 'static',
                            keyboard: false,
                            show: true
                        });
                        crossMoreTable()
                    })
                } else if (type === "其他") {
                    loadNoOther()
                }
            } else {//有对比
                if (type === "时间") {
                    loadYesTime()
                    //销毁top echarts
                } else if (type === "位置") {
                    $("#prevMap").off().on("click", function (ele) {
                        if (!$(ele).hasClass("timeActive")) {
                            $("#nextMap").removeClass("timeActive");
                            $("#nextMap").html($("#nextMap").attr("divName"));
                            $("#nextMap").css({"width": "100px", "color": "black"});
                            $("#prevMap").addClass("timeActive");
                            setTimeout(function () {
                                $("#prevMap").html('<p>当前时段</p><p>' + searchObj.startTime + ' ~ ' + searchObj.endTime + '</p>');
                            }, 500);
                            var illegalType = $("#allIllegalCodeSum .selectedTag").attr("type");
                            typeMapPointsEcharts(searchObj, function (data) {
                                formatMapData(data, "当前时段")
                            });
                        }
                    })
                    $("#nextMap").off().on("click", function (ele) {
                        if (!$(ele).hasClass("timeActive")) {
                            $("#prevMap").removeClass("timeActive");
                            $("#prevMap").html($("#prevMap").attr("divName"));
                            $("#prevMap").css({"width": "100px", "color": "black"});
                            $("#nextMap").addClass("timeActive");
                            setTimeout(function () {
                                $("#nextMap").html('<p>对比时间</p><p>' + searchObj.nextStartTime + ' ~ ' + searchObj.nextEndTime + '</p>');
                            }, 500);
                            var illegalType = $("#allIllegalCodeSum .selectedTag").attr("type");
                            typeMapPointsEcharts({
                                timeType: searchObj.timeType,
                                startTime: searchObj.nextStartTime,
                                endTime: searchObj.nextEndTime,
                                incidentType: searchObj.incidentType,
                                incidentSubType: searchObj.incidentSubType
                            }, function (data) {
                                formatMapData(data, "对比时段")
                            });
                        }
                    });
                    $("#roadTopMore").off().on("click", function () {
                        $("#pointsMore").modal({
                            backdrop: 'static',
                            keyboard: false,
                            show: true
                        });
                        pointsMoreTable()
                    });
                    $("#crossTopMore").off().on("click", function () {
                        $("#crosssMore").modal({
                            backdrop: 'static',
                            keyboard: false,
                            show: true
                        });
                        crossMoreTable()
                    })
                    loadYesPoint()
                } else if (type === "其他") {
                    loadYesOther()
                }
            }
        });
        $("#allIllegalCodeSum").mouseover(function () {
            var topAllIllegalTypesChildren = $("#topAllIllegalTypes").children();
            if (topAllIllegalTypesChildren.length > 0) {
                //判断需不需要显示（当内容不能一次展示出来时才显示）
                var contentWidth = 0;
                $.each(topAllIllegalTypesChildren, function (index, ele) {
                    contentWidth = contentWidth + $(ele).outerWidth(true);//outerWidth(true)返回元素宽高 + padding + border + margin
                });
                var topAllIllegalTypesWidth = $("#topAllIllegalTypes").outerWidth();//outerWidth()返回元素的宽高 + padding + border
                if (contentWidth > topAllIllegalTypesWidth) {
                    $("#prevIllegalImg").show();
                    $("#nextIllegalImg").show();
                }
            }
        });
        $("#allIllegalCodeSum").mouseout(function () {
            $("#prevIllegalImg").hide();
            $("#nextIllegalImg").hide();
        });
        $("#tjsjSelect").on("change", function (e) {
            var selectedValue = $(e.target).val();
            $("#prevStartTime,#prevEndTime,#nextStartTime,#nextEndTime").remove();
            $("#comparison").before("<input type='text'' class='inlineBlock timeInputLen' id='prevStartTime'><input type='text'' class='inlineBlock timeInputLen' id='prevEndTime'>")
            $("#eventTypeDiv").before("<input type='text'' class='inlineBlock timeInputLen' id='nextStartTime'><input type='text'' class='inlineBlock timeInputLen' id='nextEndTime'>")
            var checked = $("#comparison").prop("checked");
            if (!checked) {
                $("#nextStartTime,#nextEndTime").hide();
            }
            if (selectedValue == "1") {
                initDatePicker(1)
            } else if (selectedValue == "0") {
                initDatePicker(0)
            }
        });
        $("#differentWays li").off().on("click", function () {
            setTimeout(function () {
                $(".selectedTag").click()
            }, "1000")
        });
        $("a[href='#tab_808_3']").off().on("click", function () {
            schedulingCommand_Page.clearZaiJiInterval();
            statisticsSearch();
        });
        require(
            ["esri/map", "esri/layers/GraphicsLayer", "extras/ClusterLayer", "esri/geometry/Point", "esri/geometry/webMercatorUtils"],
            function (Map, GraphicsLayer, ClusterLayer, Point, webMercatorUtils) {
                //初始化地图
                map = pspMap.init("pointsRealMap", trafficeventScheduling, {
                    "tollgate": [],
                    "video": false,
                    "case": false,
                    "warnInfo": false
                });
                map.on("extent-change", function (ext) {
                    showeLayerstatistical()
                });
                prevLayer = new GraphicsLayer({id: "prevLayer"});
                map.addLayer(prevLayer, 100);
                nextLayer = new GraphicsLayer({id: "nextLayer"});
                map.addLayer(nextLayer, 101);

            });
        $("#obsoleteIncidentsBtn").off().on("click", function () {
            var row = $('#eventSearchTable').bootstrapTable('getSelections');
            var isincidentState = false;
            var incidentState = "";
            for (var i = 0; i < row.length; i++) {
                if (row[i].incidentState != "0" || row[i].validFlag == "0") {
                    isincidentState = true;
                    break
                }
                incidentState += row[i].ptiid + ",";
            }
            if (!isincidentState) {
                $.post("/trafficEvent/obsoleteIncidents", {"ptiids": incidentState.substr(0, incidentState.length - 1)}, function (data) {
                    if (data.state == "SUCCESS") {
                        common.showSuccess("作废成功");
                        loadEventTable();
                    } else {
                        common.showError("作废失败")
                    }
                }, "json")
            } else {
                common.showInfo("未处置的有效数据可作废")
            }
        });
        $("#peopleReport").off().on("click", function () {
            common.showInfo("点击地图选择位置");
            require([
                "esri/map", "esri/geometry/Extent", "esri/config", "esri/toolbars/draw", "esri/geometry/geometryEngineAsync",
                "esri/layers/GraphicsLayer", "esri/graphic", "esri/geometry/Point", "esri/geometry/webMercatorUtils", "esri/tasks/query",
                "esri/tasks/QueryTask", "esri/geometry/Extent", "esri/symbols/PictureMarkerSymbol",
                "esri/InfoTemplate", "esri/tasks/FindTask", "esri/tasks/FindParameters", "esri/tasks/IdentifyTask", "esri/tasks/IdentifyParameters", "dojo/domReady!"
            ], function (Map, Extent, esriConfig, Draw, geometryEngineAsync, GraphicsLayer, Graphic, Point, webMercatorUtils, Query, QueryTask, Extent, PictureMarkerSymbol,
                         InfoTemplate, FindTask, FindParameters, IdentifyTask, IdentifyParameters) {
                if (drawLayerPoint) {
                    drawLayerPoint.clear();
                }
                //防止开始绘制前多次绑定绘制完成的方法
                if (drawEndHandler) {
                    dojo.disconnect(drawEndHandler);
                }
                //防止开始绘制前多次初始化绘制工具
                if (drawToolBar) {
                    drawToolBar.deactivate();
                } else {
                    drawToolBar = new Draw(eventmap);
                }
                drawToolBar.activate(Draw.POINT);
                drawEndHandler = dojo.connect(drawToolBar, "onDrawEnd", function (geometry) {
                    drawToolBar.deactivate();
                    var webMercator;
                    if (ConfigSetting.isWebMercator == "true") {
                        webMercator = webMercatorUtils.xyToLngLat(geometry.x, geometry.y);
                    } else {
                        webMercator = [geometry.x, geometry.y]
                    }
                    $("#reportoptimize img").css("display", "none");
                    $("#eventReport").modal("show");
                    $("#reportoptimize select").val(null).trigger("change");
                    $("#reportoptimize input").val("");
                    $("#reportoptimize img").attr("src", "");
                    $("#eventReport").on("hide.bs.modal", function () {

                    });
                    reportbaseStr = "";
                    $("#reportfssj").datetimepicker('remove');
                    $("#reportfssj").datetimepicker({
                        autoclose: true,
                        clearBtn: true,// 自定义属性,true 显示 清空按钮 false 隐藏 默认:true
                        language: 'cn',
                        minuteStep: 1,
                        endDate: new Date(),
                        format: "yyyy-mm-dd hh:ii:ss"
                    });
                    $("#reportbjsj").datetimepicker("setDate", new Date());
                    $("#reportjd").val(webMercator[0]);
                    $("#reportwd").val(webMercator[1]);
                    $("#reportbjr").val(ConfigSetting.CURRENT_USER_NAME);
                    $("#reportbtnCaseSave").off().on("click", function () {
                        var hapTime;
                        if ($("#reportfssj").val() != "" && $("#reportfssj").val() != null) {
                            hapTime = $("#reportfssj").val()
                        } else {
                            common.showInfo("请选择发生时间")
                            return
                        }
                        var sjlx;
                        if ($("#reportsjlx").val() != "" && $("#reportsjlx").val() != null) {
                            sjlx = $("#reportsjlx").val()
                        } else {
                            common.showInfo("请选择事件类型");
                            return
                        }

                        var lddm;
                        var dldm;
                        if ($("#reportdldm").val() != "" && $("#reportdldm").val() != null) {
                            lddm = $("#reportdldm").val()
                        } else {
                            common.showInfo("请选择道路代码");
                            return
                        }
                        var KILOMETERS = "";
                        if (!reportisKill) {
                            if ($("#reportgls").val() != "" && $("#reportgls").val() != null && (/^[0-9]+$/.test($("#reportgls").val()))) {
                                KILOMETERS = $("#reportgls").val()
                            } else {
                                common.showInfo("请正确填写公里数");
                                return
                            }
                        } else {
                            if ($("#reportlkld").val() != "" && $("#reportlkld").val() != null) {
                                dldm = $("#reportlkld").val()
                            } else {
                                common.showInfo("请选择路段代码")
                                return
                            }
                        }
                        var sjdj;
                        if ($("#reportsjdjModal").val() != "" && $("#reportsjdjModal").val() != null) {
                            sjdj = $("#reportsjdjModal").val()
                        } else {
                            common.showInfo("请选择事件等级");
                            return
                        }
                        var bjnr;
                        if ($("#reportbjnr").val() != "" && $("#reportbjnr").val() != null) {
                            bjnr = $("#reportbjnr").val()
                        } else {
                            common.showInfo("请输入报警内容");
                            return
                        }
                        var obj = {
                            "incidentSource": "1",
                            "happenTime": hapTime,
                            "incidentType": sjlx,
                            "incidentSubType": $("#reportsjzl").val() != null ? $("#reportsjzl").val() : "",
                            "crossCode": dldm,
                            "roadCode": lddm,
                            "kilometers": KILOMETERS,
                            "laneDirection": $("#reportszfxModal").val() != null ? $("#reportszfxModal").val() : "",
                            "incidentLevel": sjdj,
                            "incidentPlace": $("#reportfsddModal").val(),
                            "longitude": $("#reportjd").val(),
                            "latitude": $("#reportwd").val(),
                            "alarmContent": bjnr,
                            // "alarmPerson":$("#reportbjr").val(),
                            "alarmPhone": $("#reportlxdh").val(),
                            "alarmTime": $("#reportbjsj").val(),
                            "createUser": ConfigSetting.MY_ID,
                            "imageOne": Imageobj.imageOne,
                            "imageTwo": Imageobj.imageTwo,
                            "imageThree": Imageobj.imageThree,
                            "imageFour": Imageobj.imageFour
                        };
                        $.post("/trafficEvent/addTrIncidentInfo", obj, function (data) {
                            if (data.state == "SUCCESS") {
                                $("#eventReport").modal("hide");
                                $("#reportoptimize select").val(null).trigger("change");
                                $("#reportoptimize input").val("");
                                $("#reportoptimize img").attr("src", "");
                                common.showSuccess("上报成功");
                                loadEventMap()
                            }
                            if (data.state == "FAIL") {
                                $("#eventReport").modal("hide");
                                $("#reportoptimize select").val(null).trigger("change");
                                $("#reportoptimize input").val("");
                                $("#reportoptimize img").attr("src", "");
                                common.showWarn(data.msg);
                                loadEventMap()
                            }
                            if (data.state == "ERROR") {
                                $("#eventReport").modal("hide");
                                $("#reportoptimize select").val(null).trigger("change");
                                $("#reportoptimize input").val("");
                                $("#reportoptimize img").attr("src", "");
                                common.showWarn("上报失败");
                                loadEventMap()
                            }
                        }, "json")
                    })
                });
            });
        });

    };
    var height0Percent = function (eleId) {
        var element = $(eleId);
        if (element != null && element.length > 0) {
            element.css("height", "0");
        }
    };
    var height100Percent = function (eleId) {
        var element = $(eleId);
        if (element != null && element.length > 0) {
            element.css("height", "100%");
        }
    };
    var height50Percent = function (eleId) {
        var element = $(eleId);
        if (element != null && element.length > 0) {
            element.css("height", "50%");
        }
    };
    //时间-------------------------start
    var typeTimeEcharts = function (params, func) {
        $.getJSON("/trafficEvent/statisticsGroupByTimeQuantum", params, function (data) {
            if (data != null && data.state == "SUCCESS") {
                func(data.rows);
            } else {
                common.showError("系统异常！", "警告");
            }
        });
    };
    var typeTimeleftEcharts = function (params, func) {
        $.getJSON("/trafficEvent/statisticsGroupByHappenTime", params, function (data) {
            if (data != null && data.state == "SUCCESS") {
                func(data.rows);
            } else {
                common.showError("系统异常！", "警告");
            }
        });
    };
    var loadNoTime = function () {
        //销毁top echarts
        if (timeTabTopEcharts) {
            timeTabTopEcharts.dispose();
            timeTabTopEcharts = null;
        }
        //销毁对比echarts
        if (timeTabBottomEcharts) {
            timeTabBottomEcharts.dispose();
            timeTabBottomEcharts = null;
        }
        if (timeTabTimeright) {
            timeTabTimeright.dispose();
            timeTabTimeright = null;
        }
        height0Percent("#timeTabBottomEcharts");
        height100Percent("#timeTabTopEcharts");
        typeTimeEcharts(searchObj, function (data) {
            var formatData = formatTimeData(data);
            timeEchartsright(data, null, illegalTypeName);
        });
        typeTimeleftEcharts(searchObj, function (data) {
            var formatData = formatTimeLeftData(data);
            timeEcharts1(formatData[0], formatData[1], illegalTypeName);
        });
    };
    var loadYesTime = function () {
        if (timeTabTopEcharts) {
            timeTabTopEcharts.dispose();
            timeTabTopEcharts = null;
        }
        //销毁对比echarts
        if (timeTabBottomEcharts) {
            timeTabBottomEcharts.dispose();
            timeTabBottomEcharts = null;
        }
        height50Percent("#timeTabTopEcharts");
        height50Percent("#timeTabBottomEcharts");

        typeTimeleftEcharts(searchObj, function (data) {
            var formatData = formatTimeLeftData(data);
            timeEcharts1(formatData[0], formatData[1], illegalTypeName);
        });
        typeTimeleftEcharts({
            "startTime": searchObj.nextStartTime,
            "endTime": searchObj.nextEndTime,
            "timeType": searchObj.timeType,
            "incidentType": searchObj.incidentType,
            "incidentSubType": searchObj.incidentSubType
        }, function (data) {
            var formatData = formatTimeLeftData(data);
            timeEcharts2(formatData[0], formatData[1], illegalTypeName);
        });
        if (timeTabTimeright) {
            timeTabTimeright.dispose();
            timeTabTimeright = null;
        }
        typeTimeEcharts(searchObj, function (data1) {
            typeTimeEcharts({
                "startTime": searchObj.nextStartTime,
                "endTime": searchObj.nextEndTime,
                "timeType": searchObj.timeType,
                "incidentType": searchObj.incidentType,
                "incidentSubType": searchObj.incidentSubType
            }, function (data2) {
                timeEchartsright(data1, data2, illegalTypeName);
            });
        });
    };
    var loadNoPoint = function () {
        var pointType = $("#pointType>li.active>a").attr("type");
        if (pointType === "点位图表") {
            //销毁top echarts
            if (topPointsEchartInstance) {
                topPointsEchartInstance.dispose();
                topPointsEchartInstance = null;
            }
            //清空对比echarts
            if (bottomPointsEchartInstance) {
                bottomPointsEchartInstance.dispose();
                bottomPointsEchartInstance = null;
            }
            height0Percent("#bottomPointsEchart");
            height100Percent("#topPointsEchart");
            typePointsEcharts(searchObj, function (data) {
                var formatData = formatPointsData(data);
                topPointsEchart(formatData[0], formatData[1], formatData[2], illegalTypeName);
            });
            if (otherRightBottomLeftEchart) {
                otherRightBottomLeftEchart.dispose();
                otherRightBottomLeftEchart = null;
            }
            if (otherRightBottomRightEchart) {
                otherRightBottomRightEchart.dispose();
                otherRightBottomRightEchart = null;
            }
            widthPercent("#otherRightBottomRight", "0");
            widthPercent("#otherRightBottomLeft", "100");
            otherOrgData(searchObj, function (data) {
                otherRightEcharts1(data, searchObj.startTime, searchObj.endTime);
            });
        } else if (pointType === "点位地图") {
            $("#prevMap").show();
            $("#nextMap").hide();
            typeMapPointsEcharts(searchObj, function (data) {
                formatMapData(data, "当前时段");
            });
        }
    };
    /*点位-------------------start*/
    var typeMapPointsEcharts = function (params, func) {
        $.getJSON("/trafficEvent/statisticsGroupByLocationAndType", params, function (data) {
            if (data != null && data.state == "SUCCESS") {
                func(data.rows);
            } else {
                common.showError("系统异常！", "警告");
            }
        });
    };
    var loadYesPoint = function () {
        var pointType = $("#pointType>li.active>a").attr("type");
        if (pointType === "点位图表") {
            //销毁top echarts
            if (topPointsEchartInstance) {
                topPointsEchartInstance.dispose();
                topPointsEchartInstance = null;
            }
            //清空对比echarts
            if (bottomPointsEchartInstance) {
                bottomPointsEchartInstance.dispose();
                bottomPointsEchartInstance = null;
            }
            height50Percent("#topPointsEchart");
            height50Percent("#bottomPointsEchart");
            typePointsEcharts(searchObj, function (data) {
                var formatData = formatPointsData(data);
                topPointsEchart(formatData[0], formatData[1], formatData[2], illegalTypeName);
            });
            typePointsEcharts({
                "startTime": searchObj.nextStartTime,
                "endTime": searchObj.nextEndTime,
                "timeType": searchObj.timeType,
                "incidentType": searchObj.incidentType,
                "incidentSubType": searchObj.incidentSubType
            }, function (data) {
                var formatData = formatPointsData(data);
                bottomPointsEchart(formatData[0], formatData[1], formatData[2], illegalTypeName);
            });
            if (otherRightBottomLeftEchart) {
                otherRightBottomLeftEchart.dispose();
                otherRightBottomLeftEchart = null;
            }
            if (otherRightBottomRightEchart) {
                otherRightBottomRightEchart.dispose();
                otherRightBottomRightEchart = null;
            }
            widthPercent("#otherRightBottomRight", "50");
            widthPercent("#otherRightBottomLeft", "50");
            otherOrgData(searchObj, function (data) {
                otherRightEcharts1(data, searchObj.startTime, searchObj.endTime);
            });
            otherOrgData({
                "startTime": searchObj.nextStartTime,
                "endTime": searchObj.nextEndTime,
                "timeType": searchObj.timeType,
                "incidentType": searchObj.incidentType,
                "incidentSubType": searchObj.incidentSubType
            }, function (data) {
                otherRightEcharts2(data, searchObj.nextStartTime, searchObj.nextEndTime);
            });
        } else if (pointType === "点位地图") {
            $("#prevMap").show();
            $("#nextMap").show();
            //判断显示 “当前时间”数据 还是 “对比时间”数据
            var activeDivName = $(".twoMapTab .timeActive").attr("divName");
            if (activeDivName === "当前时段") {
                typeMapPointsEcharts(searchObj, function (data) {
                    formatMapData(data, "当前时段")
                });
            } else if (activeDivName === "对比时段") {
                typeMapPointsEcharts({
                    timeType: searchObj.timeType,
                    startTime: searchObj.nextStartTime,
                    endTime: searchObj.nextEndTime,
                    incidentType: searchObj.incidentType,
                    incidentSubType: searchObj.incidentSubType
                }, function (data) {
                    formatMapData(data, "对比时段")
                });
            }
        }
    };
    var timeEchartsright = function (data, data2, illegalTypeName) {
        var legendData = [];
        var tooltipFormatter = '';
        legendData.push(searchObj.startTime + " ~ " + searchObj.endTime);
        tooltipFormatter = '{b0}<br /> ' + searchObj.startTime + ' ~ ' + searchObj.endTime + ' ' + illegalTypeName + '：{c0}<br /> ';
        var xAxisData = [];
        var seriesData = [];
        var prevObj = {
            name: searchObj.startTime + " ~ " + searchObj.endTime,
            type: 'line',
            symbol: 'image://' + baseURL + '/images/illegal/weChatColor.png',
            showSymbol: false,
            symbolSize: 15,
            smooth: true,
            itemStyle: {
                color: "#0ADD39"
            },
            lineStyle: {
                color: "#0ADD39",
                width: 2
            },
            areaStyle: {
                color: {
                    type: 'linear',
                    x: 0,
                    y: 0,
                    x2: 0,
                    y2: 1,
                    colorStops: [{
                        offset: 0, color: 'rgba(62,198,92,0.43)' // 0% 处的颜色
                    }, {
                        offset: 1, color: 'transparent' // 100% 处的颜色
                    }],
                    global: false // 缺省为 false
                }
            },
            data: []
        };
        var prevData = [];

        $.each(data, function (index, ele) {
            // var arr = ele.split("-");
            // var start = parseInt(arr[0]);
            // var end = parseInt(arr[1]);
            xAxisData.push(ele.TIME_NAME);

            prevData.push(ele.NUM);

        });
        prevObj.data = prevData;
        seriesData.push(prevObj);

        if (data2 !== null) {//有对比
            legendData.push(searchObj.nextStartTime + " ~ " + searchObj.nextEndTime);
            tooltipFormatter = tooltipFormatter + searchObj.nextStartTime + ' ~ ' + searchObj.nextEndTime + ' ' + illegalTypeName + '：{c1}<br /> ';

            var nextObj = {
                name: searchObj.nextStartTime + " ~ " + searchObj.nextEndTime,
                type: 'line',
                symbol: 'image://' + baseURL + '/images/illegal/lineHover.png',
                showSymbol: false,
                symbolSize: 15,
                smooth: true,
                itemStyle: {
                    color: "#47D4FD"
                },
                lineStyle: {
                    color: "#47D4FD",
                    width: 2
                },
                areaStyle: {
                    color: {
                        type: 'linear',
                        x: 0,
                        y: 0,
                        x2: 0,
                        y2: 1,
                        colorStops: [{
                            offset: 0, color: 'rgba(8,118,255,0.43)' // 0% 处的颜色
                        }, {
                            offset: 1, color: 'transparent' // 100% 处的颜色
                        }],
                        global: false // 缺省为 false
                    }
                },
                data: []
            };
            var nextData = [];

            $.each(data2, function (index, ele) {
                nextData.push(ele.NUM);
            });
            nextObj.data = nextData;
            seriesData.push(nextObj);
        }


        require(["../../website-js/illegal/echarts.js"], function (echarts) {
            var option = {
                backgroundColor: "rgb(30,54,80)",
                legend: {
                    data: legendData,
                    textStyle: {
                        color: "white"
                    },
                    bottom: '1%'
                },
                title: {
                    // text: '处罚金额',
                    left: 'center',
                    textStyle: {
                        color: 'white',
                        fontSize: 18,
                        fontFamily: 'MicrosoftYaHei',
                        fontWeight: 400
                    }
                },
                tooltip: {
                    trigger: 'axis',
                    backgroundColor: '#001028',
                    borderColor: '#2C5599',
                    borderWidth: 1,
                    formatter: tooltipFormatter/*'{b0}<br /> '+'2018-12-14 ~ 2018-12-20 闯红灯：{c0}<br />'+'2018-12-7 ~ 2018-12-13 闯红灯：{c1}<br />'*/,
                    axisPointer: {
                        type: 'cross',
                        label: {
                            backgroundColor: '#6a7985'
                        }
                    }
                },
                grid: {
                    left: '3%',
                    right: '3%',
                    bottom: '10%',
                    containLabel: true
                },
                xAxis: [
                    {
                        type: 'category',
                        axisLine: {
                            show: false
                        },
                        axisTick: {//刻度线
                            show: false
                        },
                        splitLine: {//分隔线
                            show: false
                        },
                        axisLabel: {
                            color: 'white'
                        },
                        boundaryGap: false,
                        axisPointer: {
                            lineStyle: {
                                color: '#54AAE3',
                                opacity: 0.5,
                                width: 1
                            },
                            label: {
                                show: true
                            }
                        },
                        data: xAxisData
                    }
                ],
                yAxis: [
                    {
                        name: '数量           ',
                        nameTextStyle: {
                            color: "white"
                        },
                        type: 'value',
                        axisLine: {
                            lineStyle: {
                                color: "rgb(101,110,113)"
                            }
                        },
                        axisTick: {//刻度线
                            show: false
                        },
                        splitLine: {//分隔线
                            show: true,
                            lineStyle: {
                                color: "rgb(101,110,113)"
                            }
                        },
                        axisLabel: {
                            color: 'white'
                        }
                    }
                ],
                series: seriesData
            };

            timeTabTimeright = echarts.init(document.getElementById("timeTabTimeright"));
            timeTabTimeright.setOption(option, true);
        });
    };
    var loadNoOther = function () {
        if (importantLeftBottomEchartInstance) {
            importantLeftBottomEchartInstance.dispose();
            importantLeftBottomEchartInstance = null;
        }
        typeImportantData(searchObj, function (data) {
            initImportantEcharts(data, null, "50%");
        });
        if (otherLeftEchartsInstance) {
            otherLeftEchartsInstance.dispose();
            otherLeftEchartsInstance = null;
        }
        otherData(searchObj, function (data1) {
            otherLeftEcharts(data1, null, illegalTypeName);
        });
    };
    var loadYesOther = function () {
        if (importantLeftBottomEchartInstance) {
            importantLeftBottomEchartInstance.dispose();
            importantLeftBottomEchartInstance = null;
        }
        typeImportantData(searchObj, function (data) {
            typeImportantData({
                "startTime": searchObj.nextStartTime,
                "endTime": searchObj.nextEndTime,
                "timeType": searchObj.timeType,
                "incidentType": searchObj.incidentType,
                "incidentSubType": searchObj.incidentSubType
            }, function (data2) {
                initImportantEcharts(data, data2, "25%");
            })
        });
        if (otherLeftEchartsInstance) {
            otherLeftEchartsInstance.dispose();
            otherLeftEchartsInstance = null;
        }
        otherData(searchObj, function (data1) {
            otherData({
                "startTime": searchObj.nextStartTime,
                "endTime": searchObj.nextEndTime,
                "timeType": searchObj.timeType,
                "incidentType": searchObj.incidentType,
                "incidentSubType": searchObj.incidentSubType
            }, function (data2) {
                otherLeftEcharts(data1, data2, illegalTypeName);
            })
        });
    };
    var otherData = function (params, func) {
        $.getJSON("/trafficEvent/statisticsGroupByState", params, function (data) {
            if (data != null && data.state == "SUCCESS") {
                func(data.rows);
            } else {
                common.showError("系统异常！", "警告");
            }
        });
    };
    var otherLeftEchartsInstance = null;
    var otherLeftEcharts = function (data, data2, illegalTypeName) {
        var legendData = [];
        var tooltipFormatter = '';
        legendData.push(searchObj.startTime + " ~ " + searchObj.endTime);
        tooltipFormatter = '{b0}<br /> ' + searchObj.startTime + ' ~ ' + searchObj.endTime + ' ' + illegalTypeName + '：{c0}<br /> ';

        var xAxisData = [];
        var seriesData = [];
        var prevObj = {
            name: searchObj.startTime + " ~ " + searchObj.endTime,
            type: 'line',
            symbol: 'image://' + baseURL + '/images/illegal/weChatColor.png',
            showSymbol: false,
            symbolSize: 15,
            smooth: true,
            itemStyle: {
                color: "#0ADD39"
            },
            lineStyle: {
                color: "#0ADD39",
                width: 2
            },
            areaStyle: {
                color: {
                    type: 'linear',
                    x: 0,
                    y: 0,
                    x2: 0,
                    y2: 1,
                    colorStops: [{
                        offset: 0, color: 'rgba(62,198,92,0.43)' // 0% 处的颜色
                    }, {
                        offset: 1, color: 'transparent' // 100% 处的颜色
                    }],
                    global: false // 缺省为 false
                }
            },
            data: []
        };
        var prevData = [];

        $.each(data, function (index, ele) {
            // var arr = ele.split("-");
            // var start = parseInt(arr[0]);
            // var end = parseInt(arr[1]);
            xAxisData.push(ele.INCIDENTSOURCENAME);

            prevData.push(ele.NUM);

        });
        prevObj.data = prevData;
        seriesData.push(prevObj);

        if (data2 !== null) {//有对比
            legendData.push(searchObj.nextStartTime + " ~ " + searchObj.nextEndTime);
            tooltipFormatter = tooltipFormatter + searchObj.nextStartTime + ' ~ ' + searchObj.nextEndTime + ' ' + illegalTypeName + '：{c1}<br /> ';

            var nextObj = {
                name: searchObj.nextStartTime + " ~ " + searchObj.nextEndTime,
                type: 'line',
                symbol: 'image://' + baseURL + '/images/illegal/lineHover.png',
                showSymbol: false,
                symbolSize: 15,
                smooth: true,
                itemStyle: {
                    color: "#47D4FD"
                },
                lineStyle: {
                    color: "#47D4FD",
                    width: 2
                },
                areaStyle: {
                    color: {
                        type: 'linear',
                        x: 0,
                        y: 0,
                        x2: 0,
                        y2: 1,
                        colorStops: [{
                            offset: 0, color: 'rgba(8,118,255,0.43)' // 0% 处的颜色
                        }, {
                            offset: 1, color: 'transparent' // 100% 处的颜色
                        }],
                        global: false // 缺省为 false
                    }
                },
                data: []
            };
            var nextData = [];

            $.each(data2, function (index, ele) {
                nextData.push(ele.NUM);
            });
            nextObj.data = nextData;
            seriesData.push(nextObj);
        }


        require(["../../website-js/illegal/echarts.js"], function (echarts) {
            var option = {
                backgroundColor: "rgb(30,54,80)",
                legend: {
                    data: legendData,
                    textStyle: {
                        color: "white"
                    },
                    bottom: '1%'
                },
                title: {
                    // text: '处罚金额',
                    left: 'center',
                    textStyle: {
                        color: 'white',
                        fontSize: 18,
                        fontFamily: 'MicrosoftYaHei',
                        fontWeight: 400
                    }
                },
                tooltip: {
                    trigger: 'axis',
                    backgroundColor: '#001028',
                    borderColor: '#2C5599',
                    borderWidth: 1,
                    formatter: tooltipFormatter/*'{b0}<br /> '+'2018-12-14 ~ 2018-12-20 闯红灯：{c0}<br />'+'2018-12-7 ~ 2018-12-13 闯红灯：{c1}<br />'*/,
                    axisPointer: {
                        type: 'cross',
                        label: {
                            backgroundColor: '#6a7985'
                        }
                    }
                },
                grid: {
                    left: '3%',
                    right: '3%',
                    bottom: '10%',
                    containLabel: true
                },
                xAxis: [
                    {
                        type: 'category',
                        axisLine: {
                            show: false
                        },
                        axisTick: {//刻度线
                            show: false
                        },
                        splitLine: {//分隔线
                            show: false
                        },
                        axisLabel: {
                            color: 'white'
                        },
                        boundaryGap: false,
                        axisPointer: {
                            lineStyle: {
                                color: '#54AAE3',
                                opacity: 0.5,
                                width: 1
                            },
                            label: {
                                show: true
                            }
                        },
                        data: xAxisData
                    }
                ],
                yAxis: [
                    {
                        name: '数量           ',
                        nameTextStyle: {
                            color: "white"
                        },
                        type: 'value',
                        axisLine: {
                            lineStyle: {
                                color: "rgb(101,110,113)"
                            }
                        },
                        axisTick: {//刻度线
                            show: false
                        },
                        splitLine: {//分隔线
                            show: true,
                            lineStyle: {
                                color: "rgb(101,110,113)"
                            }
                        },
                        axisLabel: {
                            color: 'white'
                        }
                    }
                ],
                series: seriesData/*[
                 {
                 name:'2018-12-14 ~ 2018-12-20',
                 type:'line',
                 symbol:'image://'+baseURL+'/images/illegal/weChatColor.png',
                 showSymbol:false,
                 symbolSize:15,
                 smooth: true,
                 itemStyle: {
                 color: "#0ADD39"
                 },
                 lineStyle:{
                 color:"#0ADD39",
                 width:2
                 },
                 areaStyle: {
                 color: {
                 type: 'linear',
                 x: 0,
                 y: 0,
                 x2: 0,
                 y2: 1,
                 colorStops: [{
                 offset: 0, color: 'rgba(62,198,92,0.43)' // 0% 处的颜色
                 }, {
                 offset: 1, color: 'transparent' // 100% 处的颜色
                 }],
                 global: false // 缺省为 false
                 }
                 },
                 data:[120, 132, 101, 134, 90, 230, 210]
                 },
                 {
                 name:'2018-12-7 ~ 2018-12-13',
                 type:'line',
                 symbol:'image://'+baseURL+'/images/illegal/lineHover.png',
                 showSymbol:false,
                 symbolSize:15,
                 smooth: true,
                 itemStyle: {
                 color: "#47D4FD"
                 },
                 lineStyle:{
                 color:"#47D4FD",
                 width:2
                 },
                 areaStyle: {
                 color: {
                 type: 'linear',
                 x: 0,
                 y: 0,
                 x2: 0,
                 y2: 1,
                 colorStops: [{
                 offset: 0, color: 'rgba(8,118,255,0.43)' // 0% 处的颜色
                 }, {
                 offset: 1, color: 'transparent' // 100% 处的颜色
                 }],
                 global: false // 缺省为 false
                 }
                 },
                 data:[150, 160, 170, 100, 80, 70, 120]
                 }
                 ]*/
            };

            otherLeftEchartsInstance = echarts.init(document.getElementById("otherLeftBottom"));
            otherLeftEchartsInstance.setOption(option, true);
        });
    };
    var typeImportantData = function (params, func) {
        $.getJSON("/trafficEvent/statisticsGroupBySource", params, function (data) {
            if (data != null && data.state == "SUCCESS") {
                func(data.rows);
            } else {
                common.showError("系统异常！", "警告");
            }
        });
    };
    var importantLeftBottomEchartInstance = null;
    var importantVehicleColor = {"1": "#2F63FC", "2": "#FDD32C", "3": "#47D4FD"};
    var initImportantEcharts = function (data, data2, dataCenterPercent) {
        var allType = [];
        var series = [];

        var prevObj = {
            type: 'pie',
            radius: [screen.height <= 900 ? 15 : 45, "65%"],
            center: [dataCenterPercent, '40%'],//没有对比 echarts 居中显示
            roseType: 'area',
            itemStyle: {
                normal: {
                    label: {
                        show: false,
                        formatter: [
                            '{reportState|{b}}', '{sum|{c}}{chinese| 次}'].join('\n'),
                        rich: {
                            reportState: {
                                color: '#eee',
                                fontSize: 14,
                                align: 'left'
                            },
                            sum: {
                                color: '#eee',
                                fontSize: 24
                            },
                            chinese: {
                                color: '#eee',
                                fontSize: 14
                            }
                        }
                    },
                    labelLine: {
                        lineStyle: {
                            color: "rgb(255,255,255)"
                        }
                    },
                    borderColor: 'rgb(30,54,80)',
                    borderWidth: 5
                }
            },
            data: []
        };
        var prevStatic = {
            type: 'pie',
            silent: true,//鼠标点击事件不触发
            animation: false,
            center: [dataCenterPercent, '40%'],
            radius: [screen.height <= 900 ? 17 : 47, screen.height <= 900 ? 26 : 65],
            itemStyle: {
                normal: {
                    label: {
                        show: false
                    }
                }
            },
            tooltip: {
                trigger: '随便', /*让tooltip 无效*/
                formatter: "{a} <br/>{b} : {c} ({d}%)"
            },
            data: [
                {
                    name: '', value: 1, itemStyle: {
                    normal: {
                        color: 'rgba(168,225,255,0.3)',
                        label: {
                            show: false
                        },
                        labelLine: {
                            show: false
                        }
                    }
                }
                },
                {
                    name: '', value: 0, itemStyle: {
                    normal: {
                        label: {
                            show: false
                        },
                        labelLine: {
                            show: false
                        }
                    }
                }
                }
            ]
        };

        var prevObjData = [];
        $.each(data, function (index, ele) {
            allType.push(ele.INCIDENTSOURCENAME);
            if (ele.NUM >= 0) {
                var obj = {
                    value: ele.NUM, name: ele.INCIDENTSOURCENAME,
                    itemStyle: {
                        normal: {
                            label: {
                                show: false
                            },
                            color: importantVehicleColor[ele.INCIDENTSOURCE]
                        }
                    }
                };
                prevObjData.push(obj);
            }
        });
        if (prevObjData.length > 0) {
            prevObj.data = prevObjData;
        }
        series.push(prevObj);
        if (prevObjData.length > 0) {
            series.push(prevStatic);
        }

        if (data2 !== null) {//有对比
            $("#importantLeftBottomLeftTime,#importantLeftBottomRightTime").css("width", "49%");
            $("#importantLeftBottomRightTime").show();
            $("#importantLeftBottomLeftTime").text(searchObj.startTime + "-" + searchObj.endTime);
            $("#importantLeftBottomRightTime").text(searchObj.nextStartTime + "-" + searchObj.nextEndTime);
            var nextObj = {
                type: 'pie',
                radius: [screen.height <= 900 ? 15 : 45, "65%"],
                center: ['75%', '40%'],
                roseType: 'area',
                itemStyle: {
                    normal: {
                        label: {
                            show: false,
                            formatter: [
                                '{reportState|{b}}', '{sum|{c}}{chinese| 次}'].join('\n'),
                            rich: {
                                reportState: {
                                    color: '#eee',
                                    fontSize: 14,
                                    align: 'left'
                                },
                                sum: {
                                    color: '#eee',
                                    fontSize: 24
                                },
                                chinese: {
                                    color: '#eee',
                                    fontSize: 14
                                }
                            }

                        },
                        labelLine: {
                            lineStyle: {
                                color: "rgb(255,255,255)"
                            }
                        },
                        borderColor: 'rgb(30,54,80)',
                        borderWidth: 5
                    }
                },
                data: []
            };
            var nextStatic = {
                type: 'pie',
                animation: false,
                silent: true,//鼠标点击事件不触发
                center: ['75%', '40%'],
                radius: [screen.height <= 900 ? 17 : 47, screen.height <= 900 ? 26 : 65],
                itemStyle: {
                    normal: {
                        label: {
                            show: false
                        }
                    }
                },
                tooltip: {
                    trigger: '随便', /*让tooltip 无效*/
                    formatter: "{a} <br/>{b} : {c} ({d}%)"
                },
                data: [
                    {
                        name: '', value: 1, itemStyle: {
                        normal: {
                            color: 'rgba(168,225,255,0.3)',
                            label: {
                                show: false
                            },
                            labelLine: {
                                show: false
                            }
                        }
                    }
                    },
                    {
                        name: '', value: 0, itemStyle: {
                        normal: {
                            label: {
                                show: false
                            },
                            labelLine: {
                                show: false
                            }
                        }
                    }
                    }
                ]
            };

            var nextObjData = [];
            $.each(data2, function (index, ele) {
                if (ele.NUM >= 0) {
                    var obj = {
                        value: ele.NUM, name: ele.INCIDENTSOURCENAME,
                        itemStyle: {
                            normal: {
                                label: {
                                    show: false
                                },
                                color: importantVehicleColor[ele.INCIDENTSOURCE]
                            }
                        }
                    };
                    nextObjData.push(obj);
                }
            });
            if (nextObjData.length > 0) {
                nextObj.data = nextObjData;
            }
            series.push(nextObj);
            if (nextObjData.length > 0) {
                series.push(nextStatic);
            }
        } else {
            $("#importantLeftBottomLeftTime").text(searchObj.startTime + "-" + searchObj.endTime);
            $("#importantLeftBottomLeftTime").css("width", "100%");
            $("#importantLeftBottomRightTime").css("width", "0");
            $("#importantLeftBottomRightTime").hide()
        }
        require(["../../website-js/illegal/echarts.js"], function (echarts) {
            var option = {
                backgroundColor: "rgb(30,54,80)",
                tooltip: {
                    trigger: 'item',
                    backgroundColor: '#001028',
                    borderColor: '#2C5599',
                    borderWidth: 1,
                    formatter: "{b} : {c} ({d}%)"
                },
                legend: {
                    itemWidth: 16,
                    itemHeight: 16,
                    textStyle: {
                        color: "rgb(255,255,255)"
                    },
                    bottom: '5%',
                    itemGap: 15,
                    data: allType
                },
                // calculable : true,
                series: series
            };
            importantLeftBottomEchartInstance = echarts.init(document.getElementById("importantLeftBottom"));
            importantLeftBottomEchartInstance.setOption(option, true);

        });
    };
    var otherOrgData = function (params, func) {
        params.begin = 0;
        params.limit = 10
        $.getJSON("/trafficEvent/statisticsGroupByCrossCode", params, function (data) {
            if (data != null && data.state == "SUCCESS") {
                func(data.rows);
            } else {
                common.showError("系统异常！", "警告");
            }
        });
    };
    var otherRightBottomLeftEchart = null;
    var otherRightEcharts1 = function (data, startTime, endTime) {//data 已经从大到小排过序了
        data.reverse();
        var allPointsName = [];
        var allPointsNum = [];
        var allMax = [];
        if (data != null && data.length > 0) {
            $.each(data, function (index, ele) {
                allPointsName.push($.trim(ele.CROSSNAME));
                allPointsNum.push(ele.NUM);
                allMax.push((data[data.length - 1].NUM / 10 + 1) * 10);
            });
        } else {
            for (var i = 0; i <= 9; i++) {
                allPointsName.push("");
                allPointsNum.push(0);
                allMax.push(10);
            }
        }
        require(["../../website-js/illegal/echarts.js"], function (echarts) {
            var option = {
                backgroundColor: "rgb(30,54,80)",
                title: {
                    bottom: '1%',
                    text: startTime + ' ~ ' + endTime,
                    left: 'center',
                    textStyle: {
                        color: '#47E0B8',
                        fontSize: 12,
                        fontFamily: 'MicrosoftYaHei',
                        fontWeight: 400
                    }
                },
                textStyle: {
                    color: "rgb(255,255,255)"
                },
                tooltip: {
                    trigger: 'axis',
                    backgroundColor: '#001028',
                    borderColor: '#2C5599',
                    borderWidth: 1,
                    formatter: '{b1}<br />数量：{c1}',
                    axisPointer: {            // 坐标轴指示器，坐标轴触发有效
                        type: 'none'        // 默认为直线，可选为：'line' | 'shadow'
                    }
                },
                grid: {
                    top: 0,
                    left: '3%',
                    right: '4%',
                    bottom: '10%',
                    containLabel: true
                },
                xAxis: {
                    type: 'value',
                    axisLine: {
                        lineStyle: {
                            color: "rgb(101,110,113)"
                        }
                    },
                    splitLine: {
                        show: false
                    },
                    axisTick: {
                        show: false
                    },
                    axisLabel: {
                        interval: 0,
                        rotate: -35
                    }
                },
                yAxis: {
                    type: 'category',
                    data: allPointsName,
                    axisLine: {
                        lineStyle: {
                            color: "rgb(101,110,113)"
                        }
                    },
                    splitLine: {//分隔线
                        show: false,
                        lineStyle: {
                            color: "rgb(101,110,113)"
                        }
                    },
                    axisTick: {//刻度线
                        show: false
                    }
                },
                series: [
                    screen.height <= 900 ? { // For shadow(分辨率小于等于900)
                        type: 'bar',
                        itemStyle: {
                            normal: {color: 'rgba(71,224,184,0.1)'}
                        },
                        barGap: '-87.5%',
                        barCategoryGap: '20%',
                        barWidth: 20,
                        data: allMax
                    } : { // For shadow（分辨率大于900）
                        type: 'bar',
                        itemStyle: {
                            normal: {color: 'rgba(71,224,184,0.1)'}
                        },
                        barGap: '-75%',
                        barCategoryGap: '20%',
                        barWidth: 30,
                        data: allMax
                    },
                    {
                        type: 'bar',
                        barWidth: 15,
                        animation: false,
                        itemStyle: {
                            normal: {
                                color: new echarts.graphic.LinearGradient(
                                    1, 0, 0, 0,
                                    [
                                        {offset: 0, color: 'rgba(71,224,184,1)'},
                                        {offset: 0.5, color: 'rgba(71,224,184,0.5)'},
                                        {offset: 1, color: 'rgba(71,224,184,0)'}
                                    ]
                                )
                            },
                            emphasis: {
                                color: new echarts.graphic.LinearGradient(
                                    1, 0, 0, 0,
                                    [
                                        {offset: 0, color: 'rgba(71,224,184,1)'},
                                        {offset: 0.5, color: 'rgba(71,224,184,0.9)'},
                                        {offset: 1, color: 'rgba(71,224,184,0.5)'}
                                    ]
                                )
                            }
                        },
                        label: {
                            show: true,
                            position: 'right',
                            formatter: '{c}'
                        },
                        data: allPointsNum
                    }


                ]
            };

            // 基于准备好的dom，初始化echarts实例
            otherRightBottomLeftEchart = echarts.init(document.getElementById("otherRightBottomLeft"));
            otherRightBottomLeftEchart.setOption(option);

        });
    };

    var otherRightBottomRightEchart = null;
    var otherRightEcharts2 = function (data, startTime, endTime) {//data 已经从大到小排过序了
        data.reverse();
        var allPointsName = [];
        var allPointsNum = [];
        var allMax = [];

        if (data != null && data.length > 0) {
            $.each(data, function (index, ele) {
                allPointsName.push($.trim(ele.CROSSNAME));
                allPointsNum.push(ele.NUM);
                allMax.push((data[data.length - 1].NUM / 10 + 1) * 10);
            });
        } else {
            for (var i = 0; i <= 9; i++) {
                allPointsName.push("");
                allPointsNum.push(0);
                allMax.push(10);
            }
        }

        require(["../../website-js/illegal/echarts.js"], function (echarts) {
            var option = {
                backgroundColor: "rgb(30,54,80)",
                title: {
                    bottom: '1%',
                    text: startTime + ' ~ ' + endTime,
                    left: 'center',
                    textStyle: {
                        color: '#B0DB3A',
                        fontSize: 12,
                        fontFamily: 'MicrosoftYaHei',
                        fontWeight: 400
                    }
                },
                textStyle: {
                    color: "rgb(255,255,255)"
                },
                tooltip: {
                    trigger: 'axis',
                    backgroundColor: '#001028',
                    borderColor: '#2C5599',
                    borderWidth: 1,
                    formatter: '{b1}<br />数量：{c1}',
                    axisPointer: {            // 坐标轴指示器，坐标轴触发有效
                        type: 'none'        // 默认为直线，可选为：'line' | 'shadow'
                    }
                },
                grid: {
                    top: 0,
                    left: '3%',
                    right: '4%',
                    bottom: '10%',
                    containLabel: true
                },
                xAxis: {
                    type: 'value',
                    axisLine: {
                        lineStyle: {
                            color: "rgb(101,110,113)"
                        }
                    },
                    splitLine: {
                        show: false
                    },
                    axisTick: {
                        show: false
                    },
                    axisLabel: {
                        interval: 0,
                        rotate: -35
                    }
                },
                yAxis: {
                    type: 'category',
                    data: allPointsName,
                    axisLine: {
                        lineStyle: {
                            color: "rgb(101,110,113)"
                        }
                    },
                    splitLine: {//分隔线
                        show: false,
                        lineStyle: {
                            color: "rgb(101,110,113)"
                        }
                    },
                    axisTick: {//刻度线
                        show: false
                    }
                },
                series: [
                    screen.height <= 900 ? { // For shadow(分辨率小于等于900)
                        type: 'bar',
                        itemStyle: {
                            normal: {color: 'rgba(71,224,184,0.1)'}
                        },
                        barGap: '-87.5%',
                        barCategoryGap: '20%',
                        barWidth: 20,
                        data: allMax
                    } : { // For shadow（分辨率大于900）
                        type: 'bar',
                        itemStyle: {
                            normal: {color: 'rgba(71,224,184,0.1)'}
                        },
                        barGap: '-75%',
                        barCategoryGap: '20%',
                        barWidth: 30,
                        data: allMax
                    },
                    {
                        type: 'bar',
                        barWidth: 15,
                        animation: false,
                        itemStyle: {
                            normal: {
                                color: new echarts.graphic.LinearGradient(
                                    1, 0, 0, 0,
                                    [
                                        {offset: 0, color: 'rgba(176,219,58,1)'},
                                        {offset: 0.5, color: 'rgba(176,219,58,0.5)'},
                                        {offset: 1, color: 'rgba(176,219,58,0)'}
                                    ]
                                )
                            },
                            emphasis: {
                                color: new echarts.graphic.LinearGradient(
                                    1, 0, 0, 0,
                                    [
                                        {offset: 0, color: 'rgba(176,219,58,1)'},
                                        {offset: 0.5, color: 'rgba(176,219,58,0.9)'},
                                        {offset: 1, color: 'rgba(176,219,58,0.5)'}
                                    ]
                                )
                            }
                        },
                        label: {
                            show: true,
                            position: 'right',
                            formatter: '{c}'
                        },
                        data: allPointsNum
                    }
                ]
            };

            // 基于准备好的dom，初始化echarts实例
            otherRightBottomRightEchart = echarts.init(document.getElementById("otherRightBottomRight"));
            otherRightBottomRightEchart.setOption(option);

        });
    };
    var isPrevLayer = true;
    var formatPointsData = function (data) {
        var format = [];
        if (data != null && data.length <= 0) {
            var emptyxData = new Array();
            var emptyyData = new Array();
            var emptydata3 = new Array();
            var str = " ";
            for (var i = 0; i <= 9; i++) {
                str = str + " ";
                emptyxData.push(str);
                emptyyData.push(0);
                emptydata3.push(10);
            }
            format[0] = emptyxData;
            format[1] = emptyyData;
            format[2] = emptydata3;
            return format;
        }
        var xData = [];
        var yData = [];
        $.each(data, function (index, ele) {
            xData.push($.trim(ele.ROADNAME));
            yData.push(ele.NUM);
        });
        var max = Math.max.apply(null, yData);
        max = (max === 0 ? 10 : max * 1.5);
        var data3 = new Array();
        yData.forEach(function (currentValue, index, arr) {
            data3.push(max);
        });
        format[0] = xData;
        format[1] = yData;
        format[2] = data3;
        return format;
    };
    /*点位-------------------start*/
    var typePointsEcharts = function (params, func) {
        params.begin = 0;
        params.limit = 10
        $.getJSON("/trafficEvent/statisticsGroupByRoadCode", params, function (data) {
            if (data != null && data.state == "SUCCESS") {
                func(data.rows);
            } else {
                common.showError("系统异常！", "警告");
            }
        });
    };
    var showeLayerstatistical = function () {
        if (isPrevLayer) {
            var prevLayer1 = map.getLayer("prevLayer");
            var prevLayerEventClusterLayer1 = map.getLayer("prevLayerEventClusterLayer");
            if (prevLayerEventClusterLayer1 != null) {
                if (map.getLevel() <= mapLevelAggregation) {
                    if (prevLayer1) {
                        prevLayer1.setVisibility(false);
                    }
                    if (EventClusterLayer) {
                        prevLayerEventClusterLayer1.setVisibility(true);
                    }
                } else {
                    if (prevLayer1) {
                        prevLayer1.setVisibility(true);
                    }
                    if (prevLayerEventClusterLayer1) {
                        prevLayerEventClusterLayer1.setVisibility(false);
                    }
                }
            }
        } else {
            var nextLayer1 = map.getLayer("nextLayer");
            var nextLayerEventClusterLayer1 = map.getLayer("nextLayerEventClusterLayer");
            if (nextLayerEventClusterLayer != null) {
                if (map.getLevel() <= mapLevelAggregation) {
                    if (nextLayer1) {
                        nextLayer1.setVisibility(false);
                    }
                    if (nextLayerEventClusterLayer1) {
                        nextLayerEventClusterLayer1.setVisibility(true);
                    }
                } else {
                    if (nextLayer1) {
                        nextLayer1.setVisibility(true);
                    }
                    if (nextLayerEventClusterLayer1) {
                        nextLayerEventClusterLayer1.setVisibility(false);
                    }
                }
            }
        }
    }
    var formatMapData = function (data, timeName) {
        require(["esri/dijit/OverviewMap", "extras/ClusterLayer",
                "dojo/_base/array", "esri/geometry/Point", "esri/symbols/SimpleMarkerSymbol",
                "esri/renderers/ClassBreaksRenderer", "esri/symbols/SimpleLineSymbol",
                "esri/Color", "esri/geometry/webMercatorUtils", "esri/symbols/TextSymbol"],
            function (OverviewMap, ClusterLayer, arrayUtils, Point, SimpleMarkerSymbol,
                      ClassBreaksRenderer, SimpleLineSymbol, Color, webMercatorUtils, TextSymbol) {
                var TextItems = [];
                var Data = [];
                var icon = "../../images/illegal/tollgateSum.png";
                var countyInfo = {
                    "data": []
                };
                $.each(data, function (index, ele) {
                    var centerLon = ele.longitude;
                    var centerLat = ele.latitude;
                    var attributes = {
                        "经度": centerLon,
                        "纬度": centerLat
                    };
                    if (ConfigSetting.isWebMercator === "true") {
                        var XY = webMercatorUtils.lngLatToXY(centerLon, centerLat);
                        centerLon = XY[0];
                        centerLat = XY[1];
                    }
                    var textItem = {
                        x: centerLon,
                        y: centerLat,
                        num: ele.list[0].NUM,
                        color: "#D00000"
                    };
                    for (var i = 0; i < ele.list.length; i++) {
                        var obj = {
                            "x": centerLon,
                            "y": centerLat,
                            "attributes": attributes
                        };
                        countyInfo.data.push(obj);
                    }
                    TextItems.push(textItem);
                    var Item = {
                        icon: icon,
                        x: centerLon,
                        y: centerLat,
                        // id: ele.tollgateId,
                        contentHTML: ''//气泡文本内容
                    };
                    Data.push(Item);
                });
                if (timeName === "当前时段") {
                    isPrevLayer = true;
                    var hiddenNextLayer = map.getLayer('nextLayer');
                    if (hiddenNextLayer != null) {
                        hiddenNextLayer.setVisibility(false);
                    }
                    var prevLayerEventClusterLayer1 = map.getLayer("prevLayerEventClusterLayer");
                    if (prevLayerEventClusterLayer1) {
                        map.removeLayer(prevLayerEventClusterLayer1)
                    }
                    var nextLayerEventClusterLayer1 = map.getLayer("nextLayerEventClusterLayer");
                    if (nextLayerEventClusterLayer1) {
                        nextLayerEventClusterLayer1.setVisibility(false);
                    }
                    prevLayerEventClusterLayer = new ClusterLayer({
                        "data": countyInfo.data,
                        "distance": 150,
                        "id": "prevLayerEventClusterLayer",
                        "labelColor": "#02abbd",
                        "labelOffset": 13,
                        "spatialReference": mapSatialReference,
                        "resolution": null
                    });
                    var defaultSym = new esri.symbols.SimpleMarkerSymbol().setSize(1);
                    var renderer = new ClassBreaksRenderer(defaultSym, "clusterCount");
                    var muckle = new esri.symbol.PictureMarkerSymbol("../../images/illegal/tollgateSum.png", 88, 32).setOffset(-5, 15);
                    renderer.addBreak(0, countyInfo.data.length, muckle);
                    prevLayerEventClusterLayer.setRenderer(renderer);
                    map.addLayer(prevLayerEventClusterLayer);
                    showPoints(prevLayer, Data, TextItems);
                    $("#prevMap>p:nth-child(2)").html(searchObj.startTime + " ~ " + searchObj.endTime);

                } else if (timeName === "对比时段") {
                    isPrevLayer = false;
                    var hiddenPrveLayer = map.getLayer('prevLayer');
                    if (hiddenPrveLayer != null) {
                        hiddenPrveLayer.setVisibility(false);
                    }
                    var prevLayerEventClusterLayer1 = map.getLayer("prevLayerEventClusterLayer");
                    if (prevLayerEventClusterLayer1) {
                        prevLayerEventClusterLayer1.setVisibility(false);
                    }
                    var nextLayerEventClusterLayer1 = map.getLayer("nextLayerEventClusterLayer");
                    if (nextLayerEventClusterLayer1) {
                        map.removeLayer(nextLayerEventClusterLayer1)
                    }
                    nextLayerEventClusterLayer = new ClusterLayer({
                        "data": countyInfo.data,
                        "distance": 150,
                        "id": "nextLayerEventClusterLayer",
                        "labelColor": "#02abbd",
                        "labelOffset": 13,
                        "spatialReference": mapSatialReference,
                        "resolution": null
                    });
                    var defaultSym = new esri.symbols.SimpleMarkerSymbol().setSize(1);
                    var renderer = new ClassBreaksRenderer(defaultSym, "clusterCount");
                    var muckle = new esri.symbol.PictureMarkerSymbol("../../images/illegal/tollgateSum.png", 89, 32).setOffset(-5, 15);
                    renderer.addBreak(0, countyInfo.data.length, muckle);
                    nextLayerEventClusterLayer.setRenderer(renderer);
                    map.addLayer(nextLayerEventClusterLayer);
                    showPoints(nextLayer, Data, TextItems);
                    $("#nextMap>p:nth-child(2)").html(searchObj.nextStartTime + " ~ " + searchObj.nextEndTime);
                }

            });

    };
    var showPoints = function (Layer, PointData, TextData) {
        require([
                "esri/symbols/SimpleLineSymbol", "esri/geometry/Point", "esri/symbols/SimpleFillSymbol", "dojo/colors",
                "esri/graphic", "esri/symbols/PictureMarkerSymbol", "esri/symbols/TextSymbol"
                , "esri/symbols/Font"]
            , function (SimpleLineSymbol, Point, SimpleFillSymbol, Color, Graphic, PictureMarkerSymbol, TextSymbol, Font) {
                Layer.clear();
                //展示点位
                for (var i = 0; i < PointData.length; i++) {
                    var symbol = new PictureMarkerSymbol(PointData[i].icon, 89, 32);
                    //设置图片偏移
                    symbol.setOffset(0, 32 / 2);
                    var point = new Point({
                        "x": PointData[i].x,
                        "y": PointData[i].y,
                        "id": PointData[i].id,
                        "InfoTemplate": PointData[i].contentHTML,//气泡文本内容
                        "spatialReference": map.spatialReference
                    });
                    var graphic = new Graphic(point, symbol);
                    Layer.add(graphic);
                }
                var fontOne = new Font("18px", Font.STYLE_NORMAL, Font.VARIANT_NORMAL, Font.WEIGHT_BOLD, "MicrosoftYaHei");
                for (var i = 0; i < TextData.length; i++) {
                    var num = TextData[i].num;
                    var color = TextData[i].color;
                    var textSymbol = new TextSymbol(num, fontOne, color);
                    textSymbol.setOffset(13, 12);
                    var point = new Point({
                        "x": TextData[i].x,
                        "y": TextData[i].y,
                        "spatialReference": map.spatialReference
                    });
                    var fontGraphic = new Graphic(point, textSymbol);
                    Layer.add(fontGraphic);
                }
                Layer.setVisibility(true);
                showeLayerstatistical()
            });

    };
    var topPointsEchartInstance = null;
    var topPointsEchart = function (data1, data2, data3, illegalTypeName) {
        require(["../../website-js/illegal/echarts.js"], function (echarts) {
            var option = {
                backgroundColor: "rgb(30,54,80)",
                title: {
                    top: '90%',
                    text: searchObj.startTime + ' ~ ' + searchObj.endTime,
                    left: 'center',
                    textStyle: {
                        color: '#47E0B8',
                        fontSize: 12,
                        fontFamily: 'MicrosoftYaHei',
                        fontWeight: 400
                    }
                },
                textStyle: {
                    color: "rgb(255,255,255)"
                },
                tooltip: {
                    trigger: 'axis',
                    backgroundColor: '#001028',
                    borderColor: '#2C5599',
                    borderWidth: 1,
                    formatter: '{b0}<br />' + illegalTypeName + '：{c1}',
                    axisPointer: {            // 坐标轴指示器，坐标轴触发有效
                        type: 'none'        // 默认为直线，可选为：'line' | 'shadow'
                    }
                },
                grid: {
                    left: '3%',
                    right: '4%',
                    bottom: '15%',
                    containLabel: true
                },
                xAxis: {
                    type: 'category',
                    data: data1,
                    axisLine: {
                        show: false
                    },
                    splitLine: {
                        show: false
                    },
                    axisTick: {
                        show: false
                    },
                    axisLabel: {
                        interval: 0,
                        rotate: -10
                    }
                },
                yAxis: {
                    type: 'value',
                    name: '数量         ',
                    axisLine: {
                        lineStyle: {
                            color: "rgb(101,110,113)"
                        }
                    },
                    splitLine: {//分隔线
                        show: true,
                        lineStyle: {
                            color: "rgb(101,110,113)"
                        }
                    },
                    axisTick: {//刻度线
                        show: false
                    }
                },
                series: [

                    { // For shadow
                        type: 'bar',
                        itemStyle: {
                            normal: {color: 'rgba(71,224,184,0.1)'}
                        },
                        barGap: '-100%',
                        barCategoryGap: '20%',
                        barWidth: 15,
                        data: data3,
                        animation: false
                    },
                    {
                        type: 'bar',
                        barWidth: 15,
                        itemStyle: {
                            normal: {
                                color: new echarts.graphic.LinearGradient(
                                    0, 0, 0, 1,
                                    [
                                        {offset: 0, color: 'rgba(71,224,184,1)'},
                                        {offset: 0.5, color: 'rgba(71,224,184,0.5)'},
                                        {offset: 1, color: 'rgba(71,224,184,0)'}
                                    ]
                                )
                            },
                            emphasis: {
                                color: new echarts.graphic.LinearGradient(
                                    0, 0, 0, 1,
                                    [
                                        {offset: 0, color: 'rgba(71,224,184,1)'},
                                        {offset: 0.5, color: 'rgba(71,224,184,0.9)'},
                                        {offset: 1, color: 'rgba(71,224,184,0.5)'}
                                    ]
                                )
                            }
                        },
                        data: data2
                    }


                ]
            };

            // 基于准备好的dom，初始化echarts实例
            topPointsEchartInstance = echarts.init(document.getElementById("topPointsEchart"));
            topPointsEchartInstance.setOption(option);

        });
    };

    var bottomPointsEchartInstance = null;
    var bottomPointsEchart = function (data1, data2, data3, illegalTypeName) {
        require(["../../website-js/illegal/echarts.js"], function (echarts) {
            var option = {
                backgroundColor: "rgb(30,54,80)",
                title: {
                    text: searchObj.nextStartTime + ' ~ ' + searchObj.nextEndTime,
                    left: 'center',
                    textStyle: {
                        color: '#B0DB3A',
                        fontSize: 12,
                        fontFamily: 'MicrosoftYaHei',
                        fontWeight: 400
                    }
                },
                textStyle: {
                    color: "rgb(255,255,255)"
                },
                tooltip: {
                    trigger: 'axis',
                    backgroundColor: '#001028',
                    borderColor: '#2C5599',
                    borderWidth: 1,
                    formatter: '{b0}<br />' + illegalTypeName + '：{c1}',
                    axisPointer: {            // 坐标轴指示器，坐标轴触发有效
                        type: 'none'        // 默认为直线，可选为：'line' | 'shadow'
                    }
                },
                grid: {
                    left: '3%',
                    right: '4%',
                    bottom: '15%',
                    containLabel: true
                },
                xAxis: {
                    type: 'category',
                    data: data1,
                    axisLine: {
                        show: false
                    },
                    splitLine: {
                        show: false
                    },
                    axisTick: {
                        show: false
                    },
                    axisLabel: {
                        interval: 0,
                        rotate: -10
                    }
                },
                yAxis: {
                    type: 'value',
                    name: '数量         ',
                    axisLine: {
                        lineStyle: {
                            color: "rgb(101,110,113)"
                        }
                    },
                    splitLine: {//分隔线
                        show: true,
                        lineStyle: {
                            color: "rgb(101,110,113)"
                        }
                    },
                    axisTick: {//刻度线
                        show: false
                    }
                },
                series: [

                    { // For shadow
                        type: 'bar',
                        itemStyle: {
                            normal: {color: 'rgba(71,224,184,0.1)'}
                        },
                        barGap: '-100%',
                        barCategoryGap: '20%',
                        barWidth: 15,
                        data: data3,
                        animation: false
                    },
                    {
                        type: 'bar',
                        barWidth: 15,
                        itemStyle: {
                            normal: {
                                color: new echarts.graphic.LinearGradient(
                                    0, 0, 0, 1,
                                    [
                                        {offset: 0, color: 'rgba(176,219,58,1)'},
                                        {offset: 0.5, color: 'rgba(176,219,58,0.5)'},
                                        {offset: 1, color: 'rgba(176,219,58,0)'}
                                    ]
                                )
                            },
                            emphasis: {
                                color: new echarts.graphic.LinearGradient(
                                    0, 0, 0, 1,
                                    [
                                        {offset: 0, color: 'rgba(176,219,58,1)'},
                                        {offset: 0.5, color: 'rgba(176,219,58,0.9)'},
                                        {offset: 1, color: 'rgba(176,219,58,0.5)'}
                                    ]
                                )
                            }
                        },
                        data: data2
                    }


                ]
            };

            // 基于准备好的dom，初始化echarts实例
            bottomPointsEchartInstance = echarts.init(document.getElementById("bottomPointsEchart"));
            bottomPointsEchartInstance.setOption(option);

        });
    };
    var formatTimeData = function (data) {
        var format = [];
        var xData = [];
        var yData = [];
        $.each(data, function (index, ele) {
            xData.push(ele.TIME_NAME);
            yData.push(ele.NUM);
        });
        format[0] = xData;
        format[1] = yData;
        return format;
    };
    var formatTimeLeftData = function (data) {
        var format = [];
        var xData = [];
        var yData = [];
        $.each(data, function (index, ele) {
            xData.push(ele.HAPPENTIME);
            yData.push(ele.NUM);
        });
        format[0] = xData;
        format[1] = yData;
        return format;
    };
    var widthPercent = function (eleId, percentNum) {
        var element = $(eleId);
        if (element != null && element.length > 0) {
            element.css("width", percentNum + "%");
        }
    };
    var loadEventTable = function () {
        $("#eventSearchTable").bootstrapTable('destroy');
        $("#eventSearchTable").bootstrapTable({
            toolbar: "#toolbar",
            url: "/trafficEvent/getIncidentInfoByCondition",
            pagination: true,
            striped: false,
            dataType: "json",
            sidePagination: "server",
            uniqueId: "objectId",
            idField: "objectId",
            pageSize: 15,
            pageNumber: 1,
            pageList: [],
            columns: [
                {field: "", checkbox: true, width: 50},
                {field: "happenTime", title: "发生时间"},
                {
                    field: "incidentPlace", title: "发生地点", formatter: function (value, row, index) {
                    if (value != null && value != "") {
                        return "<a style='color: rgba(0,216,255,1);' onclick=\"trafficeventScheduling.showInfoTable(" + row.ptiid + ")\" title='" + value + "' href='#'>" + value + "</a>"
                    }
                }
                },
                {field: "incidentSourceName", title: "事件来源"},
                {field: "incidentTypeName", title: "事件类型"},
                {field: "incidentSubTypeName", title: "事件子类"},
                {
                    field: "validFlag", title: "数据状态", formatter: function (value, row, index) {
                    if (value == "0") {
                        return "<span>无效数据</span>"
                    }
                    if (value == "1") {
                        return "<span  >有效数据</span>"
                    }
                }
                },
                {field: "incidentStateName", title: "事件状态"}
            ],
            queryParams: queryParams,
            formatNoMatches: function () {
                return '无符合条件的记录';
            },
            formatLoadingMessage: function () {
                return "请稍等，正在加载中...";
            },
            responseHandler: function (res) {//在加载服务器发送来的数据之前，处理数据的格式，参数res表示the response data（获取的数据）
                return {
                    "total": res.total,
                    "rows": res.rows
                }
            },
            onRefreshOptions: function () {
                // $("#xgBtn,#ckspBtn,#thfjBtn,#sytBtn,#xytBtn,#xgBtnYes,#xgBtnNo").prop("disabled",true);
            },
            onLoadSuccess: function () {

                tableScrol.init("#eventSearchTable", $(document.body).height() - 450);
                $("#eventSearchTable .slimScrollDiv").css("height", $(document.body).height() - 450 + "px")
                // $("#xgBtn,#ckspBtn,#thfjBtn,#sytBtn,#xytBtn,#xgBtnYes,#xgBtnNo").prop("disabled",false);
            },
            onPostBody: function () {
                //改变复选框样式
                changeCheckboxcss("#eventSearchTable", "eventSearchTable");
            },
            onClickRow: function (row, el, field) {
                require(["esri/geometry/webMercatorUtils"],
                    function (webMercatorUtils) {
                        var positioningLayer = eventmap.getLayer("positioningLayer");
                        if (positioningLayer != null) {
                            positioningLayer.clear();
                        } else {
                            positioningLayer = new esri.layers.GraphicsLayer({id: "positioningLayer"});
                            eventmap.addLayer(positioningLayer);
                        }
                        if (row.longitude != 0 && row.longitude != '' && row.longitude != null) {
                            var cPoint;
                            if (ConfigSetting.isWebMercator == "true") {
                                var normalizedVal = webMercatorUtils.lngLatToXY(row.longitude, row.latitude);
                                cPoint = new esri.geometry.Point(normalizedVal[0], normalizedVal[1], mapSatialReference);
                            } else {
                                cPoint = new esri.geometry.Point(row.longitude, row.latitude, mapSatialReference);
                            }
                            var alarm_g = new esri.Graphic(cPoint);
                            alarm_g.setSymbol(arraySymbol);
                            positioningLayer.add(alarm_g);
                            var nowLevel = eventmap.getLevel();
                            var level = (nowLevel > ConfigSetting.MAP_MapClusterLayer ? nowLevel : (ConfigSetting.MAP_MapClusterLayer - 0 + 1));
                            eventmap.centerAndZoom(cPoint, level);
                            $(positioningLayer.getNode()).animate({opacity: 0}, 600).animate({opacity: 0.8}, 600).animate({opacity: 0}, 600).animate({opacity: 1}, 600, function () {
                                positioningLayer.clear();
                            });
                        }
                    })
                // loadPlanPhase(row);
            },
        });
    };
    var queryParams = function (params) {
        var option = {};
        option.incidentSource = $("#eventSjly").val(); //事件来源
        option.incidentType = $("#eventSjLx").val(); //事件类型
        option.incidentSubType = $("#eventSjZl").val();//事件子类型
        option.incidentPlace = $("#eventFsdd").val();//发生地点
        option.incidentState = $("#eventCzzt").val();//处置状态
        option.validFlag = $("#eventSjzt").val();//数据状态
        option.hasAttention = $("#eventSfgz").val();//是否关注
        option.happenTimeBegin = common.getStandardTime($(carPassStartTime).val());
        option.happenTimeEnd = common.getStandardTime($(carPassEndTime).val());
        option.pageSize = params.limit;
        option.pageBegin = params.offset;
        return option
    };
    var mapEventIndex = 0;
    var drawEndHandler = null;
    var drawToolBar = null;
    var initMapEvent = function () {
        eventmap = pspMap.init($("#mapBox").attr("id"), trafficeventScheduling, {
            tollgate: [],
            video: false,
            warnInfo: false,
            case: false
        });
        eventmap.on("extent-change", function (ext) {
            showeLayer()
        });
        eventLayer = new esri.layers.GraphicsLayer({id: "eventLayer"});
        eventmap.addLayer(eventLayer, 0);
        drawLayerPoint = new esri.layers.GraphicsLayer({"id": "drawLayerPoint"});
        eventmap.addLayer(drawLayerPoint);
        drawLayerPoint = new esri.layers.GraphicsLayer({"id": "drawLayerPoint"});
        eventmap.addLayer(drawLayerPoint);
        drawEdieSigPosLayer = new esri.layers.GraphicsLayer({"id": "drawEdieSigPosLayer"});
        eventmap.addLayer(drawEdieSigPosLayer);
        eventLayer.on("click", function (evt) {
            require(["esri/geometry/webMercatorUtils"],
                function (webMercatorUtils) {
                    // map.infoWindow.hide();
                    var attr = evt.graphic.attributes;
                    var cPoint;
                    if (ConfigSetting.isWebMercator == "true") {
                        var normalizedVal = webMercatorUtils.lngLatToXY(attr.lon, attr.lat);
                        cPoint = new esri.geometry.Point(normalizedVal[0], normalizedVal[1], mapSatialReference);
                    } else {
                        cPoint = new esri.geometry.Point(attr.lon, attr.lat, mapSatialReference);
                    }
                    evt.stopPropagation();
                    var indexData = eventMapData[attr.index];
                    if (indexData.data.length == 1) {
                        eventmap.infoWindow.setTitle(indexData.data[0].incidentTypeName);
                        var html = "<div class='titleTwo'><div class='happenTime'>" + indexData.data[0].happenTime + "</div>";
                        html += "<div class='incidentPlace'>" + indexData.data[0].incidentPlace + "</div>";
                        html += "<div class='ptiid' style='display: none'>" + indexData.data[0].ptiid + "</div></div>";
                        html += "<div class='windowStyles'>";
                        if (indexData.data[0].incidentSource == "1" || indexData.data[0].incidentSource == "3") {
                            html += "<a  class='change_position' onclick= \"trafficeventScheduling.bditPositiong('" + indexData.data[0].longitude + "','" + indexData.data[0].latitude + "','" + indexData.data[0].ptiid + "')\" >编辑</a>"
                        }
                        html += "<a class='detail' onclick=\"trafficeventScheduling.showInfo(" + indexData.data[0].ptiid + ")\">详情</a>";
                        html += "<div>";
                        eventmap.infoWindow.setContent(html);
                    } else {
                        mapEventIndex = 0;
                        eventmap.infoWindow.setTitle(indexData.data[0].incidentTypeName);
                        var html = "<img style='width: 15px;position: absolute;left: 0;'  src='../../images/trafficControl/left.png' onclick=\"trafficeventScheduling.nextMapEvent(" + attr.index + "," + 0 + ")\" />";
                        html += "<div class='titleTwo'><div class='happenTime' style='text-align: center'>" + indexData.data[0].happenTime + "</div>";
                        html += "<div class='incidentPlace' style='text-align: center'>" + indexData.data[0].incidentPlace + "</div>";
                        html += "<div class='ptiid' style='display: none'>" + indexData.data[0].ptiid + "</div></div>";
                        html += "<img style='width: 15px;position: absolute;right: 0;top: 6px' src='../../images/trafficControl/right.png' onclick=\"trafficeventScheduling.nextMapEvent(" + attr.index + "," + 1 + ")\" />";
                        html += "<div>";
                        if (indexData.data[0].incidentSource == "1" || indexData.data[0].incidentSource == "3") {
                            html += "<a  class='change_position' onclick= \"trafficeventScheduling.bditPositiong('" + indexData.data[mapEventIndex].longitude + "','" + indexData.data[mapEventIndex].latitude + "','" + indexData.data[mapEventIndex].ptiid + "')\" >编辑</a>"
                        }
                        html += "<a class='detail' onclick=\"trafficeventScheduling.showInfo(" + indexData.data[mapEventIndex].ptiid + ")\" >详情</a>";
                        html += "<div>";
                        eventmap.infoWindow.setContent(html);
                    }
                    eventmap.infoWindow.show(cPoint);
                })
        });
        if (ConfigSetting.MapServiceContract == "WebTiledLayer") {
            loadEventMap()
        } else {
            eventmap.on("load", function () {
                loadEventMap()
            });
        }
    };
    var showeLayer = function () {
        var eventLayerClear = eventmap.getLayer("eventLayer");
        var EventClusterLayer1 = eventmap.getLayer("EventClusterLayer");
        if (EventClusterLayer1 != null && eventLayerClear != null) {
            if (eventmap.getLevel() <= mapLevelAggregation) {
                if (eventLayerClear) {
                    eventLayerClear.setVisibility(false);
                }
                if (EventClusterLayer1) {
                    EventClusterLayer1.setVisibility(true);
                }
            } else {
                if (eventLayerClear) {
                    eventLayerClear.setVisibility(true);
                }
                if (EventClusterLayer1) {
                    EventClusterLayer1.setVisibility(false);
                }
            }
        }

    };
    var eventBidtPos = function (lon, lat, id) {
        require([
            "esri/map", "esri/geometry/Extent", "esri/toolbars/edit", "esri/config", "esri/toolbars/draw", "esri/geometry/geometryEngineAsync",
            "esri/layers/GraphicsLayer", "esri/graphic", "esri/geometry/Point", "esri/geometry/webMercatorUtils", "esri/tasks/query",
            "esri/tasks/QueryTask", "esri/geometry/Extent", "esri/symbols/PictureMarkerSymbol",
            "esri/InfoTemplate", "esri/tasks/FindTask", "esri/tasks/FindParameters", "esri/tasks/IdentifyTask", "esri/tasks/IdentifyParameters", "dojo/domReady!"
        ], function (Map, Extent, Edit, esriConfig, Draw, geometryEngineAsync, GraphicsLayer, Graphic, Point, webMercatorUtils, Query, QueryTask, Extent, PictureMarkerSymbol,
                     InfoTemplate, FindTask, FindParameters, IdentifyTask, IdentifyParameters) {
            if (drawLayerPoint) {
                drawLayerPoint.clear();
            }
            if (drawEdieSigPosLayer) {
                drawEdieSigPosLayer.clear()
            }
            common.showInfo("移动图标选择修改位置，双击地图确定");
            var webMercatorArr = [];
            if (ConfigSetting.isWebMercator == "true") {
                webMercatorArr = webMercatorUtils.lngLatToXY(lon, lat);
            } else {
                webMercatorArr = [lon, lat]
            }
            var alarm_g = new esri.Graphic(new esri.geometry.Point(webMercatorArr[0], webMercatorArr[1], mapSatialReference));
            alarm_g.setSymbol(new esri.symbols.PictureMarkerSymbol("/images/trafficEvent/99.png", 25, 33));
            drawEdieSigPosLayer.add(alarm_g);
            var editToolbar = new Edit(eventmap);
            editToolbar.activate(Edit.MOVE | Edit.SCALE, alarm_g);
            mapDbClick = dojo.connect(eventmap, "onDblClick", function (geo) {
                dojo.disconnect(mapDbClick);
                drawEdieSigPosLayer.clear();
                var bditGra = editToolbar.getCurrentState();
                editToolbar.deactivate();
                var webMercatorArr = [];
                if (ConfigSetting.isWebMercator == "true") {
                    webMercatorArr = webMercatorUtils.xyToLngLat(bditGra.graphic.geometry.x, bditGra.graphic.geometry.y);
                } else {
                    webMercatorArr = [bditGra.graphic.geometry.x, bditGra.graphic.geometry.y]
                }

                $.post("/trafficEvent/updateTrIncidentInfo", {
                    "ptiid": id,
                    "longitude": webMercatorArr[0],
                    "latitude": webMercatorArr[1]
                }, function (data) {
                    if (data.state == "SUCCESS") {
                        common.showSuccess("编辑成功");
                        eventmap.infoWindow.hide();
                        loadEventMap()
                    }
                }, "json");
            });
            //SigCtrlFun
        });
    };
    var eventMapData;
    var EventClusterLayer;
    var loadEventMap = function () {
        require(["esri/dijit/OverviewMap", "extras/ClusterLayer",
                "dojo/_base/array", "esri/geometry/Point", "esri/symbols/SimpleMarkerSymbol",
                "esri/renderers/ClassBreaksRenderer", "esri/symbols/SimpleLineSymbol",
                "esri/Color", "esri/geometry/webMercatorUtils", "esri/symbols/TextSymbol"],
            function (OverviewMap, ClusterLayer, arrayUtils, Point, SimpleMarkerSymbol,
                      ClassBreaksRenderer, SimpleLineSymbol, Color, webMercatorUtils, TextSymbol) {
                var option = {};
                option.incidentSource = $("#eventSjly").val(); //事件来源
                option.incidentType = $("#eventSjLx").val(); //事件类型
                option.incidentSubType = $("#eventSjZl").val();//事件子类型
                option.incidentPlace = $("#eventFsdd").val();//发生地点
                option.incidentState = $("#eventCzzt").val();//处置状态
                option.validFlag = $("#eventSjzt").val();//数据状态
                option.hasAttention = $("#eventSfgz").val();//是否关注
                option.happenTimeBegin = common.getStandardTime($(carPassStartTime).val())//"2019-10-21 10:26:04";
                option.happenTimeEnd = common.getStandardTime($(carPassEndTime).val())//"2019-11-21 10:26:04";
                $.post("/trafficEvent/getIncidentMapList", option, function (data) {
                    if (data.rows.length > 0) {
                        var eventLayerClear = eventmap.getLayer("eventLayer");
                        if (eventLayerClear) {
                            eventLayerClear.clear()
                        }
                        var EventClusterLayer1 = eventmap.getLayer("EventClusterLayer");
                        if (EventClusterLayer1) {
                            eventmap.removeLayer(EventClusterLayer1)
                        }
                        var eventData = data.rows;
                        eventMapData = eventData;
                        var countyInfo = {
                            "data": []
                        };
                        for (var i = 0; i < eventData.length; i++) {
                            var alarm_x = eventData[i].longitude;
                            var alarm_y = eventData[i].latitude;
                            var webMercator;
                            var alarm_attr = {
                                "index": i,
                                "lon": eventData[i].longitude,
                                "lat": eventData[i].latitude,
                            };
                            var attributes = {
                                "经度": eventData[i].longitude,
                                "纬度": eventData[i].latitude
                            };
                            if (ConfigSetting.isWebMercator == "true") {
                                webMercator = webMercatorUtils.lngLatToXY(alarm_x, alarm_y);
                            } else {
                                webMercator = [alarm_x, alarm_y];
                            }
                            var alarm_g = new esri.Graphic(new esri.geometry.Point(webMercator[0], webMercator[1], mapSatialReference));
                            if (eventData[i].data.length == 1) {
                                alarm_g.setSymbol(new esri.symbols.PictureMarkerSymbol("/images/trafficEvent/" + eventData[i].data[0].incidentType + ".png", 25, 33));
                                eventLayer.add(alarm_g);
                                var obj = {
                                    "x": webMercator[0],
                                    "y": webMercator[1],
                                    "attributes": attributes
                                };
                                countyInfo.data.push(obj);
                            } else {
                                alarm_g.setSymbol(new esri.symbols.PictureMarkerSymbol("/images/trafficEvent/eventJh.png", 58, 32));
                                eventLayer.add(alarm_g);
                                var alarm_text = new esri.Graphic(new esri.geometry.Point(webMercator[0], webMercator[1], mapSatialReference));
                                alarm_text.setSymbol(new TextSymbol(eventData[i].data.length).setColor(new Color("#DB780F")).setOffset(14, 0));
                                eventLayer.add(alarm_text);
                                for (var i = 0; i < eventData[i].data.length; i++) {
                                    var obj = {
                                        "x": webMercator[0],
                                        "y": webMercator[1],
                                        "attributes": attributes
                                    };
                                    countyInfo.data.push(obj);
                                }
                            }

                            alarm_g.setAttributes(alarm_attr);
                        }
                        EventClusterLayer = new ClusterLayer({
                            "data": countyInfo.data,
                            "distance": 150,
                            "id": "EventClusterLayer",
                            "labelColor": "#02abbd",
                            "labelOffset": 13,
                            "spatialReference": mapSatialReference,
                            "resolution": null
                        });
                        var defaultSym = new esri.symbols.SimpleMarkerSymbol().setSize(1);
                        var renderer = new ClassBreaksRenderer(defaultSym, "clusterCount");
                        var bit = new esri.symbols.PictureMarkerSymbol("images/trafficEvent/eventJh.png", 54, 32).setOffset(0, 15);
                        var muckle = new esri.symbol.PictureMarkerSymbol("images/trafficEvent/eventJh.png", 88, 32).setOffset(-5, 15);
                        renderer.addBreak(0, 999, bit);
                        renderer.addBreak(999, countyInfo.data.length, muckle);
                        EventClusterLayer.setRenderer(renderer);
                        eventmap.addLayer(EventClusterLayer);
                        EventClusterLayer.setVisibility(false);
                        showeLayer();
                    }
                }, "json")
            });
    }
    var statisticsSearch = function () {
        var prevStartTime = $.trim($("#prevStartTime").val());
        var prevEndTime = $.trim($("#prevEndTime").val());
        if (prevStartTime === "" || prevEndTime === "") {
            common.showWarn("请选择统计时间！", "提示");
            return;
        }
        var checked = $("#comparison").prop("checked");
        var timeType;
        if ($("#tjsjSelect").val() == 0) {
            timeType = "day"
        } else {
            timeType = "month"
        }
        var sjlxTjfx = $("#sjlxTjfx").val();
        var incidentType;
        if (sjlxTjfx !== null) {
            incidentType = sjlxTjfx.join(",");
        } else {
            incidentType = "";
        }
        searchObj = {
            "incidentType": incidentType,
            "timeType": timeType,
            "endTime": prevEndTime,
            "startTime": prevStartTime
        };
        var postUrl = "/trafficEvent/statisticsGroupByType";
        if (sjlxTjfx != null && sjlxTjfx.length == 1) {
            if ($("#sjlxTjfx").val()[0] == "1" || $("#sjlxTjfx").val()[0] == "4" || $("#sjlxTjfx").val()[0] == "20") {
                postUrl = "/trafficEvent/statisticsGroupBySubType";
            }
        }
        if (checked) {//调用2次接口
            var nextStartTime = $.trim($("#nextStartTime").val());
            var nextEndTime = $.trim($("#nextEndTime").val());
            searchObj.nextStartTime = nextStartTime;
            searchObj.nextEndTime = nextEndTime;
            //前一段时间
            $.getJSON(postUrl, {
                "incidentType": incidentType,
                "timeType": timeType,
                "endTime": prevEndTime,
                "startTime": prevStartTime
            }, function (data0) {
                if (data0 != null && data0.state == "SUCCESS") {
                    var prevData = data0.rows;
                    //对比时间
                    $.getJSON(postUrl, {
                        "incidentType": incidentType,
                        "timeType": timeType,
                        "endTime": nextEndTime,
                        "startTime": nextStartTime
                    }, function (data) {
                        if (data != null && data.state == "SUCCESS") {
                            $("#topAllIllegalTypesWrapper").prev().remove();
                            $("#topAllIllegalTypes").empty();
                            $("#topAllIllegalTypes").css("left", "0");

                            //过滤数据
                            innerHtml([prevData, data.rows], "yes");

                        } else {
                            common.showError("系统异常！", "警告");
                        }
                    });
                } else {
                    common.showError("系统异常！", "警告");
                }
            });


        } else {//调用1次接口
            $.getJSON(postUrl, searchObj, function (data) {
                if (data != null && data.state == "SUCCESS") {
                    $("#topAllIllegalTypesWrapper").prev().remove();
                    $("#topAllIllegalTypes").empty();
                    $("#topAllIllegalTypes").css("left", "0");
                    innerHtml(data.rows, incidentType, "no");
                    // //过滤数据
                    // if(incidentType ===""){//查询所有
                    //
                    // }else{//查询部分
                    //     var filterData = data.rows;
                    //     filterData = filterData.filter(function (ele,index,arr) {
                    //         return (illegalTypes+",").indexOf(ele.classNo+",")>-1;
                    //     });
                    //     innerHtml(filterData,"no");
                    // }
                } else {
                    common.showError("系统异常！", "警告");
                }
            });
        }
    };
    var timeTabTopEcharts = null;
    var timeEcharts1 = function (data1, data2, illegalTypeName) {
        require(["../../website-js/illegal/echarts.js"], function (echarts) {
            var option = {
                backgroundColor: "rgb(30,54,80)",
                title: {
                    // text: '处罚金额',
                    left: 'center',
                    textStyle: {
                        color: 'white',
                        fontSize: 18,
                        fontFamily: 'MicrosoftYaHei',
                        fontWeight: 400
                    }
                },
                tooltip: {
                    trigger: 'axis',
                    backgroundColor: '#001028',
                    borderColor: '#2C5599',
                    borderWidth: 1,
                    formatter: '{b0}<br />' + illegalTypeName + '：{c0}',
                    axisPointer: {
                        type: 'cross',
                        label: {
                            backgroundColor: '#6a7985'
                        }
                    }
                },
                dataZoom: {
                    show: true,
                    type: 'inside'
                },
                grid: {
                    left: '3%',
                    right: '3%',
                    bottom: '5%',
                    containLabel: true
                },
                xAxis: [
                    {
                        type: 'category',
                        axisLine: {
                            show: false
                        },
                        axisTick: {//刻度线
                            show: false
                        },
                        splitLine: {//分隔线
                            show: false
                        },
                        axisLabel: {
                            color: 'white',
                            interval: 2,
                        },
                        boundaryGap: false,
                        axisPointer: {
                            lineStyle: {
                                color: '#0ADD39',
                                opacity: 0.5,
                                width: 1
                            },
                            label: {
                                show: true
                            }
                        },
                        data: data1
                    }
                ],
                yAxis: [
                    {
                        name: '数量           ',
                        nameTextStyle: {
                            color: "white"
                        },
                        type: 'value',
                        axisLine: {
                            lineStyle: {
                                color: "rgb(101,110,113)"
                            }
                        },
                        axisTick: {//刻度线
                            show: false
                        },
                        splitLine: {//分隔线
                            show: true,
                            lineStyle: {
                                color: "rgb(101,110,113)"
                            }
                        },
                        axisLabel: {
                            color: 'white'
                        }
                    }
                ],
                series: [
                    {
                        type: 'line',
                        symbol: 'image://' + baseURL + '/images/illegal/weChatColor.png',
                        // showSymbol:false,
                        // symbolSize:15,
                        // smooth: true,
                        lineStyle: {
                            color: "#0ADD39",
                            width: 2
                        },
                        // areaStyle: {
                        //     color: {
                        //         type: 'linear',
                        //         x: 0,
                        //         y: 0,
                        //         x2: 0,
                        //         y2: 1,
                        //         colorStops: [{
                        //             offset: 0, color: 'rgba(62,198,92,0.43)' // 0% 处的颜色
                        //         }, {
                        //             offset: 1, color: 'transparent' // 100% 处的颜色
                        //         }],
                        //         global: false // 缺省为 false
                        //     }
                        // },
                        data: data2
                    }
                ]
            };

            timeTabTopEcharts = echarts.init(document.getElementById("timeTabTopEcharts"));
            timeTabTopEcharts.setOption(option);
        });
    };
    var timeTabBottomEcharts = null;
    var timeEcharts2 = function (data1, data2, illegalTypeName) {//对比时间段 的数据
        require(["../../website-js/illegal/echarts.js"], function (echarts) {
            var option = {
                backgroundColor: "rgb(30,54,80)",
                title: {
                    // text: '处罚金额',
                    left: 'center',
                    textStyle: {
                        color: 'white',
                        fontSize: 18,
                        fontFamily: 'MicrosoftYaHei',
                        fontWeight: 400
                    }
                },
                tooltip: {
                    trigger: 'axis',
                    backgroundColor: '#001028',
                    borderColor: '#2C5599',
                    borderWidth: 1,
                    formatter: '{b0}<br />' + illegalTypeName + '：{c0}',
                    axisPointer: {
                        type: 'cross',
                        label: {
                            backgroundColor: '#6a7985'
                        }
                    }
                },
                dataZoom: {
                    show: true,
                    type: 'inside'
                },
                grid: {
                    left: '3%',
                    right: '3%',
                    bottom: '5%',
                    containLabel: true
                },
                xAxis: [
                    {
                        type: 'category',
                        axisLine: {
                            show: false
                        },
                        axisTick: {//刻度线
                            show: false
                        },
                        splitLine: {//分隔线
                            show: false
                        },
                        axisLabel: {
                            color: 'white',
                            interval: 2,
                        },
                        boundaryGap: false,
                        axisPointer: {
                            lineStyle: {
                                color: '#0ADD39',
                                opacity: 0.5,
                                width: 1
                            },
                            label: {
                                show: true
                            }
                        },
                        data: data1
                    }
                ],
                yAxis: [
                    {
                        name: '数量           ',
                        nameTextStyle: {
                            color: "white"
                        },
                        type: 'value',
                        axisLine: {
                            lineStyle: {
                                color: "rgb(101,110,113)"
                            }
                        },
                        axisTick: {//刻度线
                            show: false
                        },
                        splitLine: {//分隔线
                            show: true,
                            lineStyle: {
                                color: "rgb(101,110,113)"
                            }
                        },
                        axisLabel: {
                            color: 'white'
                        }
                    }
                ],
                series: [
                    {
                        type: 'line',
                        symbol: 'image://' + baseURL + '/images/illegal/weChatColor.png',
                        // showSymbol:false,
                        // symbolSize:15,
                        // smooth: true,
                        lineStyle: {
                            color: "#47D4FD",
                            width: 2
                        },
                        // areaStyle: {
                        //     color: {
                        //         type: 'linear',
                        //         x: 0,
                        //         y: 0,
                        //         x2: 0,
                        //         y2: 1,
                        //         colorStops: [{
                        //             offset: 0, color: 'rgba(62,198,92,0.43)' // 0% 处的颜色
                        //         }, {
                        //             offset: 1, color: 'transparent' // 100% 处的颜色
                        //         }],
                        //         global: false // 缺省为 false
                        //     }
                        // },
                        data: data2
                    }
                ]
            };

            timeTabBottomEcharts = echarts.init(document.getElementById("timeTabBottomEcharts"));
            timeTabBottomEcharts.setOption(option);
        });
    };
    var timeTabTimeright = null;
    var timeTabBottomEcharts = null;
    var innerHtml = function (data, type, flag) {
        if (flag === "no") {//没有对比
            var noSum = 0;
            data.forEach(function (item, index) {
                noSum = noSum + item.NUM;
            });
            var eventType = "";
            if ($("#sjlxTjfx").val() != null && $("#sjlxTjfx").val().length == 1) {
                if ($("#sjlxTjfx").val()[0] == "1" || $("#sjlxTjfx").val()[0] == "4" || $("#sjlxTjfx").val()[0] == "20") {
                    eventType = $("#sjlxTjfx").val()[0];
                }
            }
            var firstDom = $('<div class="one twoC" type="' + eventType + '" typename="全部">' +
                '<div class="small">总数</div>' +
                '<div class="big timer" data-to="' + noSum + '" data-speed="1500">' + noSum + '</div>' +
                '<div class="between"></div>' +
                '<div class="smallest">' +
                '<span class="redPercent">&nbsp;</span>' +
                '<span class="redPercent">&nbsp;</span>' +
                '<span>&nbsp;</span>' +
                '</div>' +
                '<div class="oneEchart" id="oneEchart"></div>' +
                '</div>');
            $("#topAllIllegalTypesWrapper").before(firstDom[0]);
            setTimeout(function () {
                sumEcharts(noSum);
            }, 100);
            data.forEach(function (item, index) {
                var percentNum = 0;
                if (item.NUM > 0) {
                    percentNum = item.NUM / (noSum * 1.0);
                    percentNum = percentNum.toFixed(2);
                    percentNum = Math.abs(100 * percentNum);
                }
                var className = "";
                var eventName = "";
                var inType = "";
                var inSubType = "";
                var isSel;
                if (item.isSelect) {
                    className = '_' + item.INCIDENTTYPE;
                    eventName = item.INCIDENTTYPENAME;
                    isSel = "clickEvent";
                    inType = item.INCIDENTTYPE;
                    inSubType = "";
                } else {
                    className = '_' + item.INCIDENTTYPE + "bg";
                    eventName = item.INCIDENTTYPENAME;
                    isSel = "";
                    inType = item.INCIDENTTYPE;
                    inSubType = "";
                }
                if ($("#sjlxTjfx").val() != null && $("#sjlxTjfx").val().length == 1) {
                    if ($("#sjlxTjfx").val()[0] == "1" || $("#sjlxTjfx").val()[0] == "4" || $("#sjlxTjfx").val()[0] == "20") {
                        className = '_' + item.SUBTYPENO;
                        isSel = isSel = "clickEvent";
                        eventName = item.SUBTYPENAME;
                        inType = item.INCIDENTTYPE;
                        inSubType = item.SUBTYPENO;
                    }
                }
                if (isSel) {
                    var htmlDom = $('<div class="twoC  ' + className + ' ' + isSel + ' " type="' + inType + '" subType = "' + inSubType + '" typeName="' + eventName + '">' +
                        '<div class="small">' + eventName + '</div>' +
                        '<div class="big timer" data-to="' + item.NUM + '" data-speed="1500">' + item.NUM + '</div>' +
                        '<div class="between">' +
                        '<span class="timer" data-to="' + percentNum + '" data-speed="1500">' + percentNum + '</span>' +
                        '<span>%</span>' +
                        '</div>' +
                        '<div class="smallest">' +
                        '<span class="redPercent">&nbsp;</span>' +
                        '<div class="progress statisticalAnalysisProgress">' +
                        '<div class="progress-bar statisticalAnalysisProgressBar" role="progressbar" style="width: ' + percentNum + '%;"></div>' +
                        '</div>' +
                        '</div>' +
                        '</div>');
                    $("#topAllIllegalTypes").append(htmlDom[0]);
                }

                // htmlDom.find(".timer").data("countToOptions", {
                //     formatter: function(b, a) {
                //         return b.toFixed(0).replace(/\B(?=(?:\d{3})+(?!\d))/g, ",")
                //     }
                // });
                // htmlDom.find(".timer").each(count);
            });
            //
            //
            // firstDom.find(".timer").data("countToOptions", {
            //     formatter: function(b, a) {
            //         return b.toFixed(0).replace(/\B(?=(?:\d{3})+(?!\d))/g, ",")
            //     }
            // });
            // firstDom.find(".timer").each(count)

        } else {//有对比
            var prevData = data[0];
            var nextData = data[1];//对比数据
            var prevSum = 0;
            var nextSum = 0;
            //计算总数
            $.each(prevData, function (index, ele) {
                prevSum = prevSum + ele.NUM;
            });
            $.each(nextData, function (index, ele) {
                nextSum = nextSum + ele.NUM;
            });
            var firstPercent = 0;
            if (nextSum > 0) {
                firstPercent = (prevSum - nextSum) / (nextSum * 1.0);
                firstPercent = firstPercent.toFixed(0);
                firstPercent = Math.abs(100 * firstPercent);
            } else {
                firstPercent = 100;
            }
            var eventType = "";
            if ($("#sjlxTjfx").val() != null && $("#sjlxTjfx").val().length == 1) {
                if ($("#sjlxTjfx").val()[0] == "1" || $("#sjlxTjfx").val()[0] == "4" || $("#sjlxTjfx").val()[0] == "20") {
                    eventType = $("#sjlxTjfx").val()[0];
                }
            }
            var yesFirstDom = $('<div class="one twoC"  type="' + eventType + '" typename="全部">' +
                '<div class="small">总数</div>' +
                '<div class="big timer" data-to="' + prevSum + '" data-speed="1500">' + prevSum + '</div>' +
                '<div class="between timer" data-to="' + nextSum + '" data-speed="1500">' + nextSum + '</div>' +
                '<div class="smallest">' +
                '<span class="' + (prevSum >= nextSum ? "redPercent" : "greenPercent") + ' timer" data-to="' + firstPercent + '" data-speed="1500">' + firstPercent + '</span>' +
                '<span class="' + (prevSum >= nextSum ? "redPercent" : "greenPercent") + '">%</span>' +
                '<span class="' + (prevSum >= nextSum ? "countUp" : "countDown") + '"></span>' +
                '</div>' +
                '<div class="oneEchart" id="oneEchart"></div>' +
                '</div>');

            $("#topAllIllegalTypesWrapper").before(yesFirstDom[0]);
            // yesFirstDom.find(".timer").data("countToOptions", {
            //     formatter: function(b, a) {
            //         return b.toFixed(0).replace(/\B(?=(?:\d{3})+(?!\d))/g, ",")
            //     }
            // });
            // yesFirstDom.find(".timer").each(count);

            setTimeout(function () {
                sumEcharts(prevSum);
            }, 100);
            for (var i = 0; i < prevData.length; i++) {
                var prevOne = prevData[i];
                for (var j = 0; j < nextData.length; j++) {
                    var nextOne = nextData[j];

                    var preeventType = prevOne.INCIDENTTYPE;
                    var nexteventType = nextOne.INCIDENTTYPE;
                    if ($("#sjlxTjfx").val() != null && $("#sjlxTjfx").val().length == 1) {
                        if ($("#sjlxTjfx").val()[0] == "1" || $("#sjlxTjfx").val()[0] == "4" || $("#sjlxTjfx").val()[0] == "20") {
                            preeventType = prevOne.SUBTYPENO;
                            nexteventType = nextOne.SUBTYPENO;
                        }
                    }
                    if (preeventType === nexteventType) {
                        var prevTypeSum = prevOne.NUM;
                        var nextTypeSum = nextOne.NUM;
                        var typePercent = 0;
                        if (nextTypeSum > 0) {
                            typePercent = (prevTypeSum - nextTypeSum) / (nextTypeSum * 1.0);
                            typePercent = typePercent.toFixed(2);
                            typePercent = Math.abs(100 * typePercent);
                        } else {
                            typePercent = 0;
                        }
                        // var className = "";
                        // var isSel;
                        // if (nextOne.isSelect) {
                        //     className = '_' + nextOne.INCIDENTTYPE;
                        //     isSel = "clickEvent"
                        // } else {
                        //     className = '_' + nextOne.INCIDENTTYPE + "bg";
                        //     isSel = ""
                        // }

                        var className = "";
                        var eventName = "";
                        var inType = "";
                        var inSubType = "";
                        var isSel;
                        if (nextOne.isSelect) {
                            className = '_' + prevOne.INCIDENTTYPE;
                            eventName = prevOne.INCIDENTTYPENAME;
                            isSel = "clickEvent";
                            inType = prevOne.INCIDENTTYPE;
                            inSubType = "";
                        } else {
                            className = '_' + prevOne.INCIDENTTYPE + "bg";
                            eventName = prevOne.INCIDENTTYPENAME;
                            isSel = "";
                            inType = prevOne.INCIDENTTYPE;
                            inSubType = "";
                        }
                        if ($("#sjlxTjfx").val() != null && $("#sjlxTjfx").val().length == 1) {
                            if ($("#sjlxTjfx").val()[0] == "1" || $("#sjlxTjfx").val()[0] == "4" || $("#sjlxTjfx").val()[0] == "20") {
                                className = '_' + prevOne.SUBTYPENO;
                                isSel = isSel = "clickEvent";
                                eventName = prevOne.SUBTYPENAME;
                                inType = prevOne.INCIDENTTYPE;
                                inSubType = prevOne.SUBTYPENO;
                            }
                        }
                        if (isSel) {
                            var htmlDom = $('<div class="twoC  ' + className + ' ' + isSel + ' " type="' + inType + '" subType = "' + inSubType + '" typeName="' + eventName + '">' +
                                // var htmlDom = $('<div class="twoC  ' + className + ' ' + isSel + ' " type="' + nextOne.INCIDENTTYPE + '" typeName="' +eventName + '">' +
                                '<div class="small">' + eventName + '</div>' +
                                '<div class="big timer" data-to="' + prevTypeSum + '" data-speed="1500">' + prevTypeSum + '</div>' +
                                '<div class="between timer" data-to="' + nextTypeSum + '" data-speed="1500">' + nextTypeSum + '</div>' +
                                '<div class="smallest">' +
                                '<span class="' + (prevTypeSum >= nextTypeSum ? "redPercent" : "greenPercent") + ' timer" data-to="' + typePercent + '" data-speed="1500">' + typePercent + '</span>' +
                                '<span class="' + (prevTypeSum >= nextTypeSum ? "redPercent" : "greenPercent") + '">%</span>' +
                                '<span class="' + (prevTypeSum >= nextTypeSum ? "countUp" : "countDown") + '"></span>' +
                                '</div>' +
                                '</div>');
                            $("#topAllIllegalTypes").append(htmlDom[0]);
                        }
                        // yesDom.find(".timer").data("countToOptions", {
                        //     formatter: function(b, a) {
                        //         return b.toFixed(0).replace(/\B(?=(?:\d{3})+(?!\d))/g, ",")
                        //     }
                        // });
                        // yesDom.find(".timer").each(count);
                        break;
                    }

                    continue;
                }
            }
        }
        setTimeout(function () {
            if ($(".clickEvent").length > 0) {
                $(".clickEvent:first").click();
            }
        }, 100)
    };
    var topPointsEchartInstance = null;
    var topPointsEchart = function (data1, data2, data3, illegalTypeName) {
        require(["../../website-js/illegal/echarts.js"], function (echarts) {
            var option = {
                backgroundColor: "rgb(30,54,80)",
                title: {
                    top: '90%',
                    text: searchObj.startTime + ' ~ ' + searchObj.endTime,
                    left: 'center',
                    textStyle: {
                        color: '#47E0B8',
                        fontSize: 12,
                        fontFamily: 'MicrosoftYaHei',
                        fontWeight: 400
                    }
                },
                textStyle: {
                    color: "rgb(255,255,255)"
                },
                tooltip: {
                    trigger: 'axis',
                    backgroundColor: '#001028',
                    borderColor: '#2C5599',
                    borderWidth: 1,
                    formatter: '{b0}<br />' + illegalTypeName + '：{c1}',
                    axisPointer: {            // 坐标轴指示器，坐标轴触发有效
                        type: 'none'        // 默认为直线，可选为：'line' | 'shadow'
                    }
                },
                grid: {
                    left: '3%',
                    right: '4%',
                    bottom: '15%',
                    containLabel: true
                },
                xAxis: {
                    type: 'category',
                    data: data1,
                    axisLine: {
                        show: false
                    },
                    splitLine: {
                        show: false
                    },
                    axisTick: {
                        show: false
                    },
                    axisLabel: {
                        interval: 0,
                        rotate: -10
                    }
                },
                yAxis: {
                    type: 'value',
                    name: '数量         ',
                    axisLine: {
                        lineStyle: {
                            color: "rgb(101,110,113)"
                        }
                    },
                    splitLine: {//分隔线
                        show: true,
                        lineStyle: {
                            color: "rgb(101,110,113)"
                        }
                    },
                    axisTick: {//刻度线
                        show: false
                    }
                },
                series: [

                    { // For shadow
                        type: 'bar',
                        itemStyle: {
                            normal: {color: 'rgba(71,224,184,0.1)'}
                        },
                        barGap: '-100%',
                        barCategoryGap: '20%',
                        barWidth: 15,
                        data: data3,
                        animation: false
                    },
                    {
                        type: 'bar',
                        barWidth: 15,
                        itemStyle: {
                            normal: {
                                color: new echarts.graphic.LinearGradient(
                                    0, 0, 0, 1,
                                    [
                                        {offset: 0, color: 'rgba(71,224,184,1)'},
                                        {offset: 0.5, color: 'rgba(71,224,184,0.5)'},
                                        {offset: 1, color: 'rgba(71,224,184,0)'}
                                    ]
                                )
                            },
                            emphasis: {
                                color: new echarts.graphic.LinearGradient(
                                    0, 0, 0, 1,
                                    [
                                        {offset: 0, color: 'rgba(71,224,184,1)'},
                                        {offset: 0.5, color: 'rgba(71,224,184,0.9)'},
                                        {offset: 1, color: 'rgba(71,224,184,0.5)'}
                                    ]
                                )
                            }
                        },
                        data: data2
                    }


                ]
            };

            // 基于准备好的dom，初始化echarts实例
            topPointsEchartInstance = echarts.init(document.getElementById("topPointsEchart"));
            topPointsEchartInstance.setOption(option);

        });
    };

    var bottomPointsEchartInstance = null;
    var bottomPointsEchart = function (data1, data2, data3, illegalTypeName) {
        require(["../../website-js/illegal/echarts.js"], function (echarts) {
            var option = {
                backgroundColor: "rgb(30,54,80)",
                title: {
                    text: searchObj.nextStartTime + ' ~ ' + searchObj.nextEndTime,
                    left: 'center',
                    textStyle: {
                        color: '#B0DB3A',
                        fontSize: 12,
                        fontFamily: 'MicrosoftYaHei',
                        fontWeight: 400
                    }
                },
                textStyle: {
                    color: "rgb(255,255,255)"
                },
                tooltip: {
                    trigger: 'axis',
                    backgroundColor: '#001028',
                    borderColor: '#2C5599',
                    borderWidth: 1,
                    formatter: '{b0}<br />' + illegalTypeName + '：{c1}',
                    axisPointer: {            // 坐标轴指示器，坐标轴触发有效
                        type: 'none'        // 默认为直线，可选为：'line' | 'shadow'
                    }
                },
                grid: {
                    left: '3%',
                    right: '4%',
                    bottom: '15%',
                    containLabel: true
                },
                xAxis: {
                    type: 'category',
                    data: data1,
                    axisLine: {
                        show: false
                    },
                    splitLine: {
                        show: false
                    },
                    axisTick: {
                        show: false
                    },
                    axisLabel: {
                        interval: 0,
                        rotate: -10
                    }
                },
                yAxis: {
                    type: 'value',
                    name: '数量         ',
                    axisLine: {
                        lineStyle: {
                            color: "rgb(101,110,113)"
                        }
                    },
                    splitLine: {//分隔线
                        show: true,
                        lineStyle: {
                            color: "rgb(101,110,113)"
                        }
                    },
                    axisTick: {//刻度线
                        show: false
                    }
                },
                series: [

                    { // For shadow
                        type: 'bar',
                        itemStyle: {
                            normal: {color: 'rgba(71,224,184,0.1)'}
                        },
                        barGap: '-100%',
                        barCategoryGap: '20%',
                        barWidth: 15,
                        data: data3,
                        animation: false
                    },
                    {
                        type: 'bar',
                        barWidth: 15,
                        itemStyle: {
                            normal: {
                                color: new echarts.graphic.LinearGradient(
                                    0, 0, 0, 1,
                                    [
                                        {offset: 0, color: 'rgba(176,219,58,1)'},
                                        {offset: 0.5, color: 'rgba(176,219,58,0.5)'},
                                        {offset: 1, color: 'rgba(176,219,58,0)'}
                                    ]
                                )
                            },
                            emphasis: {
                                color: new echarts.graphic.LinearGradient(
                                    0, 0, 0, 1,
                                    [
                                        {offset: 0, color: 'rgba(176,219,58,1)'},
                                        {offset: 0.5, color: 'rgba(176,219,58,0.9)'},
                                        {offset: 1, color: 'rgba(176,219,58,0.5)'}
                                    ]
                                )
                            }
                        },
                        data: data2
                    }


                ]
            };

            // 基于准备好的dom，初始化echarts实例
            bottomPointsEchartInstance = echarts.init(document.getElementById("bottomPointsEchart"));
            bottomPointsEchartInstance.setOption(option);

        });
    };
    var sumEcharts = function (sum) {
        require(["../../website-js/illegal/echarts.js"], function (echarts) {
            var all = sum === 0 ? 10 : 2 * sum;

            var percent = sum / all;
            var height = $("#oneEchart").height();
            var width = $("#oneEchart").width();
            height = height < width ? height : width;//较小的为height
            width = width > height ? width : height;//较小的为width
            var x_y = calcPosition(sum, all, height, width);

            var option2 = {
                tooltip: {
                    show: true,
                    trigger: 'item',
                    backgroundColor: "#2F63FC"
                },
                series: [
                    {
                        name: '总数：',
                        type: 'gauge',
                        min: 0,
                        max: all,
                        radius: '100%',
                        center: ['50%', '70%'],
                        //起始角度。圆心 正右手侧为0度，正上方为90度，正左手侧为180度。
                        startAngle: 180,
                        //结束角度。
                        endAngle: 0,
                        splitNumber: 2,
                        axisLine: {// 坐标轴线
                            lineStyle: {       // 属性lineStyle控制线条样式
                                color: [[percent, '#2F63FC'], [1, '#fff']],
                                width: 15,
                                shadowColor: '#fff', //默认透明
                            }
                        },
                        axisLabel: {// 坐标轴小标记
                            /*fontWeight: 'bolder',*/
                            fontSize: 14,
                            color: '#fff',
                            padding: 12
                        },
                        axisTick: {            // 坐标轴小标记
                            length: 0,        // 属性length控制线长
                            lineStyle: {       // 属性lineStyle控制线条样式
                                color: 'auto',
                                shadowColor: '#fff', //默认透明
                                shadowBlur: 10
                            }
                        },
                        splitLine: {           // 分隔线
                            length: 0,         // 属性length控制线长
                            lineStyle: {       // 属性lineStyle（详见lineStyle）控制线条样式
                                width: 3,
                                color: '#fff',
                                shadowColor: '#fff', //默认透明
                                shadowBlur: 10
                            }
                        },
                        pointer: {// 指针
                            show: false
                        },
                        markPoint: {
                            symbol: 'image://' + baseURL + '/images/illegal/statisticalAnalysisBlueCircle.png',
                            symbolSize: 30,
                            label: {
                                show: false
                            },
                            data: [{
                                x: x_y.split("_")[0],
                                y: x_y.split("_")[1],
                                value: sum
                            }]
                        },
                        title: {
                            show: false
                        },
                        detail: {
                            show: false
                        },
                        data: [{value: sum}],
                        tooltip: {
                            formatter: "{a} <br/>{c} {b}"
                        }
                    }

                ]
            };
            var oneEchart = echarts.init(document.getElementById("oneEchart"));
            oneEchart.setOption(option2);

        });
    };
    var calcPosition = function (sum, all, height, width) {
        var radius = height / 2.0;//半径
        var PI = Math.PI;
        var deg = (sum / all * 1.0) * 180;

        var lineWidth = 15;
        var static = lineWidth / 2.0;

        if (deg === 0) {
            var x_0 = (width - height) / 2.0 + static;
            var y_0 = height * 0.2 + radius;
            return x_0 + "_" + y_0;
        } else if (deg === 90) {
            var x_90 = width / 2.0;
            var y_90 = height * 0.2 + static;
            return x_90 + "_" + y_90;
        } else if (deg < 90) {
            var sin = Math.sin(2 * PI / 360 * deg);//通过角度计算sin值
            var cos = Math.cos(2 * PI / 360 * deg);//通过角度计算cos值
            var y_width = radius * sin;
            var x_width = radius * cos;
            if (deg < 45) {
                return (((width - height) / 2.0 + static) + (radius - x_width)) + "_" + (height * 0.2 + radius - y_width);
            } else if (deg < 65) {
                return (((width - height) / 2.0 + static) + (radius - x_width)) + "_" + (height * 0.2 + static * 0.5 + radius - y_width);
            } else {
                return (((width - height) / 2.0 + static) + (radius - x_width)) + "_" + (height * 0.2 + static + radius - y_width);
            }
        } else if (deg > 90) {
            var deg_ = 180 - deg;

            var sin_ = Math.sin(2 * PI / 360 * deg_);//通过角度计算sin值
            var cos_ = Math.cos(2 * PI / 360 * deg_);//通过角度计算cos值
            var y_width_ = radius * sin_;
            var x_width_ = radius * cos_;

            if (deg >= 135) {
                return (((width - height) / 2.0 - static) + (radius + x_width_)) + "_" + (height * 0.2 + radius - y_width_);
            }
            return (((width - height) / 2.0 - static) + (radius + x_width_)) + "_" + (height * 0.2 + static + radius - y_width_);
        }

    };
    var showNextMapinfo = function (index, num) {
        var dataArr = eventMapData[index];
        if (num == 0) {
            if (mapEventIndex - 1 < 0) {
                common.showInfo("已经是第一条数据")
            } else {
                mapEventIndex--;
                eventmap.infoWindow.setTitle(dataArr.data[mapEventIndex].incidentTypeName);
                $(".happenTime").html(dataArr.data[mapEventIndex].happenTime);
                $(".incidentPlace").html(dataArr.data[mapEventIndex].incidentPlace);
                $(".ptiid").html(dataArr.data[mapEventIndex].ptiid)
            }
        } else {
            if (mapEventIndex + 1 > dataArr.data.length - 1) {
                common.showInfo("已经是第一条数据")
            } else {
                mapEventIndex++;
                eventmap.infoWindow.setTitle(dataArr.data[mapEventIndex].incidentTypeName);
                $(".happenTime").html(dataArr.data[mapEventIndex].happenTime);
                $(".incidentPlace").html(dataArr.data[mapEventIndex].incidentPlace);
                $(".ptiid").html(dataArr.data[mapEventIndex].ptiid)
            }
        }
    };
    var showEventminuteInfo = function (pid) {
        $.post("/trafficEvent/getIncidentInfoById", {"ptiid": pid}, function (data) {
            showModal(data)
        }, "json")
    };
    var showModal = function (data) {
        $("#eventminuteInfo").modal("show");
        $("#saveDisInfo,#gdDisinfo").hide();
        $("#eventImg").empty();
        var eventInfo = data.rows.incidentObj != null ? data.rows.incidentObj : {};
        var dispatchList = data.rows.dispatchList != null ? data.rows.dispatchList : {};
        var gdList = data.rows.gdList != null ? data.rows.gdList : {};
        if (eventInfo.incidentState == "4" || eventInfo.incidentState == "5") {
            $("#ddxxTabBtn").show();
        } else {
            $("#ddxxTabBtn").hide();
        }
        //事件信息编辑
        // if (eventInfo.incidentSource != "2") {
        //     $("#eventminuteInfo input").attr("readonly",true)
        // } else {
        //     if (eventInfo.incidentState == null || eventInfo.incidentState == "0") {
        //         $("#eventminuteInfo input").attr("readonly",false)
        //     }else{
        //         $("#eventminuteInfo input").attr("readonly",true)
        //     }
        // }
        $("#optimizeId").off().on("click", function () {
            $("#saveDisInfo,#gdDisinfo").hide();
            if (eventInfo.incidentState == null || eventInfo.incidentState == "0") {
                $("#closeEvent,#ownCl,#scheduBtn").show();
                if (eventInfo.incidentSource == "1" || eventInfo.incidentSource == "3") {
                    $("#btnCaseSaveLink").show();
                    $("#optimize input,#optimize select").prop("disabled", false)
                } else {
                    $("#btnCaseSaveLink").hide();
                    $("#optimize input,#optimize select").prop("disabled", true)
                }
            } else {
                $("#btnCaseSaveLink").hide();
                $("#optimize input,#optimize select").prop("disabled", true)
                $("#closeEvent,#ownCl,#scheduBtn").hide()
            }
            $("#historyData").removeClass("addSelect");
            $("#optimizeId").addClass("addSelect");
        });
        $("#closeEvent").off().on("click", function () {

            common.showPopuConfirm("确定关闭此交通事件，请选择关闭原因：<br><input name='radio_closeEvent' value='错误事件' type='radio' id='radio_closeEvent_1' checked/><label for='radio_closeEvent_1'>错误事件</label><input type='radio' name='radio_closeEvent' value='重复事件' id='radio_closeEvent_2'/><label for='radio_closeEvent_2'>重复事件</label><input type='radio' name='radio_closeEvent' value='其他' id='radio_closeEvent_3'/><label for='radio_closeEvent_3'>其他</label>", "关闭事件", function () {
                var date = new Date();
                var param = {
                    "ptiid": eventInfo.ptiid,
                    incidentState: 1,
                    closeReason: $("input[name='radio_closeEvent']:checked").val(),
                    "disposeTime": common.getStandardTime(new Date()),
                    "disposeUser": ConfigSetting.MY_ID
                };
                $.getJSON("/trafficEvent/updateTrIncidentInfo", param, function (data) {
                    if (data.state == "SUCCESS") {
                        common.showSuccess("关闭事件成功");
                        $("#eventminuteInfo").modal("hide");
                        loadEventTable();
                    } else {
                        common.showError("关闭事件失败");
                    }
                });
            });


            // $("#closeEventModal").modal("show");
            // $("#closePeoele").val(ConfigSetting.CURRENT_USER_NAME);
            // $("#closebtnCaseSave").off().on("click",function () {
            //     if($("#closeTalk").val() == ""){
            //         common.showInfo("请输入关闭原因")
            //     }else{
            //         var obj = {
            //
            //             "incidentState":"1",
            //             "closeReason":$("#closeTalk").val(),
            //
            //         };
            //         $.post("",obj,function (data) {
            //             if(data.state == "SUCCESS"){
            //                 common.showSuccess("关闭成功");
            //                 $("#closeTalk").val("")
            //                 $("#closeEventModal").modal("hide");
            //                 $("#eventminuteInfo").modal("hide");
            //
            //             }
            //         },"json")
            //     }
            // })
        });
        $("#ownCl").off().on("click", function () {
            $("#cancleEventModal").modal("show");
            $("#cancleTalk").val("");
            $("#canclePeoele").val(ConfigSetting.CURRENT_USER_NAME);
            $("#canclebtnCaseSave").off().on("click", function () {
                if ($("#cancleTalk").val() == "") {
                    common.showInfo("请输入自撤原因")
                } else {
                    var obj = {
                        "ptiid": eventInfo.ptiid,
                        "incidentState": "2",
                        "closeReason": $("#cancleTalk").val(),
                        "disposeTime": common.getStandardTime(new Date()),
                        "disposeUser": ConfigSetting.MY_ID
                    };
                    $.post("/trafficEvent/updateTrIncidentInfo", obj, function (data) {
                        if (data.state == "SUCCESS") {
                            common.showSuccess("自撤成功");
                            $("#cancleTalk").val("");
                            $("#cancleEventModal").modal("hide");
                            $("#eventminuteInfo").modal("hide");
                            loadEventTable();
                        }
                    }, "json");
                }
            })
        });
        $("#scheduBtn").off().on("click", function () {
            var ptiid = eventInfo.ptiid;
            $("#eventminuteInfo").modal("hide");
            $("#btnCommandTask").trigger("click");
            schedulingCommand_Page.dispatcher(ptiid, eventInfo.incidentTypeName, function () {
                schedulingCommand_Map.updateEventInfo(ptiid, "incidentState", 3);
                schedulingCommand_Map.updateEventInfo(ptiid, "incidentStateName", "调度中");
            });
        });
        $("#btnCaseSaveLink").off().on("click", function () {
            saveeventInfo(eventInfo.ptiid, "1", eventInfo.dispatchId)
        });
        $("#saveDisInfo").off().on("click", function () {
            saveeventInfo(eventInfo.ptiid, "2", eventInfo.dispatchId)
        });
        $("#sjlxModal,#dldmModal").off();
        $.getJSON("/trafficEvent/getTrIncidentType", function (lxdata) {
            if (lxdata != null && lxdata.state == "SUCCESS") {
                var bindData = [];
                for (var i = 0; i < lxdata.rows.length; i++) {
                    bindData.push({id: lxdata.rows[i].TYPE_NO, text: lxdata.rows[i].TYPE_NAME});
                }
                $("#sjlxModal").select2({
                    placeholder: "请选择",
                    allowClear: true,
                    multiple: false,
                    data: bindData,
                    width: null
                }).val(null).trigger("change").on("change", function () {
                    if ($("#sjlxModal").val() != "" && $("#sjlxModal").val() != null) {
                        $.getJSON("/trafficEvent/getTrIncidentSubType", {"typeNo": $("#sjlxModal").val()}, function (zldata) {
                            if (zldata.rows.length > 0) {
                                $("#sjzlModal").prop("disabled", false);
                                var bindData = [];
                                for (var i = 0; i < zldata.rows.length; i++) {
                                    bindData.push({id: zldata.rows[i].SUBTYPE_NO, text: zldata.rows[i].SUBTYPE_NAME});
                                }
                                $("#sjzlModal").select2({
                                    placeholder: "请选择",
                                    allowClear: true,
                                    multiple: false,
                                    data: bindData,
                                    width: null
                                }).val(eventInfo.incidentSubType).trigger("change");
                            } else {
                                $("#sjzlModal").select2({
                                    placeholder: "请选择事件类型",
                                    allowClear: true,
                                    multiple: false,
                                    data: [],
                                    disabled: true,
                                    width: null
                                }).val(null).trigger("change");
                            }

                        })
                    } else {
                        $("#sjzlModal").select2({
                            placeholder: "请选择事件类型",
                            allowClear: true,
                            multiple: false,
                            data: [],
                            disabled: true,
                            width: null
                        }).val(null).trigger("change");
                    }
                });
                var selData = eventInfo.incidentType != "" ? eventInfo.incidentType : null;
                $("#sjlxModal").val(selData).trigger("change")
            }
        });
        $.getJSON("/trafficEvent/getTrRoadCode", function (data) {
            if (data != null && data.state == "SUCCESS") {
                var bindData = [];
                for (var i = 0; i < data.rows.length; i++) {
                    bindData.push({id: data.rows[i].ROAD_CODE, text: data.rows[i].ROAD_CODE + data.rows[i].ROAD_NAME});
                }
                $("#dldmModal").select2({
                    placeholder: "请选择",
                    allowClear: true,
                    multiple: false,
                    data: bindData,
                    width: null
                }).val(null).trigger("change").on("change", function () {
                    if ($("#dldmModal").val() != "" || $("#dldmModal").val() != null) {
                        $.getJSON("/trafficEvent/getTrCrossCode", {"roadCode": $("#dldmModal").val()}, function (data) {
                            var bindData = [];
                            for (var i = 0; i < data.rows.length; i++) {
                                bindData.push({
                                    id: data.rows[i].CROSS_CODE,
                                    text: data.rows[i].CROSS_CODE + data.rows[i].CROSS_NAME
                                });
                            }
                            if (bindData.length > 0) {
                                $("#lkldModal").select2({
                                    placeholder: "请选择",
                                    allowClear: true,
                                    multiple: false,
                                    data: bindData,
                                    width: null
                                }).val(eventInfo.crossCode).trigger("change");
                            } else {
                                $("#lkldModal").select2({
                                    placeholder: "请选择",
                                    allowClear: true,
                                    multiple: false,
                                    data: [],
                                    disabled: true,
                                    width: null
                                }).val(null).trigger("change");
                            }
                        })
                    } else {
                        $("#lkldModal").select2({
                            placeholder: "请选择",
                            allowClear: true,
                            multiple: false,
                            data: [],
                            disabled: true,
                            width: null
                        }).val(null).trigger("change");
                    }
                });
                var selData = eventInfo.roadCode != "" ? eventInfo.roadCode : null;
                $("#dldmModal").val(selData).trigger("change");

            }
        });
        $("#fssjModal").val(eventInfo.happenTime != null ? eventInfo.happenTime : "");
        // $("#fssjModal").datetimepicker("setDate", eventInfo.happenTime);
        // $("#dldmModal").val(eventInfo.roadCode).trigger("change");
        $("#sjlyModal").val(eventInfo.incidentSource).trigger("change");
        // $("#sjlxModal").val(eventInfo.incidentType).trigger("change");
        $("#szfxModal").val(eventInfo.laneDirection).trigger("change");
        $("#sjdjModal").val(eventInfo.incidentLevel).trigger("change");

        $("#sjztModal").val(eventInfo.incidentState).trigger("change");
        $("#fsddModal").val(eventInfo.incidentPlace != null ? eventInfo.incidentPlace : "未知");
        $("#bjnrModal").val(eventInfo.alarmContent != null ? eventInfo.alarmContent : "未知");
        $("#bjrModal").val(eventInfo.alarmPerson != null ? eventInfo.alarmPerson : "未知");
        $("#lxdhModal").val(eventInfo.alarmPhone != null ? eventInfo.alarmPhone : "未知");
        $("#bjsjModal").val(eventInfo.alarmTime != null ? eventInfo.alarmTime : "未知");
        // $("#sjzlModal").val(eventInfo.incidentSubType).trigger("change");
        // $("#lkldModal").val(eventInfo.crossCode).trigger("change");
        // $("#lkldModal").val(eventInfo.validFlag).trigger("change");
        $("#bjsjModal").val(eventInfo.alarmTime != null ? eventInfo.alarmTime : "未知");
        // $("#bjsjModal").val(eventInfo.alarmTime!=null ? eventInfo.alarmTime : "未知");
        $("#rksjModal").val(eventInfo.createTime != null ? eventInfo.createTime : "未知");
        $("#lrrModal").val(eventInfo.createUserName != null ? eventInfo.createUserName : "未知");
        $("#czryModal").val(eventInfo.disposeUserName != null ? eventInfo.disposeUserName : "未知");
        $("#czsjModal").val(eventInfo.disposeTime != null ? eventInfo.disposeTime : "未知");
        $("#gbyyModal").val(eventInfo.closeReason != null ? eventInfo.closeReason : "未知");
        if (eventInfo.imageOne != null) {
            $("#eventImg").append("<img style='width: 100%'  src=" + eventInfo.imageOne + " />")
        }
        if (eventInfo.imageTwo != null) {
            $("#eventImg").append("<img style='width: 100%'  src=" + eventInfo.imageTwo + " />")
        }
        if (eventInfo.imageThree != null) {
            $("#eventImg").append("<img style='width: 100%'  src=" + eventInfo.imageThree + " />")
        }
        $("#optimizeId").trigger("click");
        $("#historyData").off().on("click", function () {
            $("#optimizeId").removeClass("addSelect");
            $("#historyData").addClass("addSelect");
            $("#closeEvent,#ownCl,#scheduBtn,#btnCaseSaveLink").hide();
            if (eventInfo.incidentState != "5") {
                $("#saveDisInfo,#gdDisinfo").show();
                $("#gdInfo input").prop("disabled", false)
            } else {
                $("#gdInfo input").prop("disabled", true)
                $("#saveDisInfo,#gdDisinfo").hide();
            }
            $("#ddyName").html(eventInfo.disposeUserName != null ? eventInfo.disposeUserName : "未知");
            $("#dispatchListUl").empty();
            $(".grayBottom").remove()
            var divWidth = (dispatchList.length + 2) * 100 + 200;
            $("#dispatchListUl").css("width", divWidth + "px");
            var html = "";
            html += "<li style=\"width:100px\">";
            html += "<span class='' style='white-space: nowrap;text-align: center'>开始时间</span>";
            html += "<div class='borderClass' style='height: 100px;width: 1px;border-left: 1px dashed;margin: auto;'></div>";
            html += "<div style=\"background: url('/images/trafficEvent/event/yg.png') no-repeat;background-size: 100px 20px;width: 100%;color: white\">" + eventInfo.happenTime.substr(-8) + "</div>";
            html += "</li>";
            for (var i = 0; i < dispatchList.length; i++) {
                var titleInfo = dispatchList[i].ACTIONS != null ? dispatchList[i].ACTIONS : "无具体信息";
                html += "<li style=\"width:100px\">";
                html += "<img title=\"" + titleInfo + "\" class=\"\" src=\"/images/trafficEvent/event/" + dispatchList[i].DISPATCHTYPE + ".png\" style=\"display: block;margin: auto\"/>";
                html += "<span class='' style='white-space: nowrap;text-align: center'>" + dispatchList[i].DISDATCHTYPENAME + "</span>";
                html += "<div class='borderClass' style='height: 80%;width: 1px;border-left: 1px dashed;margin: auto;'></div>";
                html += "<div style=\"background: url('/images/trafficEvent/event/bg.png') no-repeat;background-size: 100px 20px;width: 100%;color: white\">" + dispatchList[i].DISPATCHTIME.substr(-8) + "</div>";
                html += "</li>"
            }
            html += "<li style=\"width:100px\">";
            html += "<span class='' style='white-space: nowrap;text-align: center'>结束时间</span>";
            html += "<div class='borderClass' style='height: 100px;width: 1px;border-left: 1px dashed;margin: auto;'></div>";
            html += "<div style=\"background: url('/images/trafficEvent/event/yg.png') no-repeat;background-size: 100px 20px;width: 100%;color: white\">" + eventInfo.disposeTime.substr(-8) + "</div>";
            html += "</li>";
            $("#dispatchListUl").append(html);
            if (dispatchList.length == 0) {
                $(".borderClass").css("height", "170px")
                $("#dispatchListUl").css("margin-top", "0px");
            } else {
                $("#dispatchListUl").css("margin-top", "14px");
            }

            $("#widthUl").append("<div class='grayBottom' style=\"width: " + divWidth + "px;height: 18px;background: #EEEEEE;position: absolute;bottom: 20px;\"></div>");
            $("#gdshrs").val(gdList.victimNum != null ? gdList.victimNum : "0");
            $("#gdssrs").val(gdList.injuredNum != null ? gdList.injuredNum : "0");
            $("#gdswrs").val(gdList.deathNum != null ? gdList.deathNum : "0");
            $("#gdjjss").val(gdList.ecoLoss != null ? gdList.ecoLoss : "0");
            $("#gdsjcls").val(gdList.vechicleNum != null ? gdList.vechicleNum : "0");
            $("#gdshcls").val(gdList.destroyedVechicle != null ? gdList.destroyedVechicle : "0");
            $("#gdcdrs").val(gdList.dispatchPeople != null ? gdList.dispatchPeople : "0");
            $("#gdcdcl").val(gdList.dispatchVechicle != null ? gdList.dispatchVechicle : "0");
            $("#gdsjqy").val(gdList.disposeKey != null ? gdList.disposeKey : "无");
            $("#gdczyd").val(gdList.incidentReason != null ? gdList.incidentReason : "无");
            $("#gdjyjx").val(gdList.expSummary != null ? gdList.expSummary : "无");
        })
    };
    var saveeventInfo = function (id, type, disId) {
        if (type == "1") {
            var obj = {
                "ptiid": id
            };
            var incidentSource = $("#sjlyModal").val();
            if (incidentSource == "" || incidentSource == null) {
                common.showWarn("请选择事件来源");
                return false
            } else {
                obj.incidentSource = incidentSource;
                obj.createUser = incidentSource == "1" ? ConfigSetting.MY_ID : "";
            }
            var happenTime = $("#fssjModal").val();
            if (happenTime == "" || happenTime == null) {
                common.showWarn("请输入发生时间");
                return false
            } else {
                obj.happenTime = happenTime;
            }
            var incidentType = $("#sjlxModal").val();
            if (incidentType == "" || incidentType == null) {
                common.showWarn("请选择事件类型");
                return false
            } else {
                obj.incidentType = incidentType;
            }
            var incidentSubType = $("#sjzlModal").val();
            if (incidentSubType == "" || incidentSubType == null) {
                common.showWarn("请选择事件子类型");
                return false
            } else {
                obj.incidentSubType = incidentSubType;
            }
            var roadCode = $("#dldmModal").val();
            if (roadCode == "" || roadCode == null) {
                common.showWarn("请选择道路代码");
                return false
            } else {
                obj.roadCode = roadCode;
            }
            var crossCode = $("#dldmModal").val();
            if (crossCode == "" || crossCode == null) {
                common.showWarn("请选择路口路段代码");
                return false
            } else {
                obj.crossCode = crossCode;
            }
            var laneDirection = $("#szfxModal").val();
            if (laneDirection == "" || laneDirection == null) {
                common.showWarn("请选择所在方向");
                return false
            } else {
                obj.laneDirection = laneDirection;
            }
            var incidentLevel = $("#sjdjModal").val();
            if (incidentLevel == "" || incidentLevel == null) {
                common.showWarn("请选择事件等级");
                return false
            } else {
                obj.incidentLevel = incidentLevel;
            }
            var incidentPlace = $("#fsddModal").val();
            if (incidentPlace == "" || incidentPlace == null) {
                common.showWarn("请输入发生地点");
                return false
            } else {
                obj.incidentPlace = incidentPlace;
            }
            var alarmContent = $("#bjnrModal").val();
            if (alarmContent == "" || alarmContent == null) {
                common.showWarn("请输入报警内容");
                return false
            } else {
                obj.alarmContent = alarmContent;
            }
            var alarmPerson = $("#bjrModal").val();
            if (alarmPerson == "" || alarmPerson == null) {
                common.showWarn("请输入报警人");
                return false
            } else {
                obj.alarmPerson = alarmPerson;
            }
            var alarmPhone = $("#lxdhModal").val();
            if (alarmPhone == "" || alarmPhone == null) {
                common.showWarn("请输入联系电话");
                return false
            } else {
                obj.alarmPhone = alarmPhone;
            }
            var alarmTime = $("#bjsjModal").val();
            if (alarmTime == "" || alarmTime == null) {
                common.showWarn("请输入报警时间");
                return false
            } else {
                obj.alarmTime = alarmTime;
            }
            if ($("#sfgzModal").val() != null) {
                obj.hasAttention = $("#sfgzModal").val();
            } else {
                obj.hasAttention = "0"
            }
            $.post("/trafficEvent/updateTrIncidentInfo", obj, function (data) {
                if (data.state == "SUCCESS") {
                    common.showSuccess("保存成功");
                    $("#eventminuteInfo").modal("hide");
                    loadEventTable();
                }
            }, "json")
        }
        ;
        if (type == "2" || type == "3") {
            var obj = {
                "ptiid": id,
                "dispatchId": disId
            }
            if (isNaN($("#gdshrs").val())) {
                obj.victimNum = $("#gdshrs").val()
            } else {
                common.showWarn("请输入受害人数");
                return false
            }
            if (isNaN($("#gdswrs").val())) {
                obj.deathNum = $("#gdswrs").val()
            } else {
                common.showWarn("请输入死亡人数");
                return false
            }
            if (isNaN($("#gdssrs").val())) {
                obj.injuredNum = $("#gdssrs").val()
            } else {
                common.showWarn("请输入受伤人数");
                return false
            }
            if (isNaN($("#gdjjss").val())) {
                obj.ecoLoss = $("#gdjjss").val()
            } else {
                common.showWarn("请输入经济损失");
                return false
            }
            if (isNaN($("#gdsjcls").val())) {
                obj.vechicleNum = $("#gdsjcls").val()
            } else {
                common.showWarn("请输入涉及车辆数");
                return false
            }
            if (isNaN($("#gdshcls").val())) {
                obj.destroyedVechicle = $("#gdshcls").val()
            } else {
                common.showWarn("请输入损毁车辆数");
                return false
            }
            if (isNaN($("#gdcdrs").val())) {
                obj.dispatchPeople = $("#gdcdrs").val()
            } else {
                common.showWarn("请输入出动人数")
                return false
            }
            if (isNaN($("#gdcdcl").val())) {
                obj.dispatchVechicle = $("#gdcdcl").val()
            } else {
                common.showWarn("请输入出动车辆");
                return false
            }
            if ($("#gdsjqy").val() != '') {
                obj.incidentReason = $("#gdsjqy").val()
            } else {
                common.showWarn("请输入事件起因");
                return false
            }
            if ($("#gdczyd").val() != '') {
                obj.disposeKey = $("#gdczyd").val()
            } else {
                common.showWarn("请输入处置要点");
                return false
            }
            if ($("#gdjyjx").val() != '') {
                obj.expSummary = $("#gdjyjx").val()
            } else {
                common.showWarn("请输入经验教训");
                return false
            }
            obj.incidentState = type == "2" ? "" : "5";
            $.post("/trafficEvent/updateTrDispatchInfo", obj, function (data) {
                if (data.state == "SUCCESS") {
                    common.showSuccess("保存成功");
                    $("#eventminuteInfo").modal("hide");
                }
            }, "json")
        }
    };
    var crossMoreTable = function () {
        var nowHeight = (510 - 20 - 15 - 34 - 58);
        if (searchObj.nextStartTime !== undefined && searchObj.nextEndTime !== undefined) {//有对比
            widthPercent("#crosssMore .crossMoreLeft", "50");
            $("#crosssMore .crossMoreRight").show();
            widthPercent("#crosssMore .crossMoreRight", "50");
            $("#crosssMore .prevData").html(searchObj.startTime + " ~ " + searchObj.endTime);
            setTimeout(function () {
                $("#crossprevTable").bootstrapTable('destroy');
                $("#crossprevTable").bootstrapTable({
                    url: "/trafficEvent/statisticsGroupByCrossCode",
                    pagination: true,
                    striped: false,
                    dataType: "json",
                    sidePagination: "server",
                    pageSize: 15,
                    pageNumber: 1,
                    pageList: [],
                    columns: [
                        {
                            field: "CROSSNAME", title: "道路名称", formatter: function (value, row, index) {
                            return "<span style='color: #009BFC;' href='#' onclick=\'\'>" + $.trim(value) + "</span>";
                        }
                        },
                        {field: "NUM", title: "事件数量"}
                    ],
                    queryParams: prevQueryParams,
                    formatNoMatches: function () {
                        return '无符合条件的记录';
                    },
                    formatLoadingMessage: function () {
                        return "请稍等，正在加载中...";
                    },
                    responseHandler: function (res) {//在加载服务器发送来的数据之前，处理数据的格式，参数res表示the response data（获取的数据）
                        return {
                            "total": res.total,
                            "rows": res.rows
                        }

                        // return {
                        //     "total": 25,
                        //     "rows": [{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3}]
                        // }
                    },
                    onLoadSuccess: function () {
                        tableScrol.init("#crossprevTable", nowHeight);
                        $("#crossprevTable .slimScrollDiv").css("height", nowHeight + "px");
                    },
                    onPostBody: function () {
                    }
                });

                $("#crosssMore .nextData").html(searchObj.nextStartTime + " ~ " + searchObj.nextEndTime);
                $("#crossnextTable").bootstrapTable('destroy');
                $("#crossnextTable").bootstrapTable({
                    url: "/trafficEvent/statisticsGroupByCrossCode",
                    pagination: true,
                    striped: false,
                    dataType: "json",
                    sidePagination: "server",
                    pageSize: 15,
                    pageNumber: 1,
                    pageList: [],
                    columns: [
                        {
                            field: "CROSSNAME", title: "道路名称", formatter: function (value, row, index) {
                            return "<span style='color: #009BFC;' href='#' onclick=\'\'>" + $.trim(value) + "</span>";
                        }
                        },
                        {field: "NUM", title: "事件数量"}
                    ],
                    queryParams: nextQueryParams,
                    formatNoMatches: function () {
                        return '无符合条件的记录';
                    },
                    formatLoadingMessage: function () {
                        return "请稍等，正在加载中...";
                    },
                    responseHandler: function (res) {//在加载服务器发送来的数据之前，处理数据的格式，参数res表示the response data（获取的数据）
                        return {
                            "total": res.total,
                            "rows": res.rows
                        }
                    },
                    onLoadSuccess: function () {
                        tableScrol.init("#crossnextTable", nowHeight);
                        $("#crossnextTable .slimScrollDiv").css("height", nowHeight + "px");
                    },
                    onPostBody: function () {
                    }
                });
            }, "600")

        } else {//没有对比
            $("#crosssMore .crossMoreRight").hide();
            widthPercent("#crosssMore .crossMoreLeft", "100");
            $("#crosssMore .prevData").html(searchObj.startTime + " ~ " + searchObj.endTime);
            setTimeout(function () {
                $("#crossprevTable").bootstrapTable('destroy');
                $("#crossprevTable").bootstrapTable({
                    url: "/trafficEvent/statisticsGroupByCrossCode",
                    pagination: true,
                    striped: false,
                    dataType: "json",
                    sidePagination: "server",
                    pageSize: 15,
                    pageNumber: 1,
                    pageList: [],
                    columns: [
                        {
                            field: "CROSSNAME", title: "道路名称", formatter: function (value, row, index) {
                            return "<span style='color: #009BFC;' href='#' onclick=\'\'>" + $.trim(value) + "</span>";
                        }
                        },
                        {field: "NUM", title: "事件数量"}
                    ],
                    queryParams: prevQueryParams,
                    formatNoMatches: function () {
                        return '无符合条件的记录';
                    },
                    formatLoadingMessage: function () {
                        return "请稍等，正在加载中...";
                    },
                    responseHandler: function (res) {//在加载服务器发送来的数据之前，处理数据的格式，参数res表示the response data（获取的数据）
                        return {
                            "total": res.total,
                            "rows": res.rows
                        }
                        // return {
                        //     "total": 25,
                        //     "rows": [{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3}]
                        // }
                    },
                    onLoadSuccess: function () {
                        tableScrol.init("#crossprevTable", nowHeight);
                        $("#crossprevTable .slimScrollDiv").css("height", nowHeight + "px");
                    },
                    onPostBody: function () {
                    }
                });
            }, "600")

        }
    };
    var pointsMoreTable = function () {
        var nowHeight = (510 - 20 - 15 - 34 - 58);
        if (searchObj.nextStartTime !== undefined && searchObj.nextEndTime !== undefined) {//有对比
            widthPercent("#pointsMore .pointsMoreLeft", "50");
            $("#pointsMore .pointsMoreRight").show();
            widthPercent("#pointsMore .pointsMoreRight", "50");
            $("#pointsMore .prevData").html(searchObj.startTime + " ~ " + searchObj.endTime);
            $("#pointsMore .nextData").html(searchObj.nextStartTime + " ~ " + searchObj.nextEndTime);

            setTimeout(function () {
                $("#prevTable").bootstrapTable('destroy');
                $("#prevTable").bootstrapTable({
                    url: "/trafficEvent/statisticsGroupByRoadCode",
                    pagination: true,
                    striped: false,
                    dataType: "json",
                    sidePagination: "server",
                    pageSize: 15,
                    pageNumber: 1,
                    pageList: [],
                    columns: [
                        {
                            field: "ROADNAME", title: "道路名称", formatter: function (value, row, index) {
                            return "<span style='color: #009BFC;' href='#' onclick=\'\'>" + $.trim(value) + "</span>";
                        }
                        },
                        {field: "NUM", title: "事件数量"}
                    ],
                    queryParams: prevQueryParams,
                    formatNoMatches: function () {
                        return '无符合条件的记录';
                    },
                    formatLoadingMessage: function () {
                        return "请稍等，正在加载中...";
                    },
                    responseHandler: function (res) {//在加载服务器发送来的数据之前，处理数据的格式，参数res表示the response data（获取的数据）
                        return {
                            "total": res.total,
                            "rows": res.rows
                        }

                        // return {
                        //     "total": 25,
                        //     "rows": [{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3}]
                        // }
                    },
                    onLoadSuccess: function () {
                        tableScrol.init("#prevTable", nowHeight);
                        $("#prevTable .slimScrollDiv").css("height", nowHeight + "px")
                    },
                    onPostBody: function () {
                    }
                });
                $("#nextTable").bootstrapTable('destroy');
                $("#nextTable").bootstrapTable({
                    url: "/trafficEvent/statisticsGroupByRoadCode",
                    pagination: true,
                    striped: false,
                    dataType: "json",
                    sidePagination: "server",
                    pageSize: 15,
                    pageNumber: 1,
                    pageList: [],
                    columns: [
                        {
                            field: "ROADNAME", title: "道路名称", formatter: function (value, row, index) {
                            return "<span style='color: #009BFC;' href='#' onclick=\'\'>" + $.trim(value) + "</span>";
                        }
                        },
                        {field: "NUM", title: "事件数量"}
                    ],
                    queryParams: nextQueryParams,
                    formatNoMatches: function () {
                        return '无符合条件的记录';
                    },
                    formatLoadingMessage: function () {
                        return "请稍等，正在加载中...";
                    },
                    responseHandler: function (res) {//在加载服务器发送来的数据之前，处理数据的格式，参数res表示the response data（获取的数据）
                        return {
                            "total": res.total,
                            "rows": res.rows
                        }
                    },
                    onLoadSuccess: function () {
                        tableScrol.init("#nextTable", nowHeight)
                        $("#nextTable .slimScrollDiv").css("height", nowHeight + "px")
                    },
                    onPostBody: function () {
                    }
                });
            }, "600")

        } else {//没有对比
            $("#pointsMore .pointsMoreRight").hide();
            widthPercent("#pointsMore .pointsMoreLeft", "100");
            $("#pointsMore .prevData").html(searchObj.startTime + " ~ " + searchObj.endTime);
            setTimeout(function () {
                $("#prevTable").bootstrapTable('destroy');
                $("#prevTable").bootstrapTable({
                    url: "/trafficEvent/statisticsGroupByRoadCode",
                    pagination: true,
                    striped: false,
                    dataType: "json",
                    sidePagination: "server",
                    pageSize: 15,
                    pageNumber: 1,
                    pageList: [],
                    columns: [
                        {
                            field: "ROADNAME", title: "道路名称", formatter: function (value, row, index) {
                            return "<span style='color: #009BFC;' href='#' onclick=\'\'>" + $.trim(value) + "</span>";
                        }
                        },
                        {field: "NUM", title: "事件数量"}
                    ],
                    queryParams: prevQueryParams,
                    formatNoMatches: function () {
                        return '无符合条件的记录';
                    },
                    formatLoadingMessage: function () {
                        return "请稍等，正在加载中...";
                    },
                    responseHandler: function (res) {//在加载服务器发送来的数据之前，处理数据的格式，参数res表示the response data（获取的数据）
                        return {
                            "total": res.total,
                            "rows": res.rows
                        }
                        // return {
                        //     "total": 25,
                        //     "rows": [{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3},{tollgateName:"点位1",num:3}]
                        // }
                    },
                    onLoadSuccess: function () {
                        tableScrol.init("#prevTable", nowHeight)
                        $("#prevTable .slimScrollDiv").css("height", nowHeight + "px");
                    },
                    onPostBody: function () {
                    }
                });
            }, "600")

        }
    };

    function prevQueryParams(params) {
        prevQueryParams.limit = params.limit;
        prevQueryParams.begin = params.offset;
        prevQueryParams.incidentType = searchObj.incidentType;
        prevQueryParams.incidentSubType = searchObj.incidentSubType;
        prevQueryParams.startTime = searchObj.startTime;
        prevQueryParams.endTime = searchObj.endTime;
        prevQueryParams.timeType = searchObj.timeType;
        return prevQueryParams;
    }

    function nextQueryParams(params) {
        nextQueryParams.limit = params.limit;
        nextQueryParams.begin = params.offset;
        nextQueryParams.incidentType = searchObj.incidentType;
        prevQueryParams.incidentSubType = searchObj.incidentSubType;
        nextQueryParams.startTime = searchObj.nextStartTime;
        nextQueryParams.endTime = searchObj.nextEndTime;
        nextQueryParams.timeType = searchObj.timeType;
        return nextQueryParams;
    }

    var reportbaseStr = "";
    var Imageobj = {
        "imageOne": "",
        "imageTwo": "",
        "imageThree": "",
        "imageFour": ""
    };
    return {
        init: function () {
            bindInit();
            initMapEvent()
        },
        nextMapEvent: function (index, num) {
            showNextMapinfo(index, num)
        },
        showInfo: function (pid) {
            showEventminuteInfo(pid)
        },
        bditPositiong: function (lon, lat, id) {
            eventBidtPos(lon, lat, id)
        },
        showInfoTable: function (id) {
            $.post("/trafficEvent/getIncidentInfoById", {"ptiid": id}, function (data) {
                showModal(data)
            }, "json")
        },
        selectImage: function (file) {
            var image = '';
            if (!file.files || !file.files[0]) {
                return;
            }
            Imageobj = {
                "imageOne": "",
                "imageTwo": "",
                "imageThree": "",
                "imageFour": ""
            };
            var reader = new FileReader();
            reader.readAsDataURL(file.files[0]);
            reader.onload = function (evt) {
                document.getElementById('reportimageOne').src = evt.target.result;
                $("#reportimageOne").show();
                image = evt.target.result;
                var base64Str = this.result;
                var startNum = base64Str.indexOf("base64,");
                startNum = startNum * 1 + 7;
                //去除前部格式信息（如果有需求）
                reportbaseStr = base64Str.slice(startNum);
                Imageobj.imageOne = base64Str.slice(startNum);
                if (file.files[1]) {
                    var reader = new FileReader();
                    reader.readAsDataURL(file.files[1]);
                    reader.onload = function (evt) {
                        document.getElementById('reportimageTwo').src = evt.target.result;
                        $("#reportimageTwo").show();
                        image = evt.target.result;
                        var base64Str = this.result;
                        var startNum = base64Str.indexOf("base64,");
                        startNum = startNum * 1 + 7;
                        //去除前部格式信息（如果有需求）
                        // reportbaseStr = base64Str.slice(startNum);
                        Imageobj.imageTwo = base64Str.slice(startNum);
                        if (file.files[2]) {
                            var reader = new FileReader();
                            reader.readAsDataURL(file.files[2]);
                            reader.onload = function (evt) {
                                document.getElementById('reportimageThree').src = evt.target.result;
                                $("#reportimageThree").show();
                                image = evt.target.result;
                                var base64Str = this.result;
                                var startNum = base64Str.indexOf("base64,");
                                startNum = startNum * 1 + 7;
                                //去除前部格式信息（如果有需求）
                                Imageobj.imageThree = base64Str.slice(startNum);
                                if (file.files[3]) {
                                    var reader = new FileReader();
                                    reader.readAsDataURL(file.files[3]);
                                    reader.onload = function (evt) {
                                        document.getElementById('reportimageFour').src = evt.target.result;
                                        $("#reportimageFour").show();
                                        image = evt.target.result;
                                        var base64Str = this.result;
                                        var startNum = base64Str.indexOf("base64,");
                                        startNum = startNum * 1 + 7;
                                        //去除前部格式信息（如果有需求）
                                        // reportbaseStr = base64Str.slice(startNum);
                                        Imageobj.imageFour = base64Str.slice(startNum);
                                    };
                                }
                            };
                        }
                    };
                }
            };

        }
    }
}();

$(function () {
    trafficeventScheduling.init()
});
```


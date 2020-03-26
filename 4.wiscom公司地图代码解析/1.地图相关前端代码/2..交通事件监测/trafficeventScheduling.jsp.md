```JSP
<%--
  Created by IntelliJ IDEA.
  User: jz
  Date: 2018/10/26
  Time: 15:05
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<link rel="stylesheet" href="../../css/illegal/public.css" type="text/css"/>
<link rel="stylesheet" href="../../css/common3.0.css" type="text/css"/>
<link rel="stylesheet" href="../../css/illegal/search.css" type="text/css"/>
<link rel="stylesheet" href="../../css/illegal/myBootstrapTable.css" type="text/css"/>
<link rel="stylesheet" href="../../css/illegal/statisticalAnalysis.css" type="text/css"/>
<%--<link rel="stylesheet" href="../../css/illegal/myTree.css" type="text/css">--%>
<%--<link rel="stylesheet" href="../../css/trafficControl/trafficeventSchedulingTreeStyle.css">--%>

<link rel="stylesheet" href="../../css/trafficControl/trafficeventScheduling.css">
<link rel="stylesheet" href="../../website-js/illegal/optiscroll/optiscroll.css" type="text/css"/><%--滚动条样式（带有横向的滚动条）--%>
<link href="/css/map/commonMap.css" rel="stylesheet" type="text/css"/>

<style>
    .wrapper-center li input{
        background: white!important;
    }
    #prevStartTime,#prevEndTime,#nextStartTime,#nextEndTime{
        width: 150px!important;
        margin-left: 10px;
    }
    .tabbable-line {
        /*border-bottom: 2px solid rgba(19, 88, 192, 1);;*/
    }
    .detail{
        background: url(/images/map/detail.png) no-repeat;
        background-size: 18px;
        text-indent: 20px;
        text-decoration: none!important;
        float: right;
        width: auto;
        padding: 0;
        font-size: 14px;
        font-family: Microsoft YaHei;
        font-weight: bold;
        color: rgba(40,116,232,1);
        margin-left: 10px;
    }
    .change_position{
        background: url(/images/map/change-position.png) no-repeat;
        background-size: 18px;
        text-indent: 20px;
        text-decoration: none!important;
        float: right;
        width: auto;
        padding: 0;
        font-size: 14px;
        font-family: Microsoft YaHei;
        font-weight: bold;
        color: rgba(40,116,232,1);
        margin-left: 10px;
    }
    .titlePane .title{
        line-height: 30px;
        font-size: 16px;
        font-family: Microsoft YaHei;
        font-weight: bold;
        color: rgba(76,76,76,1);
    }
    .esriPopup .titleButton.close{
        background-size: contain !important;
    }
    .btnCommandTask {
        background: rgba(19, 88, 192, 1);
    }

    .page-container-inner {
        padding: 0;
    }

    .wrapper-center {
        height: 140px;
    }

    .li_width {
        width: 32%;
    }

    .hW100 {
        width: 100%;
        height: 100%;
    }

    #dispatchListUl li {
        display: inline-block;
        list-style: none;
        height: 300px;
    }

    #trafic .traficCount-select li {
        float: left;
        width: 90px;
        height: 30px;
        line-height: 30px;
        cursor: pointer;
        box-sizing: border-box;
        text-align: center;
        list-style: none !important;
    }
    .one{
        width: 370px !important;
    }
    #trafic .traficCount-select li a {
        display: inline-block;
        width: 100%;
        height: 100%;
        border: 1px solid rgba(0, 155, 252, 1);
        text-decoration: none;
    }

    .traficCount-select {
        text-align: center;
    }

    .traficCount-select ul {
        display: inline-block;
    }

    .traficCount-select li {
        float: left;
        width: 90px;
        height: 30px;
        line-height: 30px;
        cursor: pointer;
        box-sizing: border-box;
    }

    .traficCount-select li a {
        display: inline-block;
        width: 100%;
        height: 100%;
        border: 1px solid rgba(0, 155, 252, 1);
        text-decoration: none;
    }

    a.addSelect {
        background: rgba(0, 155, 252, 1);
        color: white;
    }

    #optimize .span1 {
        color: #666666 !important;
    }

    #optimize .wrapper-center li select {
        background: white !important;
    }

    #optimize .wrapper-center li input {
        background: white !important;
    }

    #optimize .wrapper-center li .select2-selection--single {
        background: white !important;
    }

    #dispatchListUl li:nth-child(2n) {
        position: relative;
        height: 150px;
        /*top:-50px;*/
        z-index: 99;
    }

    #dispatchListUl li:nth-child(2n + 1) {
        position: relative;
        /*top: -50px;*/
        height: 90px;
        z-index: 99;
    }

    #dispatchListUl li {
        text-align: center;
    }

    #gdInfo .span1 {
        color: #666666 !important;
    }

    #gdInfo li select {
        background: white !important;
    }

    #gdInfo li input {
        background: white !important;
    }

    #gdInfo li .select2-selection--single {
        background: white !important;
    }

    #sjlxTjfx + span {
        width: 140px;
        display: inline-block;
        margin-bottom: 3px;
    }

    .importantLeftBottom {
        height: calc(100% - 40px);
    }

    ._1 {
        background-image: url("../../images/trafficEvent/event/jtsg.png") !important;
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }

    ._1bg {
        background-image: url("../../images/trafficEvent/event/jtsg0.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }

    ._2 {
        background-image: url("../../images/trafficEvent/event/zrzh.png") !important;
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }

    ._2bg {
        background-image: url("../../images/trafficEvent/event/zrzh0.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }

    ._3 {
        background-image: url("../../images/trafficEvent/event/eltq.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }

    ._3bg {
        background-image: url("../../images/trafficEvent/event/eltq0.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }

    ._4 {
        background-image: url("../../images/trafficEvent/event/yzwfxx.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }

    ._4bg {
        background-image: url("../../images/trafficEvent/event/yzwfxx0.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }

    ._5 {
        background-image: url("../../images/trafficEvent/event/lmsh.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }

    ._5bg {
        background-image: url("../../images/trafficEvent/event/lmsh0.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }

    ._6 {
        background-image: url("../../images/trafficEvent/event/jtyd.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }

    ._6bg {
        background-image: url("../../images/trafficEvent/event/jtyd0.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }

    ._7 {
        background-image: url("../../images/trafficEvent/event/hwsl.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }

    ._7bg {
        background-image: url("../../images/trafficEvent/event/hwsl0.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }

    ._20 {
        background-image: url("../../images/trafficEvent/event/tssj.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }

    ._20bg {
        background-image: url("../../images/trafficEvent/event/tssj0.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }

    ._99 {
        background-image: url("../../images/trafficEvent/event/qtsj.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }

    ._990 {
        background-image: url("../../images/trafficEvent/event/qtsj0.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }
    ._990 {
        background-image: url("../../images/trafficEvent/event/qtsj0.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }
    ._101 {
          background-image: url("../../images/trafficEvent/event/101.png");
          background-position: right bottom;
          background-repeat: no-repeat;
          background-origin: content-box;
      }
    ._102 {
        background-image: url("../../images/trafficEvent/event/102.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }
    ._103 {
        background-image: url("../../images/trafficEvent/event/103.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }
    ._104 {
        background-image: url("../../images/trafficEvent/event/104.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }
    ._105 {
        background-image: url("../../images/trafficEvent/event/105.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }
    ._401 {
        background-image: url("../../images/trafficEvent/event/401.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }
    ._402 {
        background-image: url("../../images/trafficEvent/event/402.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }
    ._403 {
        background-image: url("../../images/trafficEvent/event/403.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }
    ._404 {
        background-image: url("../../images/trafficEvent/event/404.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }
    ._405 {
        background-image: url("../../images/trafficEvent/event/405.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }
    ._406 {
        background-image: url("../../images/trafficEvent/event/406.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }
    ._407 {
        background-image: url("../../images/trafficEvent/event/407.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }
    ._408 {
        background-image: url("../../images/trafficEvent/event/408.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }
    ._2001 {
        background-image: url("../../images/trafficEvent/event/2001.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }
    ._2002 {
        background-image: url("../../images/trafficEvent/event/2002.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }
    ._2003 {
        background-image: url("../../images/trafficEvent/event/2003.png");
        background-position: right bottom;
        background-repeat: no-repeat;
        background-origin: content-box;
    }
    #tab_808_3 .tabbable-line > .nav-tabs > li.active a {
        border-bottom: 2px solid rgba(0, 246, 255, 1);
        color: rgba(0, 246, 255, 1);
    }

    #tab_808_3 .tabbable-line > .nav-tabs > li {
        float: left;
    }

    .nav-tabs > li > a {
        font-size: 14px !important;
    }

    #pointTab .tabbable-line {
        left: calc(100% - 200px) !important;
    }

    #timeTab {
        /*padding-bottom: 0!important;*/
    }

    /*隐藏地图弹窗的小三角标志，鹰眼图，缩放标志*/
    #schedulingCommandMap .esriPopup .pointer .top,
    #schedulingCommandMap .esriPopup .pointer .left,
    #schedulingCommandMap .esriPopup .pointer .right,
    #schedulingCommandMap .esriPopup .pointer .bottom,
    #schedulingCommandMap .esriPopup .pointer .topRight,
    #schedulingCommandMap .esriPopup .pointer .topLeft,
    #schedulingCommandMap .esriPopup .pointer .bottomRight,
    #schedulingCommandMap .esriPopup .pointer .bottomLeft,
    #schedulingCommandMap .esriPopup .pointer,
    #schedulingCommandMap .esriPopup .outerPointer,
    #schedulingCommandMap .esriPopup .maximize,
    #schedulingCommandMap .esriOverviewMap,
    #schedulingCommandMap .esriPopupWrapper .actionsPane,
    #schedulingCommandMap #schedulingCommandMap_zoom_slider {
        display: none !important;
    }

    /*将弹窗背景色设为白色*/
    #schedulingCommandMap .esriPopupWrapper,
    #schedulingCommandMap .contentPane,
    #schedulingCommandMap .titlePane,
    #schedulingCommandMap .sizer {
        background: white !important;
    }

    /*给弹窗标题文字统一样式*/
    #schedulingCommandMap .esriPopupWrapper .title {
        margin-top: 8px;
        font-size: 18px;
        font-family: Microsoft YaHei;
        font-weight: bold;
        color: rgba(76, 76, 76, 1);
    }

    /*弹窗下边缘圆角*/
    #schedulingCommandMap .esriPopupWrapper .sizer.content,
    #schedulingCommandMap .esriPopupWrapper .contentPane {
        border-bottom-right-radius: 5px !important;
        border-bottom-left-radius: 5px !important;
    }

    /*弹窗宽度自适应*/
    #schedulingCommandMap .esriPopupWrapper .sizer {
        width: unset !important;
        min-width: 280px !important;
    }

    /*弹窗基本的几种文字*/
    #schedulingCommandMap .normalFont {
        font-size: 14px;
        font-family: Microsoft YaHei;
        font-weight: 400;
        color: rgba(51, 51, 51, 1);
    }

    #schedulingCommandMap .normalBoldFont {
        font-size: 14px;
        font-family: Microsoft YaHei;
        font-weight: bold;
    }

    #schedulingCommandMap .bigFont {
        font-size: 16px;
        font-family: Microsoft YaHei;
        font-weight: bold;
        color: rgba(0, 0, 0, 1);
    }

    #schedulingCommandMap img{
        margin-right: 5px;
    }

    #schedulingCommandMap .windowRow {
        display: flex;
        align-items: center;
    }

    .btnSideSwitch {
        position: absolute;
        width: 30px;
        height: 30px;
        top: 0px;
        right: 0px;
        z-index: 1;
        cursor: pointer;
    }

    .btnSideSwitch.open1 {
        background-image: url("../../images/trafficControl/line.png");
    }
    .fixed-table-toolbar{
        display: none;
    }

    #reportoptimize .span1 {
        color: black;
    }

    .infowindow_btn {
        cursor: pointer;
    }

    #text_cancelEvent {
        height: 100px;
        width: 100%;
        margin-top: 16px;
        margin-bottom: -14px;
        color: black;
    }
    #eventSearchTable .slimScrollDiv {
        background: rgb(38,52,75);
    }
    #schedulingCommandMap_root .esriPopup .contentPane {
        padding: 10px 6px 6px 10px!important;
    }
    .wrapper-center li select{
        background: white!important;
    }
    ul li .select2-container .select2-selection{
        background: white!important;
    }
</style>
<link rel="stylesheet" href="../../css/trafficControl/videoRecordingControl.css">
<link rel="stylesheet" href="../../css/trafficControl/liveVideoControl.css">
<%--查看历史录像弹框--%>
<div id="cameraHistoryVideoModal" class="modal fade" tabindex="-1" data-width="75%" data-height="65%">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">历史视频</h4>
    </div>
    <div class="modal-body" style="padding: 0">
        <div style="z-index:100;overflow: hidden;width: 100%">
            <div>
                <form class="form-horizontal" role="form">
                    <div class="form-body">
                        <div class="row">
                            <div class="col-md-10">
                                <div class="form-group">
                                    <label class="control-label col-md-2" style="text-align: center">时间</label>
                                    <div class="col-md-4">
                                        <input type="text" class="form-control" id="startTime"/>
                                    </div>
                                    <label class="control-label col-md-1" style="text-align: center">~</label>
                                    <div class="col-md-4">
                                        <input type="text" class="form-control" id="endTime"/>
                                    </div>
                                </div>
                            </div>
                            <div class="col-md-2">
                                <div class="form-group" style="text-align: left">
                                    <div class="col-md-12">
                                        <span class="btnSpanYes" id="trafficeventSchedulingVideoPlayBackSearch">查询</span>
                                    </div>
                                </div>
                            </div>
                        </div>

                    </div>
                </form>
            </div>

            <div class="videoPlayMonitor videoObjectHave" style="width:100%;background: #0B1D30;" id="trafficeventSchedulingVideoPlayBack">
                <object style="padding:0" id='videoControlPlayBack' TYPE='application/xhanhan-activex'
                        CLSID='{AFBF5262-3FC2-4285-B016-56E7E2454AC2}' class='col-md-12'
                        event_NotifyMouseCtrl="ocxNotifyMouseCtrlPlayBack"
                        data='data:application/x-oleobject;base64,YlK/r8I/hUKwFlbn4kVKwgAAAQAVYgAAVTUAAAAAAAA='></object>
                <%--<a style="font-size:30px;color:#fff;" href="templates/firefoxAndplugins.rar">请使用IE9或指定版本的火狐浏览器,并下载视频播放插件...</a>--%>
            </div>
            <div class="video_bottom_control" style="width:100%">
                <div class="videoColumnSeting">
                    <div id="changeWinNum" class="bottom_left">
                        <div data-win="1" class="bottom_left_select addPic"></div>
                        <div data-win="4" class="bottom_left_select"></div>
                        <div data-win="6" class="bottom_left_select"></div>
                        <div data-win="8" class="bottom_left_select"></div>
                        <div data-win="9" class="bottom_left_select"></div>
                        <div data-win="10" class="bottom_left_select"></div>
                        <div data-win="13" class="bottom_left_select"></div>
                        <div data-win="16" class="bottom_left_select"></div>
                    </div>
                    <%--<div class="bottom_center" style="color: grey">--%>
                    <%--当前无用户控制相机--%>
                    <%--</div>--%>
                    <div class="bottom_right">
                        <div title="慢放" id="playSlow" class="bottom_right_select" style="background-image: url('/images/videoManger/pic2/video_next.png');background-size: cover;width:25px;height:25px"></div>
                        <div title="暂停" id="supPlay" class="bottom_right_select videoOff" style="background-size: cover;width:25px;height:25px"></div>
                        <div title="快放" id="playFast" class="bottom_right_select" style="background-image: url('/images/videoManger/pic2/video_prev.png');background-size: cover;width:25px;height:25px"></div>
                        <div title="关闭" id="closeCurrent"  class="bottom_right_select" style="background-image: url('/images/ITS/videoManger/pic3/6.png');background-size: cover;width:25px;height:25px"></div>
                        <div title="关闭全部" id="closeAllWins"  class="bottom_right_select" style="background-image: url('/images/ITS/videoManger/pic3/0.png');background-size: cover;width:25px;height:25px"></div>
                        <div title="切图" onclick="proPic()" class="bottom_right_select" style="display:none;background-image: url('/images/ITS/videoManger/pic3/8.png');background-size: cover;width:25px;height:25px"></div>
                        <div title="音量" class="bottom_right_select" style="background-image: url('/images/ITS/videoManger/pic3/4.png');background-size: cover;width:25px;height:25px"></div>
                        <%--<div title="设置" class="bottom_right_select" style="background-image: url('/images/ITS/videoManger/pic3/001.png');background-size: cover;width:25px;height:25px"></div>--%>
                        <div title="事件" class="bottom_right_select thingReportPlayBack" style="display:none;background-image: url('/images/ITS/videoManger/pic3/9.png');background-size: cover;width:25px;height:25px"></div>
                        <div title="违法" class="bottom_right_select illegalReportPlayBack" style="display:none;background-image: url('/images/ITS/videoManger/pic3/11.png');background-size: cover;width:25px;height:25px"></div>
                        <div id="searchTagBtn3" title="添加标签" class="bottom_right_select" style="display:none;background-image: url('/images/videoManger/pic2/video_last.png');background-size: cover;width:25px;height:25px"></div>
                    </div>
                    <div style="clear:both"></div>
                </div>
                <div class="video_progress show" style="height:145px;margin-top: 0px;background: black;position: relative">
                    <div class="search-result-box" id="dragDiv" style="width: 7200px;height: 80px;" ></div>
                </div>
            </div>
        </div>
    </div>
    <div class="modal-footer"></div>
</div>

<%--实况视频--%>
<div id="eventLiveVideoModal" class="modal fade" tabindex="-1" data-width="75%" data-height="65%">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title"></h4>
    </div>
    <div class="modal-body" style="padding: 0">
        <div style="z-index:100;overflow: hidden;width: 100%">
            <div class="videoPlayMonitor videoObjectHave" style="width:100%;background: #0B1D30;" id="oneMapVideoPanel">
                <object style="padding:0" id='oneMapVideoControl' TYPE='application/xhanhan-activex'
                        CLSID='{AFBF5262-3FC2-4285-B016-56E7E2454AC2}' class='col-md-12'
                        event_NotifyMouseCtrl="ocxNotifyMouseCtrl"
                        data='data:application/x-oleobject;base64,YlK/r8I/hUKwFlbn4kVKwgAAAQAVYgAAVTUAAAAAAAA='></object>
            </div>
            <div class="video_bottom_control" style="width:100%">
                <div class="videoColumnSeting">
                    <div class="bottom_left">
                        <div data-win="1" class="bottom_left_select addPic"></div>
                        <div data-win="4" class="bottom_left_select"></div>
                        <div data-win="6" class="bottom_left_select"></div>
                        <div data-win="8" class="bottom_left_select"></div>
                        <div data-win="9" class="bottom_left_select"></div>
                        <div data-win="10" class="bottom_left_select"></div>
                        <div data-win="13" class="bottom_left_select"></div>
                        <div data-win="16" class="bottom_left_select"></div>
                    </div>
                    <%--<div class="bottom_center" style="color: grey">--%>
                    <%--当前无用户控制相机--%>
                    <%--</div>--%>
                    <div class="bottom_right" style="display: none">
<%--                        <div title="慢放" id="playSlow" class="bottom_right_select" style="background-image: url('/images/videoManger/pic2/video_next.png');background-size: cover;width:25px;height:25px"></div>--%>
<%--                        <div title="暂停" id="supPlay" class="bottom_right_select videoOff" style="background-size: cover;width:25px;height:25px"></div>--%>
<%--                        <div title="快放" id="playFast" class="bottom_right_select" style="background-image: url('/images/videoManger/pic2/video_prev.png');background-size: cover;width:25px;height:25px"></div>--%>
<%--                        <div title="关闭" id="closeCurrent"  class="bottom_right_select" style="background-image: url('/images/ITS/videoManger/pic3/6.png');background-size: cover;width:25px;height:25px"></div>--%>
<%--                        <div title="关闭全部" id="closeAllWins"  class="bottom_right_select" style="background-image: url('/images/ITS/videoManger/pic3/0.png');background-size: cover;width:25px;height:25px"></div>--%>
<%--                        <div title="切图" onclick="proPic()" class="bottom_right_select" style="display:none;background-image: url('/images/ITS/videoManger/pic3/8.png');background-size: cover;width:25px;height:25px"></div>--%>
<%--                        <div title="音量" class="bottom_right_select" style="background-image: url('/images/ITS/videoManger/pic3/4.png');background-size: cover;width:25px;height:25px"></div>--%>
                        <%--<div title="设置" class="bottom_right_select" style="background-image: url('/images/ITS/videoManger/pic3/001.png');background-size: cover;width:25px;height:25px"></div>--%>
<%--                        <div title="事件" class="bottom_right_select thingReportPlayBack" style="display:none;background-image: url('/images/ITS/videoManger/pic3/9.png');background-size: cover;width:25px;height:25px"></div>--%>
<%--                        <div title="违法" class="bottom_right_select illegalReportPlayBack" style="display:none;background-image: url('/images/ITS/videoManger/pic3/11.png');background-size: cover;width:25px;height:25px"></div>--%>
<%--                        <div id="searchTagBtn3" title="添加标签" class="bottom_right_select" style="display:none;background-image: url('/images/videoManger/pic2/video_last.png');background-size: cover;width:25px;height:25px"></div>--%>
                    </div>
                    <div style="clear:both"></div>
                </div>
                <%--<div class="video_progress show" style="height:145px;margin-top: 0px;background: black;position: relative">
                    <div class="search-result-box" id="dragDiv" style="width: 7200px;height: 80px;" ></div>
                </div>--%>
            </div>
        </div>
    </div>
    <div class="modal-footer"></div>
</div>

<%--页面打开时弹窗--%>
<div id="event_handling" class="modal fade" tabindex="-1" data-width="800" data-height="400">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">提示</h4>
    </div>
    <div class="modal-body">
        <div class="warningInformation">您还有一些事件未处置完毕，请选择一条继续或忽略。</div>
        <div class="table-portlet whiteTable">
            <table id="trafficeventSchedulingWarningTable"  class="blue-table"></table>
        </div>
    </div>
    <div class="modal-footer">
        <button type="button" class="btn_cancel" data-dismiss="modal">忽略</button>
    </div>
</div>
<%--路径栏--%>
<div class="tabbable-line myHeight ">
    <ul class="nav nav-tabs">
        <li style="float: left;height: 20px;margin-top: 16px;">
            <img src="/images/trafficImgage/bread.png">
            <span style="color: white">交通组织与管控</span>
            <span class="imgArrow"></span>
            <span class="font-blue">交通事件监测</span>
        </li>
        <li>
            <a href="#tab_808_3" class="height47" data-toggle="tab" style="padding: 0;margin-right: 4px;" isloaded="no">
                <div class="nav-main">
                    <div>
                        统计分析
                    </div>
                </div>
            </a>
        </li>
        <li>
            <a href="#tab_808_2" class="height47" data-toggle="tab" style="padding: 0;margin-right: 4px;" isloaded="no">
                <div class="nav-main">
                    <div>
                        事件查询
                    </div>
                </div>

            </a>
        </li>
        <li class="active">
            <a href="#tab_808_1" class="height47" data-toggle="tab" style="padding: 0;margin-right: 4px;"
               isloaded="yes">
                <div class="nav-main" id="btnCommandTask">
                    <div>
                        指挥调度
                    </div>
                </div>
            </a>
        </li>
    </ul>
    <div class="tab-content myHeight2" style="padding-top: 0">
        <div class="tab-pane active" id="tab_808_1">
            <style>
                .topInfoWindow {
                    display: flex;
                    height: 200px;
                    border-bottom: 1px solid transparent;
                    border-image: url(../../images/trafficControl/line_solid.png) 10;
                }

                #schedulingCommandMapBox input[type='checkbox'] {
                    display: none;
                }

                #schedulingCommandMapBox input[type='checkbox'] + label {
                    width: 12px;
                    height: 12px;
                    -webkit-appearance: none;
                    -moz-appearance: none;
                    background-color: transparent;
                    border: 1px solid rgba(160, 160, 160, 1);
                    border-radius: 2px;
                    margin-right: 5px;
                    margin-bottom: 0px;
                    cursor: pointer;
                }

                #schedulingCommandMapBox input[type='checkbox']:checked + label {
                    background-image: url("../../images/illegal/whiteSelected.png");
                    background-repeat: no-repeat;
                    background-size: 10px;
                }

                .infoRow {
                    display: flex;
                    align-items: center;
                    justify-content: space-between;
                }

                .topInfoWindow .infoRow {
                    height: 20%;
                    line-height: 20px;
                    border-bottom: 1px solid transparent;
                    border-image: url(../../images/trafficControl/line_dashed.png) 10;
                    padding-right: 35px;
                    padding-left: 20px;
                }

                #eventLabelBox .infoRow {
                    height: 30px;
                }

                .eventLabel {
                    display: flex;
                    align-items: center;
                    font-size: 14px;
                    font-family: Microsoft YaHei;
                    font-weight: 400;
                    color: rgba(255, 255, 255, 1);
                    height: 140px;
                    background: rgba(58, 152, 218, 0.1);
                    position: relative;
                    padding: 10px;
                }

                .eventLabel:hover {
                    background: rgba(30, 70, 139, 0.7);
                }

                .eventLabel .eventCornerIcon {
                    position: absolute;
                    width: 20px;
                    height: 20px;
                    top: 0px;
                    left: 0px;
                }

                .eventLabel .eventIcon {
                    width: 80px;
                    height: 80px;
                    margin-right: 15px;
                }

                .attention0 {
                    display: flex;
                    align-items: center;
                    justify-content: center;
                    width: 68px;
                    height: 30px;
                    cursor: pointer;
                    border: 1px solid rgba(253, 211, 44, 1);
                    border-radius: 17px;
                    font-weight: bold;
                    color: rgba(253, 211, 44, 1);
                }

                .attention1 {
                    display: flex;
                    align-items: center;
                    justify-content: center;
                    width: 68px;
                    height: 30px;
                    cursor: pointer;
                    border: 1px solid rgba(253, 211, 44, 1);
                    background: rgba(253, 211, 44, 1);
                    border-radius: 17px;
                    font-weight: bold;
                    color: white;
                }

                .resultOption {
                    display: flex;
                    align-items: center;
                    position: relative;
                    cursor: pointer;
                }

                .resultOption.active {
                    background: rgba(41, 41, 41, 0.1);
                }

                .resultOption span {
                    text-align: center;
                    line-height: 20px;
                    float: left;
                    vertical-align: middle;
                    display: block;
                    width: 20px;
                    height: 26px;
                    background-image: url(/images/ITS/videoManger/serial.png);
                    background-size: 100% 100%;
                    color: #fff;
                    margin-left: 7px;
                    margin-right: 7px;
                }

                .resultOption img {
                    position: absolute;
                    top: calc(50% - 8px);
                    right: 7px;
                }

                .changePage {
                    width: 30px;
                    height: 30px;
                    cursor: pointer;
                }

                .changePage.prev {
                    background: url(../../images/oneMap/paging_btn_left.png) round;
                }

                .changePage.prev.forbid {
                    background: url(../../images/oneMap/paging_btn_left_active.png) round;
                }

                .changePage.next {
                    background: url(../../images/oneMap/paging_btn_right.png) round;
                }

                .changePage.next.forbid {
                    background: url(../../images/oneMap/paging_btn_right_active.png) round;
                }

                .pathSearchResult {
                    -moz-user-select: none;
                    user-select: none;
                    -webkit-user-select: none;
                }

                .pathSearchResult.pathPage {
                    display: flex;
                }

                #searchResultPage.pathSearchResult {
                    display: flex;
                    align-items: center;
                    height: 43px;
                    justify-content: space-between;
                    padding-left: 7px;
                    padding-right: 7px;
                }

                .btnShowLeftInfo{
                    position: absolute;
                    z-index:1;
                    top:15px;
                    left:10px;
                    width:80px;
                    height:80px;
                    background:rgba(0,155,252,1);
                    border:4px solid rgba(0, 0, 0, 0.2);
                    border-radius:39px;
                    display: none;
                    align-items: center;
                    justify-content: center;
                    font-size:14px;
                    font-family:Microsoft YaHei;
                    font-weight:400;
                    color:rgba(255,255,255,1);
                    cursor: pointer;
                }

                .searchInputBox{
                    position: absolute;
                    box-shadow:0px 3px 6px 0px rgba(0, 0, 0, 0.3);
                    width: 300px;
                    z-index: 1;
                    left: 15px;
                    top: 15px;
                    background:linear-gradient(0deg,rgba(244,244,244,1),rgba(255,255,255,1));
                }

                .realTimeWarning{
                    position: absolute;
                    background: red;
                    min-width: 25px;
                    height: 25px;
                    border-radius: 12.5px;
                    display: flex;
                    justify-content: center;
                    align-items: center;
                    padding-left: 5px;
                    padding-right: 5px;
                    top: 0px;
                    right: -8px;
                    font-size:14px;
                }

                #schedulingCommandMap .esriPopup .close{
                    height: 12px;
                    background-position: center !important;
                    margin-top: -5px;
                }

                /*地图工具条*/
                .map-toolbarBox {
                    position: absolute;
                    z-index: 2;
                    right: 15px;
                    top:15px;
                    padding: 0px;
                    list-style: none;
                    display: flex;
                    background: white;
                    box-shadow: 2px 2px 10px 0px rgba(0, 0, 0, 0.8);
                    border-radius: 3px;
                    padding-top: 5px;
                    padding-bottom: 5px;
                }

                .map-toolbarBox > li {
                    border-right: 2px solid rgba(145, 145, 145, 1);
                    height: 27px;
                    width: 55px;
                    display: flex;
                    justify-content: center;
                    align-items: center;
                    cursor: pointer;
                }

                .map-toolbarBox > li i {
                    width: 25px;
                    height: 25px;
                    background-size: contain;
                    background-repeat: no-repeat;
                    background-position: center;
                    display: block;
                    position:unset;
                    margin: 0px;
                    padding: 0px;
                }

                .map-toolbarBox > li span.arrow-close{
                    width: 9px;
                    height: 9px;
                    border-bottom: 4.5px solid;
                    border-left: 4.5px solid rgba(255,255,255,1);
                    border-right: 4.5px solid rgba(255,255,255,1);
                    border-top: 4.5px solid rgba(255,255,255,1);
                    margin-top: -4.5px;
                    margin-left: 3px;
                }

                .map-toolbarBox > li span.arrow-open{
                    width: 9px;
                    height: 9px;
                    border-bottom: 4.5px solid rgba(255,255,255,1);
                    border-left: 4.5px solid rgba(255,255,255,1);
                    border-right: 4.5px solid rgba(255,255,255,1);
                    border-top: 4.5px solid;
                    margin-bottom: -4.5px;
                    margin-left: 3px;
                }

                .map-toolbarBox .clearDraw i {
                    background-image: url(../../images/roadInfo/toolbar-black/toolbar-move-black.png);
                }

                .map-toolbarBox .regainMap i {
                    background-image: url(../../images/roadInfo/toolbar-black/toolbar-all-map-black.png);
                }

                .map-toolbarBox .showImage i {
                    background-image: url(../../images/roadInfo/toolbar-black/toolbar-picture-black.png);

                }

                .map-toolbarBox .ranging i {
                    background-image: url(../../images/roadInfo/toolbar-black/toolbar-shape-black.png);

                }

                .map-toolbarBox .layerChoose i {
                    background-image: url(../../images/roadInfo/toolbar-black/toolbar-coverage-black.png);
                }

                .map-toolbarBox .drop-down-box{
                    position: absolute;
                    top: calc( 100% + 25px);
                    background: white;
                    border-radius: 3px;
                    box-shadow: 2px 2px 10px 0px rgba(0, 0, 0, 0.8);
                    min-width: 110px;
                    right: 0px;
                    display: none;
                }

                .map-toolbarBox .drop-down-box.active{
                    display:unset;
                }

                .map-toolbarBox .drop-down-box .arrow-up{
                    width: 25px;
                    height: 12px;
                    border-bottom: 12px solid rgba(255,255,255,1);
                    border-left: 12px solid rgba(255,255,255,0);
                    border-right: 12px solid rgba(255,255,255,0);
                    position: absolute;
                    top: -12px;
                    right: 24px;
                }

                .map-toolbarBox .drop-down-box .second-tool-bar{
                    list-style: unset;
                    padding: 0px;
                }

                .map-toolbarBox .drop-down-box .second-tool-bar>li{
                    min-height: 28px;
                    display: flex;
                    justify-content: left;
                    align-items: center;
                    padding-left: 10px;
                    padding-right: 10px;
                    cursor: pointer;
                }

                .map-toolbarBox .drop-down-box .second-tool-bar>li:hover {
                    background: rgba(142,142,142,0.8);
                    color: white;
                }

                .map-toolbarBox .drop-down-box .second-tool-bar > li .checkBox{
                    display: inline-block;
                    width: 13px;
                    height: 13px;
                    margin-right: 10px;
                    background: unset;
                    border: 1px solid;
                    border-radius: 3px;
                }

                .map-toolbarBox .drop-down-box .second-tool-bar > li .checkBox.active{
                    display: inline-block;
                    width: 13px;
                    height: 13px;
                    margin-right: 10px;
                    background-image: url("../../images/illegal/blackSelected.png");
                    border: 1px solid;
                    border-radius: 3px;
                }
                .timeActive{
                    color: white!important;
                }
                #sjlxTjfx + .slimScrollDiv {
                    display: inline-block;
                    margin-bottom: -10px;
                }
                #crosssMore .fixed-table-pagination .page-number {
                    display: none;
                }
            </style>
            <div id="schedulingCommandMapBox" style="display: flex;">
                <div id="topInfoWindow"
                     style="height: 100%;width: 400px;background: rgba(29,50,81,1);color: white;font-size: 14px;font-family: Microsoft YaHei;font-weight: 400;color: rgba(255,255,255,1);position: relative;">
                    <div class="btnSideSwitch open1"></div>
                    <div class="topInfoWindow">
                        <div style="width: 150px;padding-left: 25px;">
                            <div style="display:flex;align-items:center;height: 20%;font-size: 16px;font-weight: bold;">
                                实时预警
                            </div>
                            <div style="display:flex;align-items:baseline;height: 26%;font-size: 12px;"><span
                                    id="realTimeCount"
                                    style="font-size: 30px;line-height: 52px;margin-right: 15px"></span>次
                            </div>
                            <div style="display:flex;align-items:center;height: 18%;line-height: 20px;"><input
                                    id="chk_hasAttention" name="chk_attentionType" value="0" type="checkbox"
                                    checked/><label for="chk_hasAttention" style="margin-bottom: 0px;"/><label
                                    for="chk_hasAttention" style="margin-bottom: 0px;cursor:pointer;">未关注</label></div>
                            <div style="display:flex;align-items:center;height: 18%;line-height: 20px;"><input
                                    id="chk_hasNoAttention" name="chk_attentionType" value="1" type="checkbox" checked/><label
                                    for="chk_hasNoAttention" style="margin-bottom: 0px;"/><label
                                    for="chk_hasNoAttention" style="margin-bottom: 0px;cursor:pointer;">已关注</label>
                            </div>
                            <div style="display:flex;align-items:center;height: 18%;font-size:12px;color:rgba(210,210,210,1);">
                                更新时间：<span id="lastUpdateTime"></span></div>
                        </div>
                        <div style="width: 250px;">
                            <div class="infoRow">
                                <input id="chk_noDisposed" name="chk_eventType" value="0" type="checkbox"
                                       checked/><label for="chk_noDisposed"/>
                                <label for="chk_noDisposed"
                                       style="width: 60px;margin-bottom: 0px;cursor:pointer;">未处置</label>
                                <span id="num_noDisposed"
                                      style="width: 117px;text-align: end;padding-right: 40px;"></span>
                                <label style="width: 10px;height: 10px;margin-bottom: 0px;background: rgba(253,74,44,1);"/>
                            </div>
                            <div class="infoRow">
                                <input id="chk_scheduling" name="chk_eventType" value="3" type="checkbox"
                                       checked/><label for="chk_scheduling"/>
                                <label for="chk_scheduling"
                                       style="width: 60px;margin-bottom: 0px;cursor:pointer;">调度中</label>
                                <span id="num_scheduling"
                                      style="width: 117px;text-align: end;padding-right: 40px;"></span>
                                <label style="width: 10px;height: 10px;margin-bottom: 0px;background: rgba(253,211,44,1);"/>
                            </div>
                            <div class="infoRow">
                                <input id="chk_scheduled" name="chk_eventType" value="4" type="checkbox" checked/><label
                                    for="chk_scheduled"/>
                                <label for="chk_scheduled"
                                       style="width: 60px;margin-bottom: 0px;cursor:pointer;">调度完毕</label>
                                <span id="num_scheduled"
                                      style="width: 117px;text-align: end;padding-right: 40px;"></span>
                                <label style="width: 10px;height: 10px;margin-bottom: 0px;background: rgba(172,253,44,1);"/>
                            </div>
                            <div class="infoRow">
                                <input id="chk_canceled" name="chk_eventType" value="2" type="checkbox" checked/><label
                                    for="chk_canceled"/>
                                <label for="chk_canceled"
                                       style="width: 60px;margin-bottom: 0px;cursor:pointer;">已自撤</label>
                                <span id="num_canceled"
                                      style="width: 117px;text-align: end;padding-right: 40px;"></span>
                                <label style="width: 10px;height: 10px;margin-bottom: 0px;background: rgba(44,231,253,1);"/>
                            </div>
                            <div class="infoRow" style="border-bottom:unset;">
                                <input id="chk_closed" name="chk_eventType" value="1" type="checkbox" checked/><label
                                    for="chk_closed"/>
                                <label for="chk_closed"
                                       style="width: 60px;margin-bottom: 0px;cursor:pointer;">已关闭</label>
                                <span id="num_closed" style="width: 117px;text-align: end;padding-right: 40px;"></span>
                                <label style="width: 10px;height: 10px;margin-bottom: 0px;background: rgba(181,181,181,1);"/>
                            </div>
                        </div>
                    </div>
                    <div id="eventLabelBoxBase" style="height: calc(100% - 200px);padding: 15px;"></div>
                </div>
                <%--左侧-调度设置 序号 标题--%>
                <div id="dispatchSettingBox" style="display: none">
                  <div id="dispatchSettingLeft">
                      <div class="dispatchSettingCon">
                          <div class="dispatchSettingWire"></div>
                          <div class="dispatchSettingListBox">
                              <ul id="dispatchSettingListNum">
                                  <li>
                                      <span class="dispatchListTitleNum">1</span>
                                      <div class="dispatchListTitleBox">
                                          <span class="dispatchListTitleImg dispatchPolice"></span>
                                          <span class="dispatchListTitleName">派警</span>
                                      </div>
                                  </li>
                                  <li>
                                      <span class="dispatchListTitleNum">2</span>
                                      <div class="dispatchListTitleBox">
                                          <span class="dispatchListTitleImg guideIssue"></span>
                                          <span class="dispatchListTitleName">诱导发布</span>
                                      </div>
                                  </li>
                                  <li>
                                      <span class="dispatchListTitleNum">3</span>
                                      <div class="dispatchListTitleBox">
                                          <span class="dispatchListTitleImg signalIntervene"></span>
                                          <span class="dispatchListTitleName">信号发布</span>
                                      </div>
                                  </li>
                                  <li>
                                      <span class="dispatchListTitleNum">4</span>
                                      <div class="dispatchListTitleBox">
                                          <span class="dispatchListTitleImg equipmentImg"></span>
                                          <span class="dispatchListTitleName">装备设施</span>
                                      </div>
                                  </li>

                              </ul>
                          </div>
                      </div>
                  </div>
                  <div id="dispatchSettingBox_right">
                        <%--上tab页 联动部门 警力资源 信息发布--%>
                        <div class="dispatchSetting_tab_top">
                            <%--上tab栏 选择--%>
                            <div class="dispatchSetting_toolBar_top">
                                <ul class="dispatchSetting_tapBar">
                                    <li class="active"><a href="#linkageDepartmentBox" data-toggle="tab" type="联动部门">联动部门</a></li>
                                    <li class=""><a href="#policeResourcesBox" data-toggle="tab" type="警力资源">警力资源</a></li>
                                    <li class=""><a href="#messageIssueBox" data-toggle="tab" type="信息发布">信息发布</a></li>
                                </ul>
                            </div>
                            <%--上tab 内容--%>
                            <div class="tab-content">
                                <%--联动部门--%>
                                <div id="linkageDepartmentBox" class="tab-pane active">
                                        <%--事件周边范围选择--%>
                                        <div class="linkageDepartment_scope">
                                            <span>事件周边</span>
                                            <input type="text" oninput = "value=value.replace(/[^\d]/g,'')" autocomplete="off" maxlength="6" id="linkageDepartmentInput">
                                            <span>米</span>
                                            <span class="coordinateImg" id="linkageDepartmentInputImg"></span>
                                        </div>
                                        <%--联动部门内容列表--%>
                                        <div class="linkageDepartment_list_box">
                                            <ul>
                                                <%--<li>
                                                    <p>南京同仁医院</p>
                                                    <p>医疗</p>
                                                </li>
                                                <li>
                                                    <p>南京同仁医院</p>
                                                    <p>医疗</p>
                                                </li>
                                                <li>
                                                    <p>南京同仁医院</p>
                                                    <p>医疗</p>
                                                </li>
                                                <li>
                                                    <p>南京同仁医院</p>
                                                    <p>医疗</p>
                                                </li>
                                                <li>
                                                    <p>南京同仁医院</p>
                                                    <p>医疗</p>
                                                </li>
                                                <li>
                                                    <p>南京同仁医院</p>
                                                    <p>医疗</p>
                                                </li>--%>
                                            </ul>
                                        </div>
                                    <div class="linkageDepartment_list_affirm">
                                        <button class="btn blue button-submit" style="width: 100%">确定</button>
                                    </div>
                                </div>
                                <%--警力资源--%>
                                <div id="policeResourcesBox" class="tab-pane">
                                    <div class="linkageDepartment_scope">
                                        <span>事件周边</span>
                                        <input type="text" oninput = "value=value.replace(/[^\d]/g,'')" autocomplete="off" maxlength="6" id="policeResourcesInput">
                                        <span>米</span>
                                        <span class="coordinateImg" id="policeResourcesImg"></span>
                                    </div>
                                    <%--警力资源内容列表--%>
                                    <div class="policeResources_list_box">
                                        <ul>
                                            <%--<li>
                                                <p><span class="policeResources_name">某某某</span>
                                                    <span class="policeResources_num">123456</span>
                                                </p>
                                                <p class="police_on_line">在线</p>
                                            </li>
                                            <li>
                                                <p><span class="policeResources_name">某某某</span>
                                                    <span class="policeResources_num">123456</span>
                                                </p>
                                                <p class="police_off_line">掉线</p>
                                            </li>
                                            <li>
                                                <p><span class="policeResources_name">某某某</span>
                                                    <span class="policeResources_num">123456</span>
                                                </p>
                                                <p class="police_on_line">在线</p>
                                            </li>
                                            <li>
                                                <p><span class="policeResources_name">某某某</span>
                                                    <span class="policeResources_num">123456</span>
                                                </p>
                                                <p class="police_off_line">掉线</p>
                                            </li>
                                            <li>
                                                <p><span class="policeResources_name">某某某</span>
                                                    <span class="policeResources_num">123456</span>
                                                </p>
                                                <p class="police_off_line">掉线</p>
                                            </li>
                                            <li>
                                                <p><span class="policeResources_name">某某某</span>
                                                    <span class="policeResources_num">123456</span>
                                                </p>
                                                <p class="police_off_line">掉线</p>
                                            </li>--%>
                                        </ul>
                                    </div>
                                    <div class="policeResources_list_affirm">
                                        <label class="policeResources_all_label"><span></span>全选</label>
                                        <button id="dispatchedPoliceBtn" class="btn blue button-submit" data-toggle="modal">派警处置</button>
                                    </div>
                                </div>
                                <%--信息发布--%>
                                <div id="messageIssueBox" class="tab-pane">
                                    <div class="linkageDepartment_scope">
                                        <span>事件周边</span>
                                        <input type="text" oninput = "value=value.replace(/[^\d]/g,'')" autocomplete="off" maxlength="6" id="messageIssueInput">
                                        <span>米</span>
                                        <span class="coordinateImg" id="messageIssueImg"></span>
                                    </div>
                                    <div class="messageIssue_list_box">
                                        <ul>
                                            <%--<li>
                                                <p><span class="carrefour_name">诚信大道与将军大道</span>
                                                    <span class="carrefour_location_green"></span>
                                                </p>
                                                <p class="police_on_line">在线</p>
                                            </li>
                                            <li>
                                                <p><span class="carrefour_name">诚信大道与将军大道</span>
                                                    <span class="carrefour_location_red"></span>
                                                </p>
                                                <p class="police_off_line">掉线</p>
                                            </li>
                                            <li>
                                                <p><span class="carrefour_name">诚信大道与将军大道</span>
                                                    <span class="carrefour_location_green"></span>
                                                </p>
                                                <p class="police_on_line">在线</p>
                                            </li>
                                            <li>
                                                <p><span class="carrefour_name">诚信大道与将军大道</span>
                                                    <span class="carrefour_location_red"></span>
                                                </p>
                                                <p class="police_off_line">掉线</p>
                                            </li>
                                            <li>
                                                <p><span class="carrefour_name">诚信大道与将军大道</span>
                                                    <span class="carrefour_location_red"></span>
                                                </p>
                                                <p class="police_off_line">掉线</p>
                                            </li>
                                            <li>
                                                <p><span class="carrefour_name">诚信大道与将军大道</span>
                                                    <span class="carrefour_location_red"></span>
                                                </p>
                                                <p class="police_off_line">掉线</p>
                                            </li>--%>
                                        </ul>
                                    </div>
                                    <div class="messageIssue_list_affirm">
                                        <label class="policeResources_all_label"><span class=""></span>全选</label>
                                        <button class="btn blue button-submit" id="setIssueContent_btn" data-toggle="modal">插播消息</button>
                                        <button class="stop_btn">手动结束</button>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <%--下tab页 专家组 装备设施--%>
                        <div class="dispatchSetting_tab_bottom">
                            <%--下tab栏 选择--%>
                            <div class="dispatchSetting_toolbar_bottom">
                                <ul class="dispatchSetting_tapBar">
                                    <li class="active"><a href="#proficientStaffBox" data-toggle="tab" type="专家组">专家组</a></li>
                                    <li class=""><a href="#equipFacilityBox" data-toggle="tab" type="装备设施">装备设施</a></li>
                                </ul>
                            </div>
                            <%--下tab 内容--%>
                            <div class="tab-content">
                                <%--专家组列表--%>
                                <div id="proficientStaffBox" class="tab-pane active">
                                    <div class="proficientStaff_content">
                                        <ul>
<%--                                            <li>
                                                <div class="proficientStaffBox_img_box"></div>
                                                <div class="proficientStaffBox_info">
                                                    <span class="proficientStaffBox_info1">事故处理专家组1</span>
                                                    <span class="proficientStaffBox_info2">某某某</span>
                                                    <span class="proficientStaffBox_info3">交通事故处理</span>
                                                    <span class="proficientStaffBox_info4">3个组员</span>
                                                </div>
                                            </li>
                                            <li>
                                                <div class="proficientStaffBox_img_box"></div>
                                                <div class="proficientStaffBox_info">
                                                    <span class="proficientStaffBox_info1">事故处理专家组1</span>
                                                    <span class="proficientStaffBox_info2">某某某</span>
                                                    <span class="proficientStaffBox_info3">交通事故处理</span>
                                                    <span class="proficientStaffBox_info4">3个组员</span>
                                                </div>
                                            </li>
                                            <li>
                                                <div class="proficientStaffBox_img_box"></div>
                                                <div class="proficientStaffBox_info">
                                                    <span class="proficientStaffBox_info1">事故处理专家组1</span>
                                                    <span class="proficientStaffBox_info2">某某某</span>
                                                    <span class="proficientStaffBox_info3">交通事故处理</span>
                                                    <span class="proficientStaffBox_info4">3个组员</span>
                                                </div>
                                            </li>
                                            <li>
                                                <div class="proficientStaffBox_img_box"></div>
                                                <div class="proficientStaffBox_info">
                                                    <span class="proficientStaffBox_info1">事故处理专家组1</span>
                                                    <span class="proficientStaffBox_info2">某某某</span>
                                                    <span class="proficientStaffBox_info3">交通事故处理</span>
                                                    <span class="proficientStaffBox_info4">3个组员</span>
                                                </div>
                                            </li>--%>
                                        </ul>
                                    </div>
                                    <div class="proficientStaff_list_affirm" style="display: none">
                                        <button class="btn blue button-submit" style="width: 100%">确定</button>
                                    </div>
                                </div>
                                <%--装备设施列表--%>
                                <div id="equipFacilityBox" class="tab-pane">
                                    <div id="equipFacilityZtreeWrapper">
                                        <ul>

                                        </ul>
                                        <%--<div id="equipFacilityZtree" class="ztree"></div>--%>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- schedulingCommandMap：用于展示地图的div -->
                <div id="schedulingCommandMap" style="height: 100%;width: calc(100% - 400px);position: relative">
                    <div class="btnShowLeftInfo">
                        <span class="realTimeWarning" style="">70</span>
                        <div style="position: relative;text-align: center">
                        <img src="../../images/trafficControl/light.png" style="margin-right: 0px" />
                            <div>实时预警</div>
                        </div>
                    </div>
                    <!-- 搜索框 -->
                    <div class="searchInputBox">
                        <div style="display: flex;height: 40px;align-items: center;">
                            <input id="searchText" type="text"
                                   style="padding-left: 7px; font-size: 19px; font-weight: bold; color: rgba(41,41,41,0.65);height: 100%;width: 233px;border: unset;background: unset;">
                            <input type="button" id="clearBtn"
                                 style="width: 30px;height: 30px;border:unset;background: url(/images/roadInfo/close.png) no-repeat center;"/>
                            <input type="button" id="searchBtn"
                                 style="width: 35px;height: 28px;border:unset;border-left: 2px solid rgba(224,224,224,1);background: url(/images/roadInfo/search.png) no-repeat center;"/>
                        </div>
                        <div class="pathSearchResult hide" id="searchResult" style="height: 414px">
                        </div>
                        <div class="pathSearchResult hide" id="searchResultPage">
                            <div style="display: flex;align-items: center;justify-content: space-between">
                                <%--                                <div>--%>
                                <span id="resultCurrentPage" style="margin-right: 5px">1</span>/<span
                                    id="resultTotalPage" style="margin-left: 5px;margin-right: 10px">1</span>
                                <%--                                </div>--%>
                                <%--                                <div>--%>
                                <span class="changePage prev" data-option="prev"></span>
                                <span class="changePage next" data-option="next"></span>
                                <%--                                </div>--%>
                            </div>
                        </div>
                    </div>
                    <%--预案操作栏--%>
                    <div class="plan_toolebar">
                        <ul>
                            <li class="matching_li" id="matching_box_btn" data-toggle="modal">
                                <span class="matchingBtn"></span>
                                <span>匹配预案</span>
                            </li>
                            <li class="linkage_li">
                                <span class="linkageBtn"></span>
                                <span>联动部门</span>
                            </li>
                            <li class="expert_li" id="expert_box_btn" data-toggle="modal">
                                <span class="expertBtn"></span>
                                <span>专家组</span>
                            </li>
                            <li class="police_li">
                                <span class="policeBtn"></span>
                                <span>警力资源</span>
                            </li>
                            <li class="equipment_li" id="equipment_box_btn" data-toggle="modal">
                                <span class="equipmentBtn"></span>
                                <span>装备设施</span>
                                <div class="equipment_box">
                                    <div class="equipment_content"></div>
                                    <div class="equipment_box_triangle"></div>
                                </div>
                            </li>
                            <li class="publish_li">
                                <span class="publishBtn"></span>
                                <span>信息发布</span>
                            </li>
                        </ul>
                        <%--联动部门 弹窗--%>
                        <div class="linkage_box" style="display: none">
                            <div class="linkage_content">
                                <div class="linkage_type clearfix">
                                    <div class="linkage_type_left"><strong>类型</strong></div>
                                    <div class="linkage_type_right">
                                        <label><input type="checkbox" name="linkDep" value="3"/>政府</label>
                                        <label><input type="checkbox" name="linkDep" value="1"/>医疗</label>
                                        <label><input type="checkbox" name="linkDep" value="2"/>消防</label>
                                        <label><input type="checkbox" name="linkDep" value="5"/>环保</label>
                                        <label><input type="checkbox" name="linkDep" value="4"/>安监</label>
                                        <label><input type="checkbox" name="linkDep" value="6"/>修理厂</label>
                                    </div>
                                </div>
                                <div class="linkage_scope clearfix">
                                    <div class="linkage_scope_left"><strong>范围</strong></div>
                                    <div class="linkage_scope_right">
                                        事件周边
                                        <input type="text" oninput = "value=value.replace(/[^\d]/g,'')" autocomplete="off" maxlength="6" id="linkDepInput">
                                        米
                                    </div>
                                </div>
                                <div class="linkage_btn_box">
                                    <span class="linkage_confirm_btn btn_confirm">确定</span>
                                    <span class="linkage_cancel_btn btn_cancel">取消</span>
                                </div>
                            </div>
                            <div class="linkage_box_triangle"></div>
                        </div>
                        <%--警力资源 弹窗--%>
                        <div class="police_box" style="display: none">
                            <div class="police_content">
                                <div class="police_scope clearfix">
                                    <div class="police_scope_left"><strong>范围</strong></div>
                                    <div class="police_scope_right">
                                        事件周边
                                        <input type="text" oninput = "value=value.replace(/[^\d]/g,'')" autocomplete="off" maxlength="6" id="policeInput">
                                        米
                                    </div>
                                </div>
                                <div class="police_btn_box">
                                    <span class="police_confirm_btn btn_confirm">确定</span>
                                    <span class="police_cancel_btn btn_cancel">取消</span>
                                </div>
                            </div>
                            <div class="police_box_triangle"></div>
                        </div>
                        <%--信息发布 弹窗--%>
                        <div class="publish_box" style="display: none">
                            <div class="publish_content">
                                <div class="publish_scope clearfix">
                                    <div class="publish_scope_left"><strong>范围</strong></div>
                                    <div class="publish_scope_right">
                                        事件周边
                                        <input type="text" oninput = "value=value.replace(/[^\d]/g,'')" autocomplete="off" maxlength="6" id="publishInput">
                                        米
                                    </div>
                                </div>
                                <div class="publish_btn_box">
                                    <span class="publish_confirm_btn btn_confirm">确定</span>
                                    <span class="publish_cancel_btn btn_cancel">取消</span>
                                </div>
                            </div>
                            <div class="publish_box_triangle"></div>
                        </div>
                    </div>
                    <%--事件地图标题--%>
                    <div class="control_bar_box">
                        <ul>
                            <li class="control_bar1"></li>
                            <li class="control_bar2"></li>
                            <li class="control_bar3">调度中</li>
                            <li class="control_bar4" id="summaryBtn" data-toggle="modal"></li>
                        </ul>
                    </div>
                    <%--预案列表窗  右侧--%>
                    <div class="planListAllBox" style="display: none">


                        <%--<div class="planListBox">
                            <div class="planListHeader">
                                <span class="planListTitle">预案</span>
                                <span class="planListImg_top"></span>
                            </div>
                            <div class="planListBody">
                                <div class="planListBody_top">
                                    <p>本预案适用于本项目路基、桥梁</p>
                                </div>
                                <div class="planListBody_bottom">
                                    <div class="planListSel">
                                        <span class="planListName">关于XXXX的说明.pdf</span>
                                        <span class="planListSize">(1.5MB)</span>
                                        <a class="planListDetails_btn"></a>
                                        <a class="planListownload_btn"></a>
                                    </div>
                                    <div class="planListSel">
                                        <span class="planListName">关于XXXX的说明.pdf</span>
                                        <span class="planListSize">(1.5MB)</span>
                                        <a class="planListDetails_btn"></a>
                                        <a class="planListownload_btn"></a>
                                    </div>
                                </div>
                            </div>
                        </div>--%>


                    </div>
                    <%--地图工具栏--%>
                    <ul class="map-toolbarBox">
                        <li class="clearDraw" id="clearDraw" title="移动"><i></i></li>
                        <li class="regainMap" id="regainMap" title="全图"><i></i></li>
                        <li class="showImage" id="showImage" title="影像图"><i></i></li>
                        <li class="ranging" id="ranging" title="测距"><i></i></li>
                        <li class="layerChoose toolBarChoose" id="rectangleSel" title="图层" style="position: relative;border: 0px;">
                            <i></i><span class="arrow-signal arrow-close"></span>
                            <%--地图工具栏下拉框--%>
                            <div class="drop-down-box">
                                <div class="arrow-up">
                                    <div class="rectangleSel_triangle"></div>
                                </div>
                                <ul class="second-tool-bar">
                                    <li class="changeLayer"><i class="checkBox active" data-value="video"></i>视频监控</li>
                                    <li class="changeLayer"><i class="checkBox active" data-value="road"></i>路况</li>
                                    <li class="changeLayer"><i class="checkBox active" data-value="police"></i>警力资源</li>
                                    <li class="changeLayer"><i class="checkBox active" data-value="assoDept"></i>联动部门</li>
                                    <li class="changeLayer"><i class="checkBox active" data-value="guidScreen"></i>诱导屏</li>
                                    <li class="changeLayer"><i class="checkBox active" data-value="signalCross"></i>交通信号</li>
                                </ul>
                            </div>
                        </li>
                    </ul>
                </div>
            </div>

        </div>
        <div class="tab-pane" id="tab_808_2" style="color: white">
            <div class="row">
                <div class="col-md-4" style="height: 100%;padding-right: 0">
                    <div id="mapBox" class="hW100">
                    </div>
                </div>
                <div class="table-portlet col-md-8" style="padding-left: 0">
                    <%--查询栏--%>
                    <div class="wrapper-center" id="" style="height: 145px;padding: 5px 0;">
                        <%--    查询第一行--%>
                        <ul class="searchUl">
                            <li class="li_width">
                                <span class="span1">事件来源</span><select id="eventSjly" class="form-control input-sm"
                                                                       placeholder="请选择"></select>
                            </li>
                            <li class="li_width">
                                <span class="span1">事件类型</span><select id="eventSjLx" class="form-control input-sm"
                                                                       placeholder="请选择"></select>
                            </li>
                            <li class="li_width">
                                <span class="span1">事件子类</span><select id="eventSjZl" class="form-control input-sm"
                                                                       placeholder="请选择"></select>
                            </li>
                        </ul>
                        <%--    查询第二行--%>
                        <ul class="searchUl">
                            <li class="li_width">
                                <span class="span1" style="float: left">发生时间</span>
                                <%--<input id="dpdCondition" type="text"--%>
                                                                      <%--class="form-control inlineBlock"--%>
                                                                      <%--autocomplete="off">--%>
                                <%--<input id="dpdCondition" class="form-control form-control-time pull-right" type="text"/>--%>
                                <input class="form-control form-control-time " style="padding: 6px 4px;display: inline-block;width:
                                calc(50% - 55px) !important;background: #e9f6fc !important;" type="text" id="carPassStartTime" name="anaTaskStartTime"/>
                                <input class="form-control form-control-time " style="padding: 6px 4px;display: inline-block;width:
                                calc(50% - 55px) !important;background: #e9f6fc !important; margin-left: 5px"
                                       type="text" id="carPassEndTime" name="anaTaskEndTime"/>
                            </li>
                            <li class="li_width">
                                <span class="span1">发生地点</span><input id="eventFsdd" type="text" style="background: #e9f6fc!important;"
                                                                      class="form-control inlineBlock" placeholder=""/>
                            </li>
                            <li class="li_width">
                                <span class="span1">处置状态</span><select id="eventCzzt" class="form-control input-sm"
                                                                       placeholder="请选择"></select>
                            </li>
                        </ul>
                        <ul class="searchUl">
                            <li class="li_width">
                                <span class="span1">数据状态</span><select id="eventSjzt" class="form-control input-sm"
                                                                       placeholder="请选择"></select>
                            </li>
                            <li class="li_width">
                                <span class="span1">是否关注</span><select id="eventSfgz" class="form-control input-sm"
                                                                       placeholder="请选择"></select>
                            </li>
                            <li class="li_width" style="text-align: right">
                                <span class="span1"></span><button class="searchs" id="eventSrcBtn">查询</button>
                            </li>
                        </ul>
                    </div>
                    <%--表格操作栏--%>
                    <div class="table-OperationBtnBox"
                         style="height: 50px;padding: 8px 20px;margin: 10px 0px;margin-bottom: 4px;background: rgba(46,141,173,0.1);">
                        <button type="button" id="peopleReport" class="table-OperationBtn table-passBtn"
                                style="margin-right: 10px;">
                            <i></i>
                            人工上报
                        </button>
                        <button id="obsoleteIncidentsBtn" type="button" class="table-OperationBtn table-noPassBtn"
                                style="margin-right: 10px;">
                            <i></i>
                            作废
                        </button>
                    </div>
                    <div class="table-portlet">
                        <table id="eventSearchTable" class="blue-table"></table>
                    </div>
                </div>
            </div>

        </div>
        <div class="tab-pane myHeight " id="tab_808_3" style="color: white">
            <div class="table-portlet portlet-body myHeight">

                <div class="search808_1">
                    <span>统计时间</span>
                    <select id="tjsjSelect" placeholder="请选择"></select>
                    <%--<span class="btnSpan" title="最近7天" id="sevenDays">最近7天</span>--%>
                    <%--<span class="btnSpan selectedSpan" title="最近30天" id="thirtyDays">最近30天</span>--%>
                    <%--<input type="text" class="inlineBlock timeInputLen" id="prevTime">--%>
                    <input type="text" class="inlineBlock timeInputLen" id="prevStartTime">
                    <input type="text" class="inlineBlock timeInputLen" id="prevEndTime">
                    <input type="checkbox" id="comparison">
                    <label for="comparison">对比</label>
                    <%--<input type="text" class="inlineBlock timeInputLen" id="nextTime" style="display: none">--%>
                    <input  type="text" class="inlineBlock timeInputLen"  id="nextStartTime" style="display: none">
                    <input  type="text" class="inlineBlock timeInputLen"  id="nextEndTime" style="display: none">
                    <span style="margin-left: 20px" id="eventTypeDiv"> 事件类型</span>
                    <select id="sjlxTjfx" placeholder="请选择"></select>
                    <span class="btnSpanYes" id="statisticsSearch" style="margin-left: 10px;">确定</span>
                </div>
                <div class="illegalCodeSum">
                    <div class="top" id="allIllegalCodeSum">
                        <div class="topAllIllegalTypesWrapper" id="topAllIllegalTypesWrapper">
                            <div class="topAllIllegalTypes" id="topAllIllegalTypes">

                            </div>
                        </div>
                        <div class="prevIllegalImg" id="prevIllegalImg" style="display: none"></div>
                        <div class="nextIllegalImg" id="nextIllegalImg" style="display: none"></div>
                    </div>
                </div>
                <div class="bottom">

                    <div class="tabbable-line">
                        <ul style="border-bottom: 1px solid rgba(0,155,252,0.1);padding-left: 0;padding-right: 0;"
                            class="nav nav-tabs" id="differentWays">
                            <li class="active" style="float: left">
                                <a href="#timeTab" data-toggle="tab" type="时间">时间</a>
                            </li>
                            <li style="float: left">
                                <a href="#pointTab" data-toggle="tab" type="位置">位置</a>
                            </li>
                            <%--<li style="float: left">--%>
                            <%--<a href="#importantTab" data-toggle="tab" type="重点车">重点车</a>--%>
                            <%--</li>--%>
                            <li>
                                <a href="#otherTab" data-toggle="tab" type="其他">其他</a>
                            </li>
                        </ul>
                    </div>

                    <div class="tab-content myHeight3">
                        <div class="tab-pane fade in active myHeight" id="timeTab">


                            <div class="otherLeft" style="height: 100%;width: 50%">
                                <div class="otherLeftTop">
                                    <span>时间分布统计</span>
                                </div>
                                <div style="height: calc(100% - 34px)">
                                    <div id="timeTabTopEcharts"></div>
                                    <div id="timeTabBottomEcharts"></div>
                                </div>

                            </div>
                            <div class="otherRight" style="height: 100%;width: 50%">
                                <div class="otherRightTop">
                                    <span>时间段分布统计</span>
                                </div>
                                <div style="height: calc(100% - 34px)">
                                    <div class="left" style="width: 100%;height: 100%" id="timeTabTimeright"></div>
                                </div>
                            </div>
                        </div>

                        <div class="tab-pane fade myHeight" id="pointTab">

                            <div class="tabbable-line">
                                <ul class="nav nav-tabs" id="pointType">
                                    <li class="active">
                                        <a data-toggle="tab" type="点位图表" href="#pointsEchart">图表</a>
                                    </li>
                                    <li>
                                        <a data-toggle="tab" type="点位地图" href="#pointsMap">地图</a>
                                    </li>
                                </ul>
                            </div>


                            <div class="tab-content myHeight">
                                <div class="tab-pane fade in active myHeight" id="pointsEchart">
                                    <div class="otherLeft" style="height: 100%;width: 50%">
                                        <div class="otherLeftTop">
                                            <span>道路采集top10</span>
                                            <span id="roadTopMore"
                                                  style="float: right;cursor: pointer;width: 80px;height: 34px;line-height: 34px;text-align: center;display: inline-block;background: transparent;font-size: 14px;font-family: MicrosoftYaHei;font-weight: 400;color: rgba(0,155,252,1);">查看更多</span>
                                        </div>
                                        <div class="otherLeftBottom" id="" style="height: calc(100% - 34px)">
                                            <div id="topPointsEchart"></div>
                                            <div id="bottomPointsEchart"></div>
                                        </div>
                                    </div>
                                    <div class="otherRight" style="width: 50%">
                                        <div class="otherRightTop">
                                            <span>路口路段采集top10</span>
                                            <span id="crossTopMore">查看更多</span>
                                        </div>

                                        <div class="otherRightBottom">
                                            <div class="left" id="otherRightBottomLeft"></div>
                                            <div class="right" id="otherRightBottomRight"></div>
                                        </div>

                                    </div>
                                </div>

                                <div class="tab-pane fade myHeight" id="pointsMap" style="background: rgb(30,54,80);">
                                    <div id="pointsRealMap"></div>
                                    <div class="twoMapTab">
                                        <div id="prevMap" class="timeActive" divName="当前时段" style="display: none">
                                            <p>当前时段</p>
                                            <p>2018-12-14 ~ 2018-12-20</p>
                                        </div>
                                        <div id="nextMap" divName="对比时段" style="display: none;color: black">
                                            对比时段
                                        </div>
                                    </div>
                                </div>
                            </div>

                        </div>

                        <%--<div class="tab-pane fade myHeight" id="importantTab">--%>

                        <%--<div class="importantLeft">--%>
                        <%--<div class="importantLeftTop">--%>
                        <%--<div class="left" id="importantPrev">--%>

                        <%--</div>--%>

                        <%--<div class="right" id="importantNext">--%>

                        <%--</div>--%>
                        <%--</div>--%>

                        <%--<div class="importantLeftBottom" id="importantLeftBottom">--%>

                        <%--</div>--%>

                        <%--</div>--%>

                        <%--<div class="importantRight" id="importantRight">--%>
                        <%--<table id="topTenTable" class="blue-table"></table>--%>
                        <%--</div>--%>

                        <%--</div>--%>

                        <div class="tab-pane fade myHeight" id="otherTab">

                            <div class="otherLeft">
                                <div class="otherLeftTop">
                                    <span>处置结果分析</span>
                                </div>
                                <div class="otherLeftBottom" id="otherLeftBottom"></div>
                            </div>

                            <div class="otherRight">

                                <div class="otherRightTop">
                                    <span>数据来源分布</span>
                                </div>

                                <div class="otherRightBottom">
                                    <%--<div class="importantLeftTop">--%>
                                    <%--<div class="left" id="importantPrev">--%>

                                    <%--</div>--%>

                                    <%--<div class="right" id="importantNext">--%>

                                    <%--</div>--%>
                                    <%--</div>--%>
                                        <div style="width: 100%;height: 40px;">
                                            <div style="display: inline-block;text-align: center;font-size: 16px;" id="importantLeftBottomLeftTime"></div>
                                            <div style="display: inline-block;text-align: center;font-size: 16px;" id="importantLeftBottomRightTime"></div>
                                        </div>
                                    <div class="importantLeftBottom" id="importantLeftBottom">

                                    </div>
                                </div>

                            </div>

                        </div>
                    </div>

                </div>
            </div>
        </div>
    </div>
</div>
<%--<script src="/website-js/trafficControl/js/varData.js"></script>--%>
<div id="eventminuteInfo" class="modal fade" tabindex="-2" data-width="70%">
    <form class="form-horizontal">
        <div class="modal-header" style="border: none;">
            <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
            <h4 class="modal-title">事件详情</h4>
        </div>
        <div class="modal-body">
            <div class="traficCount-select" style="text-align: center">
                <ul class="clearfix" style="display: inline-block">
                    <li class="active" style="list-style: none">
                        <a href="#optimize" class="addSelect btnClick" id="optimizeId" data-toggle="tab">事件信息</a>
                    </li>
                    <li style="list-style: none" id="ddxxTabBtn">
                        <a href="#monitorA" class="btnClick" id="historyData" data-toggle="tab">调度信息</a>
                    </li>
                </ul>
            </div>
            <div class="tab-content tabHeight">
                <div class="tab-pane active tabPhase" id="optimize" style="margin-top: 0px">
                    <div class="col-md-12" style="padding-right:25px!important;">
                        <div class="wrapper-center" id="" style="background: white;height: 300px">
                            <%--    查询第一行--%>
                            <ul class="searchUl">
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">事件来源</span><select id="sjlyModal" class="form-control input-sm"
                                                                           placeholder="请选择"></select>
                                </li>
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">发生时间</span><input id="fssjModal"
                                                                          class="form-control inlineBlock"
                                                                          placeholder="请选择"/>
                                </li>
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">事件类型</span><select id="sjlxModal" class="form-control input-sm"
                                                                           placeholder="请选择"></select>
                                </li>
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">事件子类</span><select id="sjzlModal" class="form-control input-sm"
                                                                           placeholder="请选择"></select>
                                </li>
                            </ul>
                            <%--    查询第二行--%>
                            <ul class="searchUl">
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">道路代码</span><select id="dldmModal" class="form-control input-sm"
                                                                           placeholder="请选择"></select>

                                </li>
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">路口路段</span><select id="lkldModal" class="form-control input-sm"
                                                                           placeholder="请选择"></select>
                                </li>
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">所在方向</span><select id="szfxModal" class="form-control input-sm"
                                                                           placeholder="请选择"></select>
                                </li>
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">事件等级</span><select id="sjdjModal" class="form-control input-sm"
                                                                           placeholder="请选择"></select>
                                </li>
                            </ul>
                            <ul class="searchUl">
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">是否关注</span><select id="sfgzModal" class="form-control input-sm"
                                                                           placeholder="请选择"></select>
                                </li>
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">发生地点</span><input id="fsddModal"
                                                                          class="form-control inlineBlock"
                                                                          placeholder=""/>
                                </li>
                                <li class="li_width" style="width: 50%;float: left">
                                    <span class="span1">报警内容</span><input id="bjnrModal"
                                                                          class="form-control inlineBlock"
                                                                          placeholder=""/>
                                </li>
                            </ul>
                            <ul class="searchUl">
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">报警人</span><input id="bjrModal" type="text"
                                                                         class="form-control inlineBlock"
                                                                         autocomplete="off">

                                </li>
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">联系电话</span><input id="lxdhModal" type="text"
                                                                          class="form-control inlineBlock"
                                                                          placeholder=""/>
                                </li>
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">报警时间</span><input id="bjsjModal" type="text"
                                                                          class="form-control inlineBlock"
                                                                          placeholder=""/>
                                </li>
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">录入人</span><input id="lrrModal" type="text"
                                                                         class="form-control inlineBlock"
                                                                         placeholder=""/>
                                </li>
                            </ul>
                            <ul class="searchUl">
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">入库时间</span><input id="rksjModal" type="text"
                                                                          class="form-control inlineBlock"
                                                                          autocomplete="off">

                                </li>
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">事件状态</span><select id="sjztModal" type="text"
                                                                          class="form-control inlineBlock"
                                                                          placeholder=""/>
                                </li>
                                <li class="li_width" style="width: 50%;float: left">
                                    <span class="span1">关闭原因</span><input id="gbyyModal"
                                                                          class="form-control inlineBlock"
                                                                          placeholder=""/>
                                </li>
                            </ul>
                            <ul class="searchUl">
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">处置人员</span><input id="czryModal" type="text"
                                                                          class="form-control inlineBlock"
                                                                          autocomplete="off">

                                </li>
                                <li class="li_width" style="width: 25%;float: left">
                                    <span class="span1">处置时间</span><input id="czsjModal" type="text"
                                                                          class="form-control inlineBlock"
                                                                          placeholder=""/>
                                </li>
                            </ul>
                        </div>
                        <div id="eventImg">
                            <%--<img src="" alt="">--%>
                        </div>
                    </div>
                </div>
                <div class="tab-pane fade tabPhase tabmonitor" id="monitorA">
                    <div id="" class="row">
                        <div style="height: 230px;width: 100px;margin-top: 40px">
                            <div style="width: 100%;margin-top: 20px;margin-left: 30px;">
                                <img src="/images/plIcon.png" alt="">
                                <span style="display: block;margin-left: 7px;margin-top: 7px;" id="ddyName"></span>
                                <a href="javascript:" data-style="" style="margin-top: 60px;" data-size="s"
                                   id="exportDisInfo"
                                   class="btn blue ladda-button button-submit"><span class="ladda-label">导出</span></a>
                            </div>
                        </div>
                        <div id="widthUl" style="height: 230px;position: absolute;top: 100px;overflow-y: auto;"
                             class="col-md-12">
                            <ul id="dispatchListUl" style="margin-left: 100px;">
                            </ul>
                        </div>
                    </div>
                    <div id="" class="row">
                        <div class="col-md-12" style="padding-right:25px!important;margin-top: 18px;">
                            <div class="wrapper-center" id="gdInfo" style="background: white;height: 300px">
                                <%--    查询第一行--%>
                                <ul class="searchUl">
                                    <li class="li_width" style="width: 25%;float: left">
                                        <span class="span1">受害人数</span><input id="gdshrs" type="text"
                                                                              class="form-control inlineBlock"
                                                                              autocomplete="off">
                                    </li>
                                    <li class="li_width" style="width: 25%;float: left;">
                                        <span class="span1">死亡人数</span><input id="gdswrs" type="text"
                                                                              class="form-control inlineBlock"
                                                                              autocomplete="off">
                                    </li>
                                    <li class="li_width" style="width: 25%;float: left;">
                                        <span class="span1">受伤人数</span><input id="gdssrs" type="text"
                                                                              class="form-control inlineBlock"
                                                                              autocomplete="off">
                                    </li>
                                    <li class="li_width" style="width: 25%;float: left;">
                                        <span class="span1">经济损失</span><input id="gdjjss" type="text"
                                                                              class="form-control inlineBlock"
                                                                              autocomplete="off">
                                    </li>
                                </ul>
                                <%--    查询第二行--%>
                                <ul class="searchUl">
                                    <li class="li_width" style="width: 25%;float: left;">
                                        <span class="span1">涉及车辆数</span><input id="gdsjcls" type="text"
                                                                               class="form-control inlineBlock"
                                                                               autocomplete="off">

                                    </li>
                                    <li class="li_width" style="width: 25%;float: left;">
                                        <span class="span1">损毁车辆数</span><input id="gdshcls" type="text"
                                                                               class="form-control inlineBlock"
                                                                               autocomplete="off">
                                    </li>
                                    <li class="li_width" style="width: 25%;float: left;">
                                        <span class="span1">出动人数</span><input id="gdcdrs" type="text"
                                                                              class="form-control inlineBlock"
                                                                              autocomplete="off">
                                    </li>
                                    <li class="li_width" style="width: 25%;float: left;">
                                        <span class="span1">出动车辆</span><input id="gdcdcl" type="text"
                                                                              class="form-control inlineBlock"
                                                                              autocomplete="off">
                                    </li>
                                </ul>
                                <ul class="searchUl">
                                    <li class="li_width" style="width: 100%;float: left">
                                        <span class="span1">事件起因</span><input id="gdsjqy" type="text"
                                                                              class="form-control inlineBlock"
                                                                              autocomplete="off">
                                    </li>
                                </ul>
                                <ul class="searchUl">
                                    <li class="li_width" style="width: 100%;float: left">
                                        <span class="span1">处置要点</span><input id="gdczyd" type="text"
                                                                              class="form-control inlineBlock"
                                                                              autocomplete="off">

                                    </li>
                                </ul>
                                <ul class="searchUl">
                                    <li class="li_width" style="width: 100%;float: left;">
                                        <span class="span1">经验教训</span><input id="gdjyjx" type="text"
                                                                              class="form-control inlineBlock"
                                                                              autocomplete="off">
                                    </li>
                                </ul>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="modal-footer" style="border: none;">
            <div style="display: inline-block;" id="">
                <a href="javascript:" data-style="expand-right" data-size="s" id="saveDisInfo"
                   class="btn blue ladda-button button-submit"><span class="ladda-label">保存</span></a>
                <a href="javascript:" data-style="expand-right" data-size="s" id="gdDisinfo"
                   class="btn blue ladda-button button-submit"><span class="ladda-label">归档</span></a>
            </div>
            <div style="display: inline-block;">
                <a href="javascript:" data-style="expand-right" data-size="s" id="closeEvent"
                   class="btn blue ladda-button button-submit"><span class="ladda-label">关闭事件</span></a>
                <a href="javascript:" data-style="expand-right" data-size="s" id="ownCl"
                   class="btn blue ladda-button button-submit"><span class="ladda-label">自撤处理</span></a>
                <a href="javascript:" data-style="expand-right" data-size="s" id="scheduBtn"
                   class="btn blue ladda-button button-submit"><span class="ladda-label">指挥调度</span></a>
            </div>
            <a href="javascript:" data-style="expand-right" data-size="s" id="btnCaseSaveLink"
               class="btn blue ladda-button button-submit"><span class="ladda-label">保存</span></a>
            <a href="javascript:" data-style="expand-right" data-size="s" data-dismiss="modal" id="btnCaseCannel"
               class="btn default ladda-button button-submit"><span class="ladda-label">关闭</span></a>
        </div>
    </form>
</div>
<div id="eventReport" class="modal fade" tabindex="-2" data-width="700">
    <form class="form-horizontal">
        <div class="modal-header" style="border: none;">
            <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
            <h4 class="modal-title">事件上报</h4>
        </div>
        <div class="modal-body">
            <div class="tab-pane active tabPhase" id="reportoptimize" style="margin-top: 0px">
                <div class="col-md-12" style="padding-right:25px!important;">
                    <div class="wrapper-center" id="" style="background: white;height: 500px;overflow-y: auto">
                        <%--    查询第一行--%>
                        <ul class="searchUl">
                            <li class="li_width" style="width: 50%;float: left">
                                <span class="span1" style="float: left">事件来源</span><input id="reportsjly"
                                                                                          style="margin-top: 6px"
                                                                                          class="form-control input-sm"
                                                                                          placeholder="人工上报" disabled/>
                            </li>
                            <li class="li_width" style="width: 50%;float: left">
                                <span class="span1">发生时间</span><input id="reportfssj" class="form-control inlineBlock"
                                                                      placeholder="请选择"/>
                            </li>

                        </ul>
                        <%--    查询第二行--%>
                        <ul class="searchUl">
                            <li class="li_width" style="width: 50%;float: left">
                                <span class="span1">事件类型</span><select id="reportsjlx" class="form-control input-sm"
                                                                       placeholder="请选择"></select>
                            </li>
                            <li class="li_width" style="width: 50%;float: left">
                                <span class="span1">事件子类</span><select id="reportsjzl" class="form-control input-sm"
                                                                       placeholder="请选择"></select>
                            </li>

                        </ul>
                        <ul class="searchUl">
                            <li class="li_width" style="width: 50%;float: left">
                                <span class="span1">道路代码</span><select id="reportdldm" class="form-control input-sm"
                                                                       placeholder="请选择"></select>

                            </li>
                            <li class="li_width" id="lkldDiv" style="width: 50%;float: left">
                                <span class="span1">路口路段</span><select id="reportlkld" class="form-control input-sm"
                                                                       placeholder="请选择"></select>
                            </li>
                            <li class="li_width" id="glsDiv" style="width: 50%;float: left;display: none">
                                <span class="span1">公里数/km</span><input id="reportgls"
                                       class="form-control inlineBlock" placeholder=""/>
                            </li>

                        </ul>
                        <ul class="searchUl">
                            <li class="li_width" style="width: 50%;float: left">
                                <span class="span1">所在方向</span><select id="reportszfxModal"
                                                                       class="form-control input-sm"
                                                                       placeholder="请选择"></select>
                            </li>
                            <li class="li_width" style="width: 50%;float: left">
                                <span class="span1">事件等级</span><select id="reportsjdjModal"
                                                                       class="form-control input-sm"
                                                                       placeholder="请选择"></select>
                            </li>
                        </ul>
                        <ul class="searchUl">
                            <li class="li_width" style="width: 100%;float: left">
                                <span class="span1">发生地点</span><input id="reportfsddModal"
                                                                      class="form-control inlineBlock" placeholder=""/>
                            </li>
                        </ul>
                        <ul class="searchUl">
                            <li class="li_width" style="width: 50%;float: left">
                                <span class="span1">经度</span><input readonly id="reportjd" type="text"
                                                                    class="form-control inlineBlock" autocomplete="off">
                            </li>
                            <li class="li_width" style="width: 50%;float: left">
                                <span class="span1">纬度</span><input readonly id="reportwd" type="text"
                                                                    class="form-control inlineBlock" autocomplete="off">
                            </li>
                        </ul>
                        <ul class="searchUl">
                            <li class="li_width" style="width: 100%;float: left">
                                <span class="span1">报警内容</span><input id="reportbjnr" class="form-control inlineBlock"
                                                                      placeholder=""/>
                            </li>
                        </ul>
                        <ul class="searchUl">
                            <li class="li_width" style="width: 50%;float: left">
                                <span class="span1">报警人</span><input id="reportbjr" type="text" disabled
                                                                     class="form-control inlineBlock"
                                                                     autocomplete="off">
                            </li>
                            <li class="li_width" style="width: 50%;float: left">
                                <span class="span1">联系电话</span><input id="reportlxdh" type="text"
                                                                      class="form-control inlineBlock" placeholder=""/>
                            </li>
                        </ul>
                        <ul class="searchUl" style="width: 630px">
                            <li class="li_width" style="width: 50%;float: left">
                                <span class="span1">报警时间</span><input id="reportbjsj" type="text"
                                                                      class="form-control inlineBlock" disabled
                                                                      placeholder=""/>
                            </li>
                        </ul>
                        <ul class="searchUl">
                        <li class="li_width" style="width: 50%;float: left">
                        <span class="span1">事件图片</span><input id="reportsjtu"  multiple type="file" style="display: inline-block;background: none !important;border: none;"  onchange="trafficeventScheduling.selectImage(this);"  class="inlineBlock"  />
                        </li>
                        </ul>
                            <div>
                                <img id="reportimageOne" src="" style="width: 100%;height: 300px;display: none"/>
                                <img id="reportimageTwo" src="" style="width: 100%;height: 300px;display: none"/>
                                <img id="reportimageThree" src="" style="width: 100%;height: 300px;display: none"/>
                                <img id="reportimageFour" src="" style="width: 100%;height: 300px;display: none"/>
                            </div>
                    </div>

                </div>
            </div>
        </div>
        <div class="modal-footer" style="border: none;">
            <a href="javascript:" data-style="expand-right" data-size="s" id="reportbtnCaseSave"
               class="btn blue ladda-button button-submit"><span class="ladda-label">确认</span></a>
            <a href="javascript:" data-style="expand-right" data-size="s" data-dismiss="modal" id=""
               class="btn default ladda-button button-submit"><span class="ladda-label">取消</span></a>
        </div>
    </form>
</div>
<div id="closeEventModal" class="modal fade" tabindex="-2" data-width="500">
    <form class="form-horizontal">
        <div class="modal-header" style="border: none;">
            <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
            <h4 class="modal-title">关闭事件</h4>
        </div>
        <div class="modal-body">
            <div class="tab-pane active tabPhase" id="" style="margin-top: 0px">
                <div class="col-md-12" style="padding-right:25px!important;">
                    <div class="wrapper-center" id="" style="background: white;height: 300px">
                        <ul class="searchUl">
                            <li class="li_width" style="width: 50%;float: left">
                                <span class="span1" style="color: black;float: left">处理人</span><input id="closePeoele"
                                                                                                      disabled
                                                                                                      class="form-control input-sm"
                                                                                                      placeholder="请选择"/>
                            </li>

                        </ul>
                        <ul class="searchUl" style="margin-top: 50px">
                            <li class="li_width" style="width: 100%;float: left">
                                <span class="span1" style="color: black;float: left">关闭原因</span><textarea
                                    style="border: 1px solid #c2cad8;width: calc(100% - 100px);height: 120px;"
                                    id="closeTalk" class="form-control input-sm" placeholder="请输入关闭原因"></textarea>
                            </li>
                        </ul>
                        <%--<ul class="searchUl">--%>
                        <%--<li class="li_width" style="width: 50%;float: left">--%>
                        <%--<span class="span1">事件图片</span><input id="reportlrr"  type="text" class="form-control inlineBlock" placeholder="" />--%>
                        <%--</li>--%>
                        <%--</ul>--%>
                    </div>
                </div>
            </div>
        </div>
        <div class="modal-footer" style="border: none;">
            <a href="javascript:" data-style="expand-right" data-size="s" id="closebtnCaseSave"
               class="btn blue ladda-button button-submit"><span class="ladda-label">确认</span></a>
            <a href="javascript:" data-style="expand-right" data-size="s" data-dismiss="modal" id=""
               class="btn default ladda-button button-submit"><span class="ladda-label">取消</span></a>
        </div>
    </form>
</div>
<div id="cancleEventModal" class="modal fade" tabindex="-2" data-width="500">
    <form class="form-horizontal">
        <div class="modal-header" style="border: none;">
            <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
            <h4 class="modal-title">自撤事件</h4>
        </div>
        <div class="modal-body">
            <div class="tab-pane active tabPhase" id="" style="margin-top: 0px">
                <div class="col-md-12" style="padding-right:25px!important;">
                    <div class="wrapper-center" id="" style="background: white;height: 300px">
                        <ul class="searchUl">
                            <li class="li_width" style="width: 50%;float: left">
                                <span class="span1" style="color: black;float: left">处理人</span><input id="canclePeoele"
                                                                                                      disabled
                                                                                                       class="form-control input-sm"
                                                                                                      placeholder=""/>
                            </li>

                        </ul>
                        <ul class="searchUl" style="margin-top: 50px">
                            <li class="li_width" style="width: 100%;float: left">
                                <span class="span1" style="color: black;float: left">关闭原因</span><textarea
                                    style="border: 1px solid #c2cad8;width: calc(100% - 100px);height: 120px;"
                                    id="cancleTalk" class="form-control input-sm" placeholder="请输入自撤原因"></textarea>
                            </li>
                        </ul>
                        <%--<ul class="searchUl">--%>
                        <%--<li class="li_width" style="width: 50%;float: left">--%>
                        <%--<span class="span1">事件图片</span><input id="reportlrr"  type="text" class="form-control inlineBlock" placeholder="" />--%>
                        <%--</li>--%>
                        <%--</ul>--%>
                    </div>
                </div>
            </div>
        </div>
        <div class="modal-footer" style="border: none;">
            <a href="javascript:" data-style="expand-right" data-size="s" id="canclebtnCaseSave"
               class="btn blue ladda-button button-submit"><span class="ladda-label">确认</span></a>
            <a href="javascript:" data-style="expand-right" data-size="s" data-dismiss="modal" id=""
               class="btn default ladda-button button-submit"><span class="ladda-label">取消</span></a>
        </div>
    </form>
</div>
<%--道路 查看更多--%>
<div id="pointsMore" class="modal fade" tabindex="-1" data-width="900" data-height="540">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">事件道路采集</h4>
    </div>
    <div class="modal-body">
        <div class="pointsMoreLeft">
            <div style="margin-bottom: 15px">
                <span class="prevData"></span>
            </div>
            <div class="table-portlet whiteTable">
                <table id="prevTable" class="blue-table"></table>
            </div>
        </div>
        <div class="pointsMoreRight">
            <div style="margin-bottom: 15px">
                <span class="nextData"></span>
            </div>
            <div class="table-portlet whiteTable">
                <table id="nextTable" class="blue-table"></table>
            </div>
        </div>
    </div>
</div>
<%--路口 查看更多--%>
<div id="crosssMore" class="modal fade" tabindex="-1" data-width="900" data-height="540">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">事件路口采集</h4>
    </div>
    <div class="modal-body">
        <div class="crossMoreLeft" style="display: inline-block">
            <div style="margin-bottom: 15px">
                <span class="prevData"></span>
            </div>
            <div class="table-portlet whiteTable">
                <table id="crossprevTable" class="blue-table"></table>
            </div>
        </div>
        <div class="crossMoreRight" style="height: 100%;float: right;padding-left: 10px;">
            <div style="margin-bottom: 15px">
                <span class="nextData"></span>
            </div>
            <div class="table-portlet whiteTable">
                <table id="crossnextTable" class="blue-table"></table>
            </div>
        </div>
    </div>
</div>

<%--匹配预案 弹窗--%>
<div id="matching_box" class="matching_box modal fade" tabindex="-1" data-width="840">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">匹配预案</h4>
    </div>
    <div class="modal-body">
        <form class="form-horizontal" role="form">
            <div class="form-body">
                <div class="row">
                    <div class="col-md-4">
                        <div class="form-group">
                            <label class="control-label col-md-5">预案编号</label>
                            <div class="col-md-7">
                                <input id="plan_num" class="form-control"/>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="form-group">
                            <label class="control-label col-md-5">预案名称</label>
                            <div class="col-md-7">
                                <input id="plan_name" class="form-control"/>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="form-group">
                            <label class="control-label col-md-5">事件类型</label>
                            <div class="col-md-7">
                                <select id="event_type" class="form-control input-sm" style="margin-top: 0;height: 34px;"></select>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
            <div class="col-md-4">
                <div class="form-group">
                    <label class="control-label col-md-5">事件等级</label>
                    <div class="col-md-7">
                        <select id="event_grade" class="form-control input-sm" style="margin-top: 0;height: 34px;"></select>
                    </div>
                </div>
            </div>
            <div class="col-md-4">
                <div class="form-group">
                    <label class="control-label col-md-5">道路类型</label>
                    <div class="col-md-7">
                        <select id="way_genre" class="form-control input-sm" style="margin-top: 0;height: 34px;"></select>
                    </div>
                </div>
            </div>
            <div class="col-md-4 text-right">
                <button class="btn_confirm" id="matchingSearc">查询</button>
                <button class="btn_confirm" style="display: none" id="matchingSelectedSearc"></button>
            </div>
        </div>
            </div>
        </form>
        <div class="row" style="padding: 0 15px;">
            <div class="" style="height: 380px;border:1px solid #c2cad8;width: 65%;float:left;">
                <div class="table-portlet whiteTable">
                    <table id="matchingTabl"  class="blue-table"></table>
                </div>
            </div>
            <div class="" style="height: 380px;border:1px solid #c2cad8;width: 33%;float:right;display: flex;align-items: center;justify-content: center" id="matchingDesc"></div>
        </div>
    </div>
    <div class="modal-footer">
        <button class="btn_confirm" id="matchingAl">调取</button>
    </div>
</div>
<%--专家组 弹窗--%>
<div id="expert_box" class="expert_box modal fade" tabindex="-1" data-width="800">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">匹配专家组</h4>
    </div>
   <div class="modal-body" style="padding:0 0 15px 0">
       <div class="expert_query clearfix">
           <ul>
               <li>
                   组长姓名
                   <input type="text" id="expertLeaderName">
               </li>
               <li>
                   专家组名称
                   <input type="text" id="expertTeamName"></li>
               <li><span class="btn_confirm" id="expertBoxSearch">查询</span><span id="refreshExpertTableWithCurrentPage" style="display: none">查询</span></li>
           </ul>
       </div>
       <div class="expert_table_box clearfix">
           <div class="expert_toolbar">
               <span>专家组列表</span>
               <span class="modal-OperationBtn modal-selectBtn" id="expertChoseAl"><i></i>选择</span>
           </div>
           <div class="expert_table">
               <div class="table-portlet whiteTable">
                   <table id="expertTabl"  class="blue-table"></table>
               </div>
           </div>
           <div class="expert_table_explain">
                <%--专业方向--%>
               <div class="professional_emphasis_box">
                   <span class="professional_emphasis_title">专业方向</span>
                   <span class="professional_emphasis_name"></span>
               </div>
               <%--组成员--%>
               <div class="group_members_box">
                   <span class="group_members_title">组成员</span>
                  <div class="group_members_body">
                      <%--<span class="group_members">某某某1</span>
                      <span class="group_members">某某某1</span>
                      <span class="group_members">某某某1</span>
                      <span class="group_members">某某某1</span>--%>
                  </div>
               </div>
           </div>
       </div>
   </div>
    <div class="modal-footer">
        <button class="btn_cancel" data-dismiss="modal" aria-hidden="true">关闭</button>
    </div>
</div>
<%--装备设施 弹窗--%>
<div id="equipment_box" class=" modal fade" tabindex="-1" data-width="1000">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">装备设施查询</h4>
    </div>
    <div class="modal-body">
        <form class="form-horizontal" role="form">
            <div class="form-body">
                <div class="row">
                    <div class="col-md-3">
                        <div class="form-group">
                            <label class="control-label col-md-5">管理机构</label>
                            <div class="col-md-7" style="padding: 0 5px;">
                                <select id="equipmentOrgCode" class="form-control input-sm" style="margin-top: 0;height: 34px;"></select>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="form-group">
                            <label class="control-label col-md-5">装备类型</label>
                            <div class="col-md-7" style="padding: 0 5px;">
                                <select id="equipmentType" class="form-control input-sm" style="margin-top: 0;height: 34px;"></select>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="form-group">
                            <label class="control-label col-md-5">装备名称</label>
                            <div class="col-md-7" style="padding: 0 5px;">
                                <input id="equipmentName" class="form-control"/>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3 text-center">
                        <button class="btn_confirm" id="equipmentSearc">查询</button>
                    </div>
                </div>
            </div>
        </form>
        <div class="row" style="padding: 0 15px;">
            <div class="col-md-12" style="height: 360px;padding: 0;border: 1px solid #c2cad8;">
                <div class="table-portlet whiteTable">
                    <table id="equipmentTabl"  class="blue-table"></table>
                </div>
            </div>
        </div>
    </div>
    <div class="modal-footer">
        <button class="btn_confirm" id="equipmentRetrieve">调取</button>
    </div>
</div>
<%--实时预警 查看详情 弹窗--%>
<div id="realTimeWarningModal" class="modal fade" tabindex="-1" data-width="1100" data-height="500">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">事件详情</h4>
    </div>
    <div class="incident_particulars_box">
        <%--事件详情tab选择栏--%>
        <div class="incident_particulars_nav" style="text-align: center;">
            <ul>
                <li class="active"><a href="#incident_info" data-toggle="tab">事件信息</a></li>
                <li class="" id="dispatchInfoTab"><a href="#dispatch_info" data-toggle="tab">调度信息</a></li>
            </ul>
        </div>
        <%--事件详情tab内容栏--%>
        <div class="tab-content">
            <%--事件信息tab页--%>
            <div id="incident_info" class="tab-pane active" style="border-top: 1px solid #EFEFEF;">
                <div class="modal-body" style="padding: 10px 15px 0 15px;height:460px;overflow:scroll;">
                    <form class="form-horizontal" role="form">
                        <div class="form-body" style="padding-top: 7px;">
                            <div class="row">
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label class="control-label col-md-4"><strong>事件来源</strong></label>
                                        <div class="col-md-8">
                                            <input id="incident_source" class="form-control" readonly/>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label class="control-label col-md-4"><strong>发生时间</strong></label>
                                        <div class="col-md-8">
                                            <input id="at_the_time" class="form-control" readonly/>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="row">
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label class="control-label col-md-4"><strong>事件类型</strong></label>
                                        <div class="col-md-8">
                                            <input id="incident_type" class="form-control" readonly/>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label class="control-label col-md-4"><strong>事件子类</strong></label>
                                        <div class="col-md-8">
                                            <input id="incident_subtype" class="form-control" readonly/>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="row">
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label class="control-label col-md-4"><strong>道路代码</strong></label>
                                        <div class="col-md-8">
                                            <input id="way_num" class="form-control" readonly/>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label class="control-label col-md-4"><strong>路口路段代码</strong></label>
                                        <div class="col-md-8">
                                            <input id="intersection_num" class="form-control" readonly/>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="row">
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label class="control-label col-md-4"><strong>所在方向</strong></label>
                                        <div class="col-md-8">
                                            <input id="direction_select" class="form-control" maxlength="9" readonly/>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label class="control-label col-md-4"><strong>事件等级</strong></label>
                                        <div class="col-md-8">
                                            <input id="incident_grade_select" class="form-control" maxlength="9" readonly/>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="row">
                                <div class="col-md-12">
                                    <div class="form-group">
                                        <label class="control-label col-md-2"><strong>发生地点</strong></label>
                                        <div class="col-md-10">
                                            <input id="occur_location" class="form-control" maxlength="9" readonly/>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="row">
                                <div class="col-md-12">
                                    <div class="form-group">
                                        <label class="control-label col-md-2"><strong>事件内容</strong></label>
                                        <div class="col-md-10">
                                            <input id="incident_details" class="form-control" maxlength="9" readonly/>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="form-body" style="border-top: 1px solid #EFEFEF;padding-top: 7px;">
                            <div class="row">
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label class="control-label col-md-4"><strong>报警人</strong></label>
                                        <div class="col-md-8">
                                            <input id="alarm_people" class="form-control" readonly/>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label class="control-label col-md-4"><strong>联系电话</strong></label>
                                        <div class="col-md-8">
                                            <input id="alarm_people_phone" class="form-control" readonly/>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="row">
                                <div class="col-md-12">
                                    <div class="form-group">
                                        <label class="control-label col-md-2"><strong>报警时间</strong></label>
                                        <div class="col-md-10">
                                            <input id="alarm_time" class="form-control" maxlength="9" readonly/>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="row">
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label class="control-label col-md-4"><strong>录入人</strong></label>
                                        <div class="col-md-8">
                                            <input id="key_boarder" class="form-control" readonly/>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label class="control-label col-md-4"><strong>入库时间</strong></label>
                                        <div class="col-md-8">
                                            <input id="input_time" class="form-control" readonly/>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="form-body" style="border-top: 1px solid #EFEFEF;padding-top: 7px;">
                            <div class="row">
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label class="control-label col-md-4"><strong>事件状态</strong></label>
                                        <div class="col-md-8">
                                            <input id="incident_state" class="form-control" readonly/>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label class="control-label col-md-4"><strong>关闭原因</strong></label>
                                        <div class="col-md-8">
                                            <input id="close_cause" class="form-control" readonly/>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="row">
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label class="control-label col-md-4"><strong>处置人员</strong></label>
                                        <div class="col-md-8">
                                            <input id="problem_person" class="form-control" readonly/>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label class="control-label col-md-4"><strong>处置时间</strong></label>
                                        <div class="col-md-8">
                                            <input id="problem_time" class="form-control" readonly/>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="row" style="margin-bottom: 10px;">
                                <div class="col-md-12" id="incident_imgWarpper">

                                </div>
                            </div>
                        </div>
                    </form>
                </div>
                <div class="modal-footer" style="padding: 10px 15px;">
                    <button class="btn_confirm" id="close_incident">关闭事件</button>
                    <button class="btn_confirm" id="oneself_withdraw" data-toggle="modal">自撤处理</button>
                    <button class="btn_confirm" id="command_dispatch">指挥调度</button>
                    <button class="btn_cancel" data-dismiss="modal" aria-hidden="true">关闭</button>
                </div>
            </div>
            <%--调度信息tab页--%>
            <div id="dispatch_info" class="tab-pane">
                    <div class="modal-body" style="padding: 10px 15px 0;height:460px;overflow:scroll;">
                            <form class="form-horizontal" role="form">
                                <div class="form-body" style="padding-top: 6px;">
                                    <div style="border-bottom: 1px solid #EFEFEF;margin-bottom: 20px">
                                        <div class="navigation posi">调度信息</div>
                                    </div>
                                    <div class="row">
                                        <div style="width: 16.66%;height: 275px;float: left">
                                            <span class="dispatchMan_img dispatchTimeline_head_portrait"></span>
                                            <span class="dispatchMan_name"></span>
                                            <div class="dispatchTimeline_left_block">
                                                <div class="dispatchTimeline_block_box">
                                                    <div></div>
                                                </div>
                                            </div>
                                        </div>
                                        <div style="width: 83.33%;height: 275px;float: right;" class="optiscroll" id="dispatchTimelineBox">
                                            <div style="height: 100%;" class="allSteps">
                                                <%--<div class="dispatchTimeline_classification_time">
                                                    <div class="dispatchTimeline_stop_end">开始时间</div>
                                                    <div class="dispatchTimeline_line_crude"></div>
                                                    <div class="dispatchTimeline_stop_end_time">
                                                        <div>2020/02/02 11:11</div>
                                                    </div>
                                                </div>
                                                <div class="dispatchTimeline_classification">
                                                    <div class="dispatchTimeline_img dispatchTimeline_police"></div>
                                                    <div class="dispatchTimeline_category ">1派警</div>
                                                    <div class="dispatchTimeline_line"></div>
                                                    <div class="dispatchTimeline_time">
                                                        <div>11:11:11</div>
                                                    </div>
                                                </div>
                                                <div class="dispatchTimeline_classification">
                                                    <div class="dispatchTimeline_img_height dispatchTimeline_announce"></div>
                                                    <div class="dispatchTimeline_category">2诱导发布</div>
                                                    <div class="dispatchTimeline_line_height"></div>
                                                    <div class="dispatchTimeline_time">
                                                        <div>11:11:11</div>
                                                    </div>
                                                </div>
                                                <div class="dispatchTimeline_classification">
                                                    <div class="dispatchTimeline_img dispatchTimeline_signal"></div>
                                                    <div class="dispatchTimeline_category">3信号干预</div>
                                                    <div class="dispatchTimeline_line"></div>
                                                    <div class="dispatchTimeline_time">
                                                        <div>11:11:11</div>
                                                    </div>
                                                </div>
                                                <div class="dispatchTimeline_classification">
                                                    <div class="dispatchTimeline_img_height dispatchTimeline_equipment"></div>
                                                    <div class="dispatchTimeline_category">4调配装备设施</div>
                                                    <div class="dispatchTimeline_line_height"></div>
                                                    <div class="dispatchTimeline_time">
                                                        <div>11:11:11</div>
                                                    </div>
                                                </div>
                                                <div class="dispatchTimeline_classification">
                                                    <div class="dispatchTimeline_img dispatchTimeline_plan"></div>
                                                    <div class="dispatchTimeline_category">5调度预案</div>
                                                    <div class="dispatchTimeline_line"></div>
                                                    <div class="dispatchTimeline_time">
                                                        <div>11:11:11</div>
                                                    </div>
                                                </div>
                                                <div class="dispatchTimeline_classification">
                                                    <div class="dispatchTimeline_img_height dispatchTimeline_department"></div>
                                                    <div class="dispatchTimeline_category">6调度联动部门</div>
                                                    <div class="dispatchTimeline_line_height"></div>
                                                    <div class="dispatchTimeline_time">
                                                        <div>11:11:11</div>
                                                    </div>
                                                </div>
                                                <div class="dispatchTimeline_classification">
                                                    <div class="dispatchTimeline_img dispatchTimeline_specialist"></div>
                                                    <div class="dispatchTimeline_category">7咨询专家组</div>
                                                    <div class="dispatchTimeline_line"></div>
                                                    <div class="dispatchTimeline_time">
                                                        <div>11:11:11</div>
                                                    </div>
                                                </div>
                                                <div class="dispatchTimeline_classification_time">
                                                    <div class="dispatchTimeline_stop_end">结束时间</div>
                                                    <div class="dispatchTimeline_line_crude"></div>
                                                    <div class="dispatchTimeline_stop_end_time">
                                                        <div>2020/02/02 11:11</div>
                                                    </div>
                                                </div>--%>
                                            </div>
                                        </div>
                                    </div>
                                    <div style="border-bottom: 1px solid #EFEFEF;margin-top: 25px;margin-bottom: 20px;position: relative">
                                        <div class="navigation posi">处置反馈</div>
                                    </div>
                                    <div class="row">
                                        <div class="col-md-6">
                                            <div class="form-group">
                                                <label class="control-label col-md-4"><strong>受害人数</strong></label>
                                                <div class="col-md-8">
                                                    <input class="form-control victim_num" readonly/>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="col-md-6">
                                            <div class="form-group">
                                                <label class="control-label col-md-4"><strong>死亡人数</strong></label>
                                                <div class="col-md-8">
                                                    <input class="form-control die_num" readonly/>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="row">
                                        <div class="col-md-6">
                                            <div class="form-group">
                                                <label class="control-label col-md-4"><strong>受伤人数</strong></label>
                                                <div class="col-md-8">
                                                    <input class="form-control bruise_num" readonly/>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="col-md-6">
                                            <div class="form-group">
                                                <label class="control-label col-md-4"><strong>经济损失</strong></label>
                                                <div class="col-md-8">
                                                    <input class="form-control financial_loss" readonly/>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="row">
                                        <div class="col-md-6">
                                            <div class="form-group">
                                                <label class="control-label col-md-4"><strong>涉及车辆数</strong></label>
                                                <div class="col-md-8">
                                                    <input class="form-control involve_car_num" readonly/>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="col-md-6">
                                            <div class="form-group">
                                                <label class="control-label col-md-4"><strong>损毁车辆数</strong></label>
                                                <div class="col-md-8">
                                                    <input class="form-control destroy_car_num" readonly/>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="row">
                                        <div class="col-md-6">
                                            <div class="form-group">
                                                <label class="control-label col-md-4"><strong>出动人数</strong></label>
                                                <div class="col-md-8">
                                                    <input class="form-control dispatch_person_num" readonly/>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="col-md-6">
                                            <div class="form-group">
                                                <label class="control-label col-md-4"><strong>出动车辆</strong></label>
                                                <div class="col-md-8">
                                                    <input class="form-control dispatch_car_num" readonly/>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="row">
                                        <div class="col-md-12">
                                            <div class="form-group">
                                                <label class="control-label col-md-2"><strong>事件起因</strong></label>
                                                <div class="col-md-10">
                                                    <input class="form-control incident_cause" readonly/>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="col-md-12">
                                            <div class="form-group">
                                                <label class="control-label col-md-2"><strong>处置要点</strong></label>
                                                <div class="col-md-10">
                                                    <input class="form-control dispose_point" readonly/>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="col-md-12">
                                            <div class="form-group">
                                                <label class="control-label col-md-2"><strong>经验教训</strong></label>
                                                <div class="col-md-10">
                                                    <input class="form-control experience" readonly/>
                                                </div>
                                            </div>
                                        </div>
                                    </div>

                                </div>
                            </form>
                        </div>
                    <div class="modal-footer" style="padding: 10px 15px;">
                        <button class="btn_cancel" data-dismiss="modal" aria-hidden="true">关闭</button>
                    </div>
            </div>
        </div>
    </div>
</div>
<%--自撤处理 按键 弹窗--%>
<div id="oneself_withdraw_box" class="modal fade" tabindex="-1" data-width="500">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">自撤事件</h4>
    </div>
    <div class="modal-body">
        <div style="text-align: center;height: 40px;line-height: 30px;font-size: 20px;">确定此事件已自行撤销，请填写处置说明：</div>
        <textarea rows="6" cols="49" style="width: 100%" id="oneselfWithdrawBoxTextarea"></textarea>
    </div>
    <div class="modal-footer">
        <button class="btn_confirm" id="oneselfWithdrawBoxYes">确定</button>
        <button class="btn_cancel" data-dismiss="modal" aria-hidden="true">取消</button>
    </div>
</div>

<%--事件详情  关闭事件模态框--%>
<div id="eventDetailCloseEventModal" class="modal fade" tabindex="-1" data-width="500" data-height="100">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">关闭事件</h4>
    </div>
    <div class="modal-body">
        <div style="text-align: center;height: 40px;line-height: 30px;font-size: 20px;">确定关闭此交通事件，请选择关闭原因：</div>

        <div style="display: flex;justify-content: center">
            <input type="radio" name="closeEventReason" value="1">错误事件  <input style="margin-left: 60px" type="radio" name="closeEventReason" value="2">重复事件 <input style="margin-left: 60px" type="radio" name="closeEventReason" value="3">其他
        </div>
    </div>
    <div class="modal-footer">
        <button type="button" class="btn_confirm" id="closeEventYes">确定</button>
        <button type="button" class="btn_cancel" data-dismiss="modal">取消</button>
    </div>
</div>
<%--信息发布 弹窗 设置发布内容--%>
<div id="setIssueContent_box" class="modal fade" tabindex="-1" data-width="700" data-height="100">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">发布信息</h4>
    </div>
    <div class="modal-body">
        <div class="row">
            <div class="col-md-6 setIssueContent_left" style="padding-right: 5px;">
                <div class="sic_left_header"><i></i><span>发布内容</span></div>
                <div class="sic_left_content">
                    <textarea id="issueInformationTe"></textarea>
                </div>
                <div class="sic_left_btn">
                  <input type="checkbox" value="" id="issueInformationCh"/>
                  <input type="text" oninput = "value=value.replace(/[^\d]/g,'')" id="issueInformationIn" class="sic_left_time">
                    <span class="sic_left_minutes">分钟</span>
                    <button type="button" class="btn_confirm" id="issueInformationSend" style="margin: 0 10px;">发送</button>
                    <button type="button" class="btn_cancel" id="issueInformationStop">结束</button>
                </div>
            </div>
            <div class="col-md-6 setIssueContent_right" style="padding-left: 5px;">
                <div class="sic_right_header"><i></i><span>诱导屏</span></div>
                <div class="sic_right_body">
                    <ul>
                        <%--<li>
                            <span class="sic_right_way">将军大道与诚信大道</span>
&lt;%&ndash;                            <span class="resend_span"></span>&ndash;%&gt;
&lt;%&ndash;                            <i class="setIssueGPS_green"></i>&ndash;%&gt;
                        </li>
                        <li>
                            <span class="sic_right_way">将军大道与诚信大道</span>
&lt;%&ndash;                            <span class="resend_span">重新发送</span>&ndash;%&gt;
&lt;%&ndash;                            <i class="setIssueGPS_red"></i>&ndash;%&gt;
                        </li>
                        <li>
                            <span class="sic_right_way">将军大道与诚信大道</span>
&lt;%&ndash;                            <span class="resend_span">重新发送</span>&ndash;%&gt;
&lt;%&ndash;                            <i class="setIssueGPS_red"></i>&ndash;%&gt;
                        </li>
                        <li>
                            <span class="sic_right_way">将军大道与诚信大道</span>
&lt;%&ndash;                            <span class="resend_span"></span>&ndash;%&gt;
&lt;%&ndash;                            <i class="setIssueGPS_green"></i>&ndash;%&gt;
                        </li>
                        <li>
                            <span class="sic_right_way">将军大道与诚信大道</span>
&lt;%&ndash;                            <span class="resend_span"></span>&ndash;%&gt;
&lt;%&ndash;                            <i class="setIssueGPS_green"></i>&ndash;%&gt;
                        </li>--%>
                    </ul>
                </div>
            </div>
        </div>
    </div>
    <div class="modal-footer">
        <span style="float: left"><font color="#FF0000">*</font>勾选代表一直发送；输入时间则固定多少分钟进行发送</span>
        <button type="button" class="btn_cancel" data-dismiss="modal">关闭</button>
    </div>
</div>
<%--总结 弹窗--%>
<div id="summaryBox" class="modal fade" tabindex="-1" data-width="1100" data-height="500">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">总结</h4>
    </div>
    <div class="modal-body">
        <form class="form-horizontal" role="form">
            <div class="form-body">
                <%--事件信息板块--%>
                <div style="border-bottom: 1px solid #EFEFEF;margin-bottom: 20px;margin-top: 5px;">
                    <div class="navigation posi">事件信息</div>
                </div>
                <div class="row">
                    <div class="col-md-3">
                        <div class="form-group">
                            <label class="control-label col-md-4">事件来源</label>
                            <div class="col-md-8">
                                <input class="form-control incidentSource" readonly/>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="form-group">
                            <label class="control-label col-md-4">发生时间</label>
                            <div class="col-md-8">
                                <input name="happenTime" class="form-control happenTime"/>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="form-group">
                            <label class="control-label col-md-4">事件类型</label>
                            <div class="col-md-8">
                                <select name="eventType" class="form-control input-sm eventType" style="margin-top: 0;height: 34px;"></select>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="form-group">
                            <label class="control-label col-md-4">事件子类</label>
                            <div class="col-md-8">
                                <select name="eventChildType" class="form-control input-sm eventChildType" style="margin-top: 0;height: 34px;"></select>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-3">
                        <div class="form-group">
                            <label class="control-label col-md-4">道路代码</label>
                            <div class="col-md-8">
                                <select name="roadCode" class="form-control input-sm roadCode" style="margin-top: 0;height: 34px;"></select>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="form-group">
                            <label class="control-label col-md-4" style="padding: 0px 8px;">路口路段/公里数</label>
                            <div class="col-md-8 crossCodeWrapper">
                                <select name="crossCode" class="form-control input-sm crossCode" style="margin-top: 0;height: 34px;"></select>
                            </div>
                            <div class="col-md-8 kilometersWrapper" style="display: none">
                                <input type="text" name="kilometers" oninput = "value=value.replace(/[^\d]/g,'')" class="form-control kilometers" maxlength="4">
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="form-group">
                            <label class="control-label col-md-4">所在方向</label>
                            <div class="col-md-8">
                                <select name="laneDirection" class="form-control input-sm laneDirection" style="margin-top: 0;height: 34px;"></select>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="form-group">
                            <label class="control-label col-md-4">事件等级</label>
                            <div class="col-md-8">
                                <select name="eventLevel" class="form-control input-sm eventLevel" style="margin-top: 0;height: 34px;"></select>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-12">
                        <div class="form-group">
                            <label class="control-label col-md-1">发生地点</label>
                            <div class="col-md-11">
                                <input name="incidentPlace" class="form-control incidentPlace"/>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-12">
                        <div class="form-group">
                            <label class="control-label col-md-1">报警内容</label>
                            <div class="col-md-11">
                                <textarea name="alarmContent" class="form-control alarmContent"/>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-3">
                        <div class="form-group">
                            <label class="control-label col-md-4">报警人</label>
                            <div class="col-md-8">
                                <input name="alarmPerson" class="form-control alarmPerson"/>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="form-group">
                            <label class="control-label col-md-4">联系电话</label>
                            <div class="col-md-8">
                                <input name="alarmPhone" class="form-control alarmPhone"/>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="form-group">
                            <label class="control-label col-md-4">报警时间</label>
                            <div class="col-md-8">
                                <input name="alarmTime" class="form-control alarmTime"/>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-3">
                        <div class="form-group">
                            <label class="control-label col-md-4">录入人</label>
                            <div class="col-md-8">
                                <input class="form-control createUserName" readonly/>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="form-group">
                            <label class="control-label col-md-4">录入时间</label>
                            <div class="col-md-8">
                                <input class="form-control createTime" readonly/>
                            </div>
                        </div>
                    </div>
                </div>
                <%--调度信息板块--%>
                <div style="border-bottom: 1px solid #EFEFEF;margin-bottom: 20px;margin-top: 15px;">
                    <div class="navigation posi">调度信息</div>
                </div>
                <div class="row">
                    <div style="width: 16.66%;height: 275px;float: left">
                        <span class="dispatchMan_img dispatchTimeline_head_portrait"></span>
                        <span class="dispatchMan_name"></span>
                        <div class="dispatchTimeline_left_block">
                            <div class="dispatchTimeline_block_box">
                                <div></div>
                            </div>
                        </div>
                    </div>
                    <div style="width: 83.33%;height: 275px;float: right;" class="optiscroll" id="summaryTimelineBox">
                        <div style="height: 100%;" class="allSteps"></div>
                    </div>
                </div>
                <%--归档反馈板块--%>
                    <div style="border-bottom: 1px solid #EFEFEF;margin-bottom: 20px;margin-top: 15px;">
                        <div class="navigation posi">归档反馈</div>
                    </div>
                    <div class="row">
                        <div class="col-md-3">
                            <div class="form-group">
                                <label class="control-label col-md-4">受害人数</label>
                                <div class="col-md-8">
                                    <input name="victimNum" class="form-control victimNum"/>
                                </div>
                            </div>
                        </div>
                        <div class="col-md-3">
                            <div class="form-group">
                                <label class="control-label col-md-4">死亡人数</label>
                                <div class="col-md-8">
                                    <input name="deathNum" class="form-control deathNum"/>
                                </div>
                            </div>
                        </div>
                        <div class="col-md-3">
                            <div class="form-group">
                                <label class="control-label col-md-4">受伤人数</label>
                                <div class="col-md-8">
                                    <input name="injuredNum" class="form-control injuredNum"/>
                                </div>
                            </div>
                        </div>
                        <div class="col-md-3">
                            <div class="form-group">
                                <label class="control-label col-md-4">经济损失</label>
                                <div class="col-md-8">
                                    <input name="ecoLoss" class="form-control ecoLoss"/>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-3">
                            <div class="form-group">
                                <label class="control-label col-md-4" style="padding-left: 8px;padding-right: 8px;">涉及车辆数</label>
                                <div class="col-md-8">
                                    <input name="vechicleNum" class="form-control vechicleNum"/>
                                </div>
                            </div>
                        </div>
                        <div class="col-md-3">
                            <div class="form-group">
                                <label class="control-label col-md-4" style="padding-left: 8px;padding-right: 8px;">损毁车辆数</label>
                                <div class="col-md-8">
                                    <input name="destroyedVechicle" class="form-control destroyedVechicle"/>
                                </div>
                            </div>
                        </div>
                        <div class="col-md-3">
                            <div class="form-group">
                                <label class="control-label col-md-4">出动人数</label>
                                <div class="col-md-8">
                                    <input name="dispatchPeople" class="form-control dispatchPeople"/>
                                </div>
                            </div>
                        </div>
                        <div class="col-md-3">
                            <div class="form-group">
                                <label class="control-label col-md-4">出动车辆</label>
                                <div class="col-md-8">
                                    <input name="dispatchVechicle" class="form-control dispatchVechicle"/>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12">
                            <div class="form-group">
                                <label class="control-label col-md-1">事件起因</label>
                                <div class="col-md-11">
                                    <textarea name="incidentReason" class="form-control incidentReason"/>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12">
                            <div class="form-group">
                                <label class="control-label col-md-1">处置要点</label>
                                <div class="col-md-11">
                                    <textarea name="disposeKey" class="form-control disposeKey"/>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12">
                            <div class="form-group">
                                <label class="control-label col-md-1">经验教训</label>
                                <div class="col-md-11">
                                    <textarea name="expSummary" class="form-control expSummary"/>
                                </div>
                            </div>
                        </div>
                    </div>
            </div>
        </form>
    </div>
    <div class="modal-footer">
        <button class="btn_confirm summarySave">保存</button>
        <button class="btn_save_Archive summarySaveAndFile">保存并归档</button>
        <button class="btn_cancel" data-dismiss="modal" aria-hidden="true">取消</button>
    </div>
</div>
<%--警情追加 弹窗--%>
<div id="conditionAddBox" class="modal fade" tabindex="-1" data-width="700" data-height="500">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">警情追加</h4>
    </div>
    <div class="modal-body">
        <form class="form-horizontal" role="form" id="conditionAddForm">
            <div class="form-body">
                <div class="row">
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">事件来源</label>
                            <div class="col-md-8">
                                <input name="incidentSource" class="form-control incidentSource" readonly/>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">发生时间</label>
                            <div class="col-md-8">
                                <input name="happenTime" class="form-control happenTime"/>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">事件类型</label>
                            <div class="col-md-8">
                                <select name="incidentType" class="form-control input-sm eventType" style="margin-top: 0;height: 34px;"></select>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">事件子类</label>
                            <div class="col-md-8">
                                <select name="incidentSubType" class="form-control input-sm eventChildType" style="margin-top: 0;height: 34px;"></select>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">道路代码</label>
                            <div class="col-md-8">
                                <select name="roadCode" class="form-control input-sm roadCode" style="margin-top: 0;height: 34px;"></select>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4" style="padding-left: 25px;padding-top: 0;">路口路段/公里数</label>
                            <div class="col-md-8 crossCodeWrapper">
                                <select name="crossCode" class="form-control input-sm crossCode" style="margin-top: 0;height: 34px;"></select>
                            </div>
                            <div class="col-md-8 kilometersWrapper" style="display: none">
                                <input type="text" name="kilometers" oninput = "value=value.replace(/[^\d]/g,'')" class="form-control kilometers" maxlength="4">
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">所在方向</label>
                            <div class="col-md-8">
                                <select name="laneDirection" class="form-control input-sm laneDirection" style="margin-top: 0;height: 34px;"></select>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">事件等级</label>
                            <div class="col-md-8">
                                <select name="incidentLevel" class="form-control input-sm eventLevel" style="margin-top: 0;height: 34px;"></select>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="row">
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">经度</label>
                            <div class="col-md-8">
                                <input type="number" name="longitude" class="form-control longitude"/>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">纬度</label>
                            <div class="col-md-8">
                                <input type="number" name="latitude" class="form-control latitude"/>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="row">
                    <div class="col-md-12">
                        <div class="form-group">
                            <label class="control-label col-md-2">发生地点</label>
                            <div class="col-md-10">
                                <input name="incidentPlace" class="form-control incidentPlace"/>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row alarmRelation">
                    <div class="col-md-12">
                        <div class="form-group">
                            <label class="control-label col-md-2">报警内容</label>
                            <div class="col-md-10">
                                <textarea name="alarmContent" class="form-control alarmContent"/>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row alarmRelation">
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">报警人</label>
                            <div class="col-md-8">
                                <input name="alarmPerson" class="form-control alarmPerson"/>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">联系电话</label>
                            <div class="col-md-8">
                                <input name="alarmPhone" class="form-control alarmPhone"/>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row alarmRelation">
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">报警时间</label>
                            <div class="col-md-8">
                                <input name="alarmTime" class="form-control alarmTime"/>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">事件图片</label>
                            <div class="col-md-8">
                                <button class="btn_cancel chooseImgs">上传图片</button>
                                <input id="conditionAddBoxImgFiles" type="file" multiple="multiple" accept="image/*" style="display: none">
                            </div>
                        </div>
                    </div>
                </div>

                <div class="row">
                    <div class="col-md-12">
                        <div class="form-group">
                            <div class="col-md-12" id="conditionAddBoxAllImgs">

                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </form>
    <div class="modal-footer">
        <button class="btn_confirm conditionAddBoxSave">保存</button>
        <button class="btn_cancel" data-dismiss="modal" aria-hidden="true">取消</button>
    </div>
    </div>
</div>
<%--装备设施ztree树 点击弹窗--%>
<div id="equipmentDetail" class="modal fade" tabindex="-1" data-width="700">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">装备设施</h4>
    </div>
    <div class="modal-body">
        <form class="form-horizontal" role="form">
            <div class="form-body">
                <div class="row">
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">装备类型</label>
                            <div class="col-md-8">
                                <input id="equipmentTyp" class="form-control" readonly="readonly"/>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">装备名称</label>
                            <div class="col-md-8">
                                <input id="equipmentNam" class="form-control" readonly="readonly"/>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">数量</label>
                            <div class="col-md-8">
                                <input id="equipSu" class="form-control" readonly="readonly"/>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">管理机构</label>
                            <div class="col-md-8">
                                <input id="equipOr" class="form-control" readonly="readonly"/>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">管理部门</label>
                            <div class="col-md-8">
                                <input id="equipDep" class="form-control" readonly="readonly"/>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">负责人</label>
                            <div class="col-md-8">
                                <input id="equipLeade" class="form-control" readonly="readonly"/>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">联系电话</label>
                            <div class="col-md-8">
                                <input id="equipTe" class="form-control" readonly="readonly"/>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </form>
    </div>
    <div class="modal-footer">
        <button class="btn_cancel" data-dismiss="modal" aria-hidden="true">关闭</button>
    </div>
</div>
<%--派警处置 弹窗--%>
<div id="dispatchedPoliceBox" class="modal fade" tabindex="-1" data-width="600">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">派警</h4>
    </div>
    <div class="modal-body">
        <form class="form-horizontal" role="form">
            <div class="form-body">
                <div class="row">
                    <div class="col-md-12">
                        <div class="form-group">
                            <label class="control-label col-md-3 inform_mode">通知方式</label>
                            <div class="col-md-2 mode_col2">
                                <label><input name="noticeMethod" type="checkbox" class="dispatched_mode_span" value="1"/>电话</label>
                            </div>
                            <div class="col-md-2 mode_col2">
                                <label><input name="noticeMethod" type="checkbox" class="dispatched_mode_span" value="2"/>对讲机</label>
                            </div>
                            <div class="col-md-2 mode_col2">
                                <label><input name="noticeMethod" type="checkbox" class="dispatched_mode_span" value="3"/>短消息</label>
                            </div>
                            <div class="col-md-3 mode_col2">
                                <label><input name="noticeMethod" type="checkbox" class="dispatched_mode_span" value="4"/>警务通消息</label>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-12">
                        <div class="form-group">
                            <label class="control-label col-md-3 sendMessageTitle">发送消息</label>
                            <div class="col-md-9">
                                <textarea id="policeMsgCon" class="form-control"/>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-12 text-right">
                        <button class="btn_confirm" id="sendMessage" style="display: none;">发送</button>
                    </div>
                </div>
                <div class="row" style="margin-top: 10px;height: 130px;">
                    <div class="col-md-12">
                        <div class="form-group">
                            <label class="control-label col-md-3 sendMessageTitle">出警人员</label>
                            <div class="col-md-9 police_contact_list_box">
                                <ul class="col-md-9 police_contact_list" id="policeContactList">
                                    <%--<li>
                                        <span class="polices_name_num">某某某（321000）</span>
                                        <span class="polices_phone">1234567890</span>
                                    </li>
                                    <li>
                                        <span class="polices_name_num">某某某（321000）</span>
                                        <span class="polices_phone">1234567890</span>
                                    </li>--%>
                                </ul>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </form>
    </div>
    <div class="modal-footer">
        <button class="btn_confirm" id="sendMsgYes">确定</button>
        <button class="btn_cancel" data-dismiss="modal" aria-hidden="true">取消</button>
    </div>
</div>

<div id="previewModal" class="modal fade" tabindex="-1" data-width="800" data-height="500">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">预览</h4>
    </div>
    <div class="modal-body">

    </div>
    <div class="modal-footer">
        <button class="btn_cancel" data-dismiss="modal" aria-hidden="true">取消</button>
    </div>
</div>

<iframe src="javascript:''" style="position:absolute; left:-10000px; top:-10000px; visibility:hidden;" id="downloadFormIframe"></iframe>

<!-- 加入线索经纬度选择地图 -->
<div class="modal fade" id="plateMapSelectMadal" tabindex="-2" data-width="920">
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">地图坐标选取</h4>
    </div>
    <div class="modal-body">
        <div id="plateSelectMap" class="form-body">
        </div>
        <div class="selectPoint">
            <div>
                <span>经度：</span>
                <span id="selectJ"></span>
            </div>
            <div>
                <span>纬度：</span>
                <span id="selectW"></span>
            </div>
        </div>
    </div>
    <div class="modal-footer">
        <div class="form-actions">
            <button id="plateBtnMapSelected" class="btn btn-default blue">确定</button>
        </div>
    </div>
</div>

<div id="seeTeamDetailModal" class="modal fade" tabindex="-1" data-width="1000" >
    <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
        <h4 class="modal-title">专家组详情</h4>
    </div>
    <div class="modal-body">
        <form class="form-horizontal" role="form">
            <div class="form-body">
                <div class="row">
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">专家组名称</label>
                            <div class="col-md-8">
                                <input class="zjamc form-control" readonly/>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">专家组组长</label>
                            <div class="col-md-8">
                                <input class="zjazz form-control" readonly/>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="control-label col-md-4">联系电话</label>
                            <div class="col-md-8">
                                <input class="zjzlxdh form-control" readonly/>
                            </div>
                        </div>
                    </div>

                </div>

                <div class="row">
                    <div class="col-md-12">
                        <div class="form-group">
                            <label class="control-label col-md-2">专业方向</label>
                            <div class="col-md-10">
                                <input class="zyfx form-control" readonly/>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="row">
                    <label class="control-label col-md-2">专家组成员</label>
                    <div class="col-md-10">
                        <div>
                            <div class="table-portlet whiteTable">
                                <table id="zjzDetailTable"  class="blue-table"></table>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-2"></div>
                </div>
            </div>
        </form>
    </div>
    <div class="modal-footer">
        <button class="btn_cancel" data-dismiss="modal" aria-hidden="true">关闭</button>
    </div>
</div>

<%--<script type="text/javascript" src="../../website-js/illegal/ztree/js/jquery.ztree.all.min.js"></script>--%>
<script type="text/javascript" src="../../website-js/illegal/illegalPublic.js"></script>
<script src="../../website-js/trafficControl/trafficeventScheduling-js.js"></script>
<script src="../../website-js/trafficControl/videoRecordingControl.js"></script>
<script src="../../website-js/trafficControl/liveVideoControl.js"></script>
<script src="../../website-js/trafficControl/trafficeventScheduling-command-js.js"></script>
<script type="text/javascript" src="../../website-js/trafficManger/videoMangerSchedule.js"></script>
<script>

</script>
```
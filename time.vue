<!--
    时间选择组件：
    tTop：界面顶部内容
    tBot：界面底部内容
    scrtor1，scrtor2，scrtor3：鼠标监听事件
    gap1，gap2，gap3：ul的位移样式（translate）监听事件
    select1，select2，select3：li点击选择事件
    callOff：取消按钮  confir：确定按钮
-->
<template>
<div class="time">
    <!--显示界面-->
    <div @click="seltimeClick">
        <h2 v-text="tTop"></h2>
        <p v-text="tBot"></p>
    </div>
    <!--遮板-->
    <div class="con" :style="shijianxuanze==true?'display:block':'display:none'">
    <!--选择时间模块-->
    <div class="modal-backdrop in" style="z-index:1040"></div>
        <div class="modal">
            <div class="c-layer">
                <div class="c-layer-box">
                    <!--顶部-->
                    <div class="hd">请选择时间</div>
                    <!--底部-->
                    <div class="bd">
                        <div class="c-roller">
                            <div class="iscroll-scroller" style="display: -webkit-box">
                                <!--日期-->
                                <div class="c-roller-bd">
                                    <div class="iscroll-wrap" style="overflow:hidden" >
                                        <div id="scrollbar" class="iscroll-scroller" style="overflow:auto" @mousewheel="moutor1" @scroll="scrtor1" >
                                            <ul class="ul-list"  :style="gap1">
                                                <li v-for="ti of timeD" :key="ti.index" :data-id="ti.index" v-text="ti.shijian" @click="select1(ti.index)" :class="num1==ti.index?'back':''"></li>
                                            </ul>
                                        </div>
                                    </div>
                                </div>
                                <!--小时-->
                                <div class="c-roller-bd">
                                    <div id="scrollbar" class="iscroll-wrap" style=" overflow:hidden" >
                                        <div id="scrollbar" class="iscroll-scroller" style="overflow:auto" @mousewheel="moutor2" @scroll="scrtor2">
                                            <ul class="ul-list" style="height:704px" :style="gap2">
                                                <li v-for="tie of timeE" :key="tie.index" :data-id="tie.index"  @click="select2(tie.index)" :class="num2==tie.index?'back':''">{{tie.shijain}}</li>
                                            </ul>
                                        </div>
                                    </div>
                                </div>
                                 <!--分钟-->
                                <div class="c-roller-bd">
                                    <div id="scrollbar" class="iscroll-wrap" style=" overflow:hidden" >
                                        <div id="scrollbar" class="iscroll-scroller" style="overflow:auto" @mousewheel="moutor3" @scroll="scrtor2">
                                            <ul class="ul-list" style="height:528px" :style="gap3">
                                                <li v-for="tif of timeF" :key="tif.index" :data-id="tif.index"  @click="select3(tif.index)" :class="num3==tif.index?'back':''">{{tif.shijain}}</li>
                                            </ul>
                                        </div>
                                    </div>
                                    <!--分钟-->
                                </div>
                            </div>
                        </div>
                        <!--确定与取消-->
                        <div class="c-btn">
                            <div class="c-btn-cancel" @click="callOff">取消</div>
                            <div class="c-btn-sure" @click="confir">确定</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
</template>
<script>
export default {
    data(){
        return {
            scroll:0,  //初始化滚动距离
            shijianxuanze:false,  //初始化时间选择组件的状态，默认不弹出
            tTop:"",  //初始化页面时间顶部内容
            tBot:"",  //初始化页面时间底部内容
            gap1:"",  //初始化第一个ul(日期和星期)的位移样式
            gap2:"",  //初始化第二个ul(小时)的位移样式
            gap3:"",  //初始化第三个ul(分钟)的位移样式
            scroll1:null,  //初始化第一个ul(日期和星期)滑轮滚动距离
            scroll2:null,  //初始化第二个ul(小时)滑轮滚动距离
            scroll3:null,  //初始化第三个ul(分钟)滑轮滚动距离
            num1:0,  //初始化选中的li的坐标，当选中的li的index与num1一样，添加选中效果
            num2:0,  //初始化选中的li的坐标，当选中的li的index与num2一样，添加选中效果
            num3:0,  //初始化选中的li的坐标，当选中的li的index与num3一样，添加选中效果
            timeD:[], //初始化第一个ul (日期和星期)循环内容
            timeE:[   //初始化第二个ul (小时)循环内容
                {shijain:'08',index:0},{shijain:'09',index:1},{shijain:10,index:2},{shijain:'11',index:3},{shijain:'12',index:4},{shijain:'13',index:5},{shijain:'14',index:6},{shijain:'15',index:7},{shijain:'16',index:8},{shijain:'17',index:9},{shijain:'18',index:10},{shijain:'19',index:11},{shijain:'20',index:12},{shijain:'21',index:13},{shijain:'22',index:14},{shijain:'23',index:15}],
            timeF:[ //初始化第三个ul (分钟)循环内容
                {shijain:'00',index:0},{shijain:'05',index:1},{shijain:10,index:2},{shijain:'15',index:3},{shijain:'20',index:4},{shijain:'25',index:5},{shijain:'30',index:6},{shijain:'35',index:7},{shijain:'40',index:8},{shijain:'45',index:9},{shijain:'50',index:10},{shijain:'55',index:11}
            ]
        }
    },
    created(){
         this.seltime(); //提前加载赋值第一个ul(日期和星期)的内容
    },
    methods:{
        moutor3:function(){ //解决当点击可视区域的底部或顶部li后，无法正常滚动的bug
            this.gap3= "transform:translate(0,0px)";
        },
        moutor2:function(){  //解决当点击可视区域的底部或顶部li后，无法正常滚动的bug
            this.gap2= "transform:translate(0,0px)";
        },
        moutor1:function(){  //解决当点击可视区域的底部或顶部li后，无法正常滚动的bug
            this.gap1= "transform:translate(0,0px)";
        },
        seltimeClick:function(){  //点击弹出时间组件
            this.shijianxuanze=true
        },
        callOff:function(){  //取消事件
            this.shijianxuanze = false;
        },
        confir:function(){  //确定事件
            var DD=this.timeD[this.num1].shijian;
            var EE=this.timeE[this.num2].shijain;
            var FF=this.timeF[this.num3].shijain;
            this.tTop=DD.slice(0,6);
            var wee=DD.slice(6);
            this.tBot=wee+" "+EE+":"+FF;
            this.shijianxuanze = false;
        },
        scrtor1:function(e){   //第一个ul的li的滚动事件
            this.scroll1=e.target.scrollTop;
        },
        scrtor2:function(e){  //第二个ul的li的滚动事件
            this.scroll2=e.target.scrollTop;
        }, 
        scrtor3:function(e){  //第三个ul的li的滚动事件
            this.scroll3=e.target.scrollTop;
        },
        select1(index){  //第一个ul的li的点击事件
            this.gap1= "transform:translate(0,"+((parseInt(index-2)*-44)+this.scroll1)+"px"+")";   //计算点击li时的移动距离，使其居中 
            this.num1=index;           // index-2*-的li高度+滚动的距离
        },
        select2(index){  //第二个ul的li的点击事件
            this.gap2= "transform:translate(0,"+((parseInt(index-2)*-44)+this.scroll2)+"px"+")";   //计算点击li时的移动距离，使其居中
            this.num2=index;           // index-2*-的li高度+滚动的距离
        },
        select3(index){  //第三个ul的li的点击事件
            this.gap3= "transform:translate(0,"+((parseInt(index-2)*-44)+this.scroll3)+"px"+")";  //计算点击li时的移动距离，使其居中
            this.num3=index;          // index-2*-的li高度+滚动的距离
        },
        filWeek:(week)=>{  //封装星期几的过滤器
            switch(week){
                    case 0:
                    week="星期日";
                    break;
                    case 1:
                    week="星期一";
                    break;
                    case 2:
                    week="星期二";
                    break;
                    case 3:
                    week="星期三";
                    break;
                    case 4:
                    week="星期四";
                    break;
                    case 5:
                    week="星期五";
                    break;
                    case 6:
                    week="星期六";
                    break;
                }
                return week;
        },
        seltime(){   //第一个ul内容的加载事件
            for(var i=0;i<30;i++){
                var timer=new Date();
                var d=timer.getDate();
                var m=timer.getMonth();
                timer.setDate(d+i)
                var nowDate=timer.getDate();
                var nowMonth=timer.getMonth()+1;
                var nowWeek=timer.getDay();
                nowWeek=this.filWeek(nowWeek);
                if(nowDate<10){nowDate="0"+nowDate};
                if(nowMonth<10){nowMonth="0"+nowMonth};
                this.timeD[i]={shijian:nowMonth+"月"+nowDate+"日"+nowWeek,index:i};
            };
            var x=new Date();
            var nMonth=x.getMonth()+1;
            var nDate=x.getDate();
            var nHours=x.getHours();
            var nMinutes=x.getMinutes();
            if(nMonth<10){nMonth="0"+nMonth};
            if(nMinutes<10){nMinutes="0"+nMinutes};
            if(nHours<10){nHours="0"+nHours};
            this.tTop=(nMonth)+"月"+nDate+"日";
            this.tBot=this.filWeek(x.getDay())+" "+nHours+":"+nMinutes
        },
    }
}
</script>
<style scoped>
.modal-backdrop.in {
    position:fixed;
    background:#000;
    opacity: .5;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
}
.modal {
    position: fixed;
    z-index: 1050;
    outline: 0;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    color: #333;
}
.c-layer {
    position: fixed;
    left: 50%;
    top: 50%;
    width: 300px;
    height: 350px;
    margin: -175px 0 0 -150px;
    z-index: 9999;
}
.c-layer-box {
    width: 300px;
    margin: auto;
    overflow: hidden;
    z-index: 9999;
}
.c-layer-box .hd {
    padding: 0 10px;
    height: 40px;
    line-height: 40px;
    color: #fff;
    background: #29b700;
    font-size: 14px;
    text-align: center;
}
.c-btn, .c-layer-box, .c-layer-box .hd, .c-roller, .c-roller-bd {
    position: relative;
}
.c-layer-box .bd {
    padding: 0;
    background: #fff;
}
.c-roller {
    width: 100%;
    overflow: hidden;
}
.c-btn {
    width: 100%;
    overflow: hidden;
    background: #fff;
    border-top: 1px solid #e5e5e5;
    font-size: 1.6em;
    text-align: center;
}
.c-roller-bd {
    -webkit-box-flex: 1;
    height: 220px;
    overflow:hidden;
}
#dateContentWrap, #ruleContentWrap, #timeContentWrap, .iscroll-wrap {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 100;
}
.iscroll-wrap{
    overflow:hidden
}
.date-select, .iscroll-scroller, .time-select {
    position: relative;
    width: 100%;
}
.c-roller-bd:nth-of-type(1) {
    -webkit-box-flex: 2;
}
.iscroll-scroller {
    margin-top:0;
}
.iscroll-scroller{
    position: relative;
    width: 100%;
}
 ul.ul-list{
    height:1408px;
 }

 .ul-list li {
    font-size: 14px;
    text-align: center;
}
.ul-list li, header {
    line-height: 44px;
    height: 44px;
}
.ul-list li {
    color: #999;
}
.back{
    background-color: #f7f7f7;
    font-size:17px!important;
    color:#000 !important;
}
.c-btn {
    position: relative;
    font-size: 14px;
    text-align: center;
    width: 100%;
    overflow: hidden;
    background: #fff;
    border-top: 1px solid #e5e5e5;
    display: -webkit-box;
}
.c-btn>div {
    -webkit-box-flex: 1;
    width: 50%;
    line-height: 40px;
}
.c-btn .c-btn-sure {
    color: #ff7e00;
}
.c-btn>div:nth-of-type(2) {
    border-left: 1px solid #eaeaea;
}
.c-roller-bd {
    -webkit-box-flex: 1;
    height: 220px;
    overflow: hidden;
}
.c-btn, .c-layer-box, .c-layer-box .hd, .c-roller, .c-roller-bd {
    position: relative;
}
#scrollbar::-webkit-scrollbar{
    display:none;
}
</style>
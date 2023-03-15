<template>
    <div class="main">
        <link href="http://213.232.124.150:63839/css/all.css" rel="stylesheet" />
        <div class="row pt-4">
            <div class="col-12 pb-2">
                <h2 class="h3 g-dashboard font-16"> ارزیابی عملکرد</h2>
            </div>
            <div class="col-12">
                <div class="d-flex gap-2">
                    <span class="border-title border-title-blue d-block"></span>
                    <span class="border-title border-title-gray d-block"></span>
                </div>
            </div>
        </div>
        <div class="rounded-1 bg-e6e4e9 mt-3 p-3">
            <span> درس و مبحث را برای مشاهده عملکرد خود انتخاب نمایید:</span>
            <div class="row">
                <div class="p-1 col-md-3">
                    <q-select
                        placeholder="انتخاب درس"
                        v-model="lessons"
                        :options="lessonOptions"
                        class="rounded-1 p-0 m-0 mb-1"
                    />
                </div>
                <div class="p-1 col-md-3">
                <q-select
                    placeholder="انتخاب مبحث"
                    v-model="articles"
                    :options="articleOptions"
                    class="rounded-1 p-0 m-0 mb-1"
                />
                </div>
                <div class="p-1 col-md-3">
                <q-select
                    placeholder="انتخاب زیر مبحث"
                    v-model="articles2"
                    :options="articleOptions2"
                    class="rounded-1 p-0 m-0 mb-1"
                />
                </div>
                <div class="p-1 col-md-3">
                <q-select
                    placeholder="انتخاب زیر مبحث"
                    v-model="articles3"
                    :options="articleOptions3"
                    class="rounded-1 p-0 m-0 mb-1"
                />
                </div>
            </div>
        </div>
        <div class="rounded-1 bg-e6e4e9 p-3 mt-3">
            &nbsp;مشاهده ارزیابی عملکرد شما &nbsp;
            در مبحث: 
            <span class="c-7244b0 ">.....</span>
            {{articleOptions3[articles3!==''?articles3:0].label}}
            <span class="c-7244b0 ">.....</span>
            &nbsp;&nbsp;
            در درس:
            <span class="c-7244b0 ">.....</span>
            {{lessonOptions[lessons!==''?lessons:0].label}}
            <span class="c-7244b0 ">.....</span>
            <div class="mt-3">
                <div class="bFirst p-3">
                    میزان اهمیت این مبحث در کنکور سراسری:
                    <div class="row c-01a4ab mt-3">
                        <div class="col-md-4">
                            <div>
                                در کنکور ۱۴۰۰:
                                <span class="c-000000">10%</span>
                                از سوالات درس ۱ از این مبحث بوده است.
                            </div>
                            <div>
                                در کنکور ۱۳۹۹:
                                <span class="c-000000">10%</span>
                                از سوالات درس ۱ از این مبحث بوده است.
                            </div>
                        </div>
                        <div class="col-md-4">
                            <div>
                                در کنکور ۱۳۹۸:
                                <span class="c-000000">10%</span>
                                از سوالات درس ۱ از این مبحث بوده است.
                            </div>
                            <div>
                                در کنکور ۱۳۹۷:
                                <span class="c-000000">10%</span>
                                از سوالات درس ۱ از این مبحث بوده است.
                            </div>
                        </div>
                        <div class="col-md-4">
                            <div>
                                در کنکور ۱۳۹۶:
                                <span class="c-000000">10%</span>
                                از سوالات درس ۱ از این مبحث بوده است.
                            </div>
                            <div>
                                در کنکور ۱۳۹۵:
                                <span class="c-000000">10%</span>
                                از سوالات درس ۱ از این مبحث بوده است.
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row mt-3">
                    <div class="col-md-4">
                        <div class="bThird">    
                            <div class="m-2 bg-light-blue rounded-1 p-2">
                                <span class="c-7244b0 fw-bold">وضعیت مطالعه این مبحث:</span>
                                <div class="text-start fw-bold">آن را فرا گرفته اید.</div>
                            </div>
                            <div class="m-2 p-2">
                                <span class="c-7244b0 fw-bold">طبق برنامه مطالعاتی:</span>
                                <div class="">تا کنون ۲۰ ساعت فرصت مطالعاتی این مبحث را داشته اید.</div>
                            </div>
                            <div class="m-2 p-2">
                                <span class="c-7244b0 fw-bold">و عملکرد مطالعاتی شما:</span>
                                <div class="">تا کنون ۱۸:۳۰ ساعت زمان صرف مطالعه این مبحث کرده اید</div>
                            </div>
                            <div class="m-2 p-2 bg-ffffff">
                                <table style="width:100%;" id="table">
                                    <thead id="tableHead">
                                    <tr height="50px" style="width:100%">
                                        <td style="text-align:center; font-weight:bold" >
                                        <div class="d-flex flex-column align-items-center">
                                            <span>عملکرد شما</span>
                                        </div>
                                        </td>
                                        <td style="text-align:center; font-weight:bold" >
                                        <div class="d-flex flex-column align-items-center">
                                            <span>طبق برنامه</span>
                                        </div>
                                        </td>
                                    </tr>
                                    </thead>
                                    <tbody>
                                    <tr height="50px" style="width:100%">
                                        <td></td>
                                        <td></td>
                                        <td>یادگیری</td>
                                    </tr>
                                    <tr height="50px" style="width:100%">
                                        <td></td>
                                        <td></td>
                                        <td>تثبیت</td>
                                    </tr>
                                    <tr height="50px" style="width:100%">
                                        <td></td>
                                        <td></td>
                                        <td>یادآوری</td>
                                    </tr>
                                    </tbody>
                                </table> 
                            </div>
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="bThird">
                            <div class="m-2 bg-light-blue rounded-1 p-2">
                                <span class="c-7244b0 fw-bold">وضعیت در آزمون ها:</span>
                                <div class="text-start fw-bold">مسلط هستید.</div>
                            </div>
                            <div class="m-2 p-2">
                                <span class="c-7244b0 fw-bold">طبق برنامه مطالعاتی:</span>
                                <div class="">تا کنون ۲۵ سوال در ۵ آزمون از این مبحث را شرکت کرده اید.</div>
                            </div>
                            <div class="m-2 p-2">
                                <span class="c-7244b0 fw-bold">و عملکرد شما در آزمون ها:</span>
                                <div class="">عملکرد شما در آزمون های داده شده به شرح زیر است:</div>
                            </div>
                            <div class="m-2 p-2 bg-ffffff stickyScrollable-left" style="max-height: 240px; overflow-y: scroll">
                                <table style="width:100%;" id="table">
                                    <thead id="tableHead">
                                    <tr height="50px" style="width:100%">
                                        <td style="text-align:center; font-weight:bold" >
                                        <div class="d-flex flex-column align-items-center">
                                            <span>تعداد نزده</span>
                                        </div>
                                        </td>
                                        <td style="text-align:center; font-weight:bold" >
                                        <div class="d-flex flex-column align-items-center">
                                            <span>تعداد غلط</span>
                                        </div>
                                        </td>
                                        <td style="text-align:center; font-weight:bold" >
                                        <div class="d-flex flex-column align-items-center">
                                            <span>تعداد صحیح</span>
                                        </div>
                                        </td>
                                        <td style="text-align:center; font-weight:bold" >
                                        <div class="d-flex flex-column align-items-center">
                                            <span>تعداد کل سوالات</span>
                                        </div>
                                        </td>
                                    </tr>
                                    </thead>
                                    <tbody>
                                    <tr height="50px" style="width:100%">
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td>آزمون یادگیری ۳ مهر</td>
                                    </tr>
                                    <tr height="50px" style="width:100%">
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td>آزمون یادگیری ۳ مهر</td>
                                    </tr>
                                    <tr height="50px" style="width:100%">
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td>آزمون یادگیری ۳ مهر</td>
                                    </tr>
                                    </tbody>
                                </table> 
                            </div>
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="bThird">
                            <div class="m-2 p-2 text-center">
                                <span class="c-7244b0 fw-bold">حالاتی که بعد از مطالعه این مبحث ثبت کرده اید:</span>
                            </div>
                            <div class="m-2 p-2 bg-ffffff stickyScrollable-left" style="max-height: 420px; overflow-y: scroll">
                                <table style="width:100%;" id="table">
                                    <thead id="tableHead">
                                    <tr height="50px" style="width:100%">
                                        <td style="text-align:center; font-weight:bold" >
                                        <div class="d-flex flex-column align-items-center">
                                            <span> میزان علاقمندی</span>
                                        </div>
                                        </td>
                                        <td style="text-align:center; font-weight:bold" >
                                        <div class="d-flex flex-column align-items-center">
                                            <span>حالت</span>
                                        </div>
                                        </td>
                                        <td style="text-align:center; font-weight:bold" >
                                        <div class="d-flex flex-column align-items-center">
                                            <span>نوع مطالعه </span>
                                        </div>
                                        </td>
                                        <td style="text-align:center; font-weight:bold" >
                                        <div class="d-flex flex-column align-items-center">
                                            <span> میزان مطالعه </span>
                                        </div>
                                        </td>
                                    </tr>
                                    </thead>
                                    <tbody>
                                    <tr height="50px" style="width:100%">
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td>  ۳ مهر</td>
                                    </tr>
                                    <tr height="50px" style="width:100%">
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td>  ۳ مهر</td>
                                    </tr>
                                    <tr height="50px" style="width:100%">
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td>  ۳ مهر</td>
                                    </tr>
                                    <tr height="50px" style="width:100%">
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td>  ۳ مهر</td>
                                    </tr>
                                    <tr height="50px" style="width:100%">
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td>  ۳ مهر</td>
                                    </tr>
                                    <tr height="50px" style="width:100%">
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td>  ۳ مهر</td>
                                    </tr>
                                    <tr height="50px" style="width:100%">
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td>  ۳ مهر</td>
                                    </tr>
                                    <tr height="50px" style="width:100%">
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td>  ۳ مهر</td>
                                    </tr>
                                    <tr height="50px" style="width:100%">
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td></td>
                                        <td>  ۳ مهر</td>
                                    </tr>
                                    </tbody>
                                </table> 
                            </div>
                        </div>
                    </div>
                </div>
                <div class="mt-3">
                    <div class="row">
                        <div class="col-md-6">
                            <div class="bSecond">
                                <div class="m-2 p-2 text-center">
                                        <span class="c-000000 fw-bold">میزان تسلط شما</span>
                                </div>
                                <div class="m-2 bg-79c9cb rounded-1 c-01a4ab p-2 px-3">
                                    <div>طبق آخرین داده های سیستمی:</div>
                                    <div> میزان تسلط شما بر این مبحث ۷۰٪ است.</div>
                                </div>
                                <div class="m-2 p-2 bg-ffffff">
                                        <table style="width:100%;" id="table">
                                            <thead id="tableHead">
                                            <tr height="50px" style="width:100%">
                                                <td style="text-align:center; font-weight:bold" >
                                                <div class="d-flex flex-column align-items-center">
                                                    <span>نظر مشاور</span>
                                                </div>
                                                </td>
                                                <td style="text-align:center; font-weight:bold" >
                                                <div class="d-flex flex-column align-items-center">
                                                    <span>نظر شما</span>
                                                </div>
                                                </td>
                                                <td style="text-align:center; font-weight:bold" >
                                                <div class="d-flex flex-column align-items-center">
                                                    <span> روز</span>
                                                </div>
                                                </td>
                                            </tr>
                                            </thead>
                                            <tbody>
                                            <tr height="50px" style="width:100%">
                                                <td></td>
                                                <td></td>
                                                <td>۱ مهر</td>
                                            </tr>
                                            <tr height="50px" style="width:100%">
                                                <td></td>
                                                <td></td>
                                                <td>۱۵ آبان</td>
                                            </tr>
                                            <tr height="50px" style="width:100%">
                                                <td></td>
                                                <td></td>
                                                <td>۳۰ آذر</td>
                                            </tr>
                                            </tbody>
                                        </table> 
                                    </div>
                                    <div style="width:80%; margin-left:auto; margin-right:auto; height: 2px; background-color:#01a4ab;" class="mt-3 mb-3"></div>
                                    <div class="d-flex flex-row justify-between p-3">
                                        <div>ثبت میزان تسلط, جدید:</div>
                                        <div>
                                            <div class="bg-79c9cb rounded-1 c-01a4ab p-2 px-3 text-center">مسلطم</div>
                                            <input type="range" id="vol" name="vol" min="0" max="50" style="direction: ltr; margin-top: 10px;">
                                        </div>
                                        <div class="rounded-1 bg-dark c-ffffff d-flex flex-row align-items-center justify-content-center font-20 w-25">
                                            ثبت
                                        </div>
                                    </div>
                            </div>
                        </div>
                        <div class="col-md-6">
                            <div class="bSecond">
                                <div class="m-2 p-2 text-center">
                                        <span class="c-000000 fw-bold">میزان علاقمندی شما</span>
                                </div>
                                <div class="m-2 bg-79c9cb rounded-1 c-01a4ab p-2 px-3">
                                    <div>طبق آخرین داده های سیستمی:</div>
                                    <div> میزان تسلط شما بر این مبحث ۷۰٪ است.</div>
                                </div>
                                <div class="m-2 p-2 bg-ffffff">
                                        <table style="width:100%;" id="table">
                                            <thead id="tableHead">
                                            <tr height="50px" style="width:100%">
                                                <td style="text-align:center; font-weight:bold" >
                                                <div class="d-flex flex-column align-items-center">
                                                    <span>نظر مشاور</span>
                                                </div>
                                                </td>
                                                <td style="text-align:center; font-weight:bold" >
                                                <div class="d-flex flex-column align-items-center">
                                                    <span>نظر شما</span>
                                                </div>
                                                </td>
                                                <td style="text-align:center; font-weight:bold" >
                                                <div class="d-flex flex-column align-items-center">
                                                    <span> روز</span>
                                                </div>
                                                </td>
                                            </tr>
                                            </thead>
                                            <tbody>
                                            <tr height="50px" style="width:100%">
                                                <td></td>
                                                <td></td>
                                                <td>۱ مهر</td>
                                            </tr>
                                            <tr height="50px" style="width:100%">
                                                <td></td>
                                                <td></td>
                                                <td>۱۵ آبان</td>
                                            </tr>
                                            <tr height="50px" style="width:100%">
                                                <td></td>
                                                <td></td>
                                                <td>۳۰ آذر</td>
                                            </tr>
                                            </tbody>
                                        </table> 
                                    </div>
                                    <div style="width:80%; margin-left:auto; margin-right:auto; height: 2px; background-color:#01a4ab;" class="mt-3 mb-3"></div>
                                    <div class="d-flex flex-row justify-between p-3">
                                        <div>ثبت میزان علاقمندی, جدید:</div>
                                        <div>
                                            <div class="bg-79c9cb rounded-1 c-01a4ab p-2 px-3 text-center">علاقه مندم</div>
                                            <input type="range" id="vol" name="vol" min="0" max="50" style="direction: ltr; margin-top: 10px;">
                                        </div>
                                        <div class="rounded-1 bg-dark c-ffffff d-flex flex-row align-items-center justify-content-center font-20 w-25">
                                            ثبت
                                        </div>
                                    </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import axios from "src/plugins/axios";
    import axiosQuestion from "src/plugins/axiosQuestion";
    import { vxm } from "src/store";
    import { Notify } from 'quasar'
    import { LocalStorage } from "quasar";
    import router from "../../router";
import { min } from "moment";
import { networkInterfaces } from "os";
    

    export default {
    data() {
        return {
            articleOptions:[{label:'بحث۱', value:0},{label:'بحث۲', value:1},{label:'بحث۳', value:2},{label:'بحث۴', value:3},
            {label:'بحث۵', value:4},{label:'بحث۶', value:5},{label:'بحث۷', value:6}],
            articles:'',
            articleOptions2:[{label:'بحث۱', value:0},{label:'بحث۲', value:1},{label:'بحث۳', value:2},{label:'بحث۴', value:3},
            {label:'بحث۵', value:4},{label:'بحث۶', value:5},{label:'بحث۷', value:6}],
            articles2:'',
            articleOptions3:[{label:'بحث۱', value:0},{label:'بحث۲', value:1},{label:'بحث۳', value:2},{label:'بحث۴', value:3},
            {label:'بحث۵', value:4},{label:'بحث۶', value:5},{label:'بحث۷', value:6}],
            articles3:'',
            lessonOptions:[{label:'درس۱', value:0},{label:'درس۲', value:1},{label:'درس۳', value:2},{label:'درس۳-۱ ', value:3},
            {label:'درس۴', value:4},{label:'درس۵', value:5},{label:'درس۶', value:6}],
            lessons:''
        };
    },
    watch: {
        
        template: {
            handler: function (val, oldVal) {
              
                let hour = 0
                let minute = 0
                let hour2 = 0
                let minute2 = 0
                let finalH = 0
                let finalM = 0
                for(let i=0 ;i<val.length; i++){
                    hour = parseInt(val[i].stTime.startClock.split(':')[0])
                    minute = parseInt(val[i].stTime.startClock.split(':')[1])
                    hour2 = parseInt(val[i].stTime.endClock.split(':')[0])
                    minute2 = parseInt(val[i].stTime.endClock.split(':')[1])
                    finalH = hour2-hour 
                    finalH<0?finalH = hour2+(24-hour):''
                    finalM = minute2-minute
                    if(finalM>=60){
                        finalH+=finalM/60
                        finalM = finalM%60
                    }else if(finalM<0){
                        finalM = minute2+(60-minute)
                        finalH-=1
                    }
                    finalH == -1? finalH = 23 : ''
                    val[i].stTime.doneHoure = finalH
                    val[i].stTime.doneMinute = finalM
                }
            },
            deep: true
        },
        startClock(newH, oldH){
            let hour = parseInt(newH.split(':')[0])
            let minute = parseInt(newH.split(':')[1])
            let hour2 = parseInt(this.endClock.split(':')[0])
            let minute2 = parseInt(this.endClock.split(':')[1])
            let finalH = hour2-hour 
            finalH<0?finalH = hour2+(24-hour):''
            let finalM = minute2-minute
            if(finalM>=60){
                finalH+=finalM/60
                finalM = finalM%60
            }else if(finalM<0){
                finalM = minute2+(60-minute)
                finalH-=1
            }
            finalH == -1? finalH = 23 : ''
            this.doneHoure = finalH
            this.doneMinute = finalM
        },
        endClock(newH, oldH){
            let hour2 = parseInt(newH.split(':')[0])
            let minute2 = parseInt(newH.split(':')[1])
            let hour = parseInt(this.startClock.split(':')[0])
            let minute = parseInt(this.startClock.split(':')[1])
            let finalH = hour2-hour 
            finalH<0?finalH = hour2+(24-hour):''
            let finalM = minute2-minute
            if(finalM>=60){
                finalH+=finalM/60
                finalM = finalM%60
            }else if(finalM<0){
                finalM = minute2+(60-minute)
                finalH-=1
            }
            finalH == -1? finalH = 23 : ''
            this.doneHoure = finalH
            this.doneMinute = finalM
        }
    },
    methods:{
    calcDoneTime(){
        let hour = this.endClock - this.startClock
        if(hour<0){
            this.doneHoure = this.endClock+(24-this.startClock)
        }
        let minute = this.endMinute - this.startMinute
        if(minute<0){
            this.doneMinute = this.endMinute+(60-this.startMinute)
        }
      
    }
    },
    mounted(){
        
    }
}
</script>
<style>

.bThird{
    border: 2px solid #7244b0;
    border-radius: 5px;
    background-color: #d0c7e5;
    max-height: 550px;
    padding-right:0px !important ;
}
.bSecond{
    border: 2px solid #01a4ab;
    border-radius: 5px;
    /* width: 49.5%; */
    background-color: #d8f2f1;
    padding-right:0px !important ;
}
.bFirst{
    border: 2px solid gray;
    border-radius: 5px;
    background-color: #d7d7d7;

}
table {
    direction: ltr;
      border-collapse: collapse;
    font-family: Tahoma, Geneva, sans-serif;
  }
  table td {
    height: 20px !important;
    width: 20px !important;
    text-align: center;
  }
  table thead td {
      background-color: #54585d;
      color: #ffffff;
      font-size: 13px;
      border: 1px solid #54585d;
    position: -webkit-sticky;
    position: sticky;
    top: 0;
    z-index: 2000;
  }
  table thead tr {
    position: -webkit-sticky;
    position: sticky;
    top: 0;
    z-index: 2000;
  }
  table tbody td {
      color: #636363;
      border: 1px solid #dddfe1;
  }
  table tbody tr {
      background-color: #f9fafb;
  }
  table tbody tr:nth-child(odd) {
      background-color: #ffffff;
  }
  .sticky {
  position: -webkit-sticky;
  position: sticky;
  top: 0px;
}

.stickyScrollable-left {
  height: 600px;
  overflow-y: scroll;
  position: -webkit-sticky;
  position: sticky;
  top: 0px;
}

.stickyScrollable-left::-webkit-scrollbar {
  width: 8px;
}

.stickyScrollable-left::-webkit-scrollbar-track {
  background: gray;
  border-left: 3px solid white;
  border-right: 3px solid white;
}

.stickyScrollable-left::-webkit-scrollbar-thumb {
  border-radius: 10px;
  background: gray;
  -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.5);
  /*background-image:url( http://i.imgur.com/ygGobeC.png);*/
}

.stickyScrollable-left .item {
  height: 20px;
  margin-bottom: 5px;
  background: gray;
}

.stickyScrollable-left .item:last-child {
  margin-bottom: 0;
}

.stickyScrollable-left {
  direction: rtl;
}

.stickyScrollable {
  height: 600px;
  overflow-y: scroll;
  position: -webkit-sticky;
  position: sticky;
  top: 0px;
}

.stickyScrollable::-webkit-scrollbar {
  width: 8px;
}

.stickyScrollable::-webkit-scrollbar-track {
  background: #2899f4;
  border-left: 3px solid white;
  border-right: 3px solid white;
}

::-webkit-scrollbar-thumb {
  border-radius: 10px;
  background: #1976d2;
  -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.5);
  /*background-image:url( http://i.imgur.com/ygGobeC.png);*/
}

.stickyScrollable .item {
  height: 20px;
  margin-bottom: 5px;
  background: gray;
}

.stickyScrollable .item:last-child {
  margin-bottom: 0;
}

.stickyScrollable {
  direction: rtl;
}

.overflow::-webkit-scrollbar {
  display: none;
}
</style>
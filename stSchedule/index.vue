<template>
    <div class="row">
        <bs-modal :show="showTaskTable" size="md" @open="showTaskTable" @close="!showTaskTable">
          <template class="bs-modal-body">
            <div class="p-2 bg-e6e4e9 h-100 rounded-1 w-50 m-auto">
                <div class="text-center c-01a4ab fw-bold">اقدامات مطالعاتی امروز من, در یک نگاه</div>
                <div class="d-flex flex-column justify-content-center align-items-center">
                    <div class="bg-01a4ab rounded-2 p-3 w-50 c-ffffff text-center mt-3 fw-bold font-14">
                            شروع ۷ صبح
                    </div>
                    <div class="d-flex flex-row justify-content-center align-items-center w-75">
                        <div class="line half"></div>
                    </div>
                </div>
                <div v-for="tem in template" class="d-flex flex-column justify-content-center align-items-center" :key="tem">
                    <div class="time">
                        {{tem.time}}
                    </div>
                    <div class="d-flex flex-row justify-content-center align-items-center w-75">
                        <div class="dars">
                            {{tem.dars}}
                            <div class="font-12 text-end">
                                {{tem.status}}
                            </div>
                        </div>
                        <div class="line"></div>
                        <div class="baaze">
                            {{tem.baaze}}
                        </div>
                    </div>
                </div>
                <div class="d-flex flex-column justify-content-center align-items-center">
                    <div class="bg-01a4ab rounded-2 p-3 w-50 c-ffffff text-center fw-bold font-14">
                            پایان ۲۴ شب
                    </div>
                </div>
            </div>
          </template>
          <template slot="footer">
            <div class=" d-flex flex-row justify-content-around w-50 mx-auto">
              <!-- <div class="bg-success rounded-1 c-ffffff text-center cursor-pointer p-2 px-4" @click="showTaskTable">بلی</div> -->
              <div class="bg-danger rounded-1 c-ffffff text-center cursor-pointer p-2 px-4" @click="showTaskTable = false">بازگشت</div>
            </div>
          </template>
        </bs-modal>
        <div class="cursor-pointer bg-light-blue rounded-1 p-2 c-ffffff fw-bold my-1 me-3 text-center" style="width:25%" @click="showTaskTable = true">مشاهده برنامه در یک نگاه</div>
        <div class="col-md-12">
            <div class="p-3 bg-e6e4e9 h-auto rounded-1">
                <div v-for="tem in template" class="d-flex flex-column" :key="tem">
                    <div class="d-flex flex-row justify-content-start align-items-center font-20 fw-bold c-888888 me-3">
                        <div style="flex:1">{{tem.time}}</div>
                        <div class="dashed-time ms-3"></div>
                    </div>
                    <div class="d-flex flex-row align-self-center justify-content-center align-items-center bg-ffffff rounded-1" style="width:85%; margin-right: 10%">
                        <div class="left-line" :class="tem.color"></div>
                        <div class="whatToDo">
                            <div style="flex:1; font-size: 16px; font-weight: bold;" :class="'c'+tem.color">{{tem.dars}}</div>
                            <div style="flex:1;">&nbsp;</div>
                            <div style="flex:3;">&nbsp;</div>
                            <div style="flex:3;">&nbsp;</div>
                            <div style="flex:3;">&nbsp;</div>
                            <div style="flex:1; font-size: 12px; font-weight: bold;">
                                مدت زمان انجام: {{tem.timeLength}} 
                            </div>
                        </div>
                        <div class="details">
                            <div class="timeContainer">
                                <div class="d-flex flex-column">
                                    <div class="d-flex flex-row justify-content-between align-items-center">
                                        <span class="c-gray font-14 c-ff7b1a">ساعت شروع:</span>
                                        <div class="d-flex flex-row justify-content-between align-items-center rounded-1" style="border:2px solid gray">
                                            <input type="time" v-model="tem.stTime.startClock" style="padding:3px;"/>
                                        </div>
                                    </div>
                                    <div class="d-flex flex-row justify-content-between align-items-center mt-1">
                                        <span class="c-gray font-14 c-ff7b1a">ساعت پایان:</span>
                                        <div class="d-flex flex-row justify-content-between align-items-center rounded-1" style="border:2px solid gray">
                                            <input type="time" v-model="tem.stTime.endClock" style="padding:3px;"/>
                                        </div>
                                    </div>
                                    <div class="font-12 me-2 w-100 mt-2">
                                        <div>
                                            مدت زمانی که شما انجام داده اید:
                                            <span class="fw-bold">
                                            {{tem.stTime.doneMinute+' : '+tem.stTime.doneHoure}}
                                            </span>
                                        </div>
                                </div>
                                </div>
                            </div>
                            <div class="m-2 p-2 bg-ffffff" v-if="tem.test!==null">
                                <table>
                                    <tbody>
                                    <tr height="10px" style="width:70%">
                                        <td  class="w-25">
                                            <input type="number" v-model="tem.test.kolTest" style="width:70%"/>
                                        </td>
                                        <td>تعداد کل تست</td>
                                    </tr>
                                    <tr height="10px" style="width:70%">
                                        <td class="w-25">
                                            <input type="number" v-model="tem.test.trueTest" style="width:70%"/>
                                        </td>
                                        <td>توانستم</td>
                                    </tr>
                                    <tr height="10px" style="width:70%">
                                        <td class="w-25">
                                            <input type="number" v-model="tem.test.falseTest" style="width:70%"/>
                                        </td>
                                        <td>یادگرفتم</td>
                                    </tr>
                                    <tr height="10px" style="width:70%">
                                        <td  class="w-25">
                                            <input type="number" v-model="tem.test.noneTest" style="width:70%"/>
                                        </td>
                                        <td>نتوانستم</td>
                                    </tr>
                                    </tbody>
                                </table> 
                            </div>
                             <div class="m-2 p-2 bg-ffffff" v-if="tem.test!==null">
                                <q-select
                                    placeholder="انتخاب منبع تست"
                                    v-model="tem.test.source"
                                    :options="testSourceOptions"
                                    class="bg-ffffff rounded-1 p-0 m-0 font-12 mb-1 selectWidth"
                                />
                                <div class="font-12 mt-2">جزییات</div>
                                <input v-model="tem.test.sourceDetail" style="margin-right: 10px; border:1px solid gray" class="inputClass mt-1"/>
                            </div>
                        </div>
                        <div class="report ">
                            <div style="flex:1; display: flex; justify-content:end ;"><img src="./../../assets/img/book.png" width="80%" height="auto" style=" border-radius: 10px; margin-top:5px;"/></div>
                            <div style="flex:1; display: flex; justify-content:end ;">
                                <div style="padding: 8px; width: 100%; border-radius: 10px; background-color: black; color:white; text-align: center; cursor: pointer;">
                                نمایش
                                </div>
                            </div>
                            <div style="flex:1">&nbsp;</div>
                            <div style="flex:1; font-size: 12px;">
                                <q-select
                                    placeholder="میزان رضایت "
                                    v-model="tem.satisfaction"
                                    :options="taskOptions"
                                    class="bg-ffffff rounded-1 p-0 m-0 font-12 mb-1 selectWidth"
                                />
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="d-flex flex-row justify-end"> 
                <div class="cursor-pointer bg-light-blue rounded-1 p-2 c-ffffff fw-bold my-1 me-3 text-center" style="width:25%" @click="showSatisfactionFunc">اتمام برنامه امروز و مشاهده رضایت</div>
            </div>
            <div class="row p-3 bg-e6e4e9 rounded-1 mx-1" v-if="showSatisfaction == true">
               <div class="col-md-6 text-center">
                    <div class="m-3 bg-ffffff rounded-1 p-3">
                        <div class="text-center fw-bold my-2">میزان رضایت شما از امروز: {{(razi/template.length)*100+'%'}}</div>
                        <div class="text-center"> دلایل رضایت شما</div>
                        <div class="d-flex flex-row me-3">
                            <q-checkbox v-model="firstRazi"/> 
                              علت ۱
                        </div>
                        <div class="d-flex flex-row me-3">
                            <q-checkbox v-model="secondRazi"/> 
                              علت ۲
                        </div>
                        <div class="d-flex flex-row me-3">
                            <q-checkbox v-model="thirdRazi"/> 
                              علت ۳
                        </div>
                    </div>
               </div>
               <div class="col-md-6 text-center">
                <div class="m-3 bg-ffffff rounded-1 p-3">
                        <div class="text-center fw-bold my-2">میزان نارضایتی شما از امروز: {{(notrazi/template.length)*100+'%'}}</div>
                        <div class="text-center"> دلایل نارضایتی شما</div>
                        <div class="d-flex flex-row me-3">
                            <q-checkbox v-model="firstNotRazi"/> 
                              علت ۱
                        </div>
                        <div class="d-flex flex-row me-3">
                            <q-checkbox v-model="secondNotRazi"/> 
                              علت ۲
                        </div>
                        <div class="d-flex flex-row me-3">
                            <q-checkbox v-model="thirdNotRazi"/> 
                              علت ۳
                        </div>
                    </div>
               </div>
               <div class="d-flex flex-row justify-end"> 
                    <div class="cursor-pointer bg-light-blue rounded-1 p-2 c-ffffff fw-bold my-1 me-3 text-center" style="width:25%" @click="showSatisfactionFunc">ثبت دلایل </div>
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
        firstRazi: false,
        secondRazi: false,
        thirdRazi: false,
        firstNotRazi: false,
        secondNotRazi: false,
        thirdNotRazi: false,
        testSourceOptions:[{label:'جزوه', value: 0},{label:'آنلاین خوان', value: 1},{label:'آزاد', value: 2}],
        razi : 0,
        poker : 0,
        notrazi : 0,
        showSatisfaction: false,
        showTaskTable: false,
        startClock: 0,
        endClock: 0,
        doneHoure: 0,
        showDone: false,
        taskOptions: [{label:':)',value:0},{label:':|',value:1},{label:':(',value:2}],
        notDoneTasks:[],
        template:[
        {time:'7:10', dars:'ریاضی ۱', baaze:'7:10 - 12:00', status:'یادگیری + تست',color:'a349a4', timeLength:'4:50',workStatus:'انجام داده ام',
        stTime:{
            startClock: 0,
            endClock: 0,
            doneHoure: 0,
            doneMinute: 0
        }, satisfaction:'',
        test:{
            kolTest: 20,
            trueTest: 0,
            falseTest: 0,
            noneTest: 0,
            source: '',
            sourceDetail: ''
        }},
        {time:'13:30', dars:'زیست ۱', baaze:'13:30 - 16:00', status:'یادگیری + تست', color:'a3f48cc', timeLength:'2:30',workStatus:'شروع نکرده ام',
        stTime:{
            startClock: 0,
            endClock: 0,
            doneHoure: 0,
            doneMinute: 0
        }, satisfaction:'', test:null},
        {time:'17:30', dars:'ادبیات فارسی ۱', baaze:'17:30 - 20:00', status:'یادگیری + تست', color:'a00a2e8', timeLength:'2:30',workStatus:'انجام داده ام',
        stTime:{
            startClock: 0,
            endClock: 0,
            doneHoure: 0,
            doneMinute: 0
        }, satisfaction:'',
        test:{
            kolTest: 20,
            trueTest: 0,
            falseTest: 0,
            noneTest: 0,
            source: '',
            sourceDetail: ''
        }},
        {time:'20:30', dars:'شیمی ۱', baaze:'20:30 - 24:00', status:'یادگیری + تست', color:'a22b14c', timeLength:'3:30',workStatus:'انجام داده ام',
        stTime:{
            startClock: 0,
            endClock: 0,
            doneHoure: 0,
            doneMinute: 0
        }, satisfaction:'', test:null}]
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
    showSatisfactionFunc(){
        this.razi = 0
        this.notrazi = 0
        this.poker = 0
      this.showSatisfaction = true
        this.template.map((x)=>{
            if(x.satisfaction == 0){
                this.razi ++
            }
            else if(x.satisfaction == 1){
                this.poker ++
            }
            else if(x.satisfaction == 2){
                this.notrazi ++
            }
        })
        this.notDoneTasks = []
        this.template.map((x)=>{
            if(x.stTime.startClock == 0 && x.stTime.endClock == 0){
                this.notDoneTasks.push(x)
            }
        })
       
    },   
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
.selectWidth{
    width: 135px;
}
.timeContainer{
    display: flex;
    flex-direction: column;
}
.ca349a4{
    color: #a349a4
}
.a349a4{
    /* background-color: #a349a4; */
    border-right: 6px solid #a349a4;
    color: #a349a4
}
.ca00a2e8{
    color: #00a2e8
}
.a00a2e8{
    /* background-color: #00a2e8; */
    border-left: 6px solid #00a2e8;
    color: #00a2e8
}
.ca3f48cc{
    color: #3f48cc
}
.a3f48cc{
    /* background-color: #3f48cc; */
    border-left: 6px solid #3f48cc;
    color: #3f48cc
}
.ca22b14c{
    color: #22b14c
}
.a22b14c{
    /* background-color: #22b14c; */
    border-left: 6px solid #22b14c;
    color: #22b14c
}
.dashed-time{
    flex: 10;
    border: none;
    border-top: 5px dotted #888888;
    height: 1px;
}
.left-line{
    /* border-left: 6px solid green; */
    height: 140px;
    margin: 5px
}
.whatToDo{
    display: flex;
    flex-direction: column;
    flex:3
}
.details{
    flex: 15;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
}
.report{
    display: flex;
    flex-direction: column;
    flex:3;
    margin-left: 5px;
    padding-bottom: 5px;
    justify-content: center;
    align-items: center;
}
.time{
    background-color: white;
    border: 2px solid #01a4ab;
    border-radius: 50%;
    width: 100px;
    height: 50px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: bold;
    color: #01a4ab;
}
.line{
    border: 2px solid #01a4ab;
    height: 55px
}
.half{
    height: 30px !important;
}
.dars{
    flex:1 !important;
    padding: 5px;
    text-align: start;
    color: #1976d2;
    font-weight: bold;
}
.baaze{
    direction: ltr;
    flex:1 !important;
    padding: 5px;
    text-align: center;
    color: #ff0048;
    font-weight: bold;
}
input {border:0;outline:0;}
input:focus {outline:none!important;}
table {
    direction: ltr;
      border-collapse: collapse;
    font-family: Tahoma, Geneva, sans-serif;
  }
  table td {
    height: 10px !important;
    width: 10px !important;
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
  .inputClass{
    width: 100px;
  }
@media  (max-width: 1000px){
      .details{
        flex-direction: column;
      }
}
@media  (max-width: 580px){
      .details{
        width:30%
      }
}
@media  (max-width: 537px){
      .details{
        flex-direction: column;
      }
      .selectWidth{
        width: 80px;
      }
}
@media  (max-width: 400px){
      .selectWidth{
        width: 40px;
      }
      .inputClass{
        width: 50px;
      }
}
</style>
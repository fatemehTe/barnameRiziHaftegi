<template>
  <div>
      <bs-modal :show="showDoubleCheckFinalize" size="md" @open="showFinalTask" @close="!showDoubleCheckFinalize">
        <template class="bs-modal-body">
          <div class="mt-4 modalHelpContainer text-center">
            ابتدا برنامه این هفته را تایید نمایید
          </div>
        </template>
        <template slot="footer">
          <div class=" d-flex flex-row justify-content-around w-50 mx-auto">
            
            <div class="bg-danger rounded-1 c-ffffff text-center cursor-pointer p-2 px-4" @click="showDoubleCheckFinalize = false">بازگشت</div>
          </div>
        </template>
      </bs-modal>
  
    <div class="operation" v-if="openOperation == true">
      <div class="py-3 w-50">
        <div class="d-flex flex-row c-ffffff justify-content-between" v-if="detailExtra != ''">
           عنوان:
          <div class="text-center">{{detailExtra}}</div>
        </div>
        <div class="d-flex flex-row c-ffffff justify-content-between" v-if="detailExtra == ''">
          درس (ها):
          <div class="text-center">{{detailDars}}</div>
        </div>
        <div class="d-flex flex-row c-ffffff justify-content-between" v-if="detailExtra == ''">
          فصل (ها):
          <div>{{detailFasl}}</div>
        </div>
        <div class="d-flex flex-row c-ffffff justify-content-between" v-if="detailExtra == ''">
          گفتار (ها):
          <div>{{detailGoftar}}</div>
        </div>
        <div class="d-flex flex-row c-ffffff justify-content-between" v-if="detailExtra == ''">
          مبحث (ها):
          <div>{{detailArticle}}</div>
        </div>
        <div class="d-flex flex-row c-ffffff justify-content-between" v-if="detailExtra == '' && tashrihi == true">
          مدت زمان مطالعه:
          <div class="text-center">{{detailTime}}</div>
        </div>
        <div class="d-flex flex-row c-ffffff justify-content-between" v-if="detailExtra == ''&& tashrihi == false">
          مدت زمان تست:
          <div>{{detailTestTime}}</div>
        </div>
        <div class="d-flex flex-row c-ffffff justify-content-between" v-if="detailExtra == '' && tashrihi == false">
          تعداد تست:
          <div>{{detailTestCount}}</div>
        </div>
        <div class="d-flex flex-row c-ffffff justify-content-between ">
          مدت زمان کل:
          <div v-if="tashrihi == true">{{detailETime}}</div>
          <div v-else>{{detailTestTime}}</div>
        </div>
      </div>
      <div class="d-flex flex-column align-items-center justify-content-around">
        <div class="operationBtn" @click="taskEdit">ویرایش برنامه</div>
        <div class="operationBtn" @click="taskDelete">حذف برنامه</div>
      </div>
      <div @click="openOperation = false" class="text-center c-ffffff cursor-pointer p-2">
          بازگشت
      </div>
    </div>
    <link
      href="http://213.232.124.150:63839/css/all.css"
      rel="stylesheet"
    />
    <div class="row pt-1">
      <div class="col-12 pb-2">
        <br />
        <h2 class="h3 g-dashboard font-16">داشبورد من</h2>
      </div>
      <div class="col-12">
        <div class="d-flex gap-2">
          <span class="border-title border-title-blue d-block"></span>
          <span class="border-title border-title-gray d-block"></span>
        </div>
      </div>
    </div>
    <br />
  <div class="row">
    <div class="d-flex flex-row mb-2 justify-content-between align-items-center">
      <div id="demo"></div>
      <div class="d-flex flex-row">
        <div class="d-flex flex-column">
        <div class="c-ffffff rounded-1 bg-blue p-3 cursor-pointer text-center ms-1" @click="retrieve"> 
        برگرداندن به حالت اولیه
        </div>  
        <div class="c-ffffff rounded-1 bg-success p-3 cursor-pointer text-center ms-1 mt-1" @click="finish"> 
         اتمام برنامه ریزی  
        </div> 
        </div>
        <div class="d-flex flex-column">
          <div v-if="weekIndex >= totalTasksArray.length-1" class="c-ffffff rounded-1 bg-blue p-3 cursor-pointer text-center" @click="goToNextWeek"> 
                برنامه ریزی برای هفته آینده
          </div>
          <div v-else class="c-ffffff rounded-1 bg-light-blue p-3 cursor-pointer text-center" @click="goToNextWeek"> 
                ویرایش برنامه  هفته آینده
          </div>
          <div v-if="weekIndex >= 1" class="c-ffffff rounded-1 bg-light-blue p-3 cursor-pointer text-center mt-1" @click="goToLastWeek"> 
                ویرایش برنامه هفته گذشته
          </div>
        </div>
      </div>
    </div>
    
      <div class="col-lg-12 col-md-12 col-sm-12" id="table">
        <div class="d-flex flex-row me-3 justify-end mb-2 ms-2" v-if="montakhab == false && notEcditable == false">
            <q-checkbox v-model="montakhab"/> 
                  برنامه (ها)ی منتخب در این هفته اعمال گردد
        </div>
        <div v-else-if="notEcditable == true && weekIndex!=0" class="mb-2"> 
          برنامه (ها)ی منتخب در این هفته اعمال نشده است
        </div>
        <div v-else-if="montakhab == true" class="mb-2"> 
          برنامه (ها)ی منتخب در این هفته اعمال شده است.
        </div>
       <table style="width:100%;" id="table">
          <thead id="tableHead" style="position:sticky; z-index: 500;">
            <tr height="100px" style="width:100%">
              <td v-for="(i,index) in caledDayWeeks" :key="index" style="text-align:center; font-weight:bold" :id="i+dayWeeks" >
                <div class="d-flex flex-column align-items-center">
                  <span>{{i}}</span>
                  <span>{{dates[7-index]}}</span>
                </div>
              </td>
            </tr>
          </thead>
          <tbody>
            <tr v-for="l in timeTable.length" :key="l" :id="l" height="100px" style="width:100%;">
              <td v-for="i in 8" :key="i"  height="100px" :name="i+'tdName'"></td>
              <td height="100px" v-if="showtimeColumn==true" class="timeTable" name="tdRight" style="color: black; font-weight: bold; font-size: 16px;">{{timeTable[l-1]}}</td>
            </tr>
          </tbody>
        </table> 
    </div>
    <div v-if="dbclicked == true" class="panelClass">
      <div class="taskCreatePanel c-7244b0 p-3 m-1 font-14">
        ایجاد برنامه هفتگی روتین های روزانه, جدید:
      </div>
      <div class="taskCreatePanel p-3 m-1 font-14">
        <div class="c-7244b0 mb-2 d-flex flex-row justify-between">
        <span>افزودن فیلد جدید:</span>
        <span class="w-50 text-end color-light-blue" v-if="pin == true"> این برنامه به عنوان منتخب انتخاب شد</span>
        </div>
        <div class="d-flex flex-row me-3 justify-end mb-2 ms-2">
            <q-checkbox v-model="pin"/> 
            این برنامه به عنوان منتخب انتخاب شود
        </div>
        <div class="d-flex flex-row me-3 justify-end mb-2 ms-2" v-if="operationPressed == false">
            <q-checkbox v-model="tashrihi"/> 
            این برنامه تشریحی است
        </div>
        <div class="d-flex flex-row justify-content-between align-items-center">
          <div class="d-flex flex-row justify-content-between align-items-center">
          انتخاب رنگ برنامه          
          <div v-if="defaultTaskColor == true">
            <div v-if="(rooz == 1 || rooz == 3 || rooz == 5)" class="colorPalette bg-c8bfe7 me-1"></div> 
            <div v-if="(rooz == 2 || rooz == 4 || rooz == 6)" class="colorPalette bg-7092be me-1"></div>
          </div>
          <div v-else-if="taskColor == 'a349a4'" class="colorPalette bg-a349a4 me-1"></div>
          <div v-else-if="taskColor == '3f48cc'" class="colorPalette bg-3f48cc me-1"></div>
          <div v-else-if="taskColor == '00a2e8'" class="colorPalette bg-00a2e8 me-1"></div>
          <div v-else-if="taskColor == '22b14c'" class="colorPalette bg-22b14c me-1"></div>
          <div v-else-if="taskColor == 'fff200'" class="colorPalette bg-fff200 me-1"></div>
          <div v-else-if="taskColor == 'ff7f27'" class="colorPalette bg-ff7f27 me-1"></div>
          <div v-else-if="taskColor == 'ed1c24'" class="colorPalette bg-ed1c24 me-1"></div>
          <div v-else-if="taskColor == '880015'" class="colorPalette bg-880015 me-1"></div>
          <div v-else-if="taskColor == '7f7f7f'" class="colorPalette bg-7f7f7f me-1"></div>
          <div v-else-if="taskColor == '000000'" class="colorPalette bg-000000 me-1"></div>
          <div v-else-if="taskColor == 'c8bfe7'" class="colorPalette bg-c8bfe7 me-1"></div>
          <div v-else-if="taskColor == '7092be'" class="colorPalette bg-7092be me-1"></div>
          <div v-else-if="taskColor == '99d9ea'" class="colorPalette bg-99d9ea me-1"></div>
          <div v-else-if="taskColor == 'c4c4c4'" class="colorPalette bg-c4c4c4 me-1"></div>
          <div v-else-if="taskColor == 'b5e61d'" class="colorPalette bg-b5e61d me-1"></div>
          <div v-else-if="taskColor == 'ffc90e'" class="colorPalette bg-ffc90e me-1"></div>
          <div v-else-if="taskColor == 'ffaec9'" class="colorPalette bg-ffaec9 me-1"></div>
          <div v-else-if="taskColor == 'b97a57'" class="colorPalette bg-b97a57 me-1"></div>
          <div v-else-if="taskColor == 'c3c3c3'" class="colorPalette bg-c3c3c3 me-1"></div>
          <div v-else-if="taskColor == 'ffffff'" class="colorPalette bg-ffffff me-1"></div>
          <div class="d-flex flex-row me-3">
            <q-checkbox v-model="defaultTaskColor"/> 
            رنگ پیش فرض
          </div>
          </div>
        <div class="d-flex flex-column w-50">
          <div class="d-flex flex-row justify-content-around my-1">
            <div class="colorPalette bg-a349a4" @click='makeTaskColor("a349a4")'></div>
            <div class="colorPalette bg-3f48cc" @click='makeTaskColor("3f48cc")'></div>
            <div class="colorPalette bg-00a2e8" @click='makeTaskColor("00a2e8")'></div>
            <div class="colorPalette bg-22b14c" @click='makeTaskColor("22b14c")'></div>
            <div class="colorPalette bg-fff200" @click='makeTaskColor("fff200")'></div>
            <div class="colorPalette bg-ff7f27" @click='makeTaskColor("ff7f27")'></div>
            <div class="colorPalette bg-ed1c24" @click='makeTaskColor("ed1c24")'></div>
            <div class="colorPalette bg-880015" @click='makeTaskColor("880015")'></div>
            <div class="colorPalette bg-7f7f7f" @click='makeTaskColor("7f7f7f")'></div>
            <div class="colorPalette bg-000000" @click='makeTaskColor("000000")'></div>
          </div>
          <div class="d-flex flex-row justify-content-around my-1">
            <div class="colorPalette bg-c8bfe7" @click='makeTaskColor("c8bfe7")'></div>
            <div class="colorPalette bg-7092be" @click='makeTaskColor("7092be")'></div>
            <div class="colorPalette bg-99d9ea" @click='makeTaskColor("99d9ea")'></div>
            <div class="colorPalette bg-c4c4c4" @click='makeTaskColor("c4c4c4")'></div>
            <div class="colorPalette bg-b5e61d" @click='makeTaskColor("b5e61d")'></div>
            <div class="colorPalette bg-ffc90e" @click='makeTaskColor("ffc90e")'></div>
            <div class="colorPalette bg-ffaec9" @click='makeTaskColor("ffaec9")'></div>
            <div class="colorPalette bg-b97a57" @click='makeTaskColor("b97a57")'></div>
            <div class="colorPalette bg-c3c3c3" @click='makeTaskColor("c3c3c3")'></div>
            <div class="colorPalette bg-ffffff" @click='makeTaskColor("ffffff")'></div>
          </div>
        </div>
        </div>
      
        <div class="d-flex flex-row justify-content-around" v-if="detailExtra == ''">
            <div class="d-flex align-items-center testWidth">فصل</div>
            <q-select
              v-model="fasls"
              :options="faslOptions"
              multiple
              clearable
              class="bg-ffffff rounded-1 testWidth pt-1 me-2"
          />
        </div>
        <div class="d-flex flex-row justify-content-around" v-if="detailExtra == ''">
            <div class="d-flex align-items-center testWidth">گفتار</div>
            <q-select
              v-model="goftars"
              :options="goftarOptions"
              multiple
              clearable
              class="bg-ffffff rounded-1 testWidth pt-1 me-2"
          />
        </div>
        <div class="d-flex flex-row justify-content-around" v-if="detailExtra == ''">
            <div class="d-flex align-items-center testWidth">مبحث</div>
            <q-select
              v-model="articles"
              :options="articleOptions"
              
              clearable
              class="bg-ffffff rounded-1 testWidth pt-1 me-2"
          />
        </div>
        <div class="d-flex flex-row justify-content-around" v-if="detailExtra == ''">
            <div class="d-flex align-items-center testWidth">نوع برنامه</div>
            <q-select
              v-model="taskType"
              :options="taskOptions"
              clearable
              class="bg-ffffff rounded-1 testWidth pt-1 me-2"
          />
        </div>
      <div class="moshaverScope">
        <div>
          <div class="d-flex flex-row justify-content-around">
            <div class="d-flex flex-column w-25" v-if="tashrihi == true">
              <div class="d-flex flex-row justify-content-between align-items-center">
                <div v-if="detailExtra == ''&& tashrihi == true">مدت زمان مطالعه </div>
                <div v-else>مدت زمان </div>
                <span>{{quarterTime*15+' : '+hourTime}}</span>
              </div>
              <div class="d-flex flex-row justify-content-between align-items-center">
                <span class="c-gray font-10">ساعت</span>
                <input type="number" v-model="hourTime" class="bg-ffffff rounded-1 pt-1 me-2" min="0" max="24" />
              </div>
              <div class="d-flex flex-row justify-content-between align-items-center">
                <span class="c-gray font-10">تعداد ۱۵دقیقه</span>
                <input type="number" v-model="quarterTime" class="bg-ffffff rounded-1 pt-1 me-2" min="0" max="3"/>
              </div>
            </div>
            <!-- <div class="vr" v-if="tashrihi == false"></div> -->
            <div class="d-flex flex-column w-25" v-if="tashrihi == false">
              <div class="d-flex flex-row justify-content-between align-items-center">
                مدت زمان تست
                <span>{{testQuarterTime*15+' : '+testHourTime}}</span>
              </div>
              <div class="d-flex flex-row justify-content-between align-items-center">
                <span class="c-gray font-10">ساعت</span>
                <input type="number" v-model="testHourTime" class="bg-ffffff rounded-1 pt-1 me-2" min="0" max="24"/>
              </div>
              <div class="d-flex flex-row justify-content-between align-items-center">
                <span class="c-gray font-10">تعداد ۱۵دقیقه</span>
                <input type="number" v-model="testQuarterTime" class="bg-ffffff rounded-1 pt-1 me-2" min="0" max="3"/>
              </div>
            </div>
            <div class="vr" v-if="tashrihi == false"></div>
            <div class="d-flex flex-column w-25" v-if="tashrihi == false">
              <div class="d-flex flex-row justify-content-between align-items-center">
                  تعداد تست
              </div>
              <div class="d-flex flex-row justify-content-between align-items-center">
                <span class="c-gray font-10">تعداد تست</span>
                <input type="number" v-model="testCount" class="bg-ffffff rounded-1 pt-1 me-2" min="0" max="1000"/>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-if="timeLack" class="c-ff0048 font-12 text-center">مدت زمان را مشخص نمایید.</div>
      <div class="d-flex flex-row justify-content-around mt-2">
        توضیحات:
        <q-input class="bg-ffffff w-75 h-25" v-model="description"/>
      </div>
      <div class="d-flex flex-row justify-content-around">
        <div v-if="showEditBtn == false" class="cursor-pointer bg-7244b0 c-ffffff p-2 rounded-1 testWidth justify-content-start text-center mt-3 " @click="addTask(false)">افزودن به برنامه</div>
        <div v-if="showEditBtn == true" class="cursor-pointer bg-3d3d3d c-ffffff p-2 rounded-1 testWidth justify-content-start text-center mt-3 " @click="editTaskFunc">ویرایش برنامه</div>
        <div class="cursor-pointer bg-3d3d3d c-ffffff p-2 rounded-1 testWidth justify-content-start text-center mt-3 " @click="cancelTask">انصراف </div>
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

import IMessageResult from "src/models/IMessageResult";

import {
  required,
  displayName,
  maxLength,
  minLength,
  requiredDdl,
  numeric,
  onlyPersianChar,
  notPersianChar
} from "src/plugins/vuelidate";
// Vue.use(drag);
export default {
  data() {
    return {
      positions0: {
        clientY: undefined,
        clientX: undefined,
        movementX: 0,
        movementY: 0
      },
      notEcditable: false,
      montakhab: false,
      montakhabArray: [],
      usesMontakhab: [],
      pinIndex: 0,
      pinCode: null,
      operationPressed: false,
      tashrihi: true,
      timeLack: false,
      readTask: false,
      testTask: false,
      rooz7: false,
      me: false,
      weekIndex: 0,
      totalTasksArray: [],
      firstDaysArray: [],
      constToday: null,
      thisWeekFirst: null,//the first day of this week
      nextWeekFirst: null,//the first day of next week
      lastWeekFirst: null,//the first day of last week
      day: 0,
      month: 0,
      year:0,
      futureTop: false,
      nextWeekDate:'',
      lastWeekDate:'',
      lastWeek: [],
      showDoubleCheckFinalize : false,
      finalizeTask: false,
      dates: [],
      datesYear: [],
      pin: false,
      caledDayWeeks: [],
      todayDayWeek: 0,//0=>1sh , 1=>2sh , ...
      detailExtra: '',
      ableExtra: true,
      ableDars: true,
      showFinalTask: false,
      showDarsErr: false,
      defaultTaskColor: true,
      taskColor:'',
      hourTime:0,
      quarterTime:0,
      testHourTime:0,
      testQuarterTime:0,
      testCount:0,
      taskWidth:0,
      row: 0,
      rooz: 0,
      dbclicked: false,
      sh:[],
      sixsh:[],
      onesh:[],
      twosh:[],
      threesh:[],
      foursh:[],
      fivesh:[],
      jome:[],
      showtimeColumn:true,
      copy: false,
      detailArticle:'',
      detailDars:'',
      detailETime:'',
      detailFasl:'',
      detailGoftar :'',
      detailSTime:'',
      detailTestCount:'',
      detailTestTime:'',
      detailTime:'',
      showDetail: false,
      showEditBtn: false,
      openOperation: false,
      divIndex : 0,
      editTask: false,
      taskArray:[],
      j:0,
      taskType:null,
      taskOptions:[{label:'یادگیری', value:0},{label:'تثبیت', value:1},{label:'یادآوری', value:2}],
      timeTable : ['مدرسه','کلاس کنکور','کلاس فوق العاده','خواب','استراحت','رفت و آمد مدرسه','رفت و آمد کلاس کنکور','رفت و آمد کلاس فوق العاده','رفت و آمد متفرقه'],
      timeTableQuarter : ['۷:۰۰-۷:۱۵','۷:۱۵-۷:۳۰','۷:۳۰-۷:۴۵','۷:۴۵-۸:۰۰','۸:۰۰-۸:۱۵','۸:۱۵-۸:۳۰','۸:۳۰-۸:۴۵','۸:۴۵-۹:۰۰','۹:۰۰-۹:۱۵','۹:۱۵-۹:۳۰','۹:۳۰-۹:۴۵','۹:۴۵-۱۰:۰۰','۱۰:۰۰-۱۰:۱۵','۱۰:۱۵-۱۰:۳۰','۱۰:۳۰-۱۰:۴۵','۱۰:۴۵-۱۱:۰۰','۱۱:۰۰-۱۱:۱۵','۱۱:۱۵-۱۱:۳۰','۱۱:۳۰-۱۱:۴۵','۱۱:۴۵-۱۲:۰۰','۱۲:۰۰-۱۲:۱۵','۱۲:۱۵-۱۲:۳۰','۱۲:۳۰-۱۲:۴۵','۱۲:۴۵-۱۳:۰۰','۱۳:۰۰-۱۳:۱۵','۱۳:۱۵-۱۳:۳۰','۱۳:۳۰-۱۳:۴۵','۱۳:۴۵-۱۴:۰۰','۱۴:۰۰-۱۴:۱۵','۱۴:۱۵-۱۴:۳۰','۱۴:۳۰-۱۴:۴۵','۱۴:۴۵-۱۵:۰۰','۱۵:۰۰-۱۵:۱۵','۱۵:۱۵-۱۵:۳۰','۱۵:۳۰-۱۵:۴۵','۱۵:۴۵-۱۶:۰۰','۱۶:۰۰-۱۶:۱۵','۱۶:۱۵-۱۶:۳۰','۱۶:۳۰-۱۶:۴۵','۱۶:۴۵-۱۷:۰۰','۱۷:۰۰-۱۷:۱۵','۱۷:۱۵-۱۷:۳۰','۱۷:۳۰-۱۷:۴۵','۱۷:۴۵-۱۸:۰۰','۱۸:۰۰-۱۸:۱۵','۱۸:۱۵-۱۸:۳۰','۱۸:۳۰-۱۸:۴۵','۱۸:۴۵-۱۹:۰۰','۱۹:۰۰-۱۹:۱۵','۱۹:۱۵-۱۹:۳۰','۱۹:۳۰-۱۹:۴۵','۱۹:۴۵-۲۰:۰۰','۲۰:۰۰-۲۰:۱۵','۲۰:۱۵-۲۰:۳۰','۲۰:۳۰-۲۰:۴۵','۲۰:۴۵-۲۱:۰۰','۲۱:۰۰-۲۱:۱۵','۲۱:۱۵-۲۱:۳۰','۲۱:۳۰-۲۱:۴۵','۲۱:۴۵-۲۲:۰۰','۲۲:۰۰-۲۲:۱۵','۲۲:۱۵-۲۲:۳۰','۲۲:۳۰-۲۲:۴۵','۲۲:۴۵-۲۳:۰۰','۲۳:۰۰-۲۳:۱۵','۲۳:۱۵-۲۳:۳۰','۲۳:۳۰-۲۳:۴۵','۲۳:۴۵-۲۴:۰۰','۰۰:۰۰-۰۰:۱۵','۰۰:۱۵-۰۰:۳۰','۰۰:۳۰-۰۰:۴۵','۰۰:۴۵-۱:۰۰','۱:۰۰-۱:۱۵','۱:۱۵-۱:۳۰','۱:۳۰-۱:۴۵','۱:۴۵-۲:۰۰','۲:۰۰-۲:۱۵','۲:۱۵-۲:۳۰','۲:۳۰-۲:۴۵','۲:۴۵-۳:۰۰','۳:۰۰-۳:۱۵','۳:۱۵-۳:۳۰','۳:۳۰-۳:۴۵','۳:۴۵-۴:۰۰','۴:۰۰-۴:۱۵','۴:۱۵-۴:۳۰','۴:۳۰-۴:۴۵','۴:۴۵-۵:۰۰','۵:۰۰-۵:۱۵','۵:۱۵-۵:۳۰','۵:۳۰-۵:۴۵','۵:۴۵-۶:۰۰','۶:۰۰-۶:۱۵','۶:۱۵-۶:۳۰','۶:۳۰-۶:۴۵','۶:۴۵-۷:۰۰'],
      dayWeeks:["جمعه","پنج شنبه","چهار شنبه","سه شنبه","دو شنبه","یک شنبه","شنبه"],
      dayWeeksDateNum: [5,4,3,2,1,0,6],
      dayOptions:[{label:'شنبه', value:6},{label:'یکشنبه', value:5},{label:'دوشنبه', value:4},{label:'سه شنبه', value:3},
      {label:'چهارشنبه', value:2},{label:'پنجشنبه', value:1},{label:'جمعه', value:0}],
      dayWeeksIds:['jome','5sh','4sh','3sh','2sh','1sh','sh','6sh'],
      days:[],
      lessonOptions:[{label:'درس۱', value:0},{label:'درس۲', value:1},{label:'درس۳', value:2},{label:'درس۳-۱ ', value:3},
      {label:'درس۴', value:4},{label:'درس۵', value:5},{label:'درس۶', value:6}],
      lessons:[],
      extraOptions:[{label:'مدرسه', value:0},{label:'کلاس کنکور', value:1},{label:'کلاس فوق العاده', value:2},{label:'خواب ', value:3},
      {label:'استراحت', value:4},{label:'رفت و آمد مدرسه', value:5},{label:'رفت و آمد کلاس کنکور', value:6},{label:'رفت و آمد کلاس فوق العاده', value:7},{label:'رفت و آمد متفرقه', value:8}],
      extra:null,
      faslOptions:[{label:'فصل۱', value:0},{label:'فصل۲', value:1},{label:'۳فصل', value:2},{label:'فصل۴ ', value:3},
      {label:'درس۴', value:4},{label:'فصل۵', value:5},{label:'فصل۶', value:6}],
      fasls:[],
      goftarOptions:[{label:'گفتار۰', value:0},{label:'گفتار۱', value:1},{label:'گفتار۲', value:2},{label:'گفتار۳ ', value:3},
      {label:'گفتار۴', value:4},{label:'گفتار۵', value:5},{label:'گغتار۶', value:6}],
      goftars:[],
      articleOptions:[{label:'بحث۱', value:0},{label:'بحث۲', value:1},{label:'بحث۳', value:2},{label:'بحث۴', value:3},
      {label:'بحث۵', value:4},{label:'بحث۶', value:5},{label:'بحث۷', value:6}],
      articles:[],
      description:'',
      resize : false,
      confirmedTasks: []
    };
  },
  watch: {
    lessons(newLessons, oldLessons) {
     if(newLessons.length>0){
      this.showDarsErr = false
      this.ableExtra = false
     }else{
      this.ableExtra = true
     }   
    },
    extra(newLessons, oldLessons) {
     if(newLessons != null){
      this.showDarsErr = false
      this.ableDars = false
     }else{
      this.ableDars = true
     }   
    },
    montakhab(newLessons, oldLessons){
      if(newLessons == true){
        if(this.weekIndex >= this.totalTasksArray.length && this.weekIndex != 0){
          this.montakhabArray.map((x)=>{
            this.prepareForPins(x)
          })
        }
      }
    }
  },
  methods:{
    finish(){
      this.totalTasksArray = LocalStorage.get.item('totalTasksArray') != null ?LocalStorage.get.item('totalTasksArray'):LocalStorage.set('totalTasksArray', [])
      this.usesMontakhab = LocalStorage.get.item('usesMontakhab') != null ?LocalStorage.get.item('usesMontakhab'):LocalStorage.set('usesMontakhab', [])
      if(this.weekIndex >= this.totalTasksArray.length){
        this.totalTasksArray.push(this.taskArray)
        this.usesMontakhab.push(this.montakhab)
        LocalStorage.set("totalTasksArray", this.totalTasksArray);
        LocalStorage.set("usesMontakhab", this.usesMontakhab);
      }
      
      this.totalTasksArray.map((x, index)=>{
        if(index == this.totalTasksArray.length-1){
          x.map((y)=>{
              (y.id!=-1)? this.confirmedTasks.push(y) : ''
          })
        }else{
            x.map((y)=>{
              (y.id!=-1 && y.rooz != 7)? this.confirmedTasks.push(y) : ''
          })
        }
      })

      LocalStorage.set('year','')
      LocalStorage.set("WeekTask", '');
      LocalStorage.set("nextWeekDate", '');
      LocalStorage.set("firstDaysArray", []);
      LocalStorage.set("totalTasksArray", []);
      LocalStorage.set("weekIndex", 0);
      LocalStorage.set("pinIndex", 0);
      LocalStorage.set("montakhabArray", []);
      LocalStorage.set("usesMontakhab", []);
      console.log(this.confirmedTasks,'CONFIRM')
      router.push('/stSchedule')
      

    },
    makePersianNum(data){
      if(data != 'آزاد'){
      let str = data.split(':')[0]
      let str2 = data.split(':')[1]
      let newStr = ''
      let newStr2 = ''
      for(let i =0 ;i<str.length;i++){
          if(str.charAt(i) == '۱') newStr+='1'
        else if(str.charAt(i)  == '۲') newStr+='2'
        else if(str.charAt(i)  == '۳') newStr+='3'
        else if(str.charAt(i)  == '۴') newStr+='4'
        else if(str.charAt(i)  == '۵') newStr+='5'
        else if(str.charAt(i)  == '۶') newStr+='6'
        else if(str.charAt(i)  == '۷') newStr+='7'
        else if(str.charAt(i)  == '۸') newStr+='8'
        else if(str.charAt(i)  == '۹') newStr+='9'
        else if(str.charAt(i)  == '۰') newStr+='0'
        }
        for(let i =0 ;i<str2.length;i++){
          if(str2.charAt(i)  == '۱') newStr2+='1'
          else if(str2.charAt(i) == '۲') newStr2+='2'
          else if(str2.charAt(i) == '۳') newStr2+='3'
          else if(str2.charAt(i) == '۴') newStr2+='4'
          else if(str2.charAt(i) == '۵') newStr2+='5'
          else if(str2.charAt(i) == '۶') newStr2+='6'
          else if(str2.charAt(i) == '۷') newStr2+='7'
          else if(str2.charAt(i) == '۸') newStr2+='8'
          else if(str2.charAt(i) == '۹') newStr2+='9'
          else if(str2.charAt(i) == '۰') newStr2+='0'
        }
        return newStr+':'+newStr2
     }
     else{
      return 0; //means azad time
     }
    },
    retrieve(){
      setTimeout(() => window.location.reload(), 1000);
        router.push('/moshaverCM')
    },
    goToNextWeek(){
      
        if(LocalStorage.get.item('firstDaysArray') === null){
          LocalStorage.set('firstDaysArray', [])
        }
        this.firstDaysArray = []
        this.firstDaysArray = LocalStorage.get.item('firstDaysArray')

        if(LocalStorage.get.item('totalTasksArray') === null){
          LocalStorage.set('totalTasksArray', [])
        }

        if(LocalStorage.get.item('usesMontakhab') === null){
          LocalStorage.set('usesMontakhab', [])
        }

        this.totalTasksArray = []
        this.totalTasksArray = LocalStorage.get.item('totalTasksArray')
        this.usesMontakhab = []
        this.usesMontakhab = LocalStorage.get.item('usesMontakhab')

        if(this.weekIndex >= this.totalTasksArray.length){
          this.totalTasksArray.push(this.taskArray.filter(x=>(x.id != -1)))
          this.usesMontakhab.push(this.montakhab)
          LocalStorage.set("usesMontakhab", this.usesMontakhab);
          this.firstDaysArray.push(this.dates[0])
          LocalStorage.set('firstDaysArray', this.firstDaysArray)
          LocalStorage.set("WeekTask", this.totalTasksArray[this.weekIndex]);
        }else{
          this.totalTasksArray[this.weekIndex] = this.taskArray.filter(x=>(x.id != -1))
          LocalStorage.set("WeekTask", this.totalTasksArray[this.weekIndex]);
        }
        this.weekIndex = this.weekIndex + 1
        LocalStorage.set('weekIndex', this.weekIndex)
        LocalStorage.set('totalTasksArray', this.totalTasksArray)
        LocalStorage.set("nextWeekDate", this.nextWeekFirst);

        setTimeout(() => window.location.reload(), 1500);
        router.push('/moshaverCM')

    },
    goToLastWeek(){

        if(LocalStorage.get.item('totalTasksArray') === null){
          LocalStorage.set('totalTasksArray', [])
        }
        if(LocalStorage.get.item('usesMontakhab') === null){
          LocalStorage.set('usesMontakhab', [])
        }
        this.totalTasksArray = []
        this.totalTasksArray = LocalStorage.get.item('totalTasksArray')
        this.usesMontakhab = []
        this.usesMontakhab = LocalStorage.get.item('usesMontakhab')

        if(this.weekIndex >= this.totalTasksArray.length && this.weekIndex != 0){
          this.totalTasksArray.push(this.taskArray.filter(x=>(x.id != -1)))
          this.usesMontakhab.push(this.montakhab)
          this.firstDaysArray.push(this.dates[0])
          LocalStorage.set('firstDaysArray', this.firstDaysArray)
          LocalStorage.set("usesMontakhab", this.usesMontakhab);
        }else{
          this.totalTasksArray[this.weekIndex] = this.taskArray.filter(x=>(x.id != -1))
        }
        LocalStorage.set('totalTasksArray', this.totalTasksArray)
        
        this.weekIndex = this.weekIndex - 1
        LocalStorage.set('weekIndex', this.weekIndex)

        if(this.weekIndex >= 0){
          LocalStorage.set("WeekTask", this.totalTasksArray[this.weekIndex]);
        }
        LocalStorage.set("nextWeekDate", this.lastWeekFirst);

        setTimeout(() => window.location.reload(), 1500);
        router.push('/moshaverCM')
  
    },
    cancelTask(){
      this.dbclicked = false
      this.operationPressed = false
      if(this.showEditBtn){
        this.editTaskFunc()
      }
    },
    taskDelete(){
      this.openOperation = false
      let rm = document.getElementById('divId'+this.divIndex)
      for(let i=0;i<this.taskArray.length;i++){
        if(this.taskArray[i].id == 'divId'+this.divIndex){
          let obj= {...this.taskArray[i]};
          obj.id = -1
          this.taskArray[i] = obj
          rm.remove()
          break
        }
      }
      this.divIndex = 0
    },
    taskCopy(){
      this.openOperation = false
      let copyObj = null
      let copyElem = this.taskArray[this.divIndex]
      copyObj = {...copyElem}
      this.taskColor = copyObj.taskColor
      this.lessons =copyObj.dars
      this.extra =copyObj.extra
      this.fasls = copyObj.fasl
      this.goftars = copyObj.goftar
      this.articles = copyObj.mabhas
      this.hourTime = copyObj.hourTime
      this.quarterTime = copyObj.quarterTime
      this.testHourTime = copyObj.testHourTime
      this.testQuarterTime = copyObj.testQuarterTime
      this.testCount = copyObj.testCount
      this.pin = copyObj.pin
      this.rooz = copyObj.rooz
      this.row = copyObj.row
      this.taskType
      
      
      = copyObj.taskType
      copyObj.id = 'divId'+this.taskArray.length
      this.taskArray.push(copyObj)
      this.divIndex = this.taskArray.length-1
      this.addTask(true ,false)
      
    },
      editTaskFunc(){
        this.operationPressed = false
        let q = 0
        let h = 0
        let unPin = false
        this.dbclicked = false
        this.showEditBtn = false
        let i = this.divIndex 
        let id = this.taskArray[i].id
        id = id.split('divId')

        this.taskArray[i].fasl = this.fasls
        this.taskArray[i].taskColor = this.taskColor
        this.taskArray[i].goftar = this.goftars
        this.taskArray[i].mabhas = this.articles
        this.taskArray[i].hourTime = this.hourTime
        this.taskArray[i].quarterTime = this.quarterTime
        this.taskArray[i].testHourTime = this.testHourTime
        this.taskArray[i].testQuarterTime = this.testQuarterTime
        this.taskArray[i].testCount = this.testCount

          if(this.taskArray[i].pin == true){
            if(this.pin == false){
              unPin = true
            }
          }

        this.taskArray[i].pin = this.pin
        if(this.pin == true){
          if(this.taskArray[i].pinCode !== null){
            this.taskArray[i].pinCode = this.taskArray[i].pinCode
          }else{
              this.pinIndex = parseInt(LocalStorage.get.item("pinIndex"))
              this.taskArray[i].pinCode = this.pinIndex+1
              LocalStorage.set("pinIndex", this.pinIndex+1)
              this.pinIndex++
          }
        }else{
            this.taskArray[i].pinCode = null
        }
        this.taskArray[i].taskType = this.taskType
        if(this.lessons.length == 0 && this.extra == null){
          this.lessons = [0]
        }
        this.taskArray[i].dars = this.lessons
        this.taskArray[i].extra = this.extra
        if(this.pin == false){
          let freez = document.getElementById('freezDiv'+i)
          freez.style.opacity = 0
        }
        if(this.pin == true){
          let freez = document.getElementById('freezDiv'+i)
          freez.style.opacity = 1
        }
        let lastColorClass = 'bg-'+this.taskArray[i].color

        let div = document.getElementById(this.taskArray[i].id)
        div.style.height = '50px'
        if(this.lessons.length == 0){
          div.style.height = '100px'
        }

        let operation = document.getElementById('operation'+id[1])
        let span11 = document.getElementById('span11'+id[1])
        let span12 = document.getElementById('darsId'+id[1])
        let span41 = document.getElementById('span41'+id[1])
        let span42 = document.getElementById('span42'+this.divIndex)
        
       q = this.tashrihi === false ?parseInt(this.testQuarterTime)*15:parseInt(this.quarterTime)*15
       h = this.tashrihi === false ?parseInt(this.testHourTime):parseInt(this.hourTime)
       if(q >= 60){
        h += Math.floor(q/60) 
        q = q%60
       }

        span42.innerHTML = (h+':'+q)
        this.taskArray[i].endTime = span42.innerHTML

        let has = false
        if(this.taskArray[i].rooz == 7){
            this.totalTasksArray = LocalStorage.get.item("totalTasksArray") !== null ?LocalStorage.get.item("totalTasksArray"):LocalStorage.set("totalTasksArray", []);
            if(this.weekIndex < this.totalTasksArray.length){
              let nextWeek = this.totalTasksArray[this.weekIndex+1]
              for(let ii=0 ;ii<nextWeek.length;ii++){
                if(nextWeek[ii].rooz7 === this.taskArray[i].rooz7){
                  has = true
                  nextWeek[ii].testHourTime=this.taskArray[i].testHourTime
                  nextWeek[ii].hourTime=this.taskArray[i].hourTime
                  nextWeek[ii].testQuarterTime=this.taskArray[i].testQuarterTime
                  nextWeek[ii].quarterTime=this.taskArray[i].quarterTime
                  nextWeek[ii].color=this.taskArray[i].color
                  nextWeek[ii].dars=this.taskArray[i].dars
                  nextWeek[ii].date=this.taskArray[i].date
                  nextWeek[ii].draged=this.taskArray[i].draged
                  nextWeek[ii].endTime=this.taskArray[i].endTime
                  nextWeek[ii].extra=this.taskArray[i].extra
                  nextWeek[ii].fasl=this.taskArray[i].fasl
                  nextWeek[ii].goftar=this.taskArray[i].goftar
                  nextWeek[ii].haveHalf=this.taskArray[i].haveHalf
                  nextWeek[ii].mabhas=this.taskArray[i].mabhas
                  nextWeek[ii].pin=this.taskArray[i].pin
                  nextWeek[ii].rooz7=this.taskArray[i].rooz7
                  nextWeek[ii].row=this.taskArray[i].row
                  nextWeek[ii].startIndex=this.taskArray[i].startIndex
                  nextWeek[ii].startTime=this.taskArray[i].startTime
                  nextWeek[ii].taskColor=this.taskArray[i].taskColor
                  nextWeek[ii].taskType=this.taskArray[i].taskType
                  nextWeek[ii].testCount=this.taskArray[i].testCount
                }
              }
              this.totalTasksArray[this.weekIndex+1] = nextWeek
            }
            LocalStorage.set('totalTasksArray',this.totalTasksArray)
        }
        else if(this.taskArray[i].rooz == 0){
            this.totalTasksArray = LocalStorage.get.item("totalTasksArray") !== null ?LocalStorage.get.item("totalTasksArray"):LocalStorage.set("totalTasksArray", []);
            if(this.weekIndex!=0){
              let lastWeek = this.totalTasksArray[this.weekIndex-1]
              for(let ii=0 ;ii<lastWeek.length;ii++){
                if( lastWeek[ii].rooz7 === this.taskArray[i].rooz7 ){
                  has = true
                  lastWeek[ii].testHourTime=this.taskArray[i].testHourTime
                  lastWeek[ii].hourTime=this.taskArray[i].hourTime
                  lastWeek[ii].testQuarterTime=this.taskArray[i].testQuarterTime
                  lastWeek[ii].quarterTime=this.taskArray[i].quarterTime
                  lastWeek[ii].color=this.taskArray[i].color
                  lastWeek[ii].dars=this.taskArray[i].dars
                  lastWeek[ii].date=this.taskArray[i].date
                  lastWeek[ii].draged=this.taskArray[i].draged
                  lastWeek[ii].endTime=this.taskArray[i].endTime
                  lastWeek[ii].extra=this.taskArray[i].extra
                  lastWeek[ii].fasl=this.taskArray[i].fasl
                  lastWeek[ii].goftar=this.taskArray[i].goftar
                  lastWeek[ii].haveHalf=this.taskArray[i].haveHalf
                  lastWeek[ii].mabhas=this.taskArray[i].mabhas
                  lastWeek[ii].pin=this.taskArray[i].pin
                  lastWeek[ii].rooz7=this.taskArray[i].rooz7
                  lastWeek[ii].row=this.taskArray[i].row
                  lastWeek[ii].startIndex=this.taskArray[i].startIndex
                  lastWeek[ii].startTime=this.taskArray[i].startTime
                  lastWeek[ii].taskColor=this.taskArray[i].taskColor
                  lastWeek[ii].taskType=this.taskArray[i].taskType
                  lastWeek[ii].testCount=this.taskArray[i].testCount
                }
              }
              this.totalTasksArray[this.weekIndex-1] = lastWeek
            }
            LocalStorage.set('totalTasksArray',this.totalTasksArray)
        }
        if(this.defaultTaskColor == false){
            if(this.taskColor == 'ff7f27'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-ff7f27')
              operation.style.color = '#000000'
              span11.style.color = '#000000'
              span41.style.color = '#000000'
              span12.style.color = '#000000'
              span42.style.color = '#000000'
            }
            if(this.taskColor == 'a349a4'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-a349a4')
              operation.style.color = '#ffffff'
              span11.style.color = '#ffffff'
              span41.style.color = '#ffffff'
              span12.style.color = '#ffffff'
              span42.style.color = '#ffffff'
            }
            if(this.taskColor == 'c8bfe7'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-c8bfe7')
              operation.style.color = '#ffffff'
              span11.style.color = '#ffffff'
              span41.style.color = '#ffffff'
              span12.style.color = '#ffffff'
              span42.style.color = '#ffffff'
            }
            if(this.taskColor == '3f48cc'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-3f48cc')
              operation.style.color = '#ffffff'
              span11.style.color = '#ffffff'
              span41.style.color = '#ffffff'
              span12.style.color = '#ffffff'
              span42.style.color = '#ffffff'
            }
            if(this.taskColor == '7092be'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-7092be')
              operation.style.color = '#000000'
              span11.style.color = '#000000'
              span41.style.color = '#000000'
              span12.style.color = '#000000'
              span42.style.color = '#000000'
            }
            if(this.taskColor == '00a2e8'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-00a2e8')
              operation.style.color = '#000000'
              span11.style.color = '#000000'
              span41.style.color = '#000000'
              span12.style.color = '#000000'
              span42.style.color = '#000000'
            }
            if(this.taskColor == '99d9ea'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-99d9ea')
              operation.style.color = '#000000'
              span11.style.color = '#000000'
              span41.style.color = '#000000'
              span12.style.color = '#000000'
              span42.style.color = '#000000'
            }
            if(this.taskColor == 'c4c4c4'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-c4c4c4')
              operation.style.color = '#000000'
              span11.style.color = '#000000'
              span41.style.color = '#000000'
              span12.style.color = '#000000'
              span42.style.color = '#000000'
            }
            if(this.taskColor == '22b14c'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-22b14c')
              operation.style.color = '#000000'
              span11.style.color = '#000000'
              span41.style.color = '#000000'
              span12.style.color = '#000000'
              span42.style.color = '#000000'
            }
            if(this.taskColor == 'b5e61d'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-b5e61d')
              operation.style.color = '#000000'
              span11.style.color = '#000000'
              span41.style.color = '#000000'
              span12.style.color = '#000000'
              span42.style.color = '#000000'
            }
            if(this.taskColor == 'fff200'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-fff200')
              operation.style.color = '#000000'
              span11.style.color = '#000000'
              span41.style.color = '#000000'
              span12.style.color = '#000000'
              span42.style.color = '#000000'
            }
            if(this.taskColor == 'ffc90e'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-ffc90e')
              operation.style.color = '#000000'
              span11.style.color = '#000000'
              span41.style.color = '#000000'
              span12.style.color = '#000000'
              span42.style.color = '#000000'
            }
            if(this.taskColor == 'ffaec9'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-ffaec9')
              operation.style.color = '#000000'
              span11.style.color = '#000000'
              span41.style.color = '#000000'
              span12.style.color = '#000000'
              span42.style.color = '#000000'
            }
            if(this.taskColor == 'ed1c24'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-ed1c24')
              operation.style.color = '#ffffff'
              span11.style.color = '#ffffff'
              span41.style.color = '#ffffff'
              span12.style.color = '#ffffff'
              span42.style.color = '#ffffff'
            }
            if(this.taskColor == '880015'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-880015')
              operation.style.color = '#ffffff'
              span11.style.color = '#ffffff'
              span41.style.color = '#ffffff'
              span12.style.color = '#ffffff'
              span42.style.color = '#ffffff'
            }
            if(this.taskColor == 'b97a57'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-b97a57')
              operation.style.color = '#000000'
              span11.style.color = '#000000'
              span41.style.color = '#000000'
              span12.style.color = '#000000'
              span42.style.color = '#000000'
            }
            if(this.taskColor == 'c3c3c3'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-c3c3c3')
              operation.style.color = '#000000'
              span11.style.color = '#000000'
              span41.style.color = '#000000'
              span12.style.color = '#000000'
              span42.style.color = '#000000'
            }
            if(this.taskColor == '7f7f7f'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-7f7f7f')
              operation.style.color = '#000000'
              span11.style.color = '#000000'
              span41.style.color = '#000000'
              span12.style.color = '#000000'
              span42.style.color = '#000000'
            }
            if(this.taskColor == '000000'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-000000')
              operation.style.color = '#ffffff'
              span11.style.color = '#ffffff'
              span41.style.color = '#ffffff'
              span12.style.color = '#ffffff'
              span42.style.color = '#ffffff'
            }
            if(this.taskColor == 'ffffff'){
              div.classList.remove(lastColorClass)
              div.classList.add('bg-ffffff')
              operation.style.color = '#000000'
              span11.style.color = '#000000'
              span41.style.color = '#000000'
              span12.style.color = '#000000'
              span42.style.color = '#000000'
            }
        }
        else if(this.taskColor == '' || this.defaultTaskColor == true){
          if(this.taskArray[i].rooz == 1||this.taskArray[i].rooz ==3||this.taskArray[i].rooz ==5){
            div.classList.remove(lastColorClass)
            div.classList.add('bg-c8bfe7')
          }else{
            div.classList.remove(lastColorClass)
            div.classList.add('bg-7092be')
          }
        }
        this.taskArray[i].color = this.taskColor
        
        let lessonStr = ''
        if(this.lessons.length>0){
            if(this.lessons.length >1){
              lessonStr = this.lessons[0]
              lessonStr += ', ...'
            }else if(this.lessons.length  == 0){
              lessonStr = 'انتخاب نشده'
            }else if(this.lessons.length == 1){
              lessonStr = this.lessons[0]
            }
          lessonStr = this.extra
        }
        span12.innerHTML = this.tashrihi === false ? this.testCount: lessonStr
      },
      editPinTasks(index, unpin){
            this.totalTasksArray = LocalStorage.get.item("totalTasksArray") !== null ?LocalStorage.get.item("totalTasksArray"):LocalStorage.set("totalTasksArray", []);
            let thisWeekFirstDay = LocalStorage.get.item("thisWeekFirst")
            let thisIndex = 0
            this.firstDaysArray.map((x, inde)=>{
              x == thisWeekFirstDay ? thisIndex = inde : ''
            })
           
            for(let i=thisIndex;i<this.totalTasksArray.length;i++){
              let exists = false
              this.totalTasksArray[i].map((xx, indexx)=>{
                if(xx.pinCode == this.taskArray[index].pinCode){
                  exists = true
                  this.totalTasksArray[i][indexx] = this.taskArray[index]
                  LocalStorage.set("totalTasksArray", this.totalTasksArray)
                }
              })
              if(exists == false && unpin == 0){
                this.totalTasksArray[i].push(this.taskArray[index])
                LocalStorage.set("totalTasksArray", this.totalTasksArray)
              }
            }
            
      },
      taskEdit(){
        this.dbclicked = true
        this.showEditBtn = true
        this.openOperation = false
        let i = this.divIndex
        this.taskColor = this.taskArray[i].taskColor
        this.lessons =this.taskArray[i].dars
        this.extra =this.taskArray[i].extra
        this.fasls = this.taskArray[i].fasl
        this.goftars = this.taskArray[i].goftar
        this.articles = this.taskArray[i].mabhas
        this.hourTime = this.taskArray[i].hourTime
        this.quarterTime = this.taskArray[i].quarterTime
        this.testHourTime = this.taskArray[i].testHourTime
        this.testQuarterTime = this.taskArray[i].testQuarterTime
        this.testCount = this.taskArray[i].testCount
        this.pin = this.taskArray[i].pin
        this.taskType = this.taskArray[i].taskType
      },
      makeTaskColor(str){
        this.defaultTaskColor = false
        this.taskColor = str
      },
      getDate(){
        var sundte = new Date();
        var yeardte = sundte.getFullYear();
        var monthdte = sundte.getMonth();
        var dtedte = sundte.getDate();
        var daydte = sundte.getDay();
        var hour = sundte.getHours();
        var min = sundte.getMinutes();
        hour += 11
        min += 30
        if(hour > 24) {
          daydte += 1 
          dtedte += 1
        }
        if(hour == 23 && min >= 60) {
          daydte += 1 
          dtedte += 1
        }
        if(daydte == 7) daydte = 0
        this.todayDayWeek = daydte
        let temp = ["جمعه","پنج شنبه","چهار شنبه","سه شنبه","دو شنبه","یک شنبه","شنبه"]
        if(daydte == 5){
          this.caledDayWeeks = temp
          this.caledDayWeeks.push(this.dayWeeks[0]) 
        }if(daydte == 4){
          temp.shift()
          this.caledDayWeeks = temp
          this.caledDayWeeks.push(this.dayWeeks[0]) 
          this.caledDayWeeks.push(this.dayWeeks[1])
        }if(daydte == 3){
          temp.shift()
          temp.shift()
          this.caledDayWeeks = temp
          this.caledDayWeeks.push(this.dayWeeks[0]) 
          this.caledDayWeeks.push(this.dayWeeks[1])
          this.caledDayWeeks.push(this.dayWeeks[2])
        }if(daydte == 2){
          temp.shift()
          temp.shift()
          temp.shift()
          this.caledDayWeeks = temp
          this.caledDayWeeks.push(this.dayWeeks[0]) 
          this.caledDayWeeks.push(this.dayWeeks[1])
          this.caledDayWeeks.push(this.dayWeeks[2])
          this.caledDayWeeks.push(this.dayWeeks[3])
        }if(daydte == 1){
          temp.shift()
          temp.shift()
          temp.shift()
          temp.shift()
          this.caledDayWeeks = temp
          this.caledDayWeeks.push(this.dayWeeks[0]) 
          this.caledDayWeeks.push(this.dayWeeks[1])
          this.caledDayWeeks.push(this.dayWeeks[2])
          this.caledDayWeeks.push(this.dayWeeks[3])
          this.caledDayWeeks.push(this.dayWeeks[4])
        }if(daydte == 0){
          temp.shift()
          temp.shift()
          temp.shift()
          temp.shift()
          temp.shift()
          this.caledDayWeeks = temp
          this.caledDayWeeks.push(this.dayWeeks[0]) 
          this.caledDayWeeks.push(this.dayWeeks[1])
          this.caledDayWeeks.push(this.dayWeeks[2])
          this.caledDayWeeks.push(this.dayWeeks[3])
          this.caledDayWeeks.push(this.dayWeeks[4])
          this.caledDayWeeks.push(this.dayWeeks[5])
        }
        if(daydte == 6){
          temp.shift()
          temp.shift()
          temp.shift()
          temp.shift()
          temp.shift()
          temp.shift()
          this.caledDayWeeks = temp
          this.caledDayWeeks.push(this.dayWeeks[0]) 
          this.caledDayWeeks.push(this.dayWeeks[1])
          this.caledDayWeeks.push(this.dayWeeks[2])
          this.caledDayWeeks.push(this.dayWeeks[3])
          this.caledDayWeeks.push(this.dayWeeks[4])
          this.caledDayWeeks.push(this.dayWeeks[5])
          this.caledDayWeeks.push(this.dayWeeks[6])
        }
      
        var sunyear;
        switch (daydte) {
          case 0:
            var today = "يکشنبه";
            break;
          case 1:
            var today = "دوشنبه";
            break;
          case 2:
            var today = "سه شنبه";
            break;
          case 3:
            var today = "چهارشنبه";
            break;
          case 4:
            var today = "پنجشنبه";
            break;
          case 5:
            var today = "جمعه";
            break;
          case 6:
            var today = "شنبه";
            break;
        }
        switch (monthdte) {
          case 0:
            sunyear = yeardte - 622;
            if (dtedte <= 20) {
              var sunmonth = "دي";
              var daysun = dtedte + 10;
            } else {
              var sunmonth = "بهمن";
              var daysun = dtedte - 20;
            }
            break;
          case 1:
            sunyear = yeardte - 622;
            if (dtedte <= 19) {
              var sunmonth = "بهمن";
              var daysun = dtedte + 11;
            } else {
              var sunmonth = "اسفند";
              var daysun = dtedte - 19;
            }
            break;
          case 2:
            {
              if ((yeardte - 621) % 4 == 0) var i = 10;
              else var i = 9;
              if (dtedte <= 20) {
                sunyear = yeardte - 622;
                var sunmonth = "اسفند";
                var daysun = dtedte + i;
              } else {
                sunyear = yeardte - 621;
                var sunmonth = "فروردين";
                var daysun = dtedte - 20;
              }
            }
            break;
          case 3:
            sunyear = yeardte - 621;
            if (dtedte <= 20) {
              var sunmonth = "فروردين";
              var daysun = dtedte + 11;
            } else {
              var sunmonth = "ارديبهشت";
              var daysun = dtedte - 20;
            }
            break;
          case 4:
            sunyear = yeardte - 621;
            if (dtedte <= 21) {
              var sunmonth = "ارديبهشت";
              var daysun = dtedte + 10;
            } else {
              var sunmonth = "خرداد";
              var daysun = dtedte - 21;
            }

            break;
          case 5:
            sunyear = yeardte - 621;
            if (dtedte <= 21) {
              var sunmonth = "خرداد";
              var daysun = dtedte + 10;
            } else {
              var sunmonth = "تير";
              var daysun = dtedte - 21;
            }
            break;
          case 6:
            sunyear = yeardte - 621;
            if (dtedte <= 22) {
              var sunmonth = "تير";
              var daysun = dtedte + 9;
            } else {
              var sunmonth = "مرداد";
              var daysun = dtedte - 22;
            }
            break;
          case 7:
            sunyear = yeardte - 621;
            if (dtedte <= 22) {
              var sunmonth = "مرداد";
              var daysun = dtedte + 9;
            } else {
              var sunmonth = "شهريور";
              var daysun = dtedte - 22;
            }
            break;
          case 8:
            sunyear = yeardte - 621;
            if (dtedte <= 22) {
              var sunmonth = "شهريور";
              var daysun = dtedte + 9;
            } else {
              var sunmonth = "مهر";
              var daysun = dtedte - 22;
            }
            break;
          case 9:
            sunyear = yeardte - 621;
            if (dtedte <= 22) {
              var sunmonth = "مهر";
              var daysun = dtedte + 8;
            } else {
              var sunmonth = "آبان";
              var daysun = dtedte - 22;
            }
            break;
          case 10:
            sunyear = yeardte - 621;
            if (dtedte <= 21) {
              var sunmonth = "آبان";
              var daysun = dtedte + 9;
            } else {
              var sunmonth = "آذر";
              var daysun = dtedte - 21;
            }
            break;
          case 11:
            sunyear = yeardte - 621;
            if (dtedte <= 21) {
              var sunmonth = "آذر";
              var daysun = dtedte + 9;
            } else {
              var sunmonth = "دي";
              var daysun = dtedte - 21;
            }
            break;
        }
        document.getElementById("demo").innerHTML =
          "امروز " +
          today +
          "&nbsp;" +
          [daysun] +
          "&nbsp;" +
          sunmonth +
          "&nbsp;" +
          sunyear
          let dates = []
          let datesYear = []
          let months = ["فروردين","ارديبهشت","خرداد","تير","مرداد","شهريور","مهر","آبان","آذر","دي","بهمن","اسفند"]
          
          let str =  daysun+' '+sunmonth
          for(let i=0;i<12;i++){
              if(sunmonth == months[i]){
                this.month = i+1
                break;
              }
            }
          let strr = sunyear+'/'+this.month+'/'+daysun
          LocalStorage.set('constToday', daysun+' '+months[this.month-1])
          if(LocalStorage.get.item('nextWeekDate')!= ''){
            let futureStr = LocalStorage.get.item('nextWeekDate')
            daysun = parseInt(futureStr.split(' ')[0])
            sunmonth = futureStr.split(' ')[1]
            str = daysun+' '+sunmonth
            for(let i=0;i<12;i++){
              if(sunmonth == months[i]){
                this.month = i+1
                break;
              }
            }
            strr = sunyear+'/'+this.month+'/'+daysun
          }
          dates.push(str)
          datesYear.push(strr)
          
          if(LocalStorage.get.item('year') != ''){
            sunyear = LocalStorage.get.item('year')
          }
          // sunmonth= 'مرداد' test
          // sunyear = 1403
          // daysun = 25
        
          for(let i=0; i<7 ; i++){
            if(daysun == 29 && sunmonth === months[11] && sunyear%4 != 3){//sale kabise not
              daysun = 1
              sunmonth = months[0]
              sunyear += 1
              LocalStorage.set('year',sunyear) 
            }
            else if(daysun==30){
              if(sunmonth === months[0] ||sunmonth === months[1] ||sunmonth === months[2] ||sunmonth === months[3] ||sunmonth === months[4] ||sunmonth === months[5] ){
                daysun = 31
              }else{
                daysun = 1
                if(sunyear%4 == 3 && sunmonth === months[11]){//sale kabise
                      sunyear += 1 
                      LocalStorage.set('year',sunyear)
                }
                for(let i = 0;i<12;i++ ){
                  if(sunmonth === months[i]){
                    i == 11? i = -1 : ''
                    sunmonth = months[i+1]
                    break
                  }
                }
              }
            }
            else if(daysun ==  31){
              daysun = 1
                for(let i = 0;i<12;i++ ){
                  if(sunmonth === months[i]){
                    i == 11? i = -1 : ''
                    sunmonth = months[i+1]
                    break
                  }
                }
            }
            else{
              daysun += 1
            }
            for(let i=0;i<12;i++){
              if(sunmonth == months[i]){
                this.month = i+1
              }
            }
            this.year = sunyear
            this.day = daysun
            let str = this.day+' '+ sunmonth
            let strr = this.year+'/'+this.month+'/'+(this.day)
            datesYear.push(strr)
            dates.push(str)
          }
          this.dates = dates
          this.datesYear = datesYear
      },
      prepareForPins(obj){
        this.defaultTaskColor = false
        obj.endTime = obj.endTime === '30:0'?'00:30':obj.endTime
        this.futureTop = false
        this.row = obj.row
        if(this.row < 0){
          this.row = 0
        }
        this.futureTop = false
        if(obj.draged%100 != 0 || obj.haveHalf == true){
          this.futureTop = true
        }
        this.tashrihi = obj.tashrihi
        this.hourTime = parseInt(obj.hourTime)
        this.quarterTime = parseInt(obj.quarterTime)
        this.taskColor = obj.color
        this.lessons = obj.dars
        this.extra = obj.extra
        this.fasls = obj.fasl
        this.goftars = obj.goftar
        this.articles = obj.mabhas
        this.testHourTime = parseInt(obj.testHourTime)
        this.testQuarterTime = parseInt(obj.testQuarterTime)
        this.testCount = obj.testCount
        this.rooz7 = obj.rooz7
        this.me == true ? 
          this.rooz = obj.rooz == 7? 7: obj.rooz:
          this.rooz = obj.rooz == 7? 0: obj.rooz
        this.pin = obj.pin
        this.pinCode = obj.pinCode
        this.taskType  = obj.taskType
        this.addTask(false)
      },
  
    addTask(copy){
      let q = 0
      let h = 0
      let arr = []
      arr.push(this.timeTable[this.row])
      this.lessons = this.row<=this.timeTable.length-10?arr:[]
      this.extra = this.row>this.timeTable.length-10?this.timeTable[this.row]:null
        if(this.row>this.timeTable.length-10){
              this.testHourTime = 0
        }
      this.dbclicked = false
      let div = ''
      let modalDiv = ''
      let deleteBtn = ''
      let closeBtn = ''
      let innerDivDetail0 = ''
      let innerDivDetail1 = ''
      let innerDivDetail2 = ''
      let innerDivDetail3 = ''
      let innerDivDetail4 = ''
      let span12 = ''
      let span22 = ''
      let span42 = ''
      let seeDatilsSpan = ''
      let deleteTaskSpan = ''
      let deleteSpan = ''
      let copySpan = ''
      let editSpan = ''
      let modalDetailDars = ''
      let modalDetailFasl = ''
      let modalDetailGoftar = ''
      let modalDetailMabhas = ''
      let modalDetailTime = ''
      let modalDetailTestTime = ''
      let modalDetailTestCount = ''
      let modalDetailStartTime = ''
      let modalDetailEndTime = ''
      let operationSpan = ''
      let freezDiv = ''
     
         div = document.createElement("div");
         let taskObject={
          id:0,
          dars:[],
          extra:null,
          fasl:[],
          goftar:[],
          mabhas:0,
          hourTime:0,
          quarterTime:0,
          testHourTime:0,
          testQuarterTime:0,
          testCount:0,
          startTime:0,
          startIndex:0,
          endTime:0,
          taskColor:'',
          rooz:0,
          row:0,
          draged:0,
          date:'',
          color:'',
          taskType: null,
          pin: false,
          haveHalf: false,
          rooz7: false,
          tashrihi: null,
          pinCode: null
         }
         div.id = 'divId'+this.j;
         modalDiv = document.createElement("div");
         modalDiv.id = 'modalId'+this.j
         deleteBtn = document.createElement("div")
         closeBtn = document.createElement("div")
        let dragDiv = document.createElement("div");
        let innerDiv = document.createElement("div");
         innerDivDetail0 = document.createElement("div")
         innerDivDetail1 = document.createElement("div")
         innerDivDetail2 = document.createElement("div")
         innerDivDetail3 = document.createElement("div")
         innerDivDetail4 = document.createElement("div")
        let span11 = document.createElement("span")
        span11.id= 'span11'+this.j
         span12 = document.createElement("span")
         span12.id = 'darsId'+this.j 

         modalDetailDars = document.createElement("span")
        modalDetailFasl = document.createElement("span")
        modalDetailGoftar = document.createElement("span")
        modalDetailMabhas = document.createElement("span")
        modalDetailTime = document.createElement("span")
        modalDetailTestTime = document.createElement("span")
        modalDetailTestCount = document.createElement("span")
        modalDetailStartTime = document.createElement("span")
        modalDetailEndTime = document.createElement("span")

         modalDetailDars.id = 'modalDarsId'+this.j 
         modalDetailFasl.id = 'modalFaslId'+this.j 
         modalDetailGoftar.id = 'modalGoftarId'+this.j 
         modalDetailMabhas.id = 'modalMabhasId'+this.j 
         modalDetailTime.id = 'modalTimeId'+this.j 
         modalDetailTestTime.id = 'modalTestTimeId'+this.j 
         modalDetailTestCount.id = 'modalTestCountId'+this.j 
         modalDetailStartTime.id = 'modalStartId'+this.j 
         modalDetailEndTime.id = 'modalEndId'+this.j 

         
        let span21 = document.createElement("span")
         span22 = document.createElement("span")
        let span41 = document.createElement("span")
        span41.id = 'span41'+this.j
        span42 = document.createElement("span")
        span42.id = 'span42'+this.j

        deleteSpan = document.createElement("span")
        operationSpan = document.createElement('span')
        freezDiv = document.createElement('div')
        operationSpan.id = 'operation'+this.j
        copySpan = document.createElement("div")
        editSpan = document.createElement("div")
        seeDatilsSpan = document.createElement("div")
        deleteTaskSpan = document.createElement("div")
       q = this.tashrihi === false ?parseInt(this.testQuarterTime)*15:parseInt(this.quarterTime)*15
       h = this.tashrihi === false ?parseInt(this.testHourTime):parseInt(this.hourTime)
       if(q >= 60){
        h += Math.floor(q/60) 
        q = q%60
       }

        span42.innerHTML = (h+':'+q)
        operationSpan.innerHTML = 'مشاهده جزییات'
        freezDiv.classList.add('freez')
        freezDiv.id = 'freezDiv'+this.j
        div.classList.add("popup")
        if(this.defaultTaskColor == false){
            if(this.taskColor == 'ff7f27'){
              taskObject.taskColor = 'ff7f27'
              div.classList.add('bg-ff7f27')
              operationSpan.style.color = '#ffffff'
              span11.style.color = '#ffffff'
              span41.style.color = '#ffffff'
              span12.style.color = '#ffffff'
              span42.style.color = '#ffffff'
            }
            if(this.taskColor == 'a349a4'){
              taskObject.taskColor = 'a349a4'
              div.classList.add('bg-a349a4')
              operationSpan.style.color = '#ffffff'
              span11.style.color = '#ffffff'
              span41.style.color = '#ffffff'
              span12.style.color = '#ffffff'
              span42.style.color = '#ffffff'
            }
            if(this.taskColor == 'c8bfe7'){
              taskObject.taskColor = 'c8bfe7'
              div.classList.add('bg-c8bfe7')
            }
            if(this.taskColor == '3f48cc'){
              taskObject.taskColor = '3f48cc'
              div.classList.add('bg-3f48cc')
              operationSpan.style.color = '#ffffff'
              span11.style.color = '#ffffff'
              span41.style.color = '#ffffff'
              span12.style.color = '#ffffff'
              span42.style.color = '#ffffff'
            }
            if(this.taskColor == '7092be'){
            taskObject.taskColor = '7092be'
              div.classList.add('bg-7092be')
            }
            if(this.taskColor == '00a2e8'){
            taskObject.taskColor = '00a2e8'
              div.classList.add('bg-00a2e8')
            }
            if(this.taskColor == '99d9ea'){
              taskObject.taskColor = '99d9ea'
              div.classList.add('bg-99d9ea')
            }
            if(this.taskColor == 'c4c4c4'){
              taskObject.taskColor = 'c4c4c4'
              div.classList.add('bg-c4c4c4')
            }
            if(this.taskColor == '22b14c'){
              taskObject.taskColor = '22b14c'
              div.classList.add('bg-22b14c')
            }
            if(this.taskColor == 'b5e61d'){
              taskObject.taskColor = 'b5e61d'
              div.classList.add('bg-b5e61d')
            }
            if(this.taskColor == 'fff200'){
              taskObject.taskColor = 'fff200'
              div.classList.add('bg-fff200')
            }
            if(this.taskColor == 'ffc90e'){
              taskObject.taskColor = 'ffc90e'
              div.classList.add('bg-ffc90e')
            }
            if(this.taskColor == 'ffaec9'){
              taskObject.taskColor = 'ffaec9'
              div.classList.add('bg-ffaec9')
            }
            if(this.taskColor == 'ed1c24'){
            taskObject.taskColor = 'ed1c24'
              div.classList.add('bg-ed1c24')
              operationSpan.style.color = '#ffffff'
              span11.style.color = '#ffffff'
              span41.style.color = '#ffffff'
              span12.style.color = '#ffffff'
              span42.style.color = '#ffffff'
            }
            if(this.taskColor == '880015'){
            taskObject.taskColor = '880015'
              div.classList.add('bg-880015')
              operationSpan.style.color = '#ffffff'
              span11.style.color = '#ffffff'
              span41.style.color = '#ffffff'
              span12.style.color = '#ffffff'
              span42.style.color = '#ffffff'
            }
            if(this.taskColor == 'b97a57'){
              taskObject.taskColor = 'b97a57'
              div.classList.add('bg-b97a57')
            }
            if(this.taskColor == 'c3c3c3'){
              taskObject.taskColor = 'c3c3c3'
              div.classList.add('bg-c3c3c3')
            }
            if(this.taskColor == '7f7f7f'){
              taskObject.taskColor = '7f7f7f'
              div.classList.add('bg-7f7f7f')
            }
            if(this.taskColor == '000000'){
              taskObject.taskColor = '000000'
              div.classList.add('bg-000000')
              operationSpan.style.color = '#ffffff'
              span11.style.color = '#ffffff'
              span41.style.color = '#ffffff'
              span12.style.color = '#ffffff'
              span42.style.color = '#ffffff'
            }
            if(this.taskColor == 'ffffff'){
              taskObject.taskColor = 'ffffff'
              div.classList.add('bg-ffffff')
            }
        }
        if(this.taskColor == '' || this.defaultTaskColor == true){
          if(this.rooz == 1||this.rooz ==3||this.rooz ==5){
            this.taskColor = 'c8bfe7'
            div.classList.add('bg-c8bfe7')
          }else{
            this.taskColor = '7092be'
            div.classList.add('bg-7092be')
          }
        }

        div.style.height = '50px'
       
        dragDiv.classList.add("popupDrag")
        div.appendChild(dragDiv)
        
        innerDiv.style.display = 'flex'
        innerDiv.style.flexDirection = 'column'
        innerDivDetail0.classList.add("innerDetail0")
        
        innerDivDetail1.classList.add("innerDetail")
        innerDivDetail2.classList.add("innerDetail")
        innerDivDetail3.classList.add("innerDetail")
        innerDivDetail4.classList.add("innerDetail")
        seeDatilsSpan.innerHTML = "مشاهده"
        deleteTaskSpan.innerHTML = "حذف"
        span11.innerHTML = this.extra!=null?"عنوان:":"درس:"
        span21.innerHTML = "مبحث:"
        span41.innerHTML = "مدت زمان:"
        deleteSpan.innerHTML = "آیا از حذف این برنامه اطمینان دارید؟"
        copySpan.innerHTML = "کپی"
        editSpan.innerHTML = "ویرایش"
        closeBtn.innerHTML = 'خیر'
        deleteBtn.innerHTML = "بلی"
        
        
        innerDivDetail0.appendChild(operationSpan)
        innerDiv.appendChild(innerDivDetail0)
        this.j+=1;
      
          innerDivDetail1.appendChild(span11)
          innerDivDetail1.appendChild(span12)
          innerDivDetail1.appendChild(freezDiv)
          innerDivDetail4.appendChild(span41)
          innerDivDetail4.appendChild(span42)
          innerDiv.appendChild(innerDivDetail1)
          innerDiv.appendChild(innerDivDetail2)
          innerDiv.appendChild(innerDivDetail4)

          div.onclick = ((event)=>{
            for(let i=0;i<this.taskArray.length;i++){
              if(this.taskArray[i].id == div.id){
                this.divIndex = i
                break
              }
            }
          })
          operationSpan.onclick = ((event)=>{
            this.operationPressed = true
            this.divIndex = parseInt(div.id.split('divId')[1])
            this.openOperation = true
            let elem = null

            let darsStr = ''
            let faslStr = ''
            let goftarStr = ''
            let mabhasStr = ''
          
            for(let i=0;i<this.taskArray.length;i++){
              if(this.taskArray[i].id == 'divId'+this.divIndex){
                elem = this.taskArray[i]
                if(elem.tashrihi === false){
                  //is test
                  this.tashrihi = false
                } else{
                  //is read
                  this.tashrihi = true
                }
                break
              }
            }
            for(let i=0;i<elem.dars.length;i++){
              let comma = true
              if(i === elem.dars.length-1){
                comma = false
              }
              darsStr+=elem.dars[i]
            }
            this.detailDars = darsStr
            this.detailExtra = elem.extra!=null?elem.extra:''
            for(let i=0;i<elem.fasl.length;i++){
              let comma = true
              if(i === elem.dars.length-1){
                comma = false
              }
              faslStr+=this.faslOptions[elem.fasl[i]].label+( comma ?' ,':'')
             
            }
            this.detailFasl = faslStr
            for(let i=0;i<elem.goftar.length;i++){
              let comma = true
              if(i === elem.dars.length-1){
                comma = false
              }
              goftarStr+=this.goftarOptions[elem.goftar[i]].label+( comma ?' ,':'')
              
            }
            this.detailGoftar = goftarStr
            // for(let i=0;i<elem.mabhas.length;i++){
            //   let comma = true
            //   if(i === elem.dars.length-1){
            //     comma = false
            //   }
            //   mabhasStr+=this.articleOptions[elem.mabhas[0]].label+( comma ?' ,':'')
             
            // }
            // this.detailArticle = mabhasStr
            this.detailArticle = this.articleOptions[elem.mabhas].label
            
            this.detailTime = this.taskArray[this.divIndex].hourTime+':'+this.taskArray[this.divIndex].quarterTime*15
            this.detailTestTime = this.taskArray[this.divIndex].testHourTime+':'+this.taskArray[this.divIndex].testQuarterTime*15
            this.detailTestCount = this.taskArray[this.divIndex].testCount
            this.detailSTime = this.taskArray[this.divIndex].startTime
            this.detailETime = this.taskArray[this.divIndex].endTime
            
              this.taskArray[this.divIndex].hourTime = parseInt(this.hourTime)
              this.taskArray[this.divIndex].quarterTime = parseInt(this.quarterTime)
         
              this.taskArray[this.divIndex].testHourTime = parseInt(this.testHourTime)
              this.taskArray[this.divIndex].testQuarterTime = parseInt(this.testQuarterTime)
          
          })

        
        dragDiv.appendChild(innerDiv)
        if(this.rooz == 1){
          this.sh[this.row].appendChild(div)
        }else if(this.rooz == 2){
          this.onesh[this.row].appendChild(div)
        }else if(this.rooz == 3){
          this.twosh[this.row].appendChild(div)
        }else if(this.rooz == 4){
          this.threesh[this.row].appendChild(div)
        }else if(this.rooz == 5){
          this.foursh[this.row].appendChild(div)
        }else if(this.rooz == 6){
          this.fivesh[this.row].appendChild(div)
        }else if(this.rooz == 7){
          this.jome[this.row].appendChild(div)
        }else if(this.rooz == 8||this.rooz == 0){
          this.sixsh[this.row].appendChild(div)
        }
        let lessonStr = ''
        let currentDate = 0
        if(this.rooz == 8 || this.rooz == 0){
          currentDate = 0
        }else{
          currentDate = this.rooz
        }
        
        if(copy == false){
          taskObject.id = div.id
          taskObject.dars = this.lessons
          taskObject.extra = this.extra
          taskObject.fasl = this.fasls
          taskObject.goftar = this.goftars
          taskObject.mabhas = this.articles
          taskObject.hourTime = this.tashrihi === false?0: this.hourTime
          taskObject.quarterTime = this.tashrihi === false?0:this.quarterTime
          taskObject.testHourTime = this.tashrihi === true?0:this.testHourTime
          taskObject.testQuarterTime = this.tashrihi === true?0:this.testQuarterTime
          taskObject.testCount = this.tashrihi === true?0:this.testCount
          taskObject.rooz = this.rooz
          taskObject.row = this.row
          taskObject.draged = 0
          taskObject.color = this.taskColor
          taskObject.endTime = this.tashrihi === false? this.testHourTime+':'+this.testQuarterTime*15:this.hourTime+':'+this.quarterTime*15
          taskObject.pin = this.pin
          if(this.pin == true){
            if(this.pinCode !== null){
              taskObject.pinCode = this.pinCode
            }else{
              this.pinIndex = parseInt(LocalStorage.get.item("pinIndex"))
              taskObject.pinCode = this.pinIndex+1
              LocalStorage.set("pinIndex", this.pinIndex+1)
              this.pinIndex++
            }
            
          }else{
              taskObject.pinCode = null
          }
          taskObject.date = this.datesYear[currentDate]
          taskObject.haveHalf = this.futureTop
          taskObject.taskType = this.taskType
          taskObject.tashrihi = this.tashrihi
          if(this.rooz == 7){
            taskObject.rooz7 = div.id
          }else{
            taskObject.rooz7 = this.rooz7
          }
          this.taskArray.push(taskObject)
          this.pinCode = null
          taskObject.pin == true? this.montakhabArray.push(taskObject):''
          LocalStorage.set('montakhabArray', this.montakhabArray)
          this.divIndex = this.taskArray.length-1
          if(this.taskArray[this.divIndex].pin == true)
          freezDiv.classList.add('show')
          else if(this.taskArray[this.divIndex].pin == false)
          freezDiv.classList.add('hide')
          if(this.lessons.length!=0){
            lessonStr = this.lessons[0]
            if(this.lessons.length >1){
              lessonStr += ', ...'
            }
          }
          else{
            lessonStr = this.extra
          }

          //_________add a task to rooz == 7
          // if(this.rooz == 7){
          //   if(this.weekIndex >= this.totalTasksArray.length && this.weekIndex != 0){
          //     this.totalTasksArray = LocalStorage.get.item("totalTasksArray") !== null ?LocalStorage.get.item("totalTasksArray"):LocalStorage.set("totalTasksArray", []);
          //     let next = this.totalTasksArray[this.weekIndex+1]
          //     let obj = {...taskObject}
          //     obj.rooz = 0
          //     next.push(obj)
          //     this.totalTasksArray[this.weekIndex+1] = next
          
          //   }
          //   LocalStorage.set("totalTasksArray", this.totalTasksArray)
          // }
          //_________add a task to rooz == 0 
          // else if(this.rooz == 0){
          //   if(this.weekIndex != 0){
          //     this.totalTasksArray = LocalStorage.get.item("totalTasksArray") !== null ?LocalStorage.get.item("totalTasksArray"):LocalStorage.set("totalTasksArray", []);
          //     let last = this.totalTasksArray[this.weekIndex-1]
          //     let obj = {...taskObject}
          //     obj.rooz = 7
          //     last.push(obj)
          //     this.totalTasksArray[this.weekIndex-1] = last
          
          //   }
          //   LocalStorage.set("totalTasksArray", this.totalTasksArray)
          // }
          //___________________end__________________
        }else{
          if(this.taskArray[this.divIndex].dars.length!=0){
            lessonStr = this.taskArray[this.divIndex].dars[0]
            if(this.taskArray[this.divIndex].dars.length >1){
              lessonStr += ', ...'
            }
          }
          else{
            lessonStr = this.taskArray[this.divIndex].extra
          }
          if(this.taskArray[this.divIndex].pin == true)
          freezDiv.classList.add('show')
          else if(this.taskArray[this.divIndex].pin == false)
          freezDiv.classList.add('hide')
        }
        span12.innerHTML = lessonStr

        let articlesStr = ''
        span22.innerHTML = articlesStr
        if(this.lessons.length != 0){
          if(this.tashrihi === false){
            div.style.top = '50px'
            span11.innerHTML = 'تعداد تست:'
            span12.innerHTML = this.testCount
            span41.innerHTML = 'مدت زمان تست:'
            span42.innerHTML = this.testHourTime+':'+this.testQuarterTime*15
          }else if(this.tashrihi === true){
            div.style.top = '0px'
          }
        }else{
          div.style.height = '100px'
          div.style.top = '0px'
        }
   
      var popups = document.getElementsByClassName("popup");
      var element = null;
      var startY, startWidth, startHeight;
      if(popups!=null){
          for (var i = 0; i < popups.length; i++) {
            var p = popups[i];
            let bottom = document.createElement("div");
              bottom.className = "resizer-bottom";
              // p.appendChild(bottom);
  
            bottom.onmouseup =(()=>{      
                let str = bottom.parentPopup.style.height.split('px')[0]
                let h = 0
                if((str/50)%1 > 0.25){
                  h= 50*(Math.ceil((str/50)))
                }else{
                  h= 50*(Math.ceil((str)/50)-1)
                }
                let newStr = h
                let hours = Math.floor(Math.ceil((newStr/50))/4)
                let mins = Math.floor(Math.ceil((newStr/50))%4)*15
                this.taskArray[bottom.parentPopup.id.split('divId')[1]].endTime = hours+':'+mins
            })
            bottom.parentPopup = p;
          }

          function initDrag(e) {
            element = this.parentPopup;
            startY = e.clientY;
            startWidth = parseInt(
              document.defaultView.getComputedStyle(element).width,
              10
            );
            startHeight = parseInt(
              document.defaultView.getComputedStyle(element).height,
              10
            );
          
            document.documentElement.addEventListener("mousemove", doDrag, false);
            document.documentElement.addEventListener("mouseup", stopDrag, false);
      
          }
          function doDrag(e) {
            element.style.height = startHeight + e.clientY - startY + "px";
          }
          function stopDrag(e) {
            element.style.height = startHeight + e.clientY - startY + "px";
            document.documentElement.removeEventListener("mousemove", doDrag, false);
            document.documentElement.removeEventListener("mouseup", stopDrag, false);
            let str = element.style.height.split('px')[0]
            if((str/50)%1 > 0.25){
              element.style.height= 50*(Math.ceil((str/50)))+'px' 
            }else{
              element.style.height= 50*(Math.ceil((str)/50)-1)+'px'
            }
            let newStr = element.style.height.split('px')[0]
            let hours = Math.floor(Math.ceil((newStr/50))/4)
            let mins = Math.floor(Math.ceil((newStr/50))%4)*15
            element.childNodes[0].getElementsByTagName('span')[6].innerHTML = hours+':'+mins
            modalDetailEndTime.innerHTML = span42.innerHTML
          }
        }
    }
  },
  mounted(){
    this.getDate()
    this.firstDaysArray = LocalStorage.get.item("firstDaysArray") !== null ?LocalStorage.get.item("firstDaysArray"):LocalStorage.set("firstDaysArray", this.dates[0]);
    this.totalTasksArray = LocalStorage.get.item("totalTasksArray") !== null ?LocalStorage.get.item("totalTasksArray"):LocalStorage.set("totalTasksArray", []);
    this.pinIndex = (LocalStorage.get.item("pinIndex") != 0 && LocalStorage.get.item("pinIndex") !== null) ?LocalStorage.get.item("pinIndex"):LocalStorage.set("pinIndex", 0);
    this.montakhabArray = LocalStorage.get.item("montakhabArray") !== null ?LocalStorage.get.item("montakhabArray"):LocalStorage.set("montakhabArray", []);
    this.usesMontakhab = LocalStorage.get.item("usesMontakhab") !== null ?LocalStorage.get.item("usesMontakhab"):LocalStorage.set("usesMontakhab", []);
    
    this.weekIndex = LocalStorage.get.item("weekIndex")!= null ?parseInt(LocalStorage.get.item("weekIndex")):0
    LocalStorage.set("nextWeekFirst", this.dates[this.dates.length-1])
    LocalStorage.set("lastWeekFirst", this.firstDaysArray[this.weekIndex])
    LocalStorage.set("thisWeekFirst", this.dates[0])
  
    this.constToday = LocalStorage.get.item("constToday") != null ?LocalStorage.get.item("constToday"):null
    this.thisWeekFirst = LocalStorage.get.item("thisWeekFirst") != null ?LocalStorage.get.item("thisWeekFirst"):null
    this.nextWeekFirst = LocalStorage.get.item("nextWeekFirst") != null ?LocalStorage.get.item("nextWeekFirst"):null;
    this.lastWeekFirst = LocalStorage.get.item("firstDaysArray") != null ?LocalStorage.get.item("firstDaysArray")[this.weekIndex-1]:null;

    this.jome = document.getElementsByName('1tdName')
    this.fivesh = document.getElementsByName('2tdName')
    this.foursh = document.getElementsByName('3tdName')
    this.threesh = document.getElementsByName('4tdName')
    this.twosh = document.getElementsByName('5tdName')
    this.onesh = document.getElementsByName('6tdName')
    this.sh = document.getElementsByName('7tdName')
    this.sixsh = document.getElementsByName('8tdName')
    for(let i=0; i<this.timeTable.length; i++){
      this.jome[i].classList.add('jome')
      this.sh[i].classList.add('jome')
      this.onesh[i].classList.add('jome')
      this.twosh[i].classList.add('jome')
      this.threesh[i].classList.add('jome')
      this.foursh[i].classList.add('jome')
      this.fivesh[i].classList.add('jome')
      this.sixsh[i].classList.add('jome')
      this.sixsh[i].addEventListener('dblclick',(e)=>{
        this.dbclicked = true
        this.row = i
        this.rooz = 8
        if(this.row>this.timeTable.length-10){
          this.detailExtra = 'extra'; this.operationPressed = true; this.tashrihi = true
        }else{
          this.detailExtra = ''
        } 
      })
      this.sh[i].addEventListener('dblclick',(e)=>{
        this.dbclicked = true
        this.row = i
        this.rooz = 1
        if(this.row>this.timeTable.length-10){
          this.detailExtra = 'extra'; this.operationPressed = true; this.tashrihi = true
        }else{
          this.detailExtra = ''
        } 
      })
      this.onesh[i].addEventListener('dblclick',(e)=>{
        this.dbclicked = true
        this.row = i
        this.rooz = 2
        if(this.row>this.timeTable.length-10){
          this.detailExtra = 'extra'; this.operationPressed = true; this.tashrihi = true
        }else{
          this.detailExtra = ''
        } 
      })
      this.twosh[i].addEventListener('dblclick',(e)=>{
        this.dbclicked = true
        this.row = i
        this.rooz = 3
        if(this.row>this.timeTable.length-10){
          this.detailExtra = 'extra'; this.operationPressed = true; this.tashrihi = true
        }else{
          this.detailExtra = ''
        } 
      })
      this.threesh[i].addEventListener('dblclick',(e)=>{
        this.dbclicked = true
        this.row = i
        this.rooz = 4
        if(this.row>this.timeTable.length-10){
          this.detailExtra = 'extra'; this.operationPressed = true; this.tashrihi = true
        }else{
          this.detailExtra = ''
        } 
      })
      this.foursh[i].addEventListener('dblclick',(e)=>{
        this.dbclicked = true
        this.row = i
        this.rooz = 5
        if(this.row>this.timeTable.length-10){
          this.detailExtra = 'extra'; this.operationPressed = true; this.tashrihi = true
        }else{
          this.detailExtra = ''
        } 
      })
      this.fivesh[i].addEventListener('dblclick',(e)=>{
        this.dbclicked = true
        this.row = i
        this.rooz = 6
        if(this.row>this.timeTable.length-10){
          this.detailExtra = 'extra'; this.operationPressed = true; this.tashrihi = true
        }else{
          this.detailExtra = ''
        } 
      })
      this.jome[i].addEventListener('dblclick',(e)=>{
        this.dbclicked = true
        this.row = i
        this.rooz = 7
        if(this.row>this.timeTable.length-10){
          this.detailExtra = 'extra'; this.operationPressed = true; this.tashrihi = true
        }else{
          this.detailExtra = ''
        } 
      })
    }
    
    if(this.weekIndex >= this.totalTasksArray.length && this.weekIndex != 0){
      this.lastWeek = this.totalTasksArray[this.weekIndex-1].filter(x=>(x.rooz == 7)&&(x.id != -1))
      if(this.montakhab == true){
        this.montakhabArray.map((x)=>{
          this.lastWeek.push(x)
        })
      }
    }
    else{
      this.me = true
      this.lastWeek = this.totalTasksArray[this.weekIndex]
      if(this.usesMontakhab[this.weekIndex] == true){
        this.montakhab = true
      }else{
        this.notEcditable = true
        this.montakhab = false
      }
    }

    if(this.lastWeek != null){
      for(let i=0;i<=this.lastWeek.length-1;i++){
        this.prepareForPins(this.lastWeek[i])
      }
    }
  }
}
</script>

<style>
@import '../../css/dashboard/styles/helpPage.css';
.vr{
  border: 1px dashed ;
}
input {
  border-top-style: hidden;
  border-right-style: hidden;
  border-left-style: hidden;
  background-color: #eee;
  width:50%
}
input :active{
  border-top-style: hidden;
  border-right-style: hidden;
  border-left-style: hidden;
  background-color: #eee;
}
.colorPalette{
  width: 30px;
  height: 30px;
  border-radius: 15px;
  cursor: pointer;
}
.timeTable{
  width: 10% !important;
}
.freez{
  width:10px;
  height: 10px;
  border-radius: 5px;
  background-color: #17d292;
}
.show{
  opacity: 1;
}
.hide{
  opacity: 0;
}
.dd{
  background-color: #17d292;
  width: 100%;
  height: 100%;
}
.jome{
  position: relative;
}
.panelClass{
  position: fixed;
  top: 10%;
  right: 10%;
  left: 10%;
  width: auto;
  background-color: #918a8a;
  z-index: 1000;
}
@font-face {
  font-family: IRANSans;
  font-style: normal;
  font-weight: normal;
  src: url("./../../css/fonts/iran-sans-fa-num-bold.woff2") format("woff2"),
    url("./../../css/fonts/iran-sans-fa-num.woff") format("woff");
}

@font-face {
  font-family: IRANSans;
  font-style: normal;
  font-weight: bold;
  src: url("./../../css/fonts/iran-sans-fa-num-bold.woff2") format("woff2"),
    url("./../../css/fonts/iran-sans-fa-num.woff") format("woff");
}

body {
  font-family: "IRANSans" !important;
}
@import "../../css/dashboard/styles/dashboardStudent.css";
.display{
  display: flex;
}
.noDisplay{
  display: none;
}
.dayWeek{
  width:auto;
  text-align: center;
  white-space: nowrap;
}
.dragHandler{
  width:10px;
  height:10px;
  border-radius: 5px;
  background-color: white;
}
.taskCreatePanel{
  background-color: #e6e4e9;
  border-radius: 5px;
}
.moshaverScope{
  border: 1px solid gray;
  border-radius: 5px;
  margin: 5px;
  padding: 5px;
}

.detail{
  display: flex;
  flex: column;
}
.innerDetail{
  direction: rtl;
  display: flex;
  flex: row;
  font-size: 10px;
  color: black;
  justify-content: center;
}
.innerDetail0{
  width: 100%;
  text-align: center;
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  direction: rtl;
  display: flex;
  flex: row;
  font-size: 10px;
  color: black;
  justify-content: center;
  /* font-weight: bold; */
  cursor: pointer;
}
.modalDetailDiv{
  color: #ffffff;
  background-color: #918a8a;
  border-radius:10px ;
  text-align: center;
  margin: auto;
  padding: 10px;
  position: absolute;
  top:50%;
  right: 20%;
  left: 20%;
  opacity: 0;
  width: auto;
  white-space: wrap !important;
  pointer-events: none;
  align-self: center;
  display: flex;
  flex-direction: column;
  cursor: pointer;
  z-index: 100000;
}
.operation{
  background-color: #918a8a;
  border-radius:10px ;
  padding: 10px;
  position: fixed;
  top:20%;
  right: 20%;
  left: 20%;
  bottom: 40%;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  z-index: 600;
  max-height: 300px;
}
.operationBtn{
  width:auto ;
  height: auto;
  color: black;
  background-color: white;
  border-radius:5px ;
  padding: 10px;
  cursor: pointer;
  text-align: center;
}
.resizeHidden{
  resize: vertical;
  background-color: #17d292;
}
.elem-width{
  width: 20%;
}
.resize {
  display: flex;
  flex: column;
  overflow: auto;
  top:0px;
  left:1px;
  width:94px;
  text-align: center;
  align-items: center;
  justify-content: center;
  transform: translate(0px, 0px);
  cursor: pointer;
  position: absolute;
  background-color: #ff6c00;
}
.testWidth{
  width: 40%;
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
table tbody tr .timeTable{
    background-color: yellowgreen;
}
table tbody tr .timeTable:nth-child(odd) {
    background-color: darkkhaki;
}
.thumbEdit {
  display: inline-block;
  width: 100%;
  height: auto;
  margin: 5px;
  border-radius: 100px;
  object-fit: cover;
  object-position: center;
  padding: 5px;
  max-width: 110px;
}

.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  border: 1px solid black;
  border-radius: 5px;
}
.radio-text {
  font-size: 10px;
}
.genderContainer {
  max-width: 200px !important;
  min-width: 180px !important;
  margin-right: 30px !important;
  padding-left: 0px !important;
  height: auto;
}
.uploadNewContainer {
  position: relative;
}
.uploadNew {
  display: none;
  position: absolute;
  border-radius: 10px;
  width: 60%;
  max-width: 80px;
  font-size: 12px;
  text-align: center;
  bottom: 20px;
  right: 20%;
  left: 20%;
  color: white;
  background-color: #17d292;
  z-index: 10;
  cursor: pointer;
}
.uploadNewContainer:hover .uploadNew {
  display: block;
}
.uploadNewContainer:hover .thumbEdit {
  opacity: 0.5 !important;
}
.side-bar-item-m-h {
  position: fixed;
  height: 50px;
  justify-content: center;
  padding: 5px 20px;
  width: 100%;
  background-color: #f2f2f2;
  padding: 0px !important;
}

.side-bar-item-m:hover {
  background-color: #fcfcfc;
}

.side-bar-tooltip {
  position: absolute;
  right: 60px;
  opacity: 0;
  width: 30px;
  height: 28px;
  background-color: #fff;
  border-radius: 5px 0px 0px 5px;
  border: 1px solid #1976d2;
  box-shadow: 1px -1px 0px 0px rgba(25, 118, 210, 1);
  text-align: center;
  vertical-align: middle;
  font-size: 12px;
  pointer-events: none;
  z-index: 1000;
}

.side-bar-item:hover {
  background-color: #ffffff;
}

.side-bar-item:hover .side-bar-tooltip {
  opacity: 1;
}

.detail {
  width: 80px;
  height: 10px;
  color: #1976d2;
  text-align: right;
  padding-top: 0px !important;
  z-index: 102;
}
/**--------------------------------------------------- */
.popup-freez {
  color: #f2f2f2 !important;
  z-index: 9;
  text-align: center;
  width: 100%;
  min-height: 40px;
  border-radius: 3px;
}

.popup-freez {
  position: absolute;
  right: 0px;
  top: 0px;
  overflow: auto;
}

.popup-freez .resizer-bottom {
  width: 100%;
  height: 5px;
  background: #337AFF;
  position: absolute;
  right: 0;
  bottom: 0;
  cursor: n-resize;
}

.popup-freez .resizer-above {
  width: 100%;
  height: 2px;
  background: #337AFF;
  position: absolute;
  right: 0;
  top: 0;
  cursor: n-resize;
}

/*NOSELECT*/

.popup-freez * {
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome and Opera */
}
/**------------------------------------------------------------------------- */
.popupDrag{
  position: absolute;
  top:0;
  height: 95%;
  z-index: 10;
  text-align: center;
  width: 100%;
  cursor: move;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #f2f2f2 !important;
}
.popup {
  color: #f2f2f2 !important;
  z-index: 9;
  text-align: center;
  width: 100%;
  min-height: 40px;
}

/*Drgable */

.popup {
  position: absolute;
  right: 0px;
  top: 0px;
  overflow: auto;
}


/*Resizeable*/

.popup .resizer-bottom {
  width: 100%;
  height: 5px;
  background: #337AFF;
  position: absolute;
  right: 0;
  bottom: 0;
  cursor: n-resize;
}

.popup .resizer-above {
  width: 100%;
  height: 2px;
  background: #337AFF;
  position: absolute;
  right: 0;
  top: 0;
  cursor: n-resize;
}

/*NOSELECT*/

.popup * {
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome and Opera */
}
@media  (max-width: 425px){
  .panelClass{
        right: 0 !important;
        left: 0 !important;
        overflow-y: scroll;
        max-height: 400px;
      }
      .operation{
        bottom: 25% !important;
        max-height: 400px !important;
      }
}
</style>
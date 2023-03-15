<template>
    <div>
     <bs-modal :show="showFinalTask" size="md" @open="showFinalTask" @close="!showFinalTask">
          <template class="bs-modal-body">
            <div class="mt-4 modalHelpContainer text-center">
              آیا از ثبت برنامه اطمینان دارید؟
            </div>
            <div class="mt-4 modalHelpContainer text-center">
              برنامه های فریز شده (دارای دایره سبز رنگ) برای هفته های آینده, الگو هستند.
            </div>
          </template>
          <template slot="footer">
            <div class=" d-flex flex-row justify-content-around w-50 mx-auto">
              <div class="bg-success rounded-1 c-ffffff text-center cursor-pointer p-2 px-4" @click="finalizeTasks">بلی</div>
              <div class="bg-danger rounded-1 c-ffffff text-center cursor-pointer p-2 px-4" @click="showFinalTask = false">خیر</div>
            </div>
          </template>
        </bs-modal>
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
          <!-- <div class="d-flex flex-row c-ffffff justify-content-between" v-if="detailExtra == ''">
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
          </div> -->
          <div class="d-flex flex-row c-ffffff justify-content-between" v-if="detailExtra == '' && tashrihi == true">
            مدت زمان مطالعه:
            <div class="text-center">{{detailTime}}</div>
          </div>
          <div class="d-flex flex-row c-ffffff justify-content-between" v-if="detailExtra == '' && tashrihi == false">
            مدت زمان تست:
            <div>{{detailTestTime}}</div>
          </div>
          <div class="d-flex flex-row c-ffffff justify-content-between" v-if="detailExtra == ''">
            تعداد تست:
            <div>{{detailTestCount}}</div>
          </div>
          <div class="d-flex flex-row c-ffffff justify-content-between">
            زمان شروع:
            <div>{{detailSTime}}</div>
          </div>
          <div class="d-flex flex-row c-ffffff justify-content-between ">
            مدت زمان کل:
            <div v-if="tashrihi == true">{{detailETime}}</div>
            <div v-else>{{detailTestTime}}</div>
          </div>
        </div>
        <div class="d-flex flex-column align-items-center justify-content-around">
         
          <div class="operationBtn" @click="taskEdit">ویرایش برنامه</div>
          <!-- <div v-if="this.showtimeColumn == true" class="operationBtn" @click="taskCopy">کپی برنامه</div> -->
          <div class="operationBtn" @click="taskDelete">حذف برنامه</div>
          <div @click="openOperation = false" class="text-center c-ffffff cursor-pointer p-1">
            بازگشت
          </div>
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
      <!-- پایان قسمت عنوان صفحه -->
      <!-- show any content -->
  
      <!-- داشبورد من -->
      <!-- گریدبندی گلتراش -->
      <br />
     
      <!-- انتخای باکس ساخت آزمون -->
      <!-- <div class="resize">
  hi
      </div> -->
      <!-- <dv v-drag>Drag me!</div> -->
    <div class="row">
      <div class="d-flex flex-row mb-2 justify-content-between align-items-center">
        <div class="d-flex flex-row mb-2">
          <q-checkbox v-model="showtimeColumn"/> 
          میخواهم ستون زمان بندی نمایان باشد
        </div>
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
              <!-- <tr height="100px" style="width:100%">
                <td class="position-relative dayWeek" v-for="id in dayWeeksIds" :key="id" :id="id">
                </td>
                <td v-if="showtimeColumn==true">۷:۰۰-۷:۳۰</td>
              </tr> -->
              <tr v-for="l in 48" :key="l" :id="l" height="100px" style="width:100%">
                <td v-for="i in 8" :key="i"  height="100px" :name="i+'tdName'"></td>
                <!-- <td><span style="opacity: 0;"  height="100px">{{odd=!odd}}</span></td> -->
                <td height="100px" v-if="showtimeColumn==true" class="timeTable">{{timeTable[l-1]}}</td>
              </tr>
            </tbody>
          </table> 
      </div>
      <div v-if="dbclicked == true" class="panelClass" style="overflow-y: scroll;">
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
        <div class="d-flex flex-row me-3 justify-end mb-2 ms-2" v-if="operationPressed == false && showTashrihi == true">
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
          <div>
       
            </div>
          <!-- <div class="d-flex flex-row justify-content-around" v-if="ableDars == true">
              <div class="d-flex align-items-center testWidth">درس</div>
              <div class="d-flex flex-column testWidth">
              <q-select
                v-model="lessons"
                :options="lessonOptions"
                multiple
                clearable
                class="bg-ffffff rounded-1 pt-1 me-2"
            />
            <span v-if="showDarsErr == true" class="c-ff0048 font-10 text-center">حداقل یک درس را انتخاب نمایید.</span>
            </div>
          </div>
          <div class="d-flex flex-row justify-content-around" v-if="ableExtra == true && tashrihi == true">
              <div class="d-flex align-items-center testWidth">متفرقه</div>
              <div class="d-flex flex-column testWidth">
              <q-select
                v-model="extra"
                :options="extraOptions"
                clearable
                class="bg-ffffff rounded-1 pt-1 me-2"
            />
            <span v-if="showDarsErr == true" class="c-ff0048 font-10 text-center">حداقل یک مورد را انتخاب نمایید.</span>
            </div>
          </div>
          <div class="d-flex flex-row justify-content-around">
              <div class="d-flex align-items-center testWidth">فصل</div>
              <q-select
                v-model="fasls"
                :options="faslOptions"
                multiple
                clearable
                class="bg-ffffff rounded-1 testWidth pt-1 me-2"
            />
          </div>
          <div class="d-flex flex-row justify-content-around">
              <div class="d-flex align-items-center testWidth">گفتار</div>
              <q-select
                v-model="goftars"
                :options="goftarOptions"
                multiple
                clearable
                class="bg-ffffff rounded-1 testWidth pt-1 me-2"
            />
          </div>
          <div class="d-flex flex-row justify-content-around">
              <div class="d-flex align-items-center testWidth">مبحث</div>
              <q-select
                v-model="articles"
                :options="articleOptions"
                multiple
                clearable
                class="bg-ffffff rounded-1 testWidth pt-1 me-2"
            />
          </div> -->
          <div class="d-flex flex-row justify-content-around" v-if="ableDars == true">
              <div class="d-flex align-items-center testWidth">درس</div>
              <div class="d-flex flex-column testWidth">
              <q-select
                v-model="lessons"
                :options="lessonOptions"
                clearable
                class="bg-ffffff rounded-1 pt-1 me-2"
            />
            <span v-if="showDarsErr == true" class="c-ff0048 font-10 text-center">حداقل یک درس را انتخاب نمایید.</span>
            </div>
          </div>
          <q-tree v-if="lessons != 0" style="padding-top:0px"
              id="treeget"
              :nodes="topicStore.treeDataByLessonId(lessons)"
              class="q-pt-lg"
              color="primary"
              node-key="Id"
              tick-strategy="none"
              :ticked.sync="weeklyLessonList[0].TopicIds"
            >
              <div slot="header-custom" slot-scope="prop">
                <template v-if="prop.node.ParentTopicId == null">
                {{ lessonOptions.filter(x=>x.value == lessons)[0].label }}
                </template>
                <div v-else>
                  <template v-if="prop.node.children.length != 0">{{ prop.node.label }}</template>
                  <template v-else-if="prop.node.children.length == 0">
                    <div class="d-flex flex-row align-items-center">
                      <input type="radio" style="width:20px; margin-left:5px" name="onlyOneArticle" v-model="selectedArticle" :value="prop.node.Id"/>
                      {{ prop.node.label }}
                    </div>
                  </template>
                </div>
              </div>
            </q-tree>
            <span v-if="showSelectArticleErr == true" class="c-ff0048 font-10 text-center">حداقل یک مبحث را از این درس انتخاب نمایید.</span>
          <!-- <div class="d-flex flex-row justify-content-around" v-if="ableExtra == true && (lessons == 0 || lessons == null)">
              <div class="d-flex align-items-center testWidth">متفرقه</div>
              <div class="d-flex flex-column testWidth">
              <q-select
                v-model="extra"
                :options="extraOptions"
                clearable
                class="bg-ffffff rounded-1 pt-1 me-2"
            />
            <span v-if="showDarsErr == true" class="c-ff0048 font-10 text-center">حداقل یک مورد را انتخاب نمایید.</span>
            </div>
          </div> -->
          <div class="d-flex flex-row justify-content-around" v-if="extra==null">
              <div class="d-flex align-items-center testWidth">نوع برنامه</div>
              <div class="d-flex flex-column testWidth">
              <q-select
                v-model="taskType"
                :options="taskOptions"
                clearable
                class="bg-ffffff rounded-1 pt-1 me-2"
            />
            <span v-if="showTaskTypeErr == true" class="c-ff0048 font-10 text-center">حداقل یک مورد را انتخاب نمایید.</span>
            </div>
          </div>
          
          <div class="moshaverScope">
        <div>
          <div class="d-flex flex-row justify-content-around">
            <div class="d-flex flex-column w-25" v-if="tashrihi == true">
              <div class="d-flex flex-row justify-content-between align-items-center">
                <div v-if="detailExtra == ''&& tashrihi == true && showTashrihi == true">مدت زمان مطالعه </div>
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
            <div class="d-flex flex-column w-25" v-if="tashrihi == false && showTashrihi == true">
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
            <div class="vr" v-if="tashrihi == false && showTashrihi == true"></div>
            <div class="d-flex flex-column w-25" v-if="tashrihi == false && showTashrihi == true">
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
        <div class="d-flex flex-row justify-content-around mt-2">
          توضیحات:
          <q-input class="bg-ffffff w-75 h-25" v-model="description"/>
        </div>
        <div class="d-flex flex-row justify-content-around">
          <div v-if="showEditBtn == false" class="cursor-pointer bg-7244b0 c-ffffff p-2 rounded-1 testWidth justify-content-start text-center mt-3 " @click="addTask(false, false)">افزودن به برنامه</div>
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
  import * as api from 'src/API';
  import { API_URL } from "src/utilities/site-config";
  
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
import { chownSync } from "fs";
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
      showSelectArticleErr: false,
      showTaskTypeErr: false,
      lessonsByReshte:[],
      reshteId:0,
      selectedArticle:null,
      topicStore: vxm.topicStore,
      weeklyLessonList: vxm.assayStore._weeklyLessonList,
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
        taskOptions:[{label:'یادگیری', value:1},{label:'تثبیت', value:2},{label:'یادآوری', value:3}],
        timeTable : ['۷:۰۰-۷:۳۰','۷:۳۰-۸:۰۰','۸:۰۰-۸:۳۰','۸:۳۰-۹:۰۰','۹:۰۰-۹:۳۰','۹:۳۰-۱۰:۰۰','۱۰:۰۰-۱۰:۳۰','۱۰:۳۰-۱۱:۰۰','۱۱:۰۰-۱۱:۳۰','۱۱:۳۰-۱۲:۰۰','۱۲:۰۰-۱۲:۳۰','۱۲:۳۰-۱۳:۰۰','۱۳:۰۰-۱۳:۳۰','۱۳:۳۰-۱۴:۰۰','۱۴:۰۰-۱۴:۳۰','۱۴:۳۰-۱۵:۰۰','۱۵:۰۰-۱۵:۳۰','۱۵:۳۰-۱۶:۰۰','۱۶:۰۰-۱۶:۳۰','۱۶:۳۰-۱۷:۰۰','۱۷:۰۰-۱۷:۳۰','۱۷:۳۰-۱۸:۰۰','۱۸:۰۰-۱۸:۳۰','۱۸:۳۰-۱۹:۰۰','۱۹:۰۰-۱۹:۳۰','۱۹:۳۰-۲۰:۰۰','۲۰:۰۰-۲۰:۳۰','۲۰:۳۰-۲۱:۰۰','۲۱:۰۰-۲۱:۳۰','۲۱:۳۰-۲۲:۰۰','۲۲:۰۰-۲۲:۳۰','۲۲:۳۰-۲۳:۰۰','۲۳:۰۰-۲۳:۳۰','۲۳:۳۰-۲۴:۰۰','۰۰:۰۰-۰۰:۳۰','۰۰:۳۰-۱:۰۰','۱:۰۰-۱:۳۰','۱:۳۰-۲:۰۰','۲:۰۰-۲:۳۰','۲:۳۰-۳:۰۰','۳:۰۰-۳:۳۰','۳:۳۰-۴:۰۰','۴:۰۰-۴:۳۰','۴:۳۰-۵:۰۰','۵:۰۰-۵:۳۰','۵:۳۰-۶:۰۰','۶:۰۰-۶:۳۰','۶:۳۰-۷:۰۰'],
        timeTableQuarter : ['۷:۰۰-۷:۱۵','۷:۱۵-۷:۳۰','۷:۳۰-۷:۴۵','۷:۴۵-۸:۰۰','۸:۰۰-۸:۱۵','۸:۱۵-۸:۳۰','۸:۳۰-۸:۴۵','۸:۴۵-۹:۰۰','۹:۰۰-۹:۱۵','۹:۱۵-۹:۳۰','۹:۳۰-۹:۴۵','۹:۴۵-۱۰:۰۰','۱۰:۰۰-۱۰:۱۵','۱۰:۱۵-۱۰:۳۰','۱۰:۳۰-۱۰:۴۵','۱۰:۴۵-۱۱:۰۰','۱۱:۰۰-۱۱:۱۵','۱۱:۱۵-۱۱:۳۰','۱۱:۳۰-۱۱:۴۵','۱۱:۴۵-۱۲:۰۰','۱۲:۰۰-۱۲:۱۵','۱۲:۱۵-۱۲:۳۰','۱۲:۳۰-۱۲:۴۵','۱۲:۴۵-۱۳:۰۰','۱۳:۰۰-۱۳:۱۵','۱۳:۱۵-۱۳:۳۰','۱۳:۳۰-۱۳:۴۵','۱۳:۴۵-۱۴:۰۰','۱۴:۰۰-۱۴:۱۵','۱۴:۱۵-۱۴:۳۰','۱۴:۳۰-۱۴:۴۵','۱۴:۴۵-۱۵:۰۰','۱۵:۰۰-۱۵:۱۵','۱۵:۱۵-۱۵:۳۰','۱۵:۳۰-۱۵:۴۵','۱۵:۴۵-۱۶:۰۰','۱۶:۰۰-۱۶:۱۵','۱۶:۱۵-۱۶:۳۰','۱۶:۳۰-۱۶:۴۵','۱۶:۴۵-۱۷:۰۰','۱۷:۰۰-۱۷:۱۵','۱۷:۱۵-۱۷:۳۰','۱۷:۳۰-۱۷:۴۵','۱۷:۴۵-۱۸:۰۰','۱۸:۰۰-۱۸:۱۵','۱۸:۱۵-۱۸:۳۰','۱۸:۳۰-۱۸:۴۵','۱۸:۴۵-۱۹:۰۰','۱۹:۰۰-۱۹:۱۵','۱۹:۱۵-۱۹:۳۰','۱۹:۳۰-۱۹:۴۵','۱۹:۴۵-۲۰:۰۰','۲۰:۰۰-۲۰:۱۵','۲۰:۱۵-۲۰:۳۰','۲۰:۳۰-۲۰:۴۵','۲۰:۴۵-۲۱:۰۰','۲۱:۰۰-۲۱:۱۵','۲۱:۱۵-۲۱:۳۰','۲۱:۳۰-۲۱:۴۵','۲۱:۴۵-۲۲:۰۰','۲۲:۰۰-۲۲:۱۵','۲۲:۱۵-۲۲:۳۰','۲۲:۳۰-۲۲:۴۵','۲۲:۴۵-۲۳:۰۰','۲۳:۰۰-۲۳:۱۵','۲۳:۱۵-۲۳:۳۰','۲۳:۳۰-۲۳:۴۵','۲۳:۴۵-۲۴:۰۰','۰۰:۰۰-۰۰:۱۵','۰۰:۱۵-۰۰:۳۰','۰۰:۳۰-۰۰:۴۵','۰۰:۴۵-۱:۰۰','۱:۰۰-۱:۱۵','۱:۱۵-۱:۳۰','۱:۳۰-۱:۴۵','۱:۴۵-۲:۰۰','۲:۰۰-۲:۱۵','۲:۱۵-۲:۳۰','۲:۳۰-۲:۴۵','۲:۴۵-۳:۰۰','۳:۰۰-۳:۱۵','۳:۱۵-۳:۳۰','۳:۳۰-۳:۴۵','۳:۴۵-۴:۰۰','۴:۰۰-۴:۱۵','۴:۱۵-۴:۳۰','۴:۳۰-۴:۴۵','۴:۴۵-۵:۰۰','۵:۰۰-۵:۱۵','۵:۱۵-۵:۳۰','۵:۳۰-۵:۴۵','۵:۴۵-۶:۰۰','۶:۰۰-۶:۱۵','۶:۱۵-۶:۳۰','۶:۳۰-۶:۴۵','۶:۴۵-۷:۰۰'],
        dayWeeks:["جمعه","پنج شنبه","چهار شنبه","سه شنبه","دو شنبه","یک شنبه","شنبه"],
        dayWeeksDateNum: [5,4,3,2,1,0,6],
        dayOptions:[{label:'شنبه', value:6},{label:'یکشنبه', value:5},{label:'دوشنبه', value:4},{label:'سه شنبه', value:3},
        {label:'چهارشنبه', value:2},{label:'پنجشنبه', value:1},{label:'جمعه', value:0}],
        dayWeeksIds:['jome','5sh','4sh','3sh','2sh','1sh','sh','6sh'],
        days:[],
        lessonOptions:[],
        lessons:0,
        extraOptions:[{label:'مدرسه', value:0},{label:'کلاس کنکور', value:1},{label:'کلاس فوق العاده', value:2},{label:'خواب ', value:3},
        {label:'استراحت', value:4},{label:'رفت و آمد مدرسه', value:5},{label:'رفت و آمد کلاس کنکور', value:6},{label:'رفت و آمد کلاس فوق العاده', value:7},{label:'رفت و آمد متفرقه ', value:8}],
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
        
       if(newLessons != 0){
        this.showDarsErr = false
        this.ableExtra = false
        this.showTashrihi = true
       }else{
        this.ableExtra = true
       }   
       if(newLessons == 0 || newLessons == null){
        this.ableExtra = true
       }
      },
      extra(newLessons, oldLessons) {
       if(newLessons != null){
        this.showDarsErr = false
        this.ableDars = false
        this.extra != null ? 
        this.showTashrihi = false:
        this.showTashrihi = true
       }else{
        this.ableDars = true
        this.showTashrihi = true
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
      getReshte(){
        axios.get(`/api/User/GetUserReshte/2`).then((response) => {
          this.reshteId = response.data
          console.log(this.reshteId,'this.reshteId')
          axiosQuestion.get(`/Api/Lesson/GetAllByEducationTreeId/${this.reshteId}`).then((responsee) => {
            this.lessonsByReshte = responsee.data;
            console.log( responsee.data,'lessons')
            this.makeLessons()
         }); 
        });   
      },
      makeLessons(){
        this.lessonsByReshte.map((lesson)=>{
          let obj = {value:lesson.Id, label:lesson.Name}
          this.lessonOptions.push(obj)
        })
      },
      print(str){
        console.log(str,'STR')
        console.log(this.selectedArticle,'selecteddd')
      },
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
      let finalTaskArray = []
      this.confirmedTasks.map((task)=>{
        let finalTask={
          MabhasId:'',
          RozHafte:'',
          Radif:0,
          Tarikh:'',
          Zaman:0,
          ZamanShoro:'',
          ZamanPayan:0,
          DaneshAmoozId:2,
          NoeBarnameId:0,
          ZamanTashrihi:0,
          ZamanTesti:0,
          TedadTest:0,
          Tozihat:''
        }
        let newDate = task.date.replaceAll('/','-')
        let date= newDate+'T00:00:00Z'
        let timeSplit = ''
        let shoro = ''
        let s = ''+task.startTime
        if(s.includes(':')){
          timeSplit = task.startTime.split(':')
          if(parseInt(timeSplit[0])<10 && timeSplit[0] != '00'){
            shoro = newDate+'T0'+task.startTime+':00Z'
          }else{
            shoro = newDate+'T'+task.startTime+':00Z'
          }
        }else{
          shoro = newDate+'T00:00:11Z'
        }
        let tashrihi = 0
        let testi = 0
        if(task.hourTime == 0 && task.quarterTime == 0){
          tashrihi = 0
          testi = task.testHourTime*60+task.testQuarterTime*15
        }else if(task.testHourTime == 0 && task.testQuarterTime == 0){
          tashrihi = task.hourTime*60+task.quarterTime*15
          testi = 0
        }
        let zaman = tashrihi+testi
        
        finalTask.MabhasId = task.MabhasId
        finalTask.RozHafte = task.rooz
        finalTask.Radif = task.row
        finalTask.Tarikh = date
        finalTask.Zaman = zaman
        finalTask.ZamanShoro = shoro
        finalTask.ZamanPayan = date
        finalTask.DaneshAmoozId = 2
        finalTask.NoeBarnameId = task.taskType
        finalTask.ZamanTashrihi = tashrihi
        finalTask.ZamanTesti = testi
        finalTask.TedadTest = task.testCount
        finalTask.Tozihat = task.description
        finalTaskArray.push(finalTask)
      })
      console.log(finalTaskArray,'FINAL')
      this.createBarname(finalTaskArray)
      router.push('/stSchedule')
    },
    createBarname(finalTaskArray){
      console.log('createBarname')
      axios.post('/api/BarnameHaftegi/CreateList',finalTaskArray)
        .then(response => {
          console.log(response,'RESPONSE')
            if(response.data.MessageType == 1)
            {
                Notify.create({
                  progress: true,
                  message: 'برنامه هفتگی با موفقیت اضافه شد',
                  color: 'green',
                  actions: [
                  ]
                })
            }
            else
            {
                Notify.create({
                  progress: true,
                  message: 'خطایی رخ داده است با مدیر تماس بگیرید',
                  color: 'green',
                  actions: [
                  ]
                })
            }
          });
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
          router.push('/moshaver')
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
      router.push('/moshaver')

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
      router.push('/moshaver')

  },
      finalizeTasks(){
        this.finalizeTask = true
        this.showFinalTask = false
        
        LocalStorage.set("WeekTask", '');
        LocalStorage.set("nextWeekDate", '');
      },
      cancelTask(){
        this.print('ss')
        this.operationPressed = false
        this.dbclicked = false
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
          this.taskArray[i].pin = this.pin
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
          div.style.height = ((parseInt(this.hourTime)+parseInt(this.testHourTime))*2+(parseInt(this.quarterTime)+parseInt(this.testQuarterTime))/2)*100+'px'
  
          let operation = document.getElementById('operation'+id[1])
          let span11 = document.getElementById('span11'+id[1])
          let span12 = document.getElementById('darsId'+id[1])
          let span31 = document.getElementById('span31'+id[1])
          let span32 = document.getElementById('span32'+id[1])
          let span41 = document.getElementById('span41'+id[1])
          let span42 = document.getElementById('span42'+this.divIndex)
       
          
          // span42.innerHTML = div.style.height
         
          span42.innerHTML = (parseInt(this.hourTime)+parseInt(this.testHourTime)+':'+(parseInt(this.quarterTime)+parseInt(this.testQuarterTime))*15)

          span11.innerHTML = this.lessons.length == 0?"عنوان:":"درس:"
          if(this.defaultTaskColor == false){
              if(this.taskColor == 'ff7f27'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-ff7f27')
                operation.style.color = '#000000'
                span11.style.color = '#000000'
                span31.style.color = '#000000'
                span41.style.color = '#000000'
                span12.style.color = '#000000'
                span32.style.color = '#000000'
                span42.style.color = '#000000'
              }
              if(this.taskColor == 'a349a4'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-a349a4')
                operation.style.color = '#ffffff'
                span11.style.color = '#ffffff'
                span31.style.color = '#ffffff'
                span41.style.color = '#ffffff'
                span12.style.color = '#ffffff'
                span32.style.color = '#ffffff'
                span42.style.color = '#ffffff'
              }
              if(this.taskColor == 'c8bfe7'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-c8bfe7')
                operation.style.color = '#ffffff'
                span11.style.color = '#ffffff'
                span31.style.color = '#ffffff'
                span41.style.color = '#ffffff'
                span12.style.color = '#ffffff'
                span32.style.color = '#ffffff'
                span42.style.color = '#ffffff'
              }
              if(this.taskColor == '3f48cc'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-3f48cc')
                operation.style.color = '#ffffff'
                span11.style.color = '#ffffff'
                span31.style.color = '#ffffff'
                span41.style.color = '#ffffff'
                span12.style.color = '#ffffff'
                span32.style.color = '#ffffff'
                span42.style.color = '#ffffff'
              }
              if(this.taskColor == '7092be'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-7092be')
                operation.style.color = '#000000'
                span11.style.color = '#000000'
                span31.style.color = '#000000'
                span41.style.color = '#000000'
                span12.style.color = '#000000'
                span32.style.color = '#000000'
                span42.style.color = '#000000'
              }
              if(this.taskColor == '00a2e8'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-00a2e8')
                operation.style.color = '#000000'
                span11.style.color = '#000000'
                span31.style.color = '#000000'
                span41.style.color = '#000000'
                span12.style.color = '#000000'
                span32.style.color = '#000000'
                span42.style.color = '#000000'
              }
              if(this.taskColor == '99d9ea'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-99d9ea')
                operation.style.color = '#000000'
                span11.style.color = '#000000'
                span31.style.color = '#000000'
                span41.style.color = '#000000'
                span12.style.color = '#000000'
                span32.style.color = '#000000'
                span42.style.color = '#000000'
              }
              if(this.taskColor == 'c4c4c4'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-c4c4c4')
                operation.style.color = '#000000'
                span11.style.color = '#000000'
                span31.style.color = '#000000'
                span41.style.color = '#000000'
                span12.style.color = '#000000'
                span32.style.color = '#000000'
                span42.style.color = '#000000'
              }
              if(this.taskColor == '22b14c'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-22b14c')
                operation.style.color = '#000000'
                span11.style.color = '#000000'
                span31.style.color = '#000000'
                span41.style.color = '#000000'
                span12.style.color = '#000000'
                span32.style.color = '#000000'
                span42.style.color = '#000000'
              }
              if(this.taskColor == 'b5e61d'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-b5e61d')
                operation.style.color = '#000000'
                span11.style.color = '#000000'
                span31.style.color = '#000000'
                span41.style.color = '#000000'
                span12.style.color = '#000000'
                span32.style.color = '#000000'
                span42.style.color = '#000000'
              }
              if(this.taskColor == 'fff200'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-fff200')
                operation.style.color = '#000000'
                span11.style.color = '#000000'
                span31.style.color = '#000000'
                span41.style.color = '#000000'
                span12.style.color = '#000000'
                span32.style.color = '#000000'
                span42.style.color = '#000000'
              }
              if(this.taskColor == 'ffc90e'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-ffc90e')
                operation.style.color = '#000000'
                span11.style.color = '#000000'
                span31.style.color = '#000000'
                span41.style.color = '#000000'
                span12.style.color = '#000000'
                span32.style.color = '#000000'
                span42.style.color = '#000000'
              }
              if(this.taskColor == 'ffaec9'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-ffaec9')
                operation.style.color = '#000000'
                span11.style.color = '#000000'
                span31.style.color = '#000000'
                span41.style.color = '#000000'
                span12.style.color = '#000000'
                span32.style.color = '#000000'
                span42.style.color = '#000000'
              }
              if(this.taskColor == 'ed1c24'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-ed1c24')
                operation.style.color = '#ffffff'
                span11.style.color = '#ffffff'
                span31.style.color = '#ffffff'
                span41.style.color = '#ffffff'
                span12.style.color = '#ffffff'
                span32.style.color = '#ffffff'
                span42.style.color = '#ffffff'
              }
              if(this.taskColor == '880015'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-880015')
                operation.style.color = '#ffffff'
                span11.style.color = '#ffffff'
                span31.style.color = '#ffffff'
                span41.style.color = '#ffffff'
                span12.style.color = '#ffffff'
                span32.style.color = '#ffffff'
                span42.style.color = '#ffffff'
              }
              if(this.taskColor == 'b97a57'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-b97a57')
                operation.style.color = '#000000'
                span11.style.color = '#000000'
                span31.style.color = '#000000'
                span41.style.color = '#000000'
                span12.style.color = '#000000'
                span32.style.color = '#000000'
                span42.style.color = '#000000'
              }
              if(this.taskColor == 'c3c3c3'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-c3c3c3')
                operation.style.color = '#000000'
                span11.style.color = '#000000'
                span31.style.color = '#000000'
                span41.style.color = '#000000'
                span12.style.color = '#000000'
                span32.style.color = '#000000'
                span42.style.color = '#000000'
              }
              if(this.taskColor == '7f7f7f'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-7f7f7f')
                operation.style.color = '#000000'
                span11.style.color = '#000000'
                span31.style.color = '#000000'
                span41.style.color = '#000000'
                span12.style.color = '#000000'
                span32.style.color = '#000000'
                span42.style.color = '#000000'
              }
              if(this.taskColor == '000000'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-000000')
                operation.style.color = '#ffffff'
                span11.style.color = '#ffffff'
                span31.style.color = '#ffffff'
                span41.style.color = '#ffffff'
                span12.style.color = '#ffffff'
                span32.style.color = '#ffffff'
                span42.style.color = '#ffffff'
              }
              if(this.taskColor == 'ffffff'){
                div.classList.remove(lastColorClass)
                div.classList.add('bg-ffffff')
                operation.style.color = '#000000'
                span11.style.color = '#000000'
                span31.style.color = '#000000'
                span41.style.color = '#000000'
                span12.style.color = '#000000'
                span32.style.color = '#000000'
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
                lessonStr = this.lessonOptions[this.lessons].label
                lessonStr += ', ...'
              }else if(this.lessons.length  == 0){
                lessonStr = 'انتخاب نشده'
              }else if(this.lessons.length == 1){
                lessonStr = this.lessonOptions[this.lessons].label
              }
          }else{
            lessonStr = this.extraOptions[this.extra].label
          }
          if(this.lessons!=0 && this.lessons!=null){
            span12.innerHTML = this.lessonOptions.filter(x=>x.value == this.lessons)[0].label 
          }else{
            span12.innerHTML = this.extraOptions.filter(x=>x.value == this.extra)[0].label
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
        this.addTask(false, true)
      },
        // prepareForPins(obj){
        //   this.defaultTaskColor = false
        //   obj.endTime = obj.endTime === '30:0'?'00:30':obj.endTime
        //   this.futureTop = false
        //   this.row = obj.row+Math.floor(parseInt(obj.draged)/100)
        //   if(this.row < 0){
        //     this.row = 0
        //   }
        //   this.futureTop = false
        //   if(obj.draged%100 != 0 || obj.haveHalf == true){
        //     this.futureTop = true
        //   }
        //   this.hourTime = parseInt(obj.endTime.split(':')[0])
        //   this.quarterTime = parseInt(obj.endTime.split(':')[1])/15
        //   this.taskColor = obj.color
        //   this.lessons = obj.dars
        //   this.extra = obj.extra
        //   this.fasls = obj.fasl
        //   this.goftars = obj.goftar
        //   this.articles = obj.mabhas
        //   this.testHourTime = obj.testHourTime
        //   this.testHourTime = obj.testQuarterTime
        //   this.testCount = obj.testCount
        //   this.rooz = obj.rooz == 7? 0: obj.rooz
        //   this.pin = obj.pin
        //   this.taskType  = obj.taskType
        //   this.addTask(false, true)
        // },
      addTask(copy , future){
        if((this.lessons == 0 || this.lessons == null) && this.extra == null && future == false){
          this.showDarsErr = true
        }else if(this.lessons != 0 && this.extra == null && this.taskType == null){
          this.showTaskTypeErr = true
        }else if(this.lessons != 0 && this.extra == null && this.selectedArticle == null){
          this.showSelectArticleErr = true
        }
        else{
          this.showDarsErr = false
          this.showTaskTypeErr = false
          this.showSelectArticleErr = false
        //   if(copy == false){
        //     this.hourTime = 0
        //     this.quarterTime = 0
        //     this.testQuarterTime = 0
        //     this.testHourTime = 0
        // }
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
        let span32 = ''
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
          MabhasId:0,
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
          pinCode: null,
          description: ''
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
          let span31 = document.createElement("span")
          span31.id = 'span31'+this.j
          span32 = document.createElement("span")
          span32.id = 'span32'+this.j
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
  
          span32.innerHTML = span32.innerHTML == 'آزاد'?'آزاد': this.timeTable[this.row].split('-')[0]
          span42.innerHTML = (parseInt(this.hourTime)+parseInt(this.testHourTime)+':'+(parseInt(this.quarterTime)+parseInt(this.testQuarterTime))*15)
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
                span31.style.color = '#ffffff'
                span41.style.color = '#ffffff'
                span12.style.color = '#ffffff'
                span32.style.color = '#ffffff'
                span42.style.color = '#ffffff'
              }
              if(this.taskColor == 'a349a4'){
                taskObject.taskColor = 'a349a4'
                div.classList.add('bg-a349a4')
                operationSpan.style.color = '#ffffff'
                span11.style.color = '#ffffff'
                span31.style.color = '#ffffff'
                span41.style.color = '#ffffff'
                span12.style.color = '#ffffff'
                span32.style.color = '#ffffff'
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
                span31.style.color = '#ffffff'
                span41.style.color = '#ffffff'
                span12.style.color = '#ffffff'
                span32.style.color = '#ffffff'
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
                span31.style.color = '#ffffff'
                span41.style.color = '#ffffff'
                span12.style.color = '#ffffff'
                span32.style.color = '#ffffff'
                span42.style.color = '#ffffff'
              }
              if(this.taskColor == '880015'){
              taskObject.taskColor = '880015'
                div.classList.add('bg-880015')
                operationSpan.style.color = '#ffffff'
                span11.style.color = '#ffffff'
                span31.style.color = '#ffffff'
                span41.style.color = '#ffffff'
                span12.style.color = '#ffffff'
                span32.style.color = '#ffffff'
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
                span31.style.color = '#ffffff'
                span41.style.color = '#ffffff'
                span12.style.color = '#ffffff'
                span32.style.color = '#ffffff'
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
          this.showtimeColumn == true?
          this.tashrihi == true? div.style.height = ((parseInt(this.hourTime))*2+(parseInt(this.quarterTime))/2)*100+'px':
          div.style.height = ((parseInt(this.testHourTime))*2+(parseInt(this.testQuarterTime))/2)*100+'px':
          div.style.height = '100px'
         
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
          span31.innerHTML = "شروع:"
          this.tashrihi == true? span41.innerHTML = "مدت زمان:":span41.innerHTML = 'مدت زمان تست:'
          this.tashrihi == false? span42.innerHTML = this.testHourTime+':'+this.testQuarterTime*15:
          span42.innerHTML = this.hourTime+':'+this.quarterTime*15
          deleteSpan.innerHTML = "آیا از حذف این برنامه اطمینان دارید؟"
          copySpan.innerHTML = "کپی"
          editSpan.innerHTML = "ویرایش"
          closeBtn.innerHTML = 'خیر'
          deleteBtn.innerHTML = "بلی"
          
          innerDivDetail0.appendChild(freezDiv)
          innerDivDetail0.appendChild(operationSpan)
          innerDiv.appendChild(innerDivDetail0)
          this.j+=1;
        
            innerDivDetail1.appendChild(span11)
            innerDivDetail1.appendChild(span12)
            innerDivDetail3.appendChild(span31)
            innerDivDetail3.appendChild(span32)
            innerDivDetail4.appendChild(span41)
            innerDivDetail4.appendChild(span42)
            
            innerDiv.appendChild(innerDivDetail1)
            innerDiv.appendChild(innerDivDetail2)
            // this.showtimeColumn == true?innerDiv.appendChild(innerDivDetail3):''
            innerDiv.appendChild(innerDivDetail3)
            if(this.showtimeColumn == false){
              span32.innerHTML = 'آزاد'
            }
            
            innerDiv.appendChild(innerDivDetail4)
  
            div.onclick = ((event)=>{
              for(let i=0;i<this.taskArray.length;i++){
                if(this.taskArray[i].id == div.id){
                  this.divIndex = i
                  break
                }
              }
            })
            // this.detailTestTime = this.taskArray[this.divIndex].testHourTime+':'+this.taskArray[this.divIndex].testQuarterTime*15
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
                darsStr+=this.lessonOptions[elem.dars[i]].label+( comma ?' ,':'')
              }
              this.detailDars = darsStr
              this.detailExtra = elem.extra!=null?this.extraOptions[elem.extra].label:''
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
              for(let i=0;i<elem.mabhas.length;i++){
                let comma = true
                if(i === elem.dars.length-1){
                  comma = false
                }
                mabhasStr+=this.articleOptions[elem.mabhas[i]].label+( comma ?' ,':'')
              }
              this.detailArticle = mabhasStr
              this.detailTestCount = this.taskArray[this.divIndex].testCount
              this.detailSTime = this.taskArray[this.divIndex].startTime
              this.detailETime = this.taskArray[this.divIndex].endTime
              if(this.taskArray[this.divIndex].hourTime!=0 || this.taskArray[this.divIndex].quarterTime!=0){
                //tashrihi
                this.taskArray[this.divIndex].hourTime = parseInt(this.detailETime.split(':')[0])
                this.taskArray[this.divIndex].quarterTime = parseInt(this.detailETime.split(':')[1]/15)
              }else{
                //testi
                this.taskArray[this.divIndex].testHourTime = parseInt(this.detailETime.split(':')[0])
                this.taskArray[this.divIndex].testQuarterTime = parseInt(this.detailETime.split(':')[1]/15)
              }
              this.detailTime = this.taskArray[this.divIndex].hourTime+':'+this.taskArray[this.divIndex].quarterTime*15
              this.detailTestTime = this.taskArray[this.divIndex].testHourTime+':'+this.taskArray[this.divIndex].testQuarterTime*15
              
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
            if(taskObject.extra != null){
              this.operationPressed = true
            }
            taskObject.fasl = this.fasls
            taskObject.goftar = this.goftars
            taskObject.mabhas = this.articles
            taskObject.hourTime = this.tashrihi == true? this.hourTime:0
            taskObject.quarterTime = this.tashrihi == true? this.quarterTime:0
            taskObject.testHourTime = this.tashrihi == false? this.testHourTime:0
            taskObject.testQuarterTime = this.tashrihi == false? this.testQuarterTime:0
            taskObject.testCount = this.testCount
            taskObject.rooz = this.rooz
            taskObject.row = this.row
            taskObject.draged = 0
            taskObject.color = this.taskColor
            taskObject.startTime = this.makePersianNum(span32.innerHTML)
            taskObject.endTime = ((taskObject.hourTime+taskObject.testHourTime)+':'+(taskObject.quarterTime+taskObject.testQuarterTime)*15)
            taskObject.pin = this.pin
            taskObject.description = this.description
            taskObject.MabhasId = this.selectedArticle

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
            lessonStr = this.lessons
            if(this.lessons.length >1){
              lessonStr += ', ...'
            }
          }
          else{
            lessonStr = this.extra
          }
          }else{
            if(this.taskArray[this.divIndex].dars.length!=0){
              lessonStr = this.lessonOptions[this.taskArray[this.divIndex].dars[0]].label
              if(this.taskArray[this.divIndex].dars.length >1){
                lessonStr += ', ...'
              }
            }
            else{
              lessonStr = this.extraOptions[this.taskArray[this.divIndex].extra].label
            }
            if(this.taskArray[this.divIndex].pin == true)
            freezDiv.classList.add('show')
            else if(this.taskArray[this.divIndex].pin == false)
            freezDiv.classList.add('hide')
          }
          if(this.lessons!=0 && this.lessons!=null){
            span12.innerHTML = this.lessonOptions.filter(x=>x.value == this.lessons)[0].label 
          }else{
            span12.innerHTML = this.extraOptions.filter(x=>x.value == this.extra)[0].label
          }
            
  
          let articlesStr = ''
          span22.innerHTML = articlesStr
          if(this.futureTop == true){
            div.style.top = '50px'
            span32.innerHTML = span32.innerHTML == 'آزاد'? 'آزاد':this.timeTableQuarter[Math.floor(div.offsetTop/50)+this.taskArray[this.divIndex].row*2].split('-')[0]
            this.taskArray[this.divIndex].startTime = this.makePersianNum(span32.innerHTML)
          } 
          dragDiv.onmousedown = ((event)=>{
            if(this.showtimeColumn == true){
            this.divIndex = parseInt(div.id.split('divId')[1])
            event.preventDefault()
            this.positions0.clientY = event.clientY
            this.positions0.clientX = event.clientX
            dragDiv.onmousemove = ((e)=>{
                e.preventDefault()
                this.positions0.movementY = this.positions0.clientY - e.clientY
                this.positions0.clientY = e.clientY
                this.positions0.movementX = this.positions0.clientX - e.clientX
                this.positions0.clientX = e.clientX
               if(this.resize == false) div.style.top = (div.offsetTop - this.positions0.movementY)+ 'px'
              div.style.left = (div.offsetLeft - this.positions0.movementX)+ 'px'
            })
            dragDiv.onmouseup = (()=>{    
              dragDiv.onmouseup = null
              dragDiv.onmousemove = null  
              let distance = parseInt((this.taskArray[this.divIndex].row)*100)
              let top = parseInt(div.offsetTop)
              if(top < -distance){
                  div.style.top = -distance+'px'
              }
              if(Math.abs(div.offsetLeft)>div.offsetWidth/2){
                let count = Math.floor(Math.abs(div.offsetLeft)/div.offsetWidth)
                let right = div.offsetLeft>0? true: false
                let remain = Math.abs(div.offsetLeft)%div.offsetWidth
                if(remain>div.offsetWidth/2){
                  count+=1
                }
                let newParent=0
                if(right== true){
                  newParent = this.rooz-count
                  this.rooz -= count
                }else{
                  newParent = this.rooz+count
                  this.rooz += count
                }
                if(newParent > 7){
                  newParent = 7
                }
                else if(newParent < 0){
                  newParent = 0
                }
                if(newParent == 0){
                      this.sixsh[this.row].appendChild(div)
                }
                if(newParent == 1){
                  this.sh[this.row].appendChild(div)
                }else if(newParent == 2){
                  this.onesh[this.row].appendChild(div)
                  
                }else if(newParent == 3){
                  this.twosh[this.row].appendChild(div)
                  
                }else if(newParent == 4){
                  this.threesh[this.row].appendChild(div)
                  
                }else if(newParent == 5){
                  this.foursh[this.row].appendChild(div)
                  
                }else if(newParent == 6){
                  this.fivesh[this.row].appendChild(div)
                  
                }else if(newParent == 7){
                  this.jome[this.row].appendChild(div)
        
                }
                this.taskArray[this.divIndex].rooz = newParent
                div.style.left = 0
              }
              else{
                div.style.left = 0
              }
              if((div.offsetTop/10)%1 > 0.25){
                div.style.top = 50*(Math.floor(div.offsetTop/50)+1)+'px'  
              }else{
                div.style.top = 50*(Math.floor(div.offsetTop/50))+'px'
              }


              this.taskArray[this.divIndex].draged = div.offsetTop
            
              span32.innerHTML = span32.innerHTML == 'آزاد'?'آزاد':this.timeTableQuarter[Math.floor(div.offsetTop/50)+this.taskArray[this.divIndex].row*2].split('-')[0]
              span42.innerHTML = Math.floor(parseInt(div.style.height.split('px')[0])/200) +':'+((parseInt(div.style.height.split('px')[0])%200)/100)*30
  
              modalDetailStartTime.innerHTML = (span32.innerHTML)
              modalDetailEndTime.innerHTML = span42.innerHTML
  
              for(let i =0;i<this.taskArray.length;i++){
                if(this.taskArray[i].id == div.id){
                  this.taskArray[i].startTime = this.makePersianNum(span32.innerHTML)
                  this.taskArray[i].endTime = span42.innerHTML
                  if(this.taskArray[i].hourTime!=0 || this.taskArray[i].quarterTime!=0){
                    this.taskArray[i].hourTime = parseInt(span42.innerHTML.split(':')[0])
                    this.taskArray[i].quarterTime = parseInt(span42.innerHTML.split(':')[1]/15)
                  }else{
                    this.taskArray[i].testHourTime = parseInt(span42.innerHTML.split(':')[0])
                    this.taskArray[i].testQuarterTime = parseInt(span42.innerHTML.split(':')[1]/15)
                  }
                }
              }
              this.taskArray[this.divIndex].date =this.datesYear[this.taskArray[this.divIndex].rooz]
            })
          }
          }) 
     
  
        //-----
     
        var popups = document.getElementsByClassName("popup");
        var element = null;
        var startY, startWidth, startHeight;
        if(popups!=null){
            for (var i = 0; i < popups.length; i++) {
              var p = popups[i];
              let bottom = document.createElement("div");
                bottom.className = "resizer-bottom";
                p.appendChild(bottom);
                if(this.showtimeColumn == true){
                  bottom.addEventListener("mousedown", initDrag, false);
                }
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
                  if(this.taskArray[bottom.parentPopup.id.split('divId')[1]].tashrihi == true){
                    this.taskArray[bottom.parentPopup.id.split('divId')[1]].hourTime = hours
                    this.taskArray[bottom.parentPopup.id.split('divId')[1]].quarterTime = mins/15
                  }
                  else if(this.taskArray[bottom.parentPopup.id.split('divId')[1]].tashrihi == false){
                    this.taskArray[bottom.parentPopup.id.split('divId')[1]].testHourTime = hours
                    this.taskArray[bottom.parentPopup.id.split('divId')[1]].testQuarterTime = mins/15
                  }
                  console.log(this.taskArray[bottom.parentPopup.id.split('divId')[1]].endTime,'END')
                  if(this.taskArray[bottom.parentPopup.id.split('divId')[1]].hourTime!==0 || this.taskArray[bottom.parentPopup.id.split('divId')[1]].quarterTime!==0){
                    //tashrihi
                    this.taskArray[bottom.parentPopup.id.split('divId')[1]].hourTime = parseInt(hours)
                    this.taskArray[bottom.parentPopup.id.split('divId')[1]].quarterTime = parseInt(mins/15)
                  }
                  else if(this.taskArray[bottom.parentPopup.id.split('divId')[1]].testHourTime!==0 || this.taskArray[bottom.parentPopup.id.split('divId')[1]].testQuarterTime!==0){
                    //tashrihi
                    this.taskArray[bottom.parentPopup.id.split('divId')[1]].testHourTime = parseInt(hours)
                    this.taskArray[bottom.parentPopup.id.split('divId')[1]].testQuarterTime = parseInt(mins/15)
                  }
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
              modalDetailStartTime.innerHTML = span32.innerHTML
              modalDetailEndTime.innerHTML = span42.innerHTML
            }
          }
          this.lessons = 0
          this.extra = null
          this.taskType = null
        }
        // this.lessons = []
        // this.extra = null
        // this.fasls = []
        // this.goftars = []
        // this.articles = []
        // this.hourTime = 0
        // this.quarterTime = 0
        // this.testCount = 0
        // this.testHourTime = 0
        // this.testQuarterTime = 0
        // this.hourTime = 0
        // this.quarterTime = 0
        
      }
    },
     mounted(){
      this.getReshte()
      this.getDate()
      let topicList = vxm.topicStore.fillListt();
      console.log(topicList,'TOPIC')
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
      for(let i=0; i<46; i++){
        this.jome[i].classList.add('jome')
        this.sh[i].classList.add('jome')
        this.onesh[i].classList.add('jome')
        this.twosh[i].classList.add('jome')
        this.threesh[i].classList.add('jome')
        this.foursh[i].classList.add('jome')
        this.fivesh[i].classList.add('jome')
        this.sixsh[i].classList.add('jome')
        this.sixsh[i].addEventListener('dblclick',(e)=>{
          this.operationPressed = false
          this.dbclicked = true
          this.row = i
          this.rooz = 8
        })
        this.sh[i].addEventListener('dblclick',(e)=>{
          this.operationPressed = false
          this.dbclicked = true
          this.row = i
          this.rooz = 1
        })
        this.onesh[i].addEventListener('dblclick',(e)=>{
          this.operationPressed = false
          this.dbclicked = true
          this.row = i
          this.rooz = 2
        })
        this.twosh[i].addEventListener('dblclick',(e)=>{
          this.operationPressed = false
          this.dbclicked = true
          this.row = i
          this.rooz = 3
        })
        this.threesh[i].addEventListener('dblclick',(e)=>{
          this.operationPressed = false
          this.dbclicked = true
          this.row = i
          this.rooz = 4
        })
        this.foursh[i].addEventListener('dblclick',(e)=>{
          this.operationPressed = false
          this.dbclicked = true
          this.row = i
          this.rooz = 5
        })
        this.fivesh[i].addEventListener('dblclick',(e)=>{
          this.operationPressed = false
          this.dbclicked = true
          this.row = i
          this.rooz = 6
        })
        this.jome[i].addEventListener('dblclick',(e)=>{
          this.operationPressed = false
          this.dbclicked = true
          this.row = i
          this.rooz = 7
        })
      }
      //-----
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
      //-----
    }
  }
    // }
  </script>
  
  <style>
  @import '../../css/dashboard/styles/helpPage.css';
  .q-icon{
    display: inline !important;
  }
  .q-tree-node .q-tree-node-child {
  padding-left: 0px !important;
  padding-right: 32px !important;
  direction: rtl !important;
}
.q-tree-node .q-tree-node-child ::after {
  content: none !important;
}
.q-tree-node .q-tree-node-child ::before {
  content: none !important;
}
.q-tree-node .q-tree-node-parent ::after {
  content: none !important;
}
.q-tree-node .q-tree-node-parent ::before {
  content: none !important;
}
.q-tree-node .q-tree-children ::after {
  content: none !important;
}
.q-tree-node .q-tree-children ::before {
  content: none !important;
}
.q-tab .row {
  width: 30% !important;
}
.q-tree .row {
  flex-wrap: nowrap !important;
  display: flex !important;
  flex-direction: row !important;
}
.q-option {
  width: 50px !important;
}
.q-option-inner {
  width: 20px !important;
}
.row > .q-tab {
  width: 20% !important;
  background-color: #1976d2 !important;
  color: white !important;
}
.q-btn-dense.q-btn-round {
  background-color: #06a971 !important;
}
  .vr{
    border: 1px dashed ;
  }
  input {
    border-top-style: hidden;
    border-right-style: hidden;
    border-left-style: hidden;
    /* border-bottom-style: groove; */
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
    /* border: 1px solid gray */
  }
  .timeTable{
    /* min-width: 50px;
    max-width: 70px; */
    width: 40px;
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
    height:520px;
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
    font-weight: bold;
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
    /* background-color: #F4D076 ; */
    text-align: center;
    width: 100%;
    /* height: 50px; */
    min-height: 100px;
    border-radius: 3px;
    /* border:1px solid gray */
  }
  
  /*Drgable */
  
  .popup-freez {
    position: absolute;
    right: 0px;
    top: 0px;
    /*resize: both; !*enable this to css resize*! */
    overflow: auto;
  }
  
  
  /*Resizeable*/
  
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
    /* background-color: #F4D076 ; */
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
    /* background-color: #F4D076 ; */
    text-align: center;
    width: 100%;
    /* height: 50px; */
    min-height: 100px;
    border-radius: 3px;
    /* border:1px solid gray */
  }
  
  /*Drgable */
  
  .popup {
    position: absolute;
    right: 0px;
    top: 0px;
    /*resize: both; !*enable this to css resize*! */
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
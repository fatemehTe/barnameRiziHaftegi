<template>
    <div class="main">
        <link href="http://213.232.124.150:63839/css/all.css" rel="stylesheet" />
        <div class="row pt-4">
            <div class="col-12 pb-2 d-flex flex-row align-items-center">
                <h2 class="h3 g-dashboard font-16">کارنامه ارزیابی کلی</h2>
                <div class="bg-01a4ab cursor-pointer rounded-1 c-ffffff text-center px-2 p-1 me-3" @click="showAddTest = true">افزودن آزمون</div>
            </div>
            <div class="col-12">
                <div class="d-flex gap-2">
                    <span class="border-title border-title-blue d-block"></span>
                    <span class="border-title border-title-gray d-block"></span>
                </div>
            </div>
        </div>
        <div>
            <table style="width:90%; margin:5% 0 5% 1%;" id="table" >
                <thead>
                  <td>نظر مشاور</td>
                  <td>منبع تست ها</td>
                  <td>نوع تست ها</td>
                  <td>تعدا تست های زده شده</td>
                  <td>میزان مطالعه</td>
                  <td>مبحث</td>
                  <td>درس</td>
                </thead>
                <tbody>
                <tr v-for="(array,index) in totalArrayDetail" :key="index" class="m-1 bg-99d9ea">
                        <td>
                            <table style="height: 100%; width: 100%;">
                            <tbody>
                              <tr v-for="(ar,i) in array.mabhas" :key="i">
                                <table v-if="i%2 == 1" style="height: 100%; width: 100%; border:2px solid #007eb4;">
                                  <tbody>
                                    <tr><td style="text-align: end; padding-right:3px; vertical-align: middle;" class="noBorder"><span style="margin-right: 5px;">کامل</span><input type="radio" :name="i+'idea'+index" value="0" v-model="ar.moshaverIdea"/></td></tr>
                                  <tr><td style="text-align: end; padding-right:3px; vertical-align: middle;" class="noBorder"><span style="margin-right: 5px;">بازخوانی</span><input type="radio" :name="i+'idea'+index" value="1" v-model="ar.moshaverIdea"/></td></tr>
                                  <tr><td style="text-align: end; padding-right:3px; vertical-align: middle;" class="noBorder"><span style="margin-right: 5px;">حذف</span><input type="radio" :name="i+'idea'+index" value="2" v-model="ar.moshaverIdea"/></td></tr>
                                  </tbody>
                                </table>
                                <table v-else style="height: 100%; width: 100%;">
                                  <tbody>
                                    <tr><td style="text-align: end; padding-right:3px; vertical-align: middle;" class="noBorder"><span style="margin-right: 5px;">کامل</span><input type="radio" :name="i+'idea'+index" value="0" v-model="ar.moshaverIdea"/></td></tr>
                                  <tr><td style="text-align: end; padding-right:3px; vertical-align: middle;" class="noBorder"><span style="margin-right: 5px;">بازخوانی</span><input type="radio" :name="i+'idea'+index" value="1" v-model="ar.moshaverIdea"/></td></tr>
                                  <tr><td style="text-align: end; padding-right:3px; vertical-align: middle;" class="noBorder"><span style="margin-right: 5px;">حذف</span><input type="radio" :name="i+'idea'+index" value="2" v-model="ar.moshaverIdea"/></td></tr>
                                  </tbody>
                                </table>
                              </tr>
                            </tbody>
                          </table>
                        </td>
                        <td> 
                          <!-- <table style="height: auto; width: 100%" v-for="ar in array.mabhas" :key="ar">
                              <tbody>
                                  <tr><td class="noBorder">1</td></tr>
                                  <tr><td class="noBorder">2</td></tr>
                                  <tr><td class="noBorder">3</td></tr>
                              </tbody>
                            </table> -->
                          <table style="height: 100%; width: 100%;">
                            <tbody>
                              <tr v-for="(ar,i) in array.mabhas" :key="i">
                                <table v-if="i%2 == 1" style="height: 100%; width: 100%; border:2px solid #007eb4">
                                  <tbody>
                                    <tr><td class="noBorder">1</td></tr>
                                    <tr><td class="noBorder">2</td></tr>
                                    <tr><td class="noBorder">3</td></tr>
                                  </tbody>
                                </table>
                                <table v-else style="height: 100%; width: 100%;">
                                  <tbody>
                                    <tr><td class="noBorder">1</td></tr>
                                    <tr><td class="noBorder">2</td></tr>
                                    <tr><td class="noBorder">3</td></tr>
                                  </tbody>
                                </table>
                              </tr>
                            </tbody>
                          </table>
                        </td>
                        <td> 
                          <!-- <table style="height: auto; width: 100%" v-for="ar in array.mabhas" :key="ar">
                              <tbody>
                                  <tr><td class="noBorder">توانستم</td></tr>
                                  <tr><td class="noBorder">یادگرفتم</td></tr>
                                  <tr><td class="noBorder">نتوانستم</td></tr>
                              </tbody>
                            </table> -->
                          <table style="height: 100%; width: 100%;">
                            <tbody>
                              <tr v-for="(ar,i) in array.mabhas" :key="i">
                                <table v-if="i%2 == 1" style="height: 100%; width: 100%; border:2px solid #007eb4">
                                  <tbody>
                                    <tr><td class="noBorder">توانستم</td></tr>
                                    <tr><td class="noBorder">یادگرفتم</td></tr>
                                    <tr><td class="noBorder">نتوانستم</td></tr>
                                  </tbody>
                                </table>
                                <table v-else style="height: 100%; width: 100%;">
                                  <tbody>
                                    <tr><td class="noBorder">توانستم</td></tr>
                                    <tr><td class="noBorder">یادگرفتم</td></tr>
                                    <tr><td class="noBorder">نتوانستم</td></tr>
                                  </tbody>
                                </table>
                              </tr>
                            </tbody>
                          </table>
                        </td>
                        <td> 
                          <!-- <table style="height: auto; width: 100%; border:2px solid #007eb4" v-for="(ar,i) in array.mabhas" :key="i">
                              <tbody>
                                  <tr><td class="noBorder">{{ar.wholeAble}}</td></tr >
                                  <tr><td class="noBorder">{{ar.wholeLearned}}</td></tr>
                                  <tr><td class="noBorder">{{ar.wholeNotAble}}</td></tr>
                              </tbody>
                            </table> -->
                          <table style="height: 100%; width: 100%;">
                            <tbody>
                              <tr v-for="(ar,i) in array.mabhas" :key="i">
                                <table v-if="i%2 == 1" style="height: 100%; width: 100%; border:2px solid #007eb4">
                                  <tbody>
                                      <tr><td class="noBorder">{{ar.wholeAble}}</td></tr >
                                      <tr><td class="noBorder">{{ar.wholeLearned}}</td></tr>
                                      <tr><td class="noBorder">{{ar.wholeNotAble}}</td></tr>
                                  </tbody>
                                </table>
                                <table v-else style="height: 100%; width: 100%;">
                                  <tbody>
                                      <tr><td class="noBorder">{{ar.wholeAble}}</td></tr >
                                      <tr><td class="noBorder">{{ar.wholeLearned}}</td></tr>
                                      <tr><td class="noBorder">{{ar.wholeNotAble}}</td></tr>
                                  </tbody>
                                </table>
                              </tr>
                            </tbody>
                          </table>
                        </td>
                        <td>   
                          <table style="height: 100%; width: 100%;">
                            <tbody>
                              <tr v-for="(ar,i) in array.mabhas" :key="i">
                                <table v-if="i%2 == 1" style="height: 100%; width: 100%; border:2px solid #007eb4">
                                  <tbody>
                                    <tr v-for="arr in 2" :key="arr">
                                      <td class="noBorder">
                                        <div v-if="arr == 1">تشریحی:{{ar.wholeReadTime}}</div>
                                        <div v-else>تستی:{{ar.wholeTestTime}}</div>
                                      </td>
                                    </tr>
                                  </tbody>
                                </table>
                                <table v-else style="height: 100%; width: 100%;">
                                  <tbody>
                                    <tr v-for="arr in 2" :key="arr">
                                      <td class="noBorder">
                                        <div v-if="arr == 1">تشریحی:{{ar.wholeReadTime}}</div>
                                        <div v-else>تستی:{{ar.wholeTestTime}}</div>
                                      </td>
                                    </tr>
                                  </tbody>
                                </table>
                              </tr>
                            </tbody>
                          </table>
                        </td>
                        <td>
                          <table style="height: 100%; width: 100%;">
                            <tbody>
                              <tr v-for="(ar,iii) in array.mabhas" :key="ar" class="m-1 bg-007eb4">
                                <td v-if="iii%2==1" class="noBorder c-ffffff">{{ar.name}}</td>
                                <td v-else class="noBorder">{{ar.name}}</td>
                              </tr>
                            </tbody>
                          </table>
                        </td>
                        <td>{{array.lessonName}}</td>
                      </tr>
                </tbody>
            </table>
            <button @click="print">tamam</button>
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
          taskTemplate:[
            {lessonName:'ریاضی',mabhas:{name:'تابع',readTime:10, testTime:4, able:10, notAble:5, learned:20, checked:0},checked:0},
            {lessonName:'ریاضی',mabhas:{name:'تابع',readTime:20, testTime:1, able:1, notAble:10, learned:0, checked:0},checked:0},
            {lessonName:'ریاضی',mabhas:{name:'مثلثات',readTime:5, testTime:3, able:5, notAble:10, learned:0, checked:0},checked:0},
            {lessonName:'ریاضی',mabhas:{name:'حد',readTime:3, testTime:0, able:0, notAble:0, learned:0, checked:0},checked:0},
            {lessonName:'ادبیات',mabhas:{name:'نمایشی',readTime:10, testTime:2, able:10, notAble:10, learned:10, checked:0},checked:0},
            {lessonName:'ادبیات',mabhas:{name:'نمایشی',readTime:5, testTime:0, able:0, notAble:0, learned:0, checked:0},checked:0},
            {lessonName:'ادبیات',mabhas:{name:'قصه',readTime:5, testTime:0, able:0, notAble:0, learned:0, checked:0},checked:0},
            {lessonName:'ادبیات',mabhas:{name:'داستان',readTime:5, testTime:0, able:0, notAble:0, learned:0, checked:0},checked:0},
            {lessonName:'ادبیات',mabhas:{name:'راوی',readTime:5, testTime:0, able:0, notAble:0, learned:0, checked:0},checked:0},
            {lessonName:'زبان',mabhas:{name:'مکالمه',readTime:2, testTime:1, able:10, notAble:0, learned:10, checked:0},checked:0},
            {lessonName:'زبان',mabhas:{name:'خواندن',readTime:2, testTime:1, able:10, notAble:0, learned:10, checked:0},checked:0},
            {lessonName:'فلسفه',mabhas:{name:'وجود',readTime:1, testTime:0, able:0, notAble:0, learned:0, checked:0},checked:0}
          ],
          totalArrayDetail:[],
          moshaverIdeaOptions:[{label:'کامل',value:0},{label:'بازخوانی',value:1},{label:'حذف',value:2}]
        };
    },
    watch: {
        
    },
    methods:{
      print(){
        
      }
    },
    mounted(){
      let totalArrayDetail = []
      let mabhasArray = []
      let totalArray = []
      let sameLessons = []

      for(let i=0; i<this.taskTemplate.length; i++){
        if(this.taskTemplate[i].checked == 0){
          sameLessons.push(this.taskTemplate[i])
          this.taskTemplate[i].checked = 1
          if(this.taskTemplate.length>0){
            for(let j=1; j<this.taskTemplate.length; j++){
              if(sameLessons[0].lessonName === this.taskTemplate[j].lessonName && this.taskTemplate[j].checked == 0){
                sameLessons.push(this.taskTemplate[j])
                this.taskTemplate[j].checked = 1
              }
            }
            totalArray.push(sameLessons)
            sameLessons = []
            }
        }   
      }
      let templateLesson = ''
      let templateMabhas = ''
      let templateMabhasWholeReadTime = 0
      let templateMabhasWholeTestTime = 0
      let templateMabhasWholeAble = 0
      let templateMabhasWholeNotAble = 0
      let templateMabhasWholeLearned = 0

      totalArray.map((x, index)=>{
        // x.map((y)=>{
        for(let i=0; i<x.length; i++){
          if(x[i].mabhas.checked == 0){
            templateLesson = x[i].lessonName
            templateMabhas = x[i].mabhas.name
            templateMabhasWholeReadTime = x[i].mabhas.readTime
            templateMabhasWholeTestTime = x[i].mabhas.testTime
            templateMabhasWholeAble = x[i].mabhas.able
            templateMabhasWholeNotAble = x[i].mabhas.notAble
            templateMabhasWholeLearned = x[i].mabhas.learned
            x[i].mabhas.checked = 1
            // if(this.taskTemplate.length>0){
              let mabhasObj = {name:'',wholeReadTime:0, wholeTestTime:0, wholeAble:0, wholeNotAble:0, wholeLearned:0, moshaverIdea: null}
              for(let j=1; j<x.length; j++){
                if(templateMabhas === x[j].mabhas.name && x[j].mabhas.checked == 0){
                  templateMabhasWholeReadTime += x[j].mabhas.readTime
                  templateMabhasWholeTestTime += x[j].mabhas.testTime
                  templateMabhasWholeAble += x[j].mabhas.able
                  templateMabhasWholeNotAble += x[j].mabhas.notAble
                  templateMabhasWholeLearned += x[j].mabhas.learned
                  x[j].mabhas.checked = 1
                }
              }
              mabhasObj.name = templateMabhas
              mabhasObj.wholeReadTime = templateMabhasWholeReadTime
              mabhasObj.wholeTestTime = templateMabhasWholeTestTime
              mabhasObj.wholeAble = templateMabhasWholeAble
              mabhasObj.wholeNotAble = templateMabhasWholeNotAble
              mabhasObj.wholeLearned = templateMabhasWholeLearned
              mabhasArray.push(mabhasObj)
          }   
        }
        let obj={lessonName:'',mabhas:[]}
              obj.lessonName = templateLesson
              obj.mabhas = mabhasArray
              totalArrayDetail.push(obj)
              mabhasArray = []
              this.totalArrayDetail = totalArrayDetail
      })
    }
}
</script>
<style>
.noBorder{
  border-left: 0;
  border-right: 0;
  border-top: 0;
}

.bThird{
    border: 2px solid #007eb4;
    border-radius: 5px;
    background-color: #d0c7e5;
    max-height: 550px;
    padding-right:0px !important ;
}
.bSecond{
    border: 2px solid #007eb4;
    border-radius: 5px;
    /* width: 49.5%; */
    background-color: #d0c7e5;
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
    z-index: 200;
  }
  table thead tr {
    position: -webkit-sticky;
    position: sticky;
    top: 0;
    z-index: 200;
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
.tableLabel{
    background-color: #e1dee8;
    border-radius:10px;
    text-align: center;
    width: auto;
}
input {border:0;outline:0;border-radius: 5px;}
input:focus {outline:none!important;}
</style>
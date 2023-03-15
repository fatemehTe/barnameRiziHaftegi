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
        <bs-modal :show="showAddTest" size="md" @open="showAddTest" @close="!showAddTest" style="z-index: 1000">
          <template class="bs-modal-body">
            <div class="p-3 bg-e6e4e9 h-100 rounded-1 w-50 m-auto">
                <div class="text-right c-01a4ab fw-bold">افزودن آزمون</div>
                <div class="d-flex flex-row justify-content-between align-items-center m-1">
                    <div class="d-inline">نام آزمون:</div>
                    <input class="d-inline" v-model="foreignTest.testName"/>
                </div>
                <div class="d-flex flex-row justify-content-between align-items-center m-1">
                    <div class="d-inline">تاریخ:</div>
                    <input class="d-inline" v-model="foreignTest.testDate"/>
                </div>
                
                <div class="d-flex flex-row justify-content-between align-items-center m-1" v-for="(lessonPercent, index) in testTemplate[0].testLessonPercent" :key="lessonPercent">
                    <div class="d-inline">{{lessonPercent.name}}</div>
                    <input class="d-inline" v-model="foreignTest.testLessonPercent[index].percent"/>
                </div>
            </div>
          </template>
          <template slot="footer">
            <div class=" d-flex flex-row justify-content-around w-50 mx-auto">
              <div v-if="editTestTest === false" class="bg-success rounded-1 c-ffffff text-center cursor-pointer p-2 px-4" @click="addTest()">افزودن</div>
              <div v-if="editTestTest === true" class="bg-primary rounded-1 c-ffffff text-center cursor-pointer p-2 px-4" @click="editOperation()">ویرایش</div>
              <div class="bg-danger rounded-1 c-ffffff text-center cursor-pointer p-2 px-4" @click="noOperation()">انصراف</div>
            </div>
          </template>
        </bs-modal>
        <div>
            <table style="width:90%; margin:5% 0 5% 1%;" id="table">
                <thead>
                <tr height="50px" style="width:100%">
                    <td style="text-align:center; font-weight:bold">
                    <div class="d-flex flex-column align-items-center">
                        <span>عملیات</span>
                    </div>
                    </td>
                    <td style="text-align:center; font-weight:bold" v-for="lesson in testTemplate[0].testLessonPercent" :key="lesson">
                    <div class="d-flex flex-column align-items-center">
                        <span>{{lesson.name}}</span>
                    </div>
                    </td>
                    <td style="text-align:center; font-weight:bold" >
                    <div class="d-flex flex-column align-items-center">
                        <span> تاریخ</span>
                    </div>
                    </td>
                    <td style="text-align:center; font-weight:bold" >
                    <div class="d-flex flex-column align-items-center">
                        <span> نام آزمون</span>
                    </div>
                    </td>
                </tr>
                </thead>
                <tbody>
                <tr height="50px" style="width:100%" v-for="(test, index) in testTemplate" :key="test">
                    <td>
                        <span v-if="index < onlineKhanTestLength" class="color-blue cursor-pointer" @click="goToReport(index)">
                            {{'مشاهده گزارش'}}
                        </span>
                        <span class="c-0a6d4ae7 cursor-pointer" @click="editTest(index)">
                            {{index >= onlineKhanTestLength ? 'ویرایش':''}}
                        </span>
                        <span class="c-ff0048 cursor-pointer" @click="deleteTest(index)">
                            {{index >=onlineKhanTestLength ? 'حذف':''}}
                        </span>
                    </td>
                    <td v-for="percent in test.testLessonPercent" :key="percent">{{percent.percent}}</td>
                    <td>{{test.testDate}}</td>
                    <td>{{test.testName}}</td>
                </tr>
                </tbody>
            </table>
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
           testIndex: 0,
           showAddTest: false,
           editTestTest: false,
           foreignTest:{
            testName:'',
            testDate:'',
            testLessonPercent: [{name:'',percent:''},{name:'',percent:''},{name:'',percent:''},{name:'',percent:''},{name:'',percent:''}]
           },
           assays:[],
           onlineKhanTestLength: 5,
           testTemplate: [
                {testName:'آزمون ۱', testDate:'۱۴۰۱/۱/۱',testLessonPercent:[{name:'ریاضی',percent:'۳۰٪'},{name:'ادبیات',percent:'۱۰٪'},{name:'منطق',percent:'۵٪'},{name:'فلسفه',percent:'۱۰٪'},{name:'روان شناسی',percent:'۲۰٪'}]},
                {testName:'آزمون ۲', testDate:'۱۴۰۱/۱/۸',testLessonPercent:[{name:'ریاضی',percent:'۲۰٪'},{name:'ادبیات',percent:'۲۰٪'},{name:'منطق',percent:'۱۰٪'},{name:'فلسفه',percent:'۱۵٪'},{name:'روانشناسی',percent:'۴۰٪'}]},
                {testName:'آزمون ۳', testDate:'۱۴۰۱/۱/۱۵',testLessonPercent:[{name:'ریاضی',percent:'۱۰٪'},{name:'ادبیات',percent:'۱۵٪'},{name:'منطق',percent:'۲۰٪'},{name:'فلسفه',percent:'۱۰٪'},{name:'روانشناسی',percent:'۵۵٪'}]},
                {testName:'آزمون ۴', testDate:'۱۴۰۱/۱/۲۲',testLessonPercent:[{name:'ریاضی',percent:'۲۵٪'},{name:'ادبیات',percent:'۸۰٪'},{name:'منطق',percent:'۳۰٪'},{name:'فلسفه',percent:'۲۰٪'},{name:'روانشناسی',percent:'۶۰٪'}]},
                {testName:'آزمون ۵', testDate:'۱۴۰۱/۱/۲۹',testLessonPercent:[{name:'ریاضی',percent:'۳۰٪'},{name:'ادبیات',percent:'۱۰۰٪'},{name:'منطق',percent:'۵۰٪'},{name:'فلسفه',percent:'۱۰٪'},{name:'روانشناسی',percent:'۷۰٪'}]}
            ]
        };
    },
    watch: {
        
    },
    methods:{
      getAllAssays()
       {
          axios.get('/api/assay/GetUserAssays').then((response) => {
            console.log(response.data.filter(test=>test.isRunned == true),'DATAAAAA')
            this.assays = response.data.filter(test=>test.isRunned == true);
         });  
       },
        addTest(){
          this.editTestTest = false
            let tem = {
                testName: this.foreignTest.testName,
                testDate: this.foreignTest.testDate,
                testLessonPercent:[]
            }
            for(let i=0;i<this.testTemplate[0].testLessonPercent.length; i++){
                let a = {name:this.testTemplate[0].testLessonPercent[i].name,percent:this.foreignTest.testLessonPercent[i].percent}
                tem.testLessonPercent.push(a)
            }
            this.testTemplate.push(tem)
              this.foreignTest = 
              {
                testName:'',
                testDate:'',
                testLessonPercent: [{name:'',percent:''},{name:'',percent:''},{name:'',percent:''},{name:'',percent:''},{name:'',percent:''}]
              },
            this.showAddTest = false
        },
        editTest(index){
            this.showAddTest = true
            this.editTestTest = true
            this.testIndex = index
            this.foreignTest.testName = this.testTemplate[index].testName
            this.foreignTest.testDate = this.testTemplate[index].testDate
            this.testTemplate[index].testLessonPercent.map((x, index)=>{
                this.foreignTest.testLessonPercent[index].name = x.name
                this.foreignTest.testLessonPercent[index].percent = x.percent
            })
        },
        editOperation(){
          this.testTemplate[this.testIndex] = this.foreignTest
          this.showAddTest = false
          this.editTestTest = false
          this.foreignTest = 
              {
                testName:'',
                testDate:'',
                testLessonPercent: [{name:'',percent:''},{name:'',percent:''},{name:'',percent:''},{name:'',percent:''},{name:'',percent:''}]
              }
        },
        deleteTest(index){
          this.testTemplate.splice(index, 1)
        },
        noOperation(){
          this.showAddTest = false
          this.editTestTest = false
          this.foreignTest = 
              {
                testName:'',
                testDate:'',
                testLessonPercent: [{name:'',percent:''},{name:'',percent:''},{name:'',percent:''},{name:'',percent:''},{name:'',percent:''}]
              }
        },
        goToReport(index){

        }
    },
    mounted(){
        this.getAllAssays()
        console.log(this.assays,'*************')
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
    border: 2px solid #7244b0;
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
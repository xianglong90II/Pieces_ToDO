<template>
    <div class="unit">
        <div class="score">{{ scoreVal }}</div>
        <div class="iconSelect">
          <IconSelect></IconSelect>
        </div>
        <div class="unitTitle">
          <input type="text" v-model="titleInput"
            style="width: 100%;
            color: #2e2e2e;
            height: 100%;  
            border-style: none; 
            font-family: Inter;
            font-size: 16px;
            font-weight: 700;
            line-height: 20px;"
            :placeholder="$t('unit.unitTitleHere')">
        </div>

        <div>
        <el-button @click="unitDelete" :icon="CloseBold" size="small" circle style="
        position: absolute;
        width: 20px;
        height: 20px;
        left: 155px;
        top: 50px;
        z-index: 10;
        " />
        </div>
        <div class="adjustables">
          <div class="firstTitle">{{ $t(val1Title) }}</div>
          <div class="secondTitle">{{ $t(val2Title) }}</div>
          <div class="firstAdjust">
            <el-rate
              v-model="val1"
              :max="3"
              :icons="icons"
              :void-icon="Help"
              :low-threshold="1"
              :high-threshold="3"
              :void-color="unitColor"
              :colors="[unitColor,unitColor,unitColor]"
            />
          </div>
          <div class="secondAdjust">
            <el-rate
              v-model="val2"
              :max="3"
              :icons="icons"
              :void-icon="Help"
              :low-threshold="1"
              :high-threshold="3"
              :void-color="unitColor"
              :colors="[unitColor,unitColor,unitColor]"
              
            />
          </div>
        </div>
        <img class="unitFrame" src="../assets/unitFrame.svg" oncontextmenu="return false;" draggable="false" alt="">
    </div>
</template>

<script setup lang="ts" name="Unit">
// a single pieces unit
import{computed, onBeforeMount, onMounted, ref}from 'vue'
import { Help, HelpFilled, CloseBold } from '@element-plus/icons-vue'
import { useWkStore, useObjStore } from '@/stores/allStore';
import { nanoid } from 'nanoid';
import IconSelect from '@/components/my-day-UI/IconSelect.vue'

let wkStore = useWkStore()
let objStore = useObjStore()
const icons = [HelpFilled,HelpFilled,HelpFilled]

//determine unit types and colors
let unitColor:string
let val1Title:string
let val2Title:string
let props = defineProps(['unitType'])
let unitType = props.unitType
if (unitType == 'working'){
  unitColor = '#5667FF' // translation will affect
  val1Title = 'unit.productivity'
  val2Title = 'unit.duriation'
}else{
  unitColor = '#FF4949'
  val1Title = "unit.difficulty"
  val2Title = "unit.volume"
}
//expose important variables
//determine id
let unitId = nanoid()
const titleInput = ref('')
let val1 = ref(0)
let val2 = ref(0)
let scoreVal = computed(()=>{return val1.value*val2.value})
let exObj = {
  id: unitId,
  title:titleInput,
  val1:val1,
  val2:val2,
}
//push objects while mounted
onMounted(()=>{
  if(unitType=='working'){
    wkStore.wkObjArray.push(exObj)
  }else if(unitType=='objectives'){
    objStore.objObjArray.push(exObj)
  }
})

//clear all units in same type
function unitDelete(){
  console.log('unit deleted')
  if(unitType=='working'){
    // wkStore.wkObjArray=wkStore.wkObjArray.filter(obj => obj.id !== unitId)
    wkStore.wkArray = []
    wkStore.wkObjArray = []
  }else if(unitType=='objectives'){
    // objStore.objObjArray=objStore.objObjArray.filter(obj => obj.id !== unitId)
    objStore.objArray = []
    objStore.objObjArray = []
  }
}

</script>

<style scoped>

.unit {
  position: relative;
  width: 190px;
  height: 150px;
  left: 10%;
}
.score {
  position: absolute;
  top: 10px;
  margin-left: 4%;
  width: 95%;
  height: 20px;
  font-family: Inter;
  font-size: 16px;
  font-weight: 700;
  line-height: 20px;
  text-align: center;
}

.iconSelect{
  position: relative;
  left: 10%;
  top: 32%;
  width: 30px;
  z-index: 5;
}

.unitTitle {
  position: absolute;
  left: 25%;
  top: 32%;
  width: 100px;
  height: 20px;
  z-index: 4;
}

input {
  border-style: none; 
  transition: border-color 0.3s ease; 
}

.adjustables {
  position: absolute;
  left: 12%;
  top: 55%;
  width: 155.71px;
  height: 45.21px;
}

.firstTitle{
  position: absolute;
  left: -5%;
  top: 0px;
  width: 88.47px;
  height: 17.71px;
  color: #2e2e2e;
  font-family: Inter;
  font-size: 14px;
  font-weight: 700;
  line-height: 16.94px;
}
.dark .firstTitle{
  color: #ebebeb;
}
.secondTitle {
  position: absolute;
  left: -5%;
  top: 25.37px;
  width: 78.66px;
  height: 12.13px;
  color: #2e2e2e;
  font-family: Inter;
  font-size: 14px;
  font-weight: 700;
  line-height: 16.94px;
}
.dark .secondTitle{
  color: #ebebeb;
}
.firstAdjust {
  position: absolute;
  left: 55%;
  top: -17%;
  width: 40%;
  z-index: 5;
}

.secondAdjust {
  position: absolute;
  left: 55%;
  top: 40%;
  width: 10%;
  z-index: 6;
}

.unitFrame {
  position: absolute;
  left: 0px;
  top: 0px;
  width: 190px;
  height: 150px;
}
</style>
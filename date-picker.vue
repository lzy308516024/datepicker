<template>
    <div v-click-outside>
        <input type="text" :value = 'formDate' >
        <div class="pannel" v-if="isVisible">
            <div class="pannel-nav">
                <span @click = "prevYear">&lt;</span>
                <span @click = "prevMonth">&lt;&lt</span>
                <span>{{time.year}}年</span>
                <span>{{time.month+1}}月</span>
                <span @click = "nextMonth">&gt;&gt;</span>
                <span @click = "nextYear">&gt;</span>

            </div>
            <div class="pannel-content">
                <div class="days">
                    <span class="title" v-for="k in 7" :key= "`_`+k">
                        {{weeks[k-1]}}
                    </span>
                    <div v-for="i in 6" :key="i" class = "cell">
                        <span v-for="j in 7" :key="j"
                            @click = "chooseDate(visibeDays[(i-1)*7+(j-1)])"
                            :class="[
                                {notCurrentMonth:!isCurrentMonth(visibeDays[(i-1)*7+(j-1)])},

                                {isToday:isToday(visibeDays[(i-1)*7+(j-1)])},
                                {isSelect:isSelect(visibeDays[(i-1)*7+(j-1)])}
                                
                            ]"
                        >
                            {{visibeDays[(i-1)*7+(j-1)].getDate()}}
                        </span>
                    </div>
                    
                </div>
            </div>
            <div class="pannel-footer" @click ="chooseToday">
                今天 
            </div>
        </div>
    </div>
</template>
<script>
import * as utils from './util'
    export default {
        directives:{
            clickOutside:{
                bind(el,bindings,vnode){
                    let handler = (e) =>{
                      
                        if(el.contains(e.target)){
                            if(!vnode.context.isVisible){
                                vnode.context.focus();
                            }
                            
                        }else{
                       
                            if(vnode.context.isVisible){
                                console.log(vnode.context.isVisible)
                                vnode.context.blur();
                            }
                           
                        }
                    }
                    el.handler = handler;
                    document.addEventListener('click',handler)
                },
                unbind(el){
                    document.addEventListener('click',handler)
                }
                
            }
        },
        data(){

            let {year,month} = utils.getYearMonthDay(this.value)
           
            return {
              isVisible:false,
              weeks:['日','一','二','三','四','五','六'],
              time:{year,month}
            }
        },
        props:{
            value:{
                type:Date,
                default:() => new Date()
            }
        },
        mounted(){
            
        },
        computed:{
            visibeDays(){
               
                let {year,month} = utils.getYearMonthDay(utils.getDate(this.time.year,this.time.month,1));
                // 现获取当前是周几
                let currentFirstDay = utils.getDate(year,month,1);
               
                // 先生成一个当前2019 5 18  2019 5 1
                // 获取当前是周几  把天数往前移动几天
                let week = currentFirstDay.getDay();

                // 当前开始的天数
                let startDay =  currentFirstDay - week * 60*60*1000*24
               
                // 循环42天
                let arr = []
                for(let i = 0;i<42;i++){
                    arr.push(new Date(startDay+i*60*60*1000*24));
                }
                return arr
            },
            formDate(){
             let {year,month,day}  =  utils.getYearMonthDay(this.value)
             
             return `${year} - ${month+1} - ${day}`
            }
        },
        methods:{
            focus(){
                this.isVisible = true
            },
            blur(){
                this.isVisible = false
            },
            isCurrentMonth(date){
                console.log(utils.getDate(this.time.year,this.time.month,1));
                let {year,month} = utils.getYearMonthDay(utils.getDate(this.time.year,this.time.month,1))
                
                
                let {year:y,month:m} = utils.getYearMonthDay(date)
                

                return year === y && month === m
            },
            isToday(date){
                let {year,month,day} = utils.getYearMonthDay(new Date());
                let {year:y,month:m,day:d} = utils.getYearMonthDay(date);
                return year === y && month === m && day === d;
            },
            chooseDate(date){
                this.time = utils.getYearMonthDay(date)
                this.$emit('input',date);
                this.isVisible = false

                console.log(this.isVisible);
            },
            isSelect(date){
                let {year,month,day} = utils.getYearMonthDay(this.value)
                let {year:y,month:m,day:d} = utils.getYearMonthDay(date);
                return year === y && month === m && day === d;
            },
            prevMonth(){
                let d = utils.getDate(this.time.year,this.time.month,1);
                d.setMonth(d.getMonth()-1);
                this.time = utils.getYearMonthDay(d);
            },
            prevYear(){
                let y = utils.getDate(this.time.year,this.time.month,1);
                console.log(y);
                y.setFullYear(y.getFullYear()-1);
                this.time = utils.getYearMonthDay(y);
            },
            nextYear(){
                let y = utils.getDate(this.time.year,this.time.month,1);
                console.log(y)
                console.log(y)
                y.setFullYear(y.getFullYear()+1);
                console.log(y)
                this.time = utils.getYearMonthDay(y);

            },
            nextMonth(){
                let d = utils.getDate(this.time.year,this.time.month,1);
                d.setMonth(d.getMonth()+1);
                this.time = utils.getYearMonthDay(d);
            },
            chooseToday(){
                
                this.time = utils.getYearMonthDay(new Date());
                console.log(this.time)
                this.blur();
            }
        }
    }
</script>
<style>
    .pannel {
        position:absolute;
        background:#fff;
        border:1px solid pink;
        box-shadow:2px 2px 2px pink;
    }
    .pannel-nav {
        display:flex;
        justify-content:space-around;
        align-item:center;
        height:30px;
        user-select:none;
    }
    .pannel-content {
        
       
    }
    .pannel-comtent .days {

    }
    .cell {
        display:flex;
        justify-content:center;
        algin-items:center;
    }
    .cell span{
        width:32px;
        height:30px;
        display:flex;
        justify-content:center;
        align-items:center;
        cursor:pointer;
        user-select:none;
        font-weight:font;
    }

    .cell span:hover,.isSelect {
        background:pink;
        color:#fff;
    }
    .pannel-footer {
        height:30px;
        text-align:center;
    }
    .notCurrentMonth {
        color:#999;
    }
    .isToday {
        color:#000;
    }
    .title {
        width:32px;
        height:30px;
        display:inline-block;
        text-align:center;
        font-weight:bold;
    }
</style>
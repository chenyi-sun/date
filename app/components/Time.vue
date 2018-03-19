<template>
    <div>
         <div class="time">
            <div class="time-head">
                <div class="year">{{showYear}}年</div>
                <div class="munth">{{showMouth}}月</div>
            </div>
            <div class="time-all-head">
                <div class="in-block" v-for="(item,i) in 7">
                        {{days[i]}}
                </div>
                <div class="next" @click="next"></div>
                <div class="pre" @click="pre"></div>
            </div>
            <div class="time-all-block">
                <div class="time-in-head" v-for="item in 7"></div>
                <div class="time-in-block" v-for="(item, i) in smallDate" @click="showdate(item)" :class="{'noborder-top':i<7}">
                    <div class="time-small-block">
                            {{item}}
                    </div>
                    <div class="in-right"></div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import './../css/time.scss';
console.log('ssssdddddd');
export default {
    data () {
        return {
           time: 'dddss',
           days: [
               "日",
               "一",
               "二",
               "三",
               "四",
               "五",
               "六"
           ],
           smallDate: [

           ],
           date: new Date(),
           showMouth: null,
           showYear: null,
        }
    },
    methods:{
        end(){
            // console.log(new Date());
            // console.log(new Date().getDay());
        },
        year(){ //获取年份
            return this.date.getFullYear();
        },
        setDateName(date){ //返回当前日期是星期几
            return new Date(date).getDay()
        },
        mounthdaylength(year,mouth){ //获取当月的长度
            var days;
            var self = this;
            if(mouth == 2){
                days= year % 4 == 0 ? 29 : 28;
            }
            else if(mouth == 1 || mouth == 3 || mouth == 5 || mouth == 7 || mouth == 8 || mouth == 10 || mouth == 12){
                //月份为：1,3,5,7,8,10,12 时，为大月.则天数为31；
                days= 31;
            }
            else{
                //其他月份，天数为：30.
                days= 30;
            }
            return days;
        },
        day(){
            var day = {
                year: this.date.getFullYear(),
                mounth: this.date.getMonth()+1,
            };
            return day;
        },
        setAllday(year,mouth){
            var self = this;
            var arr = [];
            var setDateName = year+'-'+mouth+'-'+'01';//某个月的第一天是星期几
            var emptyLength = self.setDateName(setDateName); //前面几个空出来的
            for(var i = 0; i< emptyLength; i++){
                arr.push(null);
            }
            var mounthdaylength = self.mounthdaylength(year, mouth);
            for(var i = 0; i< mounthdaylength; i++){
                arr.push(i+1);
            }
            var totalLength = Math.ceil(arr.length/7);
            var len = totalLength*7-arr.length;
            console.log('totalLength'+totalLength);
            if(arr.length<totalLength*7){
                for(var i = 0;i<len;i++){
                    arr.push(null);
                }
            }
            return arr;
        },
        pre(){
            var self = this;
            if(self.showMouth==1){
                self.showMouth = 13;
                self.showYear = self.showYear - 1;
            }
            self.showMouth = self.showMouth - 1;
            self.smallDate = self.setAllday(self.showYear,self.showMouth);
        },
        next(){
            var self = this;
            if(self.showMouth == 12){
                self.showMouth = 0;
                self.showYear = self.showYear + 1;
            }
            self.showMouth = self.showMouth + 1;
            self.smallDate = self.setAllday(self.day().year,self.showMouth);
        },
        showdate(day){
            var self = this;
            if(!day){
                return false;
            }
            console.log(self.showYear + '-'+self.showMouth+'-'+day);
        }
    },
    count(){

    },
    mounted(){
        var self = this;
        this.$nextTick(function(){
            self.end();
            self.showMouth = self.day().mounth;
            self.showYear = self.day().year;
            var date = self.day().year+'-'+self.day().mounth+'-'+'01'
            console.log(self.setDateName(date));
            self.smallDate = self.setAllday(self.day().year,self.day().mounth);
        });
    },
}
</script>

<style scoped>
#example {
    background: red;
    height: 100vh;
}
</style>
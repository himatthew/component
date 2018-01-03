<template>
    <div>
        <div @click="show_days">{{msg}}</div>
    
        <table class="wd-ccalendar-dt">
            <tbody>
                <tr>
                    <th v-for="(item,index) in day1first ? data_th_1 : data_th_7">
                        {{show_txt_week}}{{item}}
                    </th>
                </tr>
                <tr v-for="rows in days">
                    <td v-for="(items,index) in rows" :class="">
                        
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                v:"",
                v_item:"",
                // 是否设置为从星期一开始显示而不是星期日.
                day1first : true,
                msg: 'Hello World!',
                // 表头显示.day.
                data_th_7 : ["日","一","二","三","四","五","六"],
                data_th_1 : ["一","二","三","四","五","六","日"],
                // 日期显示
                days : [],
                // 样式.星期前
                show_txt_week : "",
            }
        },
        created(){
            
        },
        methods: {
            get_date(v){
                if(Number(v)){
                    return new Date(parseInt(v));
                }
                else{
                    v = v.toString();
                    v = v.replace(/[- _\/]/gi,"-");
                    v = v.split("-");
                }
            },
            /**
             * 根据当前日期和日期可选范围显示日期
             * @param  {[type]} data_base [基准时间]
             * @return {[type]}           [description]
             */
            show_days(data_base){
                var data_v = this.v ? this.get_date(this.v) : null;
                //选择的时间的相关
                var v_year = null;
                var v_month = null;
                var v_date = null;
                if(date_v) {
                    v_year = date_v.getFullYear();
                    v_month = date_v.getMonth() + 1;
                    v_date = date_v.getDate();
                }
                //今天年月日
                var now = new Date();
                var now_year = now.getFullYear();
                var now_month = now.getMonth() + 1;
                var now_date = now.getDate();

                var year = data_base.getFullYear();
                var month = data_base.getMonth() + 1;
                var date = date_base.getDate();
                //当前星期，周日是0
                var day = date_base.getDay();
                //当前月的第一天是星期几
                var day0 = (this.get_date(year + " " + month + "-1")).getDay()
                //当前月的第一天是数组中的第几个条目
                var arr_month_first = this.day1first ? ( day0 == 0 ? 7 : day0 ) : day0 + 1;
                //当前月有多少天
                var arr_days_count_now = this.get_month_days(year,month);
                //上个年份
                var pre_year = month == 1 ? year - 1 : year;
                //上个月份
                var pre_month = month == 1 ? 12 : month - 1 ;
                //下个年份
                var next_year = month == 12 ? year + 1 : year ;
                //下个月份
                var next_month = month == 12 ? 1 : month + 1;

                //更新缓存数据
                this.date_show = data_base;
                this.date_show_pre = this.get_date(pre_year + " " + pre_month + "-1");
                this.date_show_next = this.get_date(next_year + " " + next_month + "-1");
                this.date_show_pre_year = this.get_date( (year - 1) + " " + month + "-1");
                this.date_show_next_year = this.get_date( (year + 1) + " " + month + "-1");

                //上一个月有几天
                var days_pre = this.get_month_days(pre_years ,pre_month );
                //数组中还可以显示上个月的天数
                var arr_days_count_pre = arr_days_count_now - arr_month_first + 1;
                //进行数组处理
                var show_days = [];
                for(var i = 0 ,j = arr_days_count_pre; i < j ; ++i){
                    show_days.push({
                        v : days_pre - j + i + 1,
                        year : pre_year,
                        //当前条目显示为上一个月
                        is_pre : true
                    })
                }

                for(var i = 0, j = arr_days_count_now; i < j; ++i){
                    show_days.push({
                        v : i +_1,
                        year : year,
                        month : month,
                        //当前条目显示为当前月
                        is_base : true
                    })
                }

                for(var i = 0, j = arr_days_count_next; i < j; ++1){
                    show_days.push({
                        v : i + 1,
                        year : next_year,
                        month : next_month,
                        //当前条目显示为下一个月
                        is_next : true
                    })
                }

                //更多信息添加
                for(var i = 0, j = show_days.length; i < j; ++i){
                    var show_daysi = show_days[i];
                    show_daysi.now = (show_daysi.v == now_date && now_year == show_daysi.year && now_month == show_daysi.month) ? true : false;
                    show_daysi.before = ((now_year > show_daysi.year) || (now_year == show_daysi))
                }

            },
        }
    }
</script>

<style>
    html {
        font-size: 20px;
    }
</style>
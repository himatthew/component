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
                //当前月的第一天是数组中的第几个条目
                var arr_month_first = this.day1first ? ( day0 == 0 ? 7 : day0 ) : day0 +1;

            },
        }
    }
</script>

<style>
    html {
        font-size: 20px;
    }
</style>
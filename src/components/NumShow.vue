<template>
    <div class="now numCon">
        <div class="nowpeople">当前累计人数：</div>
        <div class="nowpeoplenum">{{ data.sum }} 人</div>
    </div>
    <div class="today numCon icon">
        <img src="../assets/day.svg" style="width: 40px;height: 40px;">
      <div class="numright">
        <div>摄像头1</div>
        <div class="numin">{{ data.camera1Data }}人</div>
      </div>
    </div>
    <div class="week numCon icon">
        <img src="../assets/week.svg" style="width: 40px;height: 40px;">
        <div class="numright">
          <div>摄像头2</div>
          <div class="numin">{{ data.camera2Data }}人</div>
        </div>
    </div>
    <div class="month numCon icon">
        <img src="../assets/month.svg" style="width: 45px;height: 45px;">
        <div class="numright">
          <div>摄像头3</div>
          <div class="numin">{{ data.camera3Data }}人</div>
        </div>
    </div>
    <div class="en numCon icon">
        <img src="../assets/sum.svg" style="width: 45px;height: 45px;">
        <div class="numright">
          <div>摄像头4</div>
          <div class="numin">{{ data.camera4Data }}人</div>
        </div>
    </div>
    <div class="en numCon icon">
        <img src="../assets/time.svg" style="width: 45px;height: 45px;">
        <div class="numright">
            <div>运营时长</div>
            <div class="numin">{{ data.timeold }}</div>
        </div>
    </div>
</template>
  
<script>
import { reactive, computed, watchEffect, onMounted, onBeforeUnmount } from 'vue'
import { useStore } from 'vuex'
import axios from 'axios'          //引入axios
import { useRouter } from "vue-router";
export default {
    //组件名
    name: 'NumShow',


    setup() {     
        const store = useStore()
        const router = useRouter()
        let data = reactive({
          nowpeople: 0,
          todaypeople: 0,
          monthpeople: 0,
          sum: 0,
          inter: "",
          camera1Data: 0,
          camera2Data: 0,
          camera3Data: 0,
          camera4Data: 0,
            colors: [
                { color: '#f56c6c', percentage: 20 },
                { color: '#e6a23c', percentage: 40 },
                { color: '#5cb87a', percentage: 60 },
                { color: '#1989fa', percentage: 80 },
                { color: '#6f7ad3', percentage: 100 },
            ]
        })

        function getFormattedDate() {
          return "2024-12-11";
        }

        function init() {
          axios.get('api/detection/nowpeople').then(res => {
            data.nowpeople = res.data;
          }, err => {
            console.log(err);
            if (err.response.status == 401) {
              router.push("login");
            }
          })

          // 获取数据
          axios.get('api/detection/perday/' + getFormattedDate()).then(res => {
            data.camera1Data = res.data[1] ? res.data[1].reduce((sum, value) => sum + value, 0) : 0; // 摄像头1的总数
            data.camera2Data = res.data[2] ? res.data[2].reduce((sum, value) => sum + value, 0) : 0; // 摄像头1的总数
            data.camera3Data = res.data[3] ? res.data[3].reduce((sum, value) => sum + value, 0) : 0; // 摄像头1的总数
            data.camera4Data = res.data[4] ? res.data[4].reduce((sum, value) => sum + value, 0) : 0; // 摄像头1的总数
          }, err => {
            console.log(err);
            if (err.response.status == 401) {
              router.push("login");
            }
          })

            //获取累计总人数
            axios.get('api/detection/sum/').then(res => {
                data.sum = res.data
            }, err => {
                console.log(err);
                if (err.response.status == 401) {          //未登录，重定向到登录页面
                    router.push("login");
                }
            })

            //计算运行时间
            var dateObj = new Date();   //当前时间
            var grt = new Date("11/12/2024 00:00:00");//建站时间

            //计算运行时间
            var days = (dateObj - grt) / 1000 / 60 / 60 / 24;
            var dnum = Math.floor(days);   //天
            var hours = (dateObj - grt) / 1000 / 60 / 60 - (24 * dnum);
            var hnum = Math.floor(hours);   //时
            if (String(hnum).length == 1) {
                hnum = "0" + hnum;
            }
            var minutes = (dateObj - grt) / 1000 / 60 - (24 * 60 * dnum) - (60 * hnum);
            var mnum = Math.floor(minutes);  //分
            if (String(mnum).length == 1) {
                mnum = "0" + mnum;
            }
            var seconds = (dateObj - grt) / 1000 - (24 * 60 * 60 * dnum) - (60 * 60 * hnum) - (60 * mnum);
            var snum = Math.round(seconds);    //秒
            if (String(snum).length == 1) {
                snum = "0" + snum;
            }
            //data.timeold = dnum + "天" + hnum + "时" + mnum + "分" + snum + "秒"
            data.timeold = dnum + "天" + hnum + "时"

        }

        //生命周期函数，
        //onMounted：在初始化页面完成后执行
        onMounted(() => {
            data.inter = setInterval(init, 1000);

        })

        onBeforeUnmount(()=>{
            clearInterval(data.inter);
        })

        //数据和函数都要返回
        return {
            data,

        }
    },
}
</script>
<style scoped>


.numCon {
    height: 100%;
    background-color: #ffffff;
    border-radius: 10px;
    font-family: SimHei;
    /* 设置一个通用的圆角 */
}

.now {
    width: 20%;
    background-color: #518aff;
    color: #ffffff;
    /*
    background-image: linear-gradient(to bottom left, #3fcfe0, #76dce9);
*/
}

.nowpeople {
    font-size: 20px;
    font-weight: 700;
    margin: 8px;

}

.nowpeoplenum {
    font-size: 32px;
    font-weight: bold;
    text-align: center;
    margin-top: 20px;
    Letter-spacing: 4px
}

.pro {
    margin-left: 20px;
    margin-top: 10px;
}

.peo {
    margin-left: 20px;
    font-size: small;
}

.today {
    width: 15%;
}

.week {
    width: 15%;
}

.month {
    width: 15%;
}

.en {
    width: 15%;
}



.icon {
    display: flex;
    justify-content: center;
    align-items: center
}

.icon img {
    margin-right: 20px;
}

.numright {
    text-align: center;
}

.numin {
    font-size: 22px;
    margin-top: 10px;
}

</style>
  

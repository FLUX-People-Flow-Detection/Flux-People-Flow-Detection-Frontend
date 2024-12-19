y<template>
    <div class="container">
        <div class="num">
            <NumShow /> 
        </div>
        <div class="maTa">
            <div class="table maTaCon">
                <PieChart />
            </div>
            <div class="map maTaCon"><!--地图-->
                <Map /> 
            </div>
            <div class="pic maTaCon">
                <div class="word">
                    <div>福州大学：</div>
                    <p>福州大学(Fuzhou University)，位于福建省福州市，是福建省人民政府与国家教育部共建高校，由福建省主管。</p>
                    <p>福州大学是国家“双一流”建设高校、国家“211工程”重点建设大学；是教育部首批“卓越工程师教育培养计划”高校、国家“111计划”、全国首批深化创新创业教育改革示范高校；首批高等学校科技成果转化和技术转移基地、高校国家知识产权信息服务中心。</p>
                </div>
            </div>
        </div>
        <div class="line">
            <LineChart />
        </div>
    </div>
</template>
  
<script>
import { reactive, computed, watchEffect, onMounted } from 'vue'
import LineChart from '../components/LineChart.vue'  //引入组件
import PieChart from '../components/PieChart.vue'
import Map from '../components/Map.vue'
import NumShow from '../components/NumShow.vue'

export default {
    //组件名
    name: 'Visualization',
    components: {
        LineChart,    //注册组件
        PieChart,
        Map,
        NumShow,

    },

    setup() {
        let data = reactive({
            
        })


        //生命周期函数，
        //onMounted：在初始化页面完成后执行
        onMounted(() => {
            
        })


        //数据和函数都要返回
        return {
            data,
        }
    },



}
</script>
<style scoped>

/*最外层容器*/
.container {
    background-color: #f1f8f8;
    height: 100%;
    width: 100%;
}

/*上层数据展示*/
.num {
    display: flex;
    height: 17%;
    justify-content: space-between;
    align-items: center;
}


.maTa {
    display: flex;
    height: 37%;
    justify-content: space-between;
    align-items: center;
}

.line {
    height: 45%;
    background-color: #ffffff;
    border-radius: 10px;
    /* 设置一个通用的圆角 */
}


.maTaCon {
    height: 85%;
    background-color: #ffffff;
    border-radius: 10px;
    /* 设置一个通用的圆角 */
}

.map {
    width: 40%;

}


.table {
    width: 30%;

}

.pic {
    width: 28%;
}

.word{
    margin: 20px;
    font-family: SimHei;
    height: 85%;
    overflow:auto
}
.word div{
    font-size: 18px;
    font-weight: bold;
    color: #444;
}

.word p{
    margin-top: 10px;
    font-size: 16px;
    color: #444;
}

/*宽度小于1350时*/
@media (max-width: 1350px) {}
</style>
  

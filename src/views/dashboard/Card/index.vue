<template>
  <div>
    <el-row :gutter="10">
        <el-col :span="6">
            <!-- //1 -->
            <el-card>
                <Detail :title="'总销售额'" count="￥ 12622">
                    <template slot="charts">
                        <span>
                            周同比&nbsp;&nbsp;56.67%
                            <svg t="1661326954116" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="3398" width="16" height="16"><path d="M768 384L512 128 256 384h192v512h128V384z" p-id="3399" fill="#1afa29"></path></svg>
                        </span>
                        <span>
                            日同比&nbsp;&nbsp;19.67%
                            <svg t="1661327031010" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="4487" width="16" height="16"><path d="M697.359638 689.069835l-166.241208 170.145122c-5.335519 5.459339-11.773138 8.191566-19.316951 8.191566-7.54279 0-13.982456-2.732228-19.315928-8.191566l-166.241208-170.145122c-8.002254-8.18645-10.251481-19.718087-5.92085-30.417777 4.329609-10.697643 13.921058-17.203824 25.234731-17.203824l134.206608 0 0-452.229708c0-17.838274 14.397918-32.296567 32.236192-32.296567 17.833157 0 32.232099 14.458293 32.232099 32.296567l0 452.229708L678.04371 641.448234c11.31572 0 20.908192 6.506181 25.236777 17.203824C707.610096 669.351748 705.358823 680.883385 697.359638 689.069835z" p-id="4488" fill="#d81e06"></path></svg>
                        </span>
                    </template>
                    <template slot="footer">
                        <span>日销售额￥ 12321</span>
                    </template>
                </Detail>
            </el-card>
        </el-col>
        <!-- //2 -->
        <el-col :span="6">
            <el-card>
                <Detail :title="'访问量'">
                    <template slot="charts">
                        <lineChart></lineChart>
                    </template>
                    <template slot="footer">
                        <span>日访问量 1232</span>
                    </template>
                </Detail>
            </el-card>
        </el-col>
        <!-- //3 -->
        <el-col :span="6">
            <el-card>
                <Detail :title="'总销售额'">
                    <template slot="charts">
                        <barCharts></barCharts>
                    </template>
                    <template slot="footer">
                        <span>转化率 64%</span>
                    </template>
                </Detail>
            </el-card>
        </el-col>
        <!-- //4 -->
        <el-col :span="6">
            <el-card>
                <Detail :title="'总销售额'">
                    <template slot="charts">
                        <progressCharts></progressCharts>
                    </template>
                    <template slot="footer">
                        <span>
                            周同比&nbsp;&nbsp;56.67%
                            <svg t="1661326954116" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="3398" width="16" height="16"><path d="M768 384L512 128 256 384h192v512h128V384z" p-id="3399" fill="#1afa29"></path></svg>
                        </span>
                        <span>
                            日同比&nbsp;&nbsp;19.67%
                            <svg t="1661327031010" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="4487" width="16" height="16"><path d="M697.359638 689.069835l-166.241208 170.145122c-5.335519 5.459339-11.773138 8.191566-19.316951 8.191566-7.54279 0-13.982456-2.732228-19.315928-8.191566l-166.241208-170.145122c-8.002254-8.18645-10.251481-19.718087-5.92085-30.417777 4.329609-10.697643 13.921058-17.203824 25.234731-17.203824l134.206608 0 0-452.229708c0-17.838274 14.397918-32.296567 32.236192-32.296567 17.833157 0 32.232099 14.458293 32.232099 32.296567l0 452.229708L678.04371 641.448234c11.31572 0 20.908192 6.506181 25.236777 17.203824C707.610096 669.351748 705.358823 680.883385 697.359638 689.069835z" p-id="4488" fill="#d81e06"></path></svg>
                        </span>
                    </template>
                </Detail>
            </el-card>
        </el-col>
    </el-row>
  </div>
</template>

<script>
import Detail from './Detail'
import lineChart from './lineChart'
import barCharts from './barCharts'
import progressCharts from './progressCharts'
export default {
    name:'',
    components:{
        Detail,
        lineChart,
        barCharts,
        progressCharts
    }
}
</script>

<style>

</style>
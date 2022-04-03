<template>
    <div class="fillcontain">
        <head-top></head-top>
        <div class="table_container">
            <el-table
                :data="tableData"
                highlight-current-row
                style="width: 100%">
                <el-table-column
                  type="index"
                  width="100">
                </el-table-column>
                <el-table-column
                  property="userName"
                  label="姓名"
                  width="220">
                </el-table-column>
                <el-table-column
                  property="jobContent"
                  label="工作内容"
                  width="220">
                </el-table-column>
                <el-table-column
                  property="date"
                  label="日期">
                </el-table-column>
            </el-table>
            <div class="Pagination" style="text-align: left;margin-top: 10px;">
                <el-pagination
                  @size-change="handleSizeChange"
                  @current-change="handleCurrentChange"
                  :current-page="currentPage"
                  :page-size="20"
                  layout="total, prev, pager, next"
                  :total="count">
                </el-pagination>
            </div>
        </div>
    </div>
</template>

<script>
    import headTop from '../components/headTop'
    import {getUserList, getUserCount, getJobHistory} from '@/api/getData'
    export default {
        data(){
            return {
                tableData: [],
                currentRow: null,
                offset: 0,
                limit: 20,
                count: 0,
                currentPage: 1,
                userName: '',
            }
        },
    	components: {
    		headTop,
    	},
        created(){
            let userName = JSON.parse(sessionStorage.getItem("userName"))
            this.userName = userName.userName
            // this.initData();
            this.JobHistory();
        },
        methods: {
            // async initData(){
            //     try{
            //         const countData = await getUserCount();
            //         if (countData.status == 1) {
            //             this.count = countData.count;
            //         }else{
            //             throw new Error('获取数据失败');
            //         }
            //         this.getUsers();
            //     }catch(err){
            //         console.log('获取数据失败', err);
            //     }
            // },
            handleSizeChange(val) {
                console.log(`每页 ${val} 条`);
            },
            handleCurrentChange(val) {
                this.currentPage = val;
                this.offset = (val - 1)*this.limit;
                // this.getUsers()
            },
            async JobHistory(){
                const jobHistory = await getJobHistory({userName: this.userName});
                this.tableData = [];
                jobHistory['data'].forEach(item => {
                    const tableDict = {};
                    tableDict.userName = this.userName;
                    tableDict.jobContent = item.JobContent;
                    tableDict.date = item.Date;
                    this.tableData.push(tableDict);
                })
                this.count = this.tableData.length
            }
        },
    }
</script>

<style lang="less">
	@import '../style/mixin';
    .table_container{
        padding: 20px;
    }
</style>

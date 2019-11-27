<template>
    <div class="table">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="iconfont icon-wen-book"></i> 图书管理</el-breadcrumb-item>
                <el-breadcrumb-item>图书列表</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="handle-box">
                <el-button type="primary" icon="delete" @click="addDetail">添加详情</el-button>
                <el-button type="primary" icon="delete" class="handle-del mr10" @click="delAll">批量删除</el-button>
                <el-input v-model="select_word" placeholder="筛选关键词" class="handle-input mr10"></el-input>
                <el-button type="primary" icon="search" @click="search">搜索</el-button>
            </div>
            <el-table :data="tableData" border class="table" ref="multipleTable" @selection-change="handleSelectionChange">
                <el-table-column type="selection" width="55" align="center"></el-table-column>
                <el-table-column prop="product.name" label="书名" width="160">
                </el-table-column>
                <el-table-column prop="product.press" label="出版社" width="140">
                </el-table-column>
                <el-table-column prop="format" label="图书开张" width="80">
                </el-table-column>
                <el-table-column prop="paper" label="图书纸张" width="100">
                </el-table-column>
                <el-table-column prop="pack" label="图书包装" width="100">
                </el-table-column>
                <el-table-column prop="isSuit" label="是否套装" width="80" :formatter="formatIsSuit">
                </el-table-column>
                <el-table-column prop="bookNumber" label="国际标准书号">
                </el-table-column>
                <el-table-column prop="createTime" label="创建时间" sortable width="160">
                </el-table-column>
                <el-table-column label="操作" width="190" align="center">
                    <template slot-scope="scope">
                        <el-button type="text" icon="iconfont icon-wen-chakan" @click="handleQuery(scope.$index, scope.row)">查看</el-button>
                        <el-button type="text" icon="iconfont icon-wen-update1" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                        <el-button type="text" icon="iconfont icon-wen-custom-delete" class="red" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
                    </template>
                </el-table-column>
            </el-table>
            <div class="pagination">
                <el-pagination background @current-change="handleCurrentChange" layout="prev, pager, next" :total="1000">
                </el-pagination>
            </div>
        </div>
        <!-- 查看弹出框 -->
        <el-dialog title="查看详情" :visible.sync="queryVisible" width="80%">
            <div>
                <div class="query-show-left">
                    <span class="query-show-title">书名 </span>{{bookDetail.name}}
                </div style="float: left;">
                <div>
                    <span class="query-show-title">出版社 </span>{{bookDetail.press}}
                </div>
            </div>
            <div class="query-show">
                <div class="query-show-left">
                    <span class="query-show-title">图书开张 </span>{{bookDetail.format}}
                </div style="float: left;">
                <div>
                    <span class="query-show-title">图书纸张 </span>{{bookDetail.paper}}
                </div>
            </div>
            <div class="query-show">
                <div class="query-show-left">
                    <span class="query-show-title">图书包装 </span>{{bookDetail.pack}}
                </div style="float: left;">
                <div>
                    <span class="query-show-title">是否套装 </span>{{bookDetail.isSuit}}
                </div>
            </div>
            <div class="query-show">
                <div class="query-show-left">
                    <span class="query-show-title">国际标准书号 </span>{{bookDetail.bookNumber}}
                </div style="float: left;">
                <div>
                    <span class="query-show-title">创建时间 </span>{{bookDetail.createTime}}
                </div>
            </div>
            <div class="query-show">
                <div>
                    <span class="query-show-title">具体详情</span>
                </div>
                <div>
                    <p v-html="bookDetail.content">{{bookDetail.content}}</p>
                </div>
            </div>
            <span slot="footer" class="dialog-footer">
                <el-button type="primary" @click="toBack">返 回</el-button>
            </span>
        </el-dialog>

        <!-- 编辑弹出框 -->
        <el-dialog title="编辑" :visible.sync="editVisible" width="30%">
            <el-form ref="form" :model="form" label-width="50px">
                <el-form-item label="日期">
                    <el-date-picker type="date" placeholder="选择日期" v-model="form.date" value-format="yyyy-MM-dd" style="width: 100%;"></el-date-picker>
                </el-form-item>
                <el-form-item label="姓名">
                    <el-input v-model="form.name"></el-input>
                </el-form-item>
                <el-form-item label="地址">
                    <el-input v-model="form.address"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="editVisible = false">取 消</el-button>
                <el-button type="primary" @click="saveEdit">确 定</el-button>
            </span>
        </el-dialog>

        <!-- 删除提示框 -->
        <el-dialog title="提示" :visible.sync="delVisible" width="300px" center>
            <div class="del-dialog-cnt">删除不可恢复，是否确定删除？</div>
            <span slot="footer" class="dialog-footer">
                <el-button @click="delVisible = false">取 消</el-button>
                <el-button type="primary" @click="deleteRow">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script>
    export default {
        name: 'basetable',
        data() {
            return {
                url: './vuetable.json',
                tableData: [],
                cur_page: 1,
                multipleSelection: [],
                select_cate: '',
                select_word: '',
                del_list: [],
                is_search: false,
                queryVisible: false,
                editVisible: false,
                delVisible: false,
                form: {
                    name: '',
                    date: '',
                    address: ''
                },
                bookDetail: {},
                idx: -1
            }
        },
        created() {
            var self = this
            this.$axios.get("/product-server/product/newDetail").then((res) => {
                if(res.data.status == 1){
                    // 数组复制
                    self.tableData = res.data.data.slice()
                    console.log(res.data.data)
                    this.$message({
                        message: '恭喜你，这是一条成功消息',
                        type: 'success'
                    })
                }else{
                    this.$message.error(res.data.msg)
                }
            })
        },
        computed: {
            data() {
                return this.tableData.filter((d) => {
                    let is_del = false;
                    for (let i = 0; i < this.del_list.length; i++) {
                        if (d.name === this.del_list[i].name) {
                            is_del = true;
                            break;
                        }
                    }
                    // if (!is_del) {
                    //     if (d.address.indexOf(this.select_cate) > -1 &&
                    //         (d.username.indexOf(this.select_word) > -1 ||
                    //             d.address.indexOf(this.select_word) > -1)
                    //     ) {
                    //         return d;
                    //     }
                    // }
                })
            }
        },
        methods: {
            // 分页导航
            handleCurrentChange(val) {
                this.cur_page = val;
                this.getData();
            },
            // 获取 easy-mock 的模拟数据
            getData() {
                // 开发环境使用 easy-mock 数据，正式环境使用 json 文件
                if (process.env.NODE_ENV === 'development') {
                    this.url = '/ms/table/list';
                };
                this.$axios.post(this.url, {
                    page: this.cur_page
                }).then((res) => {
                    this.tableData = res.data.list;
                })
            },
            search() {
                this.is_search = true;
            },
            filterTag(value, row) {
                return row.tag === value;
            },
            handleQuery(index, row){
                const item = this.tableData[index];
                console.log(item)
                this.bookDetail = {
                    name: item.product.name,
                    press: item.product.press,
                    format: item.format,
                    paper: item.paper,
                    pack: item.pack,
                    isSuit: item.isSuit === 1?'是':'否',
                    bookNumber: item.bookNumber,
                    createTime: item.createTime,
                    content: item.content
                }
                this.queryVisible = true;
            },
            handleEdit(index, row) {
                this.idx = index;
                const item = this.tableData[index];
                this.form = {
                    name: item.name,
                    date: item.date,
                    address: item.address
                }
                this.editVisible = true;
            },
            handleDelete(index, row) {
                this.idx = index;
                this.delVisible = true;
            },
            delAll() {
                const length = this.multipleSelection.length;
                let str = '';
                this.del_list = this.del_list.concat(this.multipleSelection);
                for (let i = 0; i < length; i++) {
                    str += this.multipleSelection[i].name + ' ';
                }
                this.$message.error('删除了' + str);
                this.multipleSelection = [];
            },
            handleSelectionChange(val) {
                this.multipleSelection = val;
            },
            // 退出查看
            toBack(){
                this.queryVisible = false
            },
            // 保存编辑
            saveEdit() {
                this.$set(this.tableData, this.idx, this.form);
                this.editVisible = false;
                this.$message.success(`修改第 ${this.idx+1} 行成功`);
            },
            // 确定删除
            deleteRow(){
                this.tableData.splice(this.idx, 1);
                this.$message.success('删除成功');
                this.delVisible = false;
            },
            formatIsSuit(row, column){
                return row.email === 1 ? '是' : '否';
            },
            addDetail(){
                this.$router.push("/addBookDetail")
            }
        }
    }

</script>

<style scoped>
    .handle-box {
        margin-bottom: 20px;
    }

    .handle-select {
        width: 120px;
    }

    .handle-input {
        width: 300px;
        display: inline-block;
    }
    .del-dialog-cnt{
        font-size: 16px;
        text-align: center
    }
    .table{
        width: 100%;
        font-size: 14px;
    }
    .red{
        color: #ff0000;
    }
    .mr10{
        margin-right: 10px;
    }
    .query-show{
        margin-top: 20px;
    }
    .query-show-left{
        float: left;
        width: 600px;
    }
    .query-show-title{
        color: #ff0000;
        font-size: 16px;
        font-weight: bold;
    }
</style>

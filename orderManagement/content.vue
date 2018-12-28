<template>
    <div class="main-box">
        <div class="search-box">
            <el-row :gutter="20">
                <el-col :span="6">
                    <span class='tit'>身份信息</span>
                    <el-input @input="changeValue" v-model="searchData.realName" size='small' type="text" placeholder="搜索姓名"></el-input>
                </el-col>
                <el-col :span="6">
                    <span class='tit'>手机号</span>
                    <el-input @input="changeValue" v-model="searchData.phone" size='small' type="number" placeholder="搜索0可汇总"></el-input>
                </el-col>
                <el-col :span="6">
                    <span class='tit'>身份证号</span>
                    <el-input @input="changeValue" v-model="searchData.idcard" size='small' type="text" placeholder="搜索0可汇总"></el-input>
                </el-col>
                <el-col :span="6">
                    <span class='tit'>性别</span>
                    <el-select @change="changeValue" v-model="searchData.sex" size="small" placeholder="请选择">
                        <el-option label="男" value="0"></el-option>
                        <el-option label="女" value="1"></el-option>
                    </el-select>
                </el-col>
            </el-row>
            <el-row :gutter="20">
                <el-col :span="6">
                    <span class='tit'>芝麻分</span>
                    <el-input @input="changeValue" v-model="searchData.zhimaScore" size='small' type="text" placeholder="搜索支持>、<、a"></el-input>
                </el-col>
                <el-col :span="6">
                    <span class='tit'>ip定位</span>
                    <el-input @input="changeValue" v-model="searchData.ipCity" size='small' type="text" placeholder=""></el-input>
                </el-col>
                <el-col :span="6">
                    <span class='tit'>来源</span>
                    <el-input @input="changeValue" v-model="searchData.refer" size='small' type="text" placeholder="搜索0可汇总"></el-input>
                </el-col>
                <el-col :span="6">
                    <span class='tit'>分配员</span>
                    <el-select @change="changeValue" v-model="searchData.sendUid" size="small" placeholder="请选择">
                        <!-- <el-option></el-option> -->
                    </el-select>
                </el-col>
            </el-row>
            <el-row :gutter="20">
                <el-col :span="6">
                    <span class='tit'>审核员</span>
                    <el-select @change="changeValue" v-model="searchData.examUid" size="small" placeholder="请选择">
                        <!-- <el-option></el-option> -->
                    </el-select>
                </el-col>
                <el-col :span="6">
                    <span class='tit'>已认证项</span>
                    <el-select @change="changeValue" multiple v-model="searchData.vTags" size="small" placeholder="请选择">
                        <el-option label="运营商" value="yys"></el-option>
                        <el-option label="芝麻分" value="zmf"></el-option>
                    </el-select>
                </el-col>
                <el-col :span="6">
                    <span class='tit'>备注</span>
                    <el-input @input="changeValue" v-model="searchData.note" size='small' type="text" placeholder=""></el-input>
                </el-col>
                <el-col :span="6" v-if="status =='ALL'">
                    <span class='tit'>状态</span>
                    <el-select @change="changeValue" v-model="searchData.state" size="small" placeholder="请选择">
                        <el-option label="未分配" value="0"></el-option>
                        <el-option label="审核中" value="3"></el-option>
                        <el-option label="已通过" value="6"></el-option>
                        <el-option label="已拒绝" value="9"></el-option>
                    </el-select>
                </el-col>
            </el-row>
            <el-row :gutter="20">
                <el-col :span="10">
                    <span class='tit'>创建时间</span>
                    <el-date-picker
                    @change="changeValue"
                    type="daterange"
                    size="small"
                    :disabled="searchData.unlimitedTime"
                    v-model="searchData.createTime"
                    range-separator="至"
                    start-placeholder="开始日期"
                    end-placeholder="结束日期">
                    </el-date-picker>
                </el-col>
                <el-col :span="2">
                    <el-checkbox @change="changeValue" v-model="searchData.unlimitedTime">不限时间</el-checkbox>
                </el-col>
            </el-row>
        </div>
        <div class="table-box">
            <el-table border :data="list" @selection-change="handleSelectionChange">
                <el-table-column
                type="selection"
                width="55"
                v-if="status =='0'">
                </el-table-column>
                <el-table-column label="ID" prop="id"></el-table-column>
                <el-table-column width='150px' label="身份信息" prop="">
                    <template slot-scope="scope">
                        <div>
                            <p>
                                {{scope.row.realName}} 
                                <svg-icon v-if="scope.row.sex == 1" style="color: #409eff" class-name="disabled" icon-class="man" />
                                <svg-icon v-else style="color: #f54545" class-name="disabled" icon-class="lady" />
                                {{(new Date().getFullYear()) - scope.row.age}}</p>
                            <p class="phone">
                                <svg-icon style="color: #409eff" class-name="disabled" icon-class="phone" />
                                {{scope.row.mobile ? scope.row.mobile.substring(0,3)+'****'+scope.row.mobile.substr(-4) : ''}}
                            </p>
                        </div>
                    </template>
                </el-table-column>
                <el-table-column label="年龄" prop="">
                    <template slot-scope="scope">
                        <div>
                            <p>{{scope.row.age}}</p>
                            <el-popover
                            placement="right"
                            trigger="click">
                                <div>
                                    <img width="450" :src="scope.row.idcardImg" alt="">
                                </div>
                                <img slot="reference" width="100" :src="scope.row.idcardImg" alt="">
                            </el-popover>
                            
                        </div>
                    </template>
                </el-table-column>
                <el-table-column width='100px' label="欲借(元)" prop="loanYuan"></el-table-column>
                <el-table-column label="芝麻分" prop="">
                    <template slot-scope="scope">
                        <div class="fw-b">
                            {{scope.row.zhimaScore}}
                        </div>
                    </template>
                </el-table-column>
                <el-table-column label="ip定位" prop="ipCity">
                </el-table-column>
                <el-table-column label="来源" prop="refer">
                </el-table-column>
                <el-table-column label="分配员" prop="">
                    <template slot-scope="scope">
                        <div></div>
                    </template>
                </el-table-column>
                <el-table-column label="审核员" prop="">
                    <template slot-scope="scope">
                        <div></div>
                    </template>
                </el-table-column>
                <el-table-column width='150px' label="备注" prop="">
                    <template slot-scope="scope">
                        <div>
                            <el-tag
                            :key="tag"
                            v-for="tag in scope.row.note"
                            size="small"
                            closable
                            :disable-transitions="false"
                            @close="handleClose(tag,scope.$index)">
                            {{tag}}
                            </el-tag>
                            <el-input
                            class="input-new-tag"
                            v-if="scope.row.focus"
                            v-model="scope.row.tagsVal"
                            ref="saveTagInput"
                            size="small"
                            @keyup.enter.native="handleInputConfirm(scope.$index)"
                            @blur="handleInputConfirm(scope.$index)"
                            >
                            </el-input>
                            <i class="el-icon-edit blue" @click="showInput(scope.$index,scope.row)"></i>
                            
                        </div>
                    </template>
                </el-table-column>
                <el-table-column label="状态" prop="">
                    <template slot-scope="scope">
                        <div v-html="filterState(scope.state)"></div>
                    </template>
                </el-table-column>
                <el-table-column label="创建时间" prop="createdAt">
                </el-table-column>
                <el-table-column fixed="right" label="操作" prop="">
                    <template slot-scope="scope">
                        <div>
                            <el-button type="text" @click="seeDetail(scope.row.id)">查看</el-button>
                        </div>
                    </template>
                </el-table-column>
            </el-table>
        </div>
        <div class="page-footer">
            <div class="inline-b">
                <el-button v-if="status == '0'" size="small" type="primary" @click="assigningJob()">批量分配</el-button>
                <el-button :loading="loading" size="small" type="primary" plain @click="getDataList()">刷新</el-button>
                <el-button v-if="list.length" size="small" plain @click="exportExcel()">导出本页{{searchData.limit > list.length ? list.length : searchData.limit}}条记录</el-button>
            </div>
            <el-pagination
            class="rt"
            @size-change="handleSizeChange"
            @current-change="getDataList"
            :current-page="searchData.start"
            :page-sizes="[20, 100, 200, 300, 400]"
            :page-size="searchData.limit"
            layout="total, sizes, prev, pager, next, jumper"
            :total="pageCount">
            </el-pagination>
        </div>
        <!-- 详情弹窗 -->
        <div class="uesr-detail">
            <el-dialog :visible.sync="userDialogVisible" width="90%">
                <div slot="title" class="title">
                    <h3>xxx订单详情</h3>
                </div>
                <userDialog></userDialog>
            </el-dialog>
        </div>
    </div>
</template>
<style lang="scss" scoped>
    .main-box{
        .search-box{
            max-width: 1200px;
            .el-row{
                margin-bottom: 15px;
                line-height: 32px;
                .tit{
                    display: inline-block;
                    width: 75px;
                }
            }
            .el-input,.el-select{
                width: calc(100% - 80px);
                width: -webkit-calc(100% - 80px);
                input{
                    width: 100%;
                }
            }
            .el-select{
                .el-input{
                    width: 100%;
                }
            }
        }
        .table-box{
            .phone{
                background-color: rgba(64,158,255,.1);
                padding: 0 10px;
                height: 25px;
                line-height: 25px;
                font-size: 12px;
                color: #409eff;
                border-radius: 4px;
                box-sizing: border-box;
                border: 1px solid rgba(64,158,255,.2);
                white-space: nowrap;
                display: inline-block;
            }
        }
    }
    
</style>
<style lang="scss">
    .main-box{
        .uesr-detail{
            .el-dialog{
                border-radius: 5px;
            }
        }
    }
</style>
<script>
    import { loanApply } from '@/api/order'
    import Cookies from 'js-cookie'
    import { export_json_to_excel } from '@/vendor/Export2Excel'
    import userDialog from '@/components/uesrDetailDialog'
    export default{
        data(){
            return{
                list: [],
                searchData: {
                    start: 1,
                    limit: 20,
                    Id: Cookies.get('userId') || '',
                    relName: '',
                    phone: '',
                    idcard: '',
                    sex: '',
                    zhimaScore: '',
                    ipCity: '',
                    refer: '',
                    sendUid: '',
                    examUid: '',
                    vTags: [],
                    note: '',
                    state: '',
                    createTime: [],
                    unlimitedTime: false
                },
                pageCount: 0,
                loading: false,
                timer: null,
                multipleSelection: [],
                // 弹窗
                userDialogVisible: false
            }
        },
        props: {
            status: {
                type: String,
                default: ''
            }
        },
        created() {
            let self = this
            let startMonth = '',endMonth='';
            let date = new Date();
            let yy = date.getFullYear()
            let mm = date.getMonth()+1
            let dd = date.getDate()+1
            startMonth = yy + '-' + mm + '-01'
            endMonth = yy + '-' + mm + '-' + dd
            self.searchData.createTime.push(startMonth)
            self.searchData.createTime.push(endMonth)
            self.status !='ALL' ? self.searchData.state = self.status : ''
            self.getDataList()
        },
        mounted() {
        },
        methods: {
            // 批量分配
            assigningJob() {
                let self = this
                self.$message.error('没有接口')
            },
            // 选择用户
            handleSelectionChange(val) {
                console.log(val)
                this.multipleSelection = val
            },
            //导出
            exportExcel() {
                let self = this
                const tHeader = ['ID','姓名','性别','生日','手机号','年龄','照片','欲借(元)','芝麻分','ip定位','来源','分配员','审核员','备注','状态','创建时间']
                const filterVal = ['id','realName','sex','birthday','mobile','age','idcardImg','loanYuan','zhimaScore','ipCity','refer','sendUid','examUid','note','state','createdAt']
                let list = []
                let arr = JSON.parse(JSON.stringify(self.list))
                for(let item of arr) {
                    item.birthday = (new Date().getFullYear()) - item.age
                    item.state = self.filterState(item.state)
                }
                list = arr
                const data = self.formatJson(filterVal,list)
                export_json_to_excel(tHeader,data,'全部订单列表')
                
            },
            formatJson(filterVal, jsonData) {
                return jsonData.map(v => filterVal.map(j => { return v[j]}))
            },
            // 监控搜索条件
            changeValue(){
                let self = this
                clearTimeout(self.timer)
                self.timer = setTimeout(() => {
                    self.getDataList(1)
                }, 800);
            },
            // 搜索数据
            getDataList(page) {
                let self = this
                page ? self.searchData.start = page : ''
                let tag = JSON.parse(JSON.stringify(self.searchData))
                self.loading = true
                tag.unlimitedTime ? delete tag.createTime : ''
                loanApply(tag).then(res => {
                    self.loading = false
                    let arr = res.data.rows
                    for(let item of arr) {
                        item.focus = false
                        item.tagsVal = ''
                    }
                    self.list = arr
                    self.pageCount = res.data.count
                }).catch(err => { console.log(err)})
            },
            // 点击查看详情
            seeDetail() {
                let self = this
                self.userDialogVisible = true
                // self.$router.push({ name: 'orderDetail'})
            },
            // 一页多少条内容
            handleSizeChange(val) {
                let self = this
                self.searchData.limit = val
                self.getDataList()
            },
            // 删除标签
            handleClose(tag,index) {
                let self = this
                self.$message.error('没有接口')
            },
            // 显示编辑备注输入框
            showInput(index,item) {
                let self = this 
                if(self.list[index].focus) {
                    self.list[index].focus = false
                    return
                }else{
                    self.list[index].focus = true
                }
                this.$nextTick( _ => {
                    this.$refs.saveTagInput.$refs.input.focus()
                    $('.input-new-tag input').focus()
                    console.log(this.$refs.saveTagInput.$refs.input)
                });
            },
            // 失去焦点或回车键触发保存备注
            handleInputConfirm(index) {
                let self = this
                let inputValue = self.list[index].tagsVal
                if (inputValue.trim()) {
                    // 保存标签
                    self.$message.error('没有接口')
                }
                self.list[index].tagsVal = ''
                self.list[index].focus = false
            },
            // 过滤状态
            filterState(state) {
                let txt = ''
                switch(state) {
                    case '0':
                        txt = '未分配'
                        break
                    case '3':
                        txt = '审核中'
                        break
                    case '6':
                        txt = '已通过'
                        break
                    case '9':
                        txt = '已拒绝'
                        break
                }
                return txt
            }
        },
        components: {
            userDialog
        }
    }
</script>
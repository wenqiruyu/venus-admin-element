<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="iconfont icon-wen-book"></i> 图书管理</el-breadcrumb-item>
                <el-breadcrumb-item>图书详情</el-breadcrumb-item>
                <el-breadcrumb-item>添加详情</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="form-box">
                <el-form ref="form" :rules="rules" :model="form" label-width="120px">
                    <el-form-item prop="name" label="图书名称">
                        <el-input v-model="form.name"></el-input>
                    </el-form-item>
                    <el-form-item prop="format" label="图书开张">
                        <el-input v-model="form.format"></el-input>
                    </el-form-item>
                    <el-form-item prop="paper" label="图书纸型">
                        <el-input v-model="form.paper"></el-input>
                    </el-form-item>
                    <el-form-item prop="pack" label="图书包装">
                        <el-input v-model="form.pack"></el-input>
                    </el-form-item>
                    <el-form-item prop="isSuit" label="是否套装">
                        <el-radio-group v-model="form.isSuit">
                            <el-radio label="1">是</el-radio>
                            <el-radio label="0">否</el-radio>
                        </el-radio-group>
                    </el-form-item>
                    <el-form-item prop="bookNumber" label="国际标准书号">
                        <el-input v-model="form.bookNumber"></el-input>
                    </el-form-item>
                    <el-form-item prop="bookFeature" label="图书特色">
                        <el-input type="textarea" rows="5" v-model="form.bookFeature"></el-input>
                    </el-form-item>
                    <el-form-item prop="bookEditor" label="编辑推荐">
                        <el-input type="textarea" rows="5" v-model="form.bookEditor"></el-input>
                    </el-form-item>
                    <el-form-item prop="bookContent" label="内容简介">
                        <el-input type="textarea" rows="5" v-model="form.bookContent"></el-input>
                    </el-form-item>
                    <el-form-item prop="bookAuthor" label="作者简介">
                        <el-input type="textarea" rows="5" v-model="form.bookAuthor"></el-input>
                    </el-form-item>
                    <el-form-item prop="bookCatalog" label="目录">
                        <el-input type="textarea" rows="5" v-model="form.bookCatalog"></el-input>
                    </el-form-item>
                    <el-form-item prop="bookPreface" label="前言">
                        <el-input type="textarea" rows="5" v-model="form.bookPreface"></el-input>
                    </el-form-item>     
                    <el-form-item>
                        <el-button type="primary" @click="addDetail('form')">确定</el-button>
                        <el-button @click="toBack">取消</el-button>
                    </el-form-item>
                </el-form>
            </div>
        </div>

    </div>
</template>

<script>
    export default {
        name: 'baseform',
        data: function(){
            return {
                name:'',
                form: {},
                rules: {
                    name: [
                        { required: true, message: '书本名称不能为空哦！', trigger: 'blur' }
                    ],
                    format: [
                        { required: true, message: '书本开本不能为空哦！', trigger: 'blur' }
                    ],
                    paper: [
                        { required: true, message: '书本纸张类型不能为空哦！', trigger: 'blur' }
                    ],
                    pack: [
                        { required: true, message: '书本包装类型不能为空哦！', trigger: 'blur' }
                    ],
                    isSuit: [
                        { required: true, message: '请选择书本是否为套装！', trigger: 'blur' }
                    ],
                    bookNumber: [
                        { required: true, message: '书本国际标准书号不能为空哦！', trigger: 'blur' }
                    ],
                    bookFeature: [
                        { required: true, message: '书本特色不能为空哦！', trigger: 'blur' }
                    ],
                    bookEditor: [
                        { required: true, message: '书本编辑推荐不能为空哦！', trigger: 'blur' }
                    ],
                    bookContent: [
                        { required: true, message: '书本内容介绍不能为空哦！', trigger: 'blur' }
                    ],
                    bookAuthor: [
                        { required: true, message: '书本作者介绍不能为空哦！', trigger: 'blur' }
                    ],
                    bookCatalog: [
                        { required: true, message: '书本目录不能为空哦！', trigger: 'blur' }
                    ],
                    bookPreface: [
                        { required: true, message: '书本前言不能为空哦！', trigger: 'blur' }
                    ]
                }
            }
        },
        methods: {
            onSubmit() {
                this.$message.success('提交成功！');
            },
            toBack(){
                this.$router.push('/bookDetail')
            },
            addDetail(formName){
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        var bookId = null
                        // 根据商品名称查询id
                        this.$axios.get("/product-server/product/name/" + this.form.name).then((res) => {
                            if(res.data.status == 1){
                                bookId = res.data.data.id
                            }else{
                                this.$notify.error({
                                    title: '书名错误',
                                    message: '书名相关的书籍不存在哟！请检查书名'
                                });
                            }
                        })
                        this.$axios.post("/product-server/product/detail",{
                            productId: bookId,
                            format: this.form.format,
                            paper: this.form.paper,
                            pack: this.form.pack,
                            isSuit: this.form.isSuit,
                            bookNumber: this.form.bookNumber,
                            bookFeature: this.form.bookFeature,
                            bookEditor: this.form.bookEditor,
                            bookContent: this.form.bookContent,
                            bookAuthor: this.form.bookAuthor,
                            bookCatalog: this.form.bookCatalog,
                            bookPreface: this.form.bookPreface
                        }).then((res) => {
                            if(res.data.status == 1){
                                this.$message.success(res.data.msg)
                                this.$router.push('/bookDetail')
                            }
                        })
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                })
            }
        }
    }
</script>

<style>

</style>

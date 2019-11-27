<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="iconfont icon-wen-book"></i> 图书管理</el-breadcrumb-item>
                <el-breadcrumb-item>图书上架</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="form-box">
                <el-form ref="form" :model="form" label-width="120px">
                    <el-form-item label="图书所属分类">
                        <el-cascader :options="options" v-model="form.options"></el-cascader>
                    </el-form-item>
                    <el-form-item label="书名">
                        <el-input v-model="form.name"></el-input>
                    </el-form-item>
                    <el-form-item label="作者">
                        <el-input v-model="form.author"></el-input>
                    </el-form-item>
                    <el-form-item label="副标题">
                        <el-input type="textarea" rows="5" v-model="form.subtitle"></el-input>
                    </el-form-item>
                    <el-form-item label="出版社">
                        <el-input v-model="form.press"></el-input>
                    </el-form-item>
                    <el-form-item label="出版日期">
                        <el-date-picker v-model="form.publishDate" type="date" placeholder="选择日期"></el-date-picker>
                    </el-form-item>
                    <el-form-item label="图书主图">
                        <div class="crop-demo">
                            <div class="pre-img" style="width:200px;height:200px">
                                <img v-if="cropImg" :src="cropImg" class="pre-img" style="width:200px;height:200px">
                                <div v-else style="margin-top: 85px;margin-left: 70px;color:#d9d9d9;">
                                    <span>200 x 200</span>
                                </div>
                            </div>
                            <div class="crop-demo-btn">选择图片
                                <input class="crop-input" type="file" name="image" accept="image/*" @change="setImage"/>
                            </div>
                        </div>
                        <!-- <el-dialog title="裁剪图片" :visible.sync="dialogVisible" width="30%">
                            <vue-cropper ref='cropper' :src="imgSrc" :ready="cropImage" :zoom="cropImage" :cropmove="cropImage" 
                            style="width:100%;height:320px;"></vue-cropper>
                            <span slot="footer" class="dialog-footer">
                                <el-button @click="cancelCrop">取 消</el-button>
                                <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
                            </span>
                        </el-dialog> -->
                    </el-form-item>
                    <el-form-item label="图书附图(4张)">
                        <div class="crop-demo" style="width:780px;">
                            <div>
                                <div class="pre-img" style="width:320px;height:320px;">
                                    <img v-if="subCropImg1" :src="subCropImg1" class="pre-img" style="width:320px;height:320px;">
                                    <div v-else style="margin-top: 145px;margin-left: 130px;color:#d9d9d9;">
                                        <span>320 x 320</span>
                                    </div>
                                </div>
                                <div class="pre-img" style="width:320px;height:320px;">
                                    <img v-if="subCropImg2" :src="subCropImg2" class="pre-img" style="width:320px;height:320px;">
                                    <div v-else style="margin-top: 145px;margin-left: 130px;color:#d9d9d9;">
                                        <span>320 x 320</span>
                                    </div>
                                </div>
                            </div>
                            <div>
                                <div class="pre-img" style="width:320px;height:320px;">
                                    <img v-if="subCropImg3" :src="subCropImg3" class="pre-img" style="width:320px;height:320px;">
                                    <div v-else style="margin-top: 145px;margin-left: 130px;color:#d9d9d9;">
                                        <span>320 x 320</span>
                                    </div>
                                </div>
                                <div class="pre-img" style="width:320px;height:320px;">
                                    <img v-if="subCropImg4" :src="subCropImg4" class="pre-img" style="width:320px;height:320px;">
                                    <div v-else style="margin-top: 145px;margin-left: 130px;color:#d9d9d9;">
                                        <span>320 x 320</span>
                                    </div>
                                </div>
                            </div>
                            <div class="crop-demo-btn">选择图片
                                <input class="crop-input" type="file" name="image" accept="image/*" @change="setSubImage"/>
                            </div>
                        </div>
                        <!-- <el-dialog title="裁剪图片" :visible.sync="subDialogVisible" width="30%">
                            <vue-cropper ref='subCropper' :src="subImgSrc" :ready="subCropImage" :zoom="subCropImage" :cropmove="subCropImage" 
                            style="width:100%;height:320px;"></vue-cropper>
                            <span slot="footer" class="dialog-footer">
                                <el-button @click="subCancelCrop">取 消</el-button>
                                <el-button type="primary" @click="subDialogVisible = false">确 定</el-button>
                            </span>
                        </el-dialog> -->
                    </el-form-item>
                    <el-form-item label="缩略图">
                        <div style="color:#d9d9d9;">
                            <span>请上传附图对应的缩略图</span>
                        </div>
                        <div class="crop-demo">
                            <div class="pre-img-thumbnail">
                                <img v-if="thumbnailCropImg1" :src="thumbnailCropImg1" class="pre-img-thumbnail">
                                <div v-else>
                                </div>
                            </div>
                            <div class="pre-img-thumbnail">
                                <img v-if="thumbnailCropImg2" :src="thumbnailCropImg2" class="pre-img-thumbnail">
                                <div v-else>
                                </div>
                            </div>
                            <div class="pre-img-thumbnail">
                                <img v-if="thumbnailCropImg3" :src="thumbnailCropImg3" class="pre-img-thumbnail">
                                <div v-else>
                                </div>
                            </div>
                            <div class="pre-img-thumbnail">
                                <img v-if="thumbnailCropImg4" :src="thumbnailCropImg4" class="pre-img-thumbnail">
                                <div v-else>
                                </div>
                            </div>
                            <div class="crop-demo-btn">选择图片
                                <input class="crop-input" type="file" name="image" accept="image/*" @change="setThumbnailImage"/>
                            </div>
                        </div>
                    </el-form-item>
                    <el-form-item label="图书详情">
                        <el-input type="textarea" rows="5" v-model="form.detail"></el-input>
                    </el-form-item>
                    <el-form-item label="图书定价">
                        <el-input-number v-model="form.price" :precision="2" :step="0.1" :min="0.00" placeholder="0.00"></el-input-number>
                    </el-form-item>
                    <el-form-item label="图书库存">
                        <el-input-number v-model="form.stock" :step="10" :min="0" placeholder="0"></el-input-number>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" @click="onSubmit">表单提交</el-button>
                        <el-button>取消</el-button>
                    </el-form-item>
                </el-form>
            </div>
        </div>

    </div>
</template>

<script>
    import VueCropper  from 'vue-cropperjs';
    export default {
        name: 'addbook',
        inject: ['reload'],
        data: function(){
            return {
                // 主图使用
                imgSrc: '',
                cropImg: '',
                dialogVisible: false,
                // 附图使用
                subImgSrc:'',
                subCropImg1: '',
                subCropImg2: '',
                subCropImg3: '',
                subCropImg4: '',
                // 缩略图
                thumbnailCropImg1:'',
                thumbnailCropImg2:'',
                thumbnailCropImg3:'',
                thumbnailCropImg4:'',
                subDialogVisible: false,
                options:[],
                form: {},
            }
        },
        components: {
            VueCropper
        },
        created(){
            var self = this
            // 获取分类
            this.$axios.get("/category-server/category").then((res) => {
                if(res.data.status == 1){
                    self.getCateJson(res.data.data)
                }
            })
        },
        methods: {
            getCateJson(value){
                var arr = []
                for(var i = 0, len = value.length; i < len; i++){
                    if(value[i].parentId == 0){
                        arr.push(value[i])
                    }
                }
                for(var j = 0, le = arr.length; j < le; j++){
                    var cateJson = '{"value":"' + arr[j].id + '","label":"' + arr[j].name + '","children":['
                    for(var k = 0, l = value.length; k < l; k++){
                        if(value[k].parentId == arr[j].id){
                            cateJson += '{"value":"' + value[k].id + '","label":"' + value[k].name + '"},'
                        }
                    }
                    cateJson += ']}'
                    this.options.push(JSON.parse(cateJson.replace(',]',']')))
                    cateJson = ''
                }
            },
            // 上传主图
            setImage(e){
                // 获取上传文件的信息
                const file = e.target.files[0];
                // 判断上传文件的类型是否为图片
                if (!file.type.includes('image/')) {
                    return;
                }
                const reader = new FileReader();
                reader.onload = (event) => {
                    this.dialogVisible = true;
                    // 获得上传的图片的base64码
                    this.imgSrc = event.target.result;
                    this.cropImg = event.target.result;
                    this.$refs.cropper && this.$refs.cropper.replace(event.target.result);
                };
                reader.readAsDataURL(file);
            },
            cropImage () {
                this.cropImg = this.$refs.cropper.getCroppedCanvas().toDataURL();
            },
            cancelCrop(){
                // 取消
                this.dialogVisible = false;
                this.cropImg = ''
            },
            // 上传附图
            setSubImage(e){
                // 获取上传文件的信息
                const file = e.target.files[0];
                // 判断上传文件的类型是否为图片
                if (!file.type.includes('image/')) {
                    return;
                }
                const reader = new FileReader();
                reader.onload = (event) => {
                    this.subDialogVisible = true;
                    // 获得上传的图片的base64码
                    this.subImgSrc = event.target.result;
                    // 剪切不在该步骤赋值
                    if(this.subCropImg1 == ''){
                        this.subCropImg1 = event.target.result;
                    }else if(this.subCropImg2 == ''){
                        this.subCropImg2 = event.target.result;
                    }else if(this.subCropImg3 == ''){
                        this.subCropImg3 = event.target.result;
                    }else{
                        this.subCropImg4 = event.target.result;
                    }
                    this.$refs.subCropper && this.$refs.subCropper.replace(event.target.result);
                };
                reader.readAsDataURL(file);
            },
            subCropImage () {
                if(this.subCropImg1 == '' || this.subCropImg1 == null){
                    this.subCropImg1 = this.$refs.subCropper.getCroppedCanvas().toDataURL();
                }else if(this.subCropImg2 == '' || this.subCropImg2 == null){
                    this.subCropImg2 = this.$refs.subCropper.getCroppedCanvas().toDataURL();
                }else{
                    this.subCropImg3 = this.$refs.subCropper.getCroppedCanvas().toDataURL();
                }
            },
            subCancelCrop(){
                // 取消
                this.subDialogVisible = false;
                if(this.subCropImg3 != ''){
                    this.subCropImg3 = ''
                }else if(this.subCropImg2 != ''){
                    this.subCropImg2 = ''
                }else{
                    this.subCropImg1 = ''
                }
            },
            // 上传缩略图
            setThumbnailImage(e){
                // 获取上传文件的信息
                const file = e.target.files[0];
                // 判断上传文件的类型是否为图片
                if (!file.type.includes('image/')) {
                    return;
                }
                const reader = new FileReader();
                reader.onload = (event) => {
                    this.subDialogVisible = true;
                    // 获得上传的图片的base64码
                    this.subImgSrc = event.target.result;
                    // 剪切不在该步骤赋值
                    if(this.thumbnailCropImg1 == ''){
                        this.thumbnailCropImg1 = event.target.result;
                    }else if(this.thumbnailCropImg2 == ''){
                        this.thumbnailCropImg2 = event.target.result;
                    }else if(this.thumbnailCropImg3 == ''){
                        this.thumbnailCropImg3 = event.target.result;
                    }else{
                        this.thumbnailCropImg4 = event.target.result;
                    }
                    this.$refs.subCropper && this.$refs.subCropper.replace(event.target.result);
                };
                reader.readAsDataURL(file);
            },
            imageuploaded(res) {
                console.log(res)
            },
            handleError(){
                this.$notify.error({
                    title: '上传失败',
                    message: '图片上传接口上传失败，可更改为自己的服务器接口'
                });
            },
            onSubmit() {
                // 附图
                // var subImg = []
                // subImg.push(this.subCropImg1)
                // subImg.push(this.subCropImg2)
                // subImg.push(this.subCropImg3)
                var subImg = this.subCropImg1 + "言念君子" + this.subCropImg2 + "言念君子" + this.subCropImg3 + "言念君子" + this.subCropImg4
                // 缩略图
                // var thumbnail = []
                // thumbnail.push(this.thumbnailCropImg1)
                // thumbnail.push(this.thumbnailCropImg2)
                // thumbnail.push(this.thumbnailCropImg3)
                // thumbnail.push(this.thumbnailCropImg4)
                var thumbnail = this.thumbnailCropImg1 + "言念君子" + this.thumbnailCropImg2 + "言念君子" + this.thumbnailCropImg3 + "言念君子" + this.thumbnailCropImg4
                // 提交后台，进行存储数据库
                // const params = new URLSearchParams()
                // params.append('categoryId', this.form.options[1])
                // params.append('name', this.form.name)
                // params.append('author', this.form.author)
                // params.append('subtitle', this.form.subtitle)
                // params.append('press', this.form.press)
                // params.append('publishDate', this.form.publishDate)
                // params.append('mainImg', this.form.cropImg)
                // params.append('subImg', subImg)
                // params.append('thumbnail', thumbnail)
                // params.append('detail', this.form.detail)
                // params.append('price', this.form.price)
                // params.append('stock', this.form.stock)
                this.$axios.post("/product-server/product",{
                    categoryId:this.form.options[1],
                    name:this.form.name,
                    author: this.form.author,
                    subtitle: this.form.subtitle,
                    press: this.form.press,
                    publishDate: this.form.publishDate,
                    mainImg: this.cropImg,
                    subImg: subImg,
                    thumbnail: thumbnail,
                    detail: this.form.detail,
                    price: this.form.price,
                    stock: this.form.stock,
                }).then((res) => {
                    if(res.data.status == 1){
                        this.$message.success(res.data.msg)
                        this.reload()
                    }
                })
            }
        }
    }
</script>

<style>
    .pre-img{   
        border: 1px dashed #d9d9d9;
        border-radius: 6px;
        cursor: pointer;
        position: relative;
        overflow: hidden;
    }
    .pre-img-thumbnail{
        width: 54px;
        height: 54px;
        border: 1px dashed #d9d9d9;
        border-radius: 6px;
        cursor: pointer;
        position: relative;
        overflow: hidden;
    }
    .crop-demo{
        display: flex;
        align-items: flex-end;
    }
    .crop-demo-btn{
        position: relative;
        width: 100px;
        height: 40px;
        line-height: 40px;
        padding: 0 20px;
        margin-left: 10px;
        background-color: #409eff;
        color: #fff;
        font-size: 14px;
        border-radius: 4px;
        box-sizing: border-box;
    }
    .crop-input{
        position: absolute;
        width: 100px;
        height: 40px;
        left: 0;
        top: 0;
        opacity: 0;
        cursor: pointer;
    }
</style>

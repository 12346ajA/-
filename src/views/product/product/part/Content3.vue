<template>
  <!--
    	作者：luoyiming
    	时间：2019-10-26
    	描述：商品管理-商品编辑-商品内容
    -->
  <div class="basic-setting-content pl16 pr16">
    <!--商品详情-->
    <div class="common-form">商品详情</div>
    <el-form-item label="详情类型：">
      <el-radio-group v-model="form.model.is_picture">
        <el-radio :label="0">图文</el-radio>
        <el-radio :label="1">纯图</el-radio>
      </el-radio-group>
    </el-form-item>
    <!--内容-->
    <el-form-item label="详情内容：" v-if="form.model.is_picture == 0">
      <div class="edit_container">
        <Uediter :text="form.model.content" :config="ueditor.config" ref="ue" @contentChange="contentChangeFunc"></Uediter>
      </div>
    </el-form-item>
    <el-form-item label="详情图片：" v-if="form.model.is_picture == 1">
      <div class="draggable-list">
        <draggable class="wrapper" v-model="form.model.contentImage">
          <transition-group>
            <div class="item" v-for="(item, index) in form.model.contentImage" :key="item.file_path">
              <img v-img-url="item.file_path" />
              <a href="javascript:void(0);" class="delete-btn" @click.stop="deleteImg(index)"><i class="el-icon-close"></i></a>
            </div>
          </transition-group>
        </draggable>
        <div class="item img-select" @click="openUpload"><i class="el-icon-plus"></i></div>
      </div>
    </el-form-item>
    <!--商品图片组件-->
   


    

    <!--团品实拍2-->
  
    <!--商品图片组件-->
    

    <!--资质授权3-->
    
    <!--商品图片组件-->
    

   
    <!--商品图片组件-->
    <Upload v-if="is_upload" :config="config" :isupload="is_upload" @returnImgs="returnImgsFunc">上传图片</Upload>
  </div>
</template>

<script>
import Uediter from '@/components/UE.vue';
import Upload from '@/components/file/Upload';
import draggable from 'vuedraggable';
export default {
  components: {
    /*编辑器*/
    Uediter,
    Upload,
    draggable
  },
  data() {
    return {
      /*富文本框配置*/
      ueditor: {
        text: '',
        config: {
          initialFrameWidth: 400,
          initialFrameHeight: 500
        }
      },
      is_upload: false,
      config: {
        total: 9,file_type: 'image',
      }
    };
  },
  created() {
    //this.ueditor.text = this.form.model.content;
  },
  inject: ['form'],
  methods:{

    /*获取富文本框内容*/
    getContent: function() {
      //return this.$refs.ue.getUEContent();
    },

    /*获取富文本内容*/
    contentChangeFunc(e){
      this.form.model.content=e;
    },
     /*获取富文本内容*/
    contentChangeFunc1(e){
      this.form.model.content1=e;
    },
     /*获取富文本内容*/
    contentChangeFunc2(e){
      this.form.model.content2=e;
    },
     /*获取富文本内容*/
    contentChangeFunc3(e){
      this.form.model.content3=e;
    },
    openUpload(){
      this.is_upload = true;
    },
    /*上传商品图片*/
    returnImgsFunc(e) {
      if (e != null) {
        let imgs = this.form.model.contentImage.concat(e);
        this.$set(this.form.model, 'contentImage', imgs);
      }
      this.is_upload = false;
    },
     /*上传商品图片*/
    returnImgsFunc1(e) {
      if (e != null) {
        let imgs = this.form.model.contentImage1.concat(e);
        this.$set(this.form.model, 'contentImage1', imgs);
      }
      this.is_upload = false;
    },
     /*上传商品图片*/
    returnImgsFunc2(e) {
      if (e != null) {
        let imgs = this.form.model.contentImage2.concat(e);
        this.$set(this.form.model, 'contentImage2', imgs);
      }
      this.is_upload = false;
    },
    
    /*删除商品图片*/
    deleteImg(index) {
      this.form.model.contentImage.splice(index, 1);
    },
       /*删除商品图片*/
    deleteImg1(index) {
      this.form.model.contentImage1.splice(index, 1);
    },
       /*删除商品图片*/
    deleteImg2(index) {
      this.form.model.contentImage2.splice(index, 1);
    },

  }
};
</script>

<style></style>

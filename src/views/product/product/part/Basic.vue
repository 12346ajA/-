<template>
  <!--
    	作者：luoyiming
    	时间：2019-10-26
    	描述：商品管理-商品编辑-基础信息
    -->
  <div class="basic-setting-content pl16 pr16">
    <!--基本信息-->
    <div class="common-form">基本信息</div>
    <el-form-item label="商品名称：" :rules="[{ required: true, message: '请填写商品名称' }]" prop="model.product_name">
      <el-input v-model="form.model.product_name" class="max-w460"></el-input>
    </el-form-item>
    <el-form-item label="所属分类：" :rules="[{ required: true, message: '你选择商品分类' }]" prop="model.category_id">
      <el-select v-model="form.model.category_id">
        <template v-for="cat in form.category">
          <el-option :value="cat.category_id" :key="cat.category_id" :label="cat.name"></el-option>
          <template v-if="cat.child !== undefined" v-for="two in cat.child">
            <el-option :value="two.category_id" :key="two.category_id" :label="two.name" style="padding-left: 30px;"></el-option>
            <template v-if="two.child !== undefined" v-for="three in two.child">
              <el-option :value="three.category_id" :key="three.category_id" :label="three.name" style="padding-left: 60px;"></el-option>
            </template>
          </template>
        </template>
      </el-select>
    </el-form-item>
      <el-form-item label="选择出售日期" :rules="[{ required: true, message: '选择出售日期' }]" prop="model.Listing_time">
      <el-date-picker
        v-model="form.model.Listing_time"
        type="datetime"
        value-format="yyyy-MM-dd"
        placeholder="选择日期">
      </el-date-picker>
    </el-form-item>
    <el-form-item label="预告商品：">
      <el-radio-group v-model="form.model.is_preview">
        <el-radio :label="1">开启</el-radio>
        <el-radio :label="0">关闭</el-radio>
      </el-radio-group>
    </el-form-item>
    <el-form-item v-if="form.model.is_preview==1" label="预告开启购买时间" :rules="[{ required: true, message: '请选择开启购买时间' }]" prop="model.preview_time">
      <el-date-picker
        v-model="form.model.preview_time"
        type="datetime"
        value-format="yyyy-MM-dd HH:mm:ss"
        placeholder="选择日期">
      </el-date-picker>
    </el-form-item>
    <el-form-item label="商品图片：" :rules="[{ required: true, message: '请上传商品图片' }]" prop="model.image">
      <div class="draggable-list">
        <draggable class="wrapper" v-model="form.model.image">
          <transition-group>
            <div class="item" v-for="(item, index) in form.model.image" :key="item.file_path">
              <img v-img-url="item.file_path" />
              <a href="javascript:void(0);" class="delete-btn" @click.stop="deleteImg(index)"><i class="el-icon-close"></i></a>
            </div>
          </transition-group>
        </draggable>
        <div class="item img-select" @click="openProductUpload('image', 'image')"><i class="el-icon-plus"></i></div>
      </div>
    </el-form-item>
    <el-form-item label="商品视频：">
      <el-row>
        <div class="draggable-list">
          <div class="item img-select" v-if="form.model.video_id==0" @click="openProductUpload('video', 'video')"><i class="el-icon-plus"></i></div>
          <div v-if="form.model.video_id!=0">
            <video width="150" height="150" :src="form.model.video.file_path" :autoplay="false" controls>
              您的浏览器不支持 video 标签
            </video>
            <div>
              <el-button icon="el-icon-picture-outline" @click="delVideo">删除视频</el-button>
            </div>
          </div>
        </div>
      </el-row>
    </el-form-item>
    <el-form-item label="视频封面：">
      <el-row>
        <div class="draggable-list">
          <div class="item img-select" v-if="form.model.poster_id==0" @click="openProductUpload('image', 'poster')"><i class="el-icon-plus"></i></div>
          <div v-if="form.model.poster_id!=0" class="item" @click="openProductUpload('image', 'poster')">
            <img :src="form.model.poster.file_path" width="100" height="100" />
          </div>
        </div>
      </el-row>
    </el-form-item>
    <el-form-item label="商品卖点：">
      <el-input type="textarea" v-model="form.model.selling_point" class="max-w460"></el-input>
    </el-form-item>
    <!--商品图片组件-->
    <Upload v-if="isProductUpload" :config="config" :isupload="isProductUpload" @returnImgs="returnProductImgsFunc">上传图片</Upload>
  </div>
</template>

<script>

import Upload from '@/components/file/Upload';
import draggable from 'vuedraggable';
export default {
  components: {
    Upload,
    draggable
  },
  data() {
    return {
      isProductUpload: false,
      config: {},
      file_name: 'image'
    };
  },
  inject: ['form'],
  created() {

  },
  methods: {

    /*打开上传图片*/
    openProductUpload: function(file_type, file_name) {
      this.file_name = file_name;
      if(file_type == 'image'){
        this.config = {
          total: 9,
          file_type: 'image'
        };
      }else{
        this.config = {
          total: 1,
          file_type: 'video'
        };
      }
      this.isProductUpload = true;
    },

    /*上传商品图片*/
    returnProductImgsFunc(e) {
      if (e != null) {
        if(this.file_name == 'video'){
          this.form.model.video_id = e[0].file_id;
          this.form.model.video = e[0];
        }else if(this.file_name == 'image'){
          let imgs = this.form.model.image.concat(e);
          this.$set(this.form.model, 'image', imgs);
        }else if(this.file_name == 'poster'){
          this.form.model.poster_id = e[0].file_id;
          this.form.model.poster = e[0];
        }
      }
      this.isProductUpload = false;
    },

    /*删除商品图片*/
    deleteImg(index) {
      this.form.model.image.splice(index, 1);
    },
    delVideo(){
      this.form.model.video_id = 0;
      this.form.model.video = {};
    },
  }
};
</script>

<style>
.edit_container {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  line-height: 20px;
  color: #2c3e50;
}

.ql-editor {
  height: 400px;
}

.draggable-list {
  display: flex;
  justify-content: flex-start;
  flex-wrap: wrap;
}
.draggable-list .wrapper > span {
  display: flex;
  justify-content: flex-start;
  flex-wrap: wrap;
}

.draggable-list .item {
  position: relative;
  width: 110px;
  height: 110px;
  margin-top: 10px;
  margin-right: 10px;
  border-radius: 8px;
  overflow: hidden;
  border: 1px solid #dddddd;
}

.draggable-list .delete-btn {
  position: absolute;
  top: 0;
  right: 0;
  width: 16px;
  height: 16px;
  background: red;
  line-height: 16px;
  font-size: 16px;
  color: #ffffff;
  display: none;
}

.draggable-list .item:hover .delete-btn {
  display: block;
}

.draggable-list .item img {
  position: absolute;
  top: 50%;
  left: 50%;
  -webkit-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  max-height: 100%;
  max-width: 100%;
}

.draggable-list .img-select {
  display: flex;
  justify-content: center;
  align-items: center;
  border: 1px dashed #dddddd;
  font-size: 30px;
}

.draggable-list .img-select i {
  color: #409eff;
}
</style>

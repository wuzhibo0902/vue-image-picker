<template>
  <div class="image-picker">
    <div
      v-for="(item, i) in images"
      :key="i"
      class="image-picker__holder">
      <div
        :class="{'image-picker__holder__preview--cover': i === coverIndex }"
        class="image-picker__holder__preview"
        title="点击设为封面"
        @click="coverIndex = i">
        <img :src="item.src" class="image-picker__holder__preview__img"/>
        <div class="image-picker__holder__preview__close" title="移除图片" @click.stop="removeFile(i)">
          <!-- <Icon type="ios-close" size="24" color="#f00"/> -->
          <img :src="imgClose"/>
        </div>
        <div class="image-picker__holder__preview__cover-tip">封面</div>
      </div>
      <input
        v-model="item.title"
        placeholder="图片标题"
        class="image-picker__holder__title"/>
    </div>
    <!--此处只能使用v-show，如果使用v-if，则绑定的click事件将取消-->
    <div
      v-show="max === 0 || images.length < max"
      class="image-picker__action">
      <div
        class="image-picker__action__plus"
        title="选择图片">
        <!-- <Icon
          type="ios-add"
          size="32"/> -->
        <span class="image-picker__action__plus__icon">
          <img :src="imgPlus"/>
        </span>
        <input
          ref="imagePickerTrigger"
          type="file"
          accept="image/*"
          :multiple="multiple"
          class="image-picker-trigger image-picker__action__plus__trigger"/>
      </div>
      <div v-if="images.length > 0" class="image-picker__action__more">
        <a href="javascript:;" @click="removeAll">全部移除</a>
      </div>
    </div>
  </div>
</template>

<script>
import imgPlus from '../assets/ios-plus-empty.png'
import imgClose from '../assets/ios-close-empty.png'

//
export default {
  //
  props: {
    // 最多可选择的图片数，0-不限制
    max: {
      type: Number,
      default: 0
    },
    // 是否开启多选
    multiple: {
      type: Boolean,
      default: false
    },
    // 是否去除重复-通过文件名判断
    removeDuplication: {
      type: Boolean,
      default: false
    }
  },

  //
  data () {
    return {
      files: [],
      images: [],
      coverIndex: -1,
      imgPlus,
      imgClose
    }
  },

  //
  mounted () {
    const vueIns = this
    const imgPickerTrigger = this.$refs['imagePickerTrigger']
    imgPickerTrigger.addEventListener('change', function () {
      const fileList = this.files
      for(let i = 0; i < fileList.length; i++) {
        vueIns.addFile(fileList[i])
      }
    }, false)
    // 避免选择同一文件时，change事件不触发
    imgPickerTrigger.addEventListener('click', function () {
      this.value = null
    })
  },

  //
  methods: {
    //
    addFile (file) {
      // 根据文件名去重
      if (this.removeDuplication && this.images.findIndex(img => img.name === file.name ) !== -1) {
        return
      }

      this.files.push(file)

      let tmp = file.name.split('.')
      let src = window.URL.createObjectURL(file)
      this.images.push({
        name: file.name,
        type: file.type,
        src: src,
        title: (tmp && tmp[0]) || ''
      })
    },

    //
    removeFile (index) {
      // 如果当前移除的是封面图片
      if (this.coverIndex === index) this.coverIndex = -1

      window.URL.revokeObjectURL(this.images[index].src)
      this.images.splice(index, 1)
      this.files.splice(index, 1)
    },

    //
    removeAll () {
      this.coverIndex = -1
      this.images.forEach(img => {
        window.URL.revokeObjectURL(img.src)
      })
      this.images = []
      this.files = []
    },

    //
    getFiles () {
      return this.files.map((file, i) => {
        return {
          title: this.images[i].title,
          isCover: this.coverIndex === i,
          file
        }
      })
    },

    //
    reset () {
      this.removeAll()
    }
  }
}
</script>

<style lang="less">
.image-picker {
  position: relative;
  overflow: hidden;
  margin-bottom: -10px;
  margin-left: -10px;
  box-sizing: border-box;

  &__action {
    float: left;
    width: 100px;
    margin-left: 10px;
    margin-bottom: 10px;

    &__plus {
      position: relative;
      display: flex;
      width: 100px;
      height: 100px;
      border: 1px dashed #ddd;
      justify-content: center;
      align-items: center;
      box-sizing: border-box;
      
      &__icon {
        // font-size: 32px;
        // color: #666;
        display: block;
        width: 32px;
        height: 32px;
        & > img {
          width: 100%;
          height: 100%;
        }
      }
      &__trigger {
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        overflow: hidden;
        outline: none;
        opacity: 0;
        cursor: pointer;
      }
    }

    &__more {
      height: 20px;
      margin-top: 6px;
      line-height: 20px;
      text-align: center;
      // border-bottom: 1px dashed #ddd;
      font-size: 12px;
    }
  }

  &__holder {
    float: left;
    width: 100px;
    margin-left: 10px;
    margin-bottom: 10px;

    &__preview {
      position: relative;
      width: 100px;
      height: 100px;
      overflow: hidden;
      border: 1px solid #ddd;
      padding: 2px;
      box-sizing: border-box;
      
      &__img {
        width: 100%;
        height: 100%;
      }
      &__close {
        position: absolute;
        display: flex;
        width: 20px;
        height: 20px;
        right: 0;
        top: 0;
        justify-content: center;
        align-items: center;
        background-color: rgba(0,0,0,.2);
        & > img {
          width: 100%;
          height: 100%;
        }
      }

      &__cover-tip {
        position: absolute;
        display: none;
        top: 0;
        left: 0;
        width: 40px;
        height: 20px;
        text-align: center;
        line-height: 20px;
        background-color: #fb0;
        color: #fff;
        font-size: 12px;
      }

      &:hover {
        border-color: #f90;
      }

      &--cover {
        border-color: #f90;

        .image-picker__holder__preview__cover-tip {
          display: block;
        }
      }
    }

    &__title {
      width: 100%;
      height: 20px;
      border: 0;
      border-bottom: 1px solid #ddd;
      outline: none;
      text-align: center;
      &:focus {
        border-color: #f90;
      }
    }
  }
}
</style>
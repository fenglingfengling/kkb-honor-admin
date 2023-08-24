<template>
  <image-crop-upload
    v-model="show"
    :field="field"
    :url="url"
    :width="width"
    :height="height"
    :params="params"
    :headers="headers"
    :lang-type="language"
    :with-credentials="true"
    img-format="png"
    @src-file-set="srcFileSet"
    @crop-success="cropSuccess"
    @crop-upload-success="cropUploadSuccess"
    @crop-upload-fail="cropUploadFail"
  />
</template>

<script lang="ts">
/*
git init
删除当前项⽬remote git remote remove origin
To add an exception for this directory, call: 解决 git config --global --add safe.directory D:/xampp/htdocs/kaikeba/FullStack/Vue
查看远程  git remote  查看远程链接 git remote get-url origin
安装插件 swagger Viewer 在swagger.yml ctrl+P 输入 Swagger preView 可以看到可视化的swagger 或者上面书本图标 打开可视化界面API
fatal: unable to access 'https://github.com/nhn/raphael.git/': Failed to connect to github.com port 443 after 21669 ms: Timed out
1 查看一下自己的node版本，一般高版本下载会出问题，建议使用 12.17.0版本1：查看一下自己的node版本，一般高版本下载会出问题，建议使用 12.17.0版本
2 github账户问题fatal: unable to access 'https://github.com/nhn/raphael.git/': Failed to connect to github.com port 443 after 21521 ms: Timed out
3 秘钥问题 fatal: expected flush after ref listing  服务器上的 Git - 生成 SSH 公钥 解决git config http.sslVerify “false”
4 github找不到 fatal: unable to connect to github.com:  查看全局配置: git config --global -l  
*/  
import ImageCropUpload from 'vue-image-crop-upload'
import { Component, Prop, Vue } from 'vue-property-decorator'
import { AppModule } from '@/store/modules/app'

@Component({
  name: 'AvatarUpload',
  components: {
    ImageCropUpload
  }
})
export default class extends Vue {
  // You can add more Prop, see: https://github.com/dai-siki/vue-image-crop-upload#usage
  @Prop({ required: true }) private value!: boolean
  @Prop({ required: true }) private url!: string
  @Prop({ required: true }) private field!: string
  @Prop({ default: 300 }) private width!: number
  @Prop({ default: 300 }) private height!: number
  @Prop({ default: () => {} }) private params!: object
  @Prop({ default: () => {} }) private headers!: object

  // https://github.com/dai-siki/vue-image-crop-upload#language-package
  private languageTypeList: { [key: string]: string } = {
    'en': 'en',
    'zh': 'zh',
    'es': 'es-MX',
    'ja': 'ja'
  }

  get show() {
    return this.value
  }

  set show(value) {
    this.$emit('input', value)
  }

  get language() {
    return this.languageTypeList[AppModule.language]
  }

  private srcFileSet(fileName: string, fileType: string, fileSize: number) {
    this.$emit('src-file-set', fileName, fileType, fileSize)
  }

  private cropSuccess(imgDataUrl: string, field: string) {
    this.$emit('crop-success', imgDataUrl, field)
  }

  private cropUploadSuccess(jsonData: any, field: string) {
    this.$emit('crop-upload-success', jsonData, field)
  }

  private cropUploadFail(status: boolean, field: string) {
    this.$emit('crop-upload-fail', status, field)
  }
}
</script>

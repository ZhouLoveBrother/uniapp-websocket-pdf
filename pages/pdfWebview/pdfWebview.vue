<template>
    <view style="width: 100%;" >
        <web-view  :src="allUrl"></web-view>
    </view>
</template>

<script>
    export default {
        data() {
            return {
            src: '',
            allUrl:'',
            viewerUrl: '/hybrid/html/web/viewer.html', // 格式化文件流的一个js 文件 文末会分享出来
           }
        },
        onLoad(options) {
                let fileUrl = encodeURIComponent(options.links) // encodeURIComponent 函数可把字符串作为 URI 组件进行编码。
                this.allUrl = this.viewerUrl + '?file=' + fileUrl
          }
    }
</script>

<style>

</style>

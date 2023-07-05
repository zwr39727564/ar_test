<template>
  <view class="wrap"> </view>
</template>

<script>
export default {
  data() {
    return {
      title: "Hello",
			avatar: ""
    };
  },
  onLoad() {
    this.createVk();
  },
  methods: {
    createVk() {
      const session = wx.createVKSession({
				version: 'v2',
        track: {
          face: { mode: 2 }, // mode: 1 - 使用摄像头；2 - 手动传入图像
        },
      });

      // 静态图片检测模式下，每调一次 detectFace 接口就会触发一次 updateAnchors 事件
      session.on("updateAnchors", (anchors) => {
        anchors.forEach((anchor) => {
          console.log("anchor.points", anchor.points);
          console.log("anchor.origin", anchor.origin);
          console.log("anchor.size", anchor.size);
          console.log("anchor.angle", anchor.angle);
        });
      });

      // 需要调用一次 start 以启动
      session.start((errno) => {
        if (errno) {
          // 如果失败，将返回 errno
        } else {
          // 否则，返回null，表示成功
					wx.chooseMedia({
						count: 1,
						mediaType: ['image'],
						sourceType: ['album'],
						success(res) {
							console.log(res.tempFiles.tempFilePath)
							console.log(res.tempFiles.size)
							wx.getFileSystemManager().readFile({
								filePath: res.tempFiles.tempFilePath,
								encoding: 'base64',
								success: (result)=>{
									console.log('result :>> ', result);
								},
								fail: ()=>{},
								complete: ()=>{}
							});
							session.detectFace({
								frameBuffer, // 图片 ArrayBuffer 数据。人脸图像像素点数据，每四项表示一个像素点的 RGBA
								width, // 图像宽度
								height, // 图像高度
								scoreThreshold: 0.5, // 评分阈值
								sourceType: 1,
								modelMode: 1,
							});
						}
					})
          
        }
      });
    },
  },
};
</script>

<style lang="less" scoped>
</style>

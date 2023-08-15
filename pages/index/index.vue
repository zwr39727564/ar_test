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
        track: {
          plane: { mode: 3 },
        },
        version: 'v2',
      })
      session.start(err => {
        if (err) return console.table('VK error: ', err)
        // do something
      })
    },
  },
};
</script>

<style lang="less" scoped></style>

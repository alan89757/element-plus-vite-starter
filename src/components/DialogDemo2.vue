<template>
  <div>
    <el-button @click="openDialog" style="z-index: 10">打开对话框</el-button>
    
    <div class="background-content" style="z-index: 111111;">
      <!-- 这里是其他可以点击的内容 -->
      <el-button @click="handleClick">后台按钮</el-button>
    </div>

    <el-dialog
      v-model="dialogVisible" 
      title="示例对话框"
      :close-on-click-modal="false" 
      :before-close="handleClose"
      custom-class="no-modal"
      :modal="false"
    >
      <span>这是一个示例对话框内容。</span>
      <span>您可以点击后面的内容。</span>

      <template #footer>
        <el-button @click="dialogVisible = false">关闭</el-button>
      </template>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dialogVisible: false,
    };
  },
  methods: {
    openDialog() {
      this.dialogVisible = true;
    },
    handleClose(done) {
      this.dialogVisible = false; // 关闭时改变 dialogVisible 的状态
      done(); // 结束关闭操作
    },
    handleClick() {
      alert("后台按钮被点击了！");
    }
  },
};
</script>

<style>
/* 确保弹框 z-index 低于可点击的元素 */
.no-modal .el-dialog__wrapper {
  z-index: 5; /* 设定弹框的 z-index */
}

.background-content {
  position: relative;
  z-index: 10; /* 可点击内容的 z-index，确保高于弹框 */
}

/* 可选样式 */
.el-dialog {
  background-color: transparent; /* 可选：去掉对话框背景 */
  box-shadow: none; /* 去掉对话框阴影 */
}
</style>

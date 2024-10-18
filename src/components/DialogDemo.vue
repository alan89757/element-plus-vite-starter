<template>
  <div>
    <el-button type="primary" @click="openDialog">打开弹框</el-button>

    <!-- 遍历多个弹框 -->
    <el-dialog
      v-for="(dialog, index) in dialogs"
      :key="dialog.id"
      :title="dialog.title"
      v-model="dialog.visible"
      draggable
      @close="removeDialog(dialog.id)"
      :close-on-click-modal="false"
      :before-close="handleBeforeClose"
      :append-to-body="true"
      :class="dialog.class"
      :zIndex="dialog.zIndex"
      @click="handleClick($event, dialog)"
      :style="{
        top: dialog.position.top,
        left: dialog.position.left,
        position: 'absolute',
      }"
    >
      <div
        class="dialog-content"
        @mousedown="startDrag($event, index)"
        @mouseup="stopDrag"
        @mouseleave="stopDrag"
        @mousemove="drag($event, index)"
      >
        <p>这是弹框内容 {{ dialog.id }}</p>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import { ref } from "vue";
import { ElButton, ElDialog } from "element-plus";

/*
ep-overlay-dialog 点击的时候切换值，

*/

export default {
  components: {
    ElButton,
    ElDialog,
  },
  setup() {
    const dialogs = ref([
      {
        id: 1000,
        title: `1000`,
        visible: false,
        position: { top: "50px", left: "50px" }, // 初始位置
      },
      {
        id: 1001,
        title: `1001`,
        visible: false,
        position: { top: "150px", left: "150px" }, // 初始位置
      },
      {
        id: 1002,
        title: `1002`,
        visible: false,
        position: { top: "250px", left: "350px" }, // 初始位置
      },
    ]); // 弹框数据
    let isDragging = false; // 是否正在拖动
    let dragStartX = 0; // 拖动开始的X坐标
    let dragStartY = 0; // 拖动开始的Y坐标
    let currentDialogIndex = null; // 当前正在拖动的弹框索引

    // 打开新弹框
    const openDialog = () => {
      const newDialog = {
        id: Date.now(),
        title: `弹框 ${dialogs.value.length + 1}`,
        visible: true,
        position: { top: "50px", left: "50px" }, // 初始位置
        zIndex: 100,
        class: `zIndex900`,
      };
      dialogs.value.push(newDialog); // 使用 push 使 Vue 监测到变化
    };

    // 删除弹框
    const removeDialog = (id) => {
      // 使用 filter 创建新的数组，以确保 Vue 能够检测到变化
      dialogs.value = dialogs.value.filter((dialog) => dialog.id !== id);
    };

    const handleClick = (e, item) => {
      // parent && parent.classList.contains('target-class')
      const dialog1 = dialogs.value.map((dialog) => {
        if (dialog.id === item.id) {
          return {
            ...dialog,
            class: 'zIndex900',
          };
        } else {
          return {
            ...dialog,
            class: 'zIndex100',
          };
        }
      });
      console.log("000222---", dialog1);
      dialogs.value = dialog1;
    };

    // 关闭弹框前的回调
    const handleBeforeClose = (done) => {
      done();
    };

    // 开始拖动
    const startDrag = (event, index) => {
      isDragging = true; // 标记拖动状态
      dragStartX = event.clientX; // 记录起始X坐标
      dragStartY = event.clientY; // 记录起始Y坐标
      currentDialogIndex = index; // 记录当前拖动的弹框索引
    };

    // 停止拖动
    const stopDrag = () => {
      isDragging = false;
      currentDialogIndex = null; // 清空当前拖动索引
    };

    // 拖动中更新弹框位置
    const drag = (event, index) => {
      if (!isDragging || currentDialogIndex === null) return;

      const dialog = dialogs.value[currentDialogIndex];
      const deltaX = event.clientX - dragStartX; // 鼠标水平移动距离
      const deltaY = event.clientY - dragStartY; // 鼠标垂直移动距离

      // 更新弹框位置
      dialog.position.top = `${parseInt(dialog.position.top) + deltaY}px`;
      dialog.position.left = `${parseInt(dialog.position.left) + deltaX}px`;

      // 更新起始位置为当前鼠标位置
      dragStartX = event.clientX;
      dragStartY = event.clientY;
    };

    return {
      dialogs,
      openDialog,
      handleClick,
      removeDialog,
      startDrag,
      stopDrag,
      drag,
    };
  },
};
</script>

<style>
/* 弹框内容区域，设置鼠标移动时的光标样式 */
.dialog-content {
  cursor: move; /* 设置光标为移动 */
  user-select: none; /* 禁止文本选中 */
  padding: 10px;
}

/* 设置弹框为绝对定位 */
.draggable-dialog {
  /* position: absolute; 使弹框可相对定位 */
}
.ep-overlay {
  position: unset;
}
.ep-overlay-dialog {
  position: unset;
}
.zIndex100 {
  z-index: 100
}
.zIndex900 {
  z-index: 900
}
</style>

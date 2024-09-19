---
tags:
  - Blog
  - Experiences
  - Working
---
#Done 
## 定义组件
```
// EditableInput.vue
<script setup>
import { ref, onMounted, onUnmounted, computed, reactive, useSlots, nextTick } from 'vue'
// 定义组件属性
const props = defineProps({
  modelValue: {
    type: String,
    default: ''
  },
  clearable: {
    type: Boolean,
    default: true
  }
});

// 获取插槽
const slots = useSlots();

// 定义组件事件
const emit = defineEmits(['update:modelValue', 'blur', 'focus']);

// 处理输入事件
const handleInput = (value) => {
  console.log('监听变化', value);

  emit('update:modelValue', value);
};

// 编辑模式标志
const isEditing = ref(false);

// 输入框引用
const inputRef = ref(null);
const editRef = ref(null);

// 切换编辑模式
// const toggleEditMode = () => {
//   if (!isEditing.value) {
//     isEditing.value = !isEditing.value;
//   }

//   if (isEditing.value) {
//     // 如果进入编辑模式，激活输入框
//     console.log('激活输入框');

//     nextTick(() => {
//       inputRef.value?.$el.querySelector('.el-input__inner')?.focus();
//     });
//   }
// };

// 更新文本值
const updateText = (event) => {
  text.value = event.target.value;
  console.log('text.value', text.value);

};

// 获取焦点时激活输入框
const onFocus = () => {
  inputRef.value?.focus();
};


// 监听文档的 click 事件
const documentClickHandler = (event) => {



  if (!isEditing.value && editRef.value?.contains(event.target)) {
    // 如果点击的是输入框元素，则进入编辑模式
    console.log('如果点击的是输入框元素，则进入编辑模式', editRef.value);
    isEditing.value = true;
    nextTick(() => {
      inputRef.value?.$el.querySelector('.el-input__inner')?.focus();
    });

  } else if (isEditing.value && !inputRef.value?.$el.querySelector('.el-input__inner').contains(event.target)) {
    // 如果点击的不是输入框本身或其子元素，则退出编辑模式
    console.log('如果点击的不是输入框本身或其子元素，则退出编辑模式', inputRef.value);

    isEditing.value = false;
  }
};

// 组件挂载时添加事件监听器
onMounted(() => {
  document.addEventListener('click', documentClickHandler);
});

// 组件卸载时移除事件监听器
onUnmounted(() => {
  document.removeEventListener('click', documentClickHandler);
});
</script>

<template>
  <div :class="{ editable: !isEditing }" @dblclick="toggleEditMode" ref="editRef">
    <span v-if="!isEditing" contenteditable="false">{{ modelValue }}</span>
    <el-input v-else :model-value="modelValue" @input="handleInput" :clearable="clearable" v-bind="$attrs"
      @blur="$emit('blur', $event)" @focus="$emit('focus', $event)" ref="inputRef" class="input-style">
      <template v-for="(slot, slotName) in slots" #[slotName]="slotProps">
        <slot :name="slotName" v-bind="slotProps"></slot>
      </template>
    </el-input>
  </div>
</template>

<style scoped>
.editable {
  width: 100%;
  height: 30px;
  box-sizing: border-box;
  padding: 4px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
```
## 使用组件
```
<script setup>
import { EditableInput } from '@/components/ui'
</script>
<template>
<EditableInput v-model="scope.row.num" @blur="inputNum(scope.row, scope.$index)" onkeyup="value=value.replace(/[^0-9.]/g,'')" @input="handleChange" :clearable="false" />
</template>
```
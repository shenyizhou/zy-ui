<template>
  <label class="input-group">
    <span class="input-group-label">{{label}}</span>
    <input :value="value"
      @input="handleInput"
      @compositionstart="stopInput"
      @compositionend="continueInput"
      ref="input"
      type="text">
  </label>
</template>

<script>
export default {
  name: 'zy-input-group', // 很重要，即标签名
  props: {
    label: String,
    value: String,
  },

  data() {
    return {
      isComposition: false, // 是否在输入法拼写中
    };
  },

  methods: {
    handleInput(event) {
      if (this.isComposition) {
        return;
      }
      // event为事件，event.target为事件对象，在这里即为目标input，
      // event还有个类似的属性srcElement即事件源，但是FireFox没有
      // srcElement，所以只能用target
      this.$emit('input', event.target.value);
    },

    // 当使用中文输入法，在拼写的时候屏蔽输出
    stopInput() {
      this.isComposition = true;
    },

    // 在拼写完毕后重新允许输出
    continueInput(event) {
      this.isComposition = false;
      this.handleInput(event);
    },
  },
};
</script>

<style lang="sass" scoped>
.input-group
  display: flex
  align-items: center
  margin-bottom: 8px

  &-label
    min-width: 40px
    margin-right: 8px
    font-size: 14px

  input
    flex: 1
    padding: 2px 4px
    line-height: 22px
    border: 1px solid #999
    border-radius: 4px

</style>

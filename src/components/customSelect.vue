<template>
  <div class="custom-select" :tabindex="tabindex" @blur="open = false">
    <div class="selected" :class="{ open: open }" @click="open = !open">
      {{ selected }}
    </div>
    <div class="items" :class="{ selectHide: !open }">
      <div
        v-for="(option, i) of options"
        :key="i"
        @click="
          selected = option;
          open = false;
          $emit('input', option);
        "
      >
        {{ option }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    options: {
      type: Array,
      required: true,
    },
    default: {
      type: String,
      required: false,
      default: null,
    },
    tabindex: {
      type: Number,
      required: true,
      default: 0,
    },
  },
  data() {
    return {
      selected: this.default
        ? this.default
        : this.options.length > 0
        ? this.options[0]
        : null,
      open: false,
    };
  },
  mounted() {
    this.$emit("input", this.selected);
  },
};
</script>

<style scoped>
.custom-select {
  margin-top: 5px;
  position: relative;
  width: 100%;
  text-align: left;
  outline: none;
}

.custom-select:focus-visible {
  outline: 2px solid #a1dff7;
  border-radius: 12px;
}

.custom-select .selected {
  padding: 8px 10px;
  border: 1px solid #5b5b5b;
  border-radius: 12px;
  color: black;
  cursor: pointer;
  user-select: none;
  font-size: 18px;
}

.custom-select .selected:hover {
  transition: background-color 0.3s ease-in-out;
  background-color: #ebebeb;
}

.custom-select .selected.open {
  border: 1px solid #5b5b5b;
  border-radius: 12px 12px 0px 0px;
}

.custom-select .selected:after {
  position: absolute;
  content: "";
  top: 18px;
  right: 12px;
  width: 0;
  height: 0;
  border: 5px solid transparent;
  border-color: black transparent transparent transparent;
}

.custom-select .items {
  color: black;
  border-radius: 0px 0px 12px 12px;
  overflow: hidden;
  border-right: 1px solid #5b5b5b;
  border-left: 1px solid #5b5b5b;
  border-bottom: 1px solid #5b5b5b;
  position: absolute;
  background-color: white;
  left: 0;
  right: 0;
  z-index: 1;
}

.custom-select .items div {
  color: black;
  padding: 8px 10px;
  cursor: pointer;
  user-select: none;
}

.custom-select .items div:hover {
  transition: background-color 0.3s ease-in-out;
  background-color: #a1dff7;
}

.selectHide {
  display: none;
}
</style>

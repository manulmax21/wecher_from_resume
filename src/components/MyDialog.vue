<template>
  <div
      class="dialog"
      @click.stop="hideDialog"
      v-if="show"
  >
    <div @click.stop class="dialog__content">
      <div class="nav">
        <p>Settings</p>
        <i @click="hideDialog" class="fi fi-rr-cross"></i>
      </div>
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "MyDialog",
  props:{
    show:{
      type: Boolean,
      default: false
    }
  }
  ,
  methods:{
    hideDialog(){
      this.$emit('update:show', false)
    },
  }
}
</script>

<style scoped>
@import url('https://cdn-uicons.flaticon.com/uicons-regular-rounded/css/uicons-regular-rounded.css');
p{
  margin: 0;
}
.dialog{
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  background: rgba(0,0,0,0.5);
  position: fixed;
  display: flex;
  z-index: 3;
}
.dialog__content{
  color: black;
  margin: auto;
  background: white;
  border-radius: 8px;
  min-height: 50px;
  min-width: 300px;
  padding: 20px;
}
.nav{
  display: flex;
  justify-content: space-between;
}
i:hover{
  color: #0ad2d2;
  cursor: pointer;
}
</style>
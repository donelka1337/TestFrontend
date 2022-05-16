<template>
  <div class="dialog" v-if ="show" @click.stop="hideDialog">
      <div @click.stop class="dialog_content">
          <slot></slot>
      </div>
  </div>
</template>

<script>
export default {
    methods:{
        hideDialog() {
            this.$emit('updateShow', false)
        }
    },
    props:{
        show:{
            type:Boolean,
            default: false
        }

    }
}
</script>

<style scoped>
    .dialog {
        top:0;
        bottom: 0;
        right: 0;
        left: 0; 
        background: rgba(0, 0, 0, 0.5);
        position: fixed;
        display: flex;   
    }

    .dialog_content {
        margin: auto;
        background: rgb(28, 28, 28);
        min-height: 50px;
        min-width: 300px;
    }
</style>
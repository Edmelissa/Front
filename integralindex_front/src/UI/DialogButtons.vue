<template>
<div class="dialog" v-if="show" @click.stop="hide_dialog">
  <div @click.stop class="dialog_main">
    <div class="dialog_header">
      Диалоговые кнопки
    </div>
    <div class="dialog_buttons">
      <button class="dialog_btn"> Текст1 </button>
      <button class="dialog_btn"> Текст2 </button>
    </div>
  </div>
</div>
    
</template>

<script>
export default {
  name: 'dialog-buttons',
  props:{
    show:{
      type: Boolean,
      default: false
    }
  },
  methods:{
    hide_dialog(){
      this.$emit('update:show', false)

    }
  }
}
</script>

<style scoped>
.dialog{
  position: fixed;
  display: flex;

  top: 0;
  bottom: 0;
  left: 0;
  right: 0;

  background: rgba(0,0,0,0.5);
}
.dialog_header{
  font-size: 20px;
  color: #26495c;
  margin-bottom: 2%;
}
.dialog_main{
  display: flex;
  flex-direction: column;
  align-items: left;
  justify-content: center;

  margin: auto;
  background: white;
  min-height: 10%;
  min-width: 30%;
  border: 2px solid #234455;
  border-radius: 4px; 
  padding: 1%;
}

.dialog_buttons{
  margin: 2%;
  display: flex;
  justify-content: space-between;
}
.dialog_btn{
  display: flex;
  justify-content: center;
  width: 30%; 
  background-color: #26495c; 
  border: 2px solid #234455;
  border-radius: 10px; 
  box-shadow: 0 0 10px #1e3a49;
  color: #e5e5dc;
  transition: box-shadow 300ms ease-in-out, color 300ms ease-in-out;
}
.dialog_btn:hover{
  display: flex;
  justify-content: center;
  width: 30%; 
  background-color: #26495c; 
  border: 2px solid #234455;
  border-radius: 10px; 
  box-shadow: 0 0 5px #1e3a49 inset;
  color: #e5e5dc;
}
</style>

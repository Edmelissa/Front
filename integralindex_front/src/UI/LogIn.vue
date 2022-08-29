<template>
<div class="dialog" v-if="show" @click.stop="hide_dialog">
  <div @click.stop class="dialog_main">
    <div class="dialog_header">
      Войти
    </div>
    <div class="dialog_content">
      <div class="content"> 
        <b>Логин</b>
        <input class="dialog_inp" type="text" id="login">
      </div>
      <div class="content">
        <b>Пароль</b>
        <input class="dialog_inp" type="text" id="password">
      </div>
    </div>
    <div class="dialog_buttons">
      <button class="dialog_btn">
        Войти
      </button>
    </div>
  </div>
</div>
    
</template>

<script>
export default {
  name: 'dialogbox-login',
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
}
.dialog_main{
  display: flex;
  flex-direction: column;
  align-items: left;
  justify-content: center;

  margin: auto;
  background: white;
  min-height: 30%;
  min-width: 40%;
  border: 2px solid #234455;
  border-radius: 4px; 
  padding: 1%;
}
.dialog_content{
  display: flex;
  flex-direction: column;
  align-items: left;
  justify-content: center;

  margin: auto;
  padding: 1%;

  width: 100%;
}
.content{
  display: flex;
  flex-direction: column;
  margin: 1%;
}
.dialog_inp{
  background-color: #e5e5dc; 
  border: 2px solid #435d6b;
  border-radius: 4px; 
  padding-left: 1%;
  padding-right: 1%;
  margin: 0.25%;
  width: 100%;
}
.dialog_buttons{
  display: flex;
  justify-content: center;
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

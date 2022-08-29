<template>
<div class="app">
  <div class="header">
    <div style="display: flex;">
      <input class="cb_transparent" type="checkbox" id="id_menu" v-model="menu_check" true-value="0" false-value="20"/>
      <label for="id_menu" class="header_menu">
        <label for="id_menu" class="strip"></label>
        <label for="id_menu" class="strip"></label>
        <label for="id_menu" class="strip"></label>
      </label>
    </div>
    <div class="header_text">
      <div class="header_title">Интегральный индекс</div>
      <div class="header_registration">
        <input class="cb_transparent" type="checkbox" id="login_but" v-model="login_reg_check[0]" true-value=true false-value=false/>
        <label for="login_but" class="reg_label">Вход</label>
        <input class="cb_transparent" type="checkbox" id="reg_but" v-model="login_reg_check[1]" true-value=true false-value=false/>
        <label for="reg_but" class="reg_label">Регистрация</label>
      </div>
    </div>
  </div>
  <div class="main">  
    <div v-show="menu_check==='20'">
      <div class="function">
        <div v-for="tname in topics_name" v-bind:key=tname>
          <div v-if="tname.name!=='Общая характеристика здания'">
            <h4 class="treecb1">
              {{tname.name}}
            </h4>
            <div v-for="top in topics" v-bind:key=top>
              <div v-if="top.topic===tname.name">
                <h5 class="treecb2">
                  <input type="checkbox" :id=top.name v-model="top.check" true-value="true" false-value="false"/>
                  <label :for=top.name> {{top.title}} </label>  
                </h5>
              </div>
            </div>
          </div>
        </div>
      </div> 
    </div>
    <div :style="{'width': 100+'%'}">
    <div class="solution" :style="{'padding-left': menu_check+'%'}">
      <div class="btn_div_global">
        <div class="btn_patterns" >  
          <div class="btn_loadpat">
            <button class="btn_pat" @click="global_check_loadpat = !global_check_loadpat">
              <div style="margin: 2%;"> Загрузить шаблон </div>
            </button>
            <div class="show_pat" v-if="global_check_loadpat">
              <div class="content"> 
                <b>Выберите шаблон</b>
                <!--
                <select  v-model="load_pat.select.picked">
                  <option style="" v-for="(build) in load_pat.select.variants" v-bind:key=build >
                    {{build}}
                  </option>
                </select>
                <div v-if="top.loadpat_error.show">
                  <div v-if="top.loadpat_error.text == ''">
                    <b style="color:red">
                      Ошибка загрузки шаблона
                    </b>
                  </div>
                  <div v-if="top.loadpat_error.text != ''">
                    <b style="color:red">
                      {{top.loadpat_error.text}}
                    </b>
                  </div>
                </div>
                -->
              </div>
              <div class="pat_buttons">
                <button class="pat_btn" @click="import_from_server()">
                  <div style="margin: 2%;"> ОК </div>
                </button>
                <button class="pat_btn" @click="global_check_loadpat = !global_check_loadpat">
                  <div style="margin: 2%;"> Отмена </div>
                </button>
              </div>
            </div>
          </div>
          <div class="btn_savepat">
            <button class="btn_pat" @click="global_check_savepat = !global_check_savepat">
              <div style="margin: 2%;"> Сохранить шаблон </div>
            </button> 
            <div class="show_pat" v-if="global_check_savepat">
              <div class="content"> 
                <b>Название шаблона</b>
                <!--
                <input class="inp_pat" type="text" v-model="functions[0].input[0][2]">
                <br>
                <div>
                  <input type="radio" value="Загружать полностью" v-model="savepat.radio.picked">
                  <label>Сохранять полностью</label>
                </div>
                <div>
                  <input type="radio" value="Сохранять только из выбранных расчётов" v-model="savepat.radio.picked">
                  <label>Сохранять только из выбранных расчётов</label>
                </div>
                <br>
                <div v-if="top.savepat_error.show">
                  <div v-if="top.savepat_error.text == ''">
                    <b style="color:red"> Ошибка сохранения шаблона </b>
                  </div>
                  <div v-if="top.savepat_error.text != ''">
                    <b style="color:red">
                      {{top.savepat_error.text}}
                    </b>
                  </div>
                </div>
                -->
              </div>
              <div class="pat_buttons">
                <button class="pat_btn" @click="export_to_server()">  
                  <div style="margin: 2%;">ОК</div>
                </button>
                <button class="pat_btn" @click="global_check_savepat = !global_check_savepat">
                 <div style="margin: 2%;"> Отмена </div>
                </button>
              </div>
            </div>
          </div>
        </div>
        <button class="btn_calc">
          <div style="margin: 2%;"> Расчёт </div>
        </button>
      </div>
      <div v-for="top in topics" v-bind:key=top>
        <div v-show="top.check==='true'">
        <div class="topics" :id="top.id">
          <div class="mega_block_title"> {{top.title}} </div>
          <div class="mega_block_borders">
            <div class="mega_block">
              <div class="main_block" :style="{'width': top.main_block_width+'%'}">
                <div v-for="(func, index1) in functions" v-bind:key=func>
                  <div v-if="func.id===top.name && func.render !== false">
                    <div class="block" :id="func.id +'_'+ index1">
                      <div class="block_title">{{ func.title }}</div><br>
                      <div v-for="(inp, indexinp) in func.input" v-bind:key=inp>
                        {{inp[0]}}
                        <input class="block_inp" type="text" :id="func.id +'_'+ index1+'_inp_'+indexinp"  v-model.trim="inp[2]" @input="inp[4] = validate(inp[2], inp[3])"/>
                        {{inp[1]}}
                        <div v-if="!inp[4]">
                        <h5 v-if="inp[2] === ''">поле не заполнено</h5>
                        <h5 v-if="inp[2] !== '' && (inp[3] == 'int' || inp[3] == 'uint') ">неправильный формат числа</h5>
                        </div>
                      </div>
                      <div v-for="(btn, indexrbtn) in func.r_btn" v-bind:key=btn>
                        <input type="radio" name={{func}} :value=btn :id="func.id +'_'+ index1+'_rbtn_'+indexrbtn" v-model="func.radio_elem">
                        <label>{{btn}}</label>
                      </div>
                      <table>
                        <tr>
                          <div v-for="(sel, indexsel) in func.select" v-bind:key=sel>  
                            <td>{{sel[0]}}</td>
                            <select v-model="sel[3]">
                              <option style="" v-for="(selbody, indexbody) in sel[1]" v-bind:key=selbody :id="func.id +'_'+ index1+'_sel_'+indexsel+'_selnum_'+indexbody">
                                {{selbody}}
                              </option>
                            </select>
                           </div>
                        </tr>
                      </table>
                      <p v-for="(box, indexcbox) in func.c_box" v-bind:key=box>
                        <input type="checkbox" :id="func.id +'_'+ index1+'_cbox_'+indexcbox" v-model="func.check_elem[indexcbox]">
                        {{box}}
                      </p>
                      <div v-for="(d, indexdate) in func.date" v-bind:key=d>
                        {{d[0]}}
                        <input type="date" :value=d[1] :id="func.id +'_'+ index1+'_date_'+indexdate">
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <div v-show="top.name!=='general'" class="res_block">
                <div class="block_r">
                  <div v-for="result in results" v-bind:key=result>
                    <div v-if="result.id===top.name">
                      <span v-html="result.val"></span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div v-for="result in results" v-bind:key=result>
              <div v-if="result.id===top.name" >
                <div class="btn_div">
                  <!--
                  <div class="btn_patterns" :style="{'width': top.main_block_width+'%'}">  
                    <div class="btn_loadpat">
                      <button class="btn_pat" @click="Change_check_loadpat(top.name)">
                        Загрузить шаблон
                      </button>
                      <div class="show_pat" v-if="top.check_loadpat">
                        <div class="content"> 
                          <b>Выберите шаблон</b>
                          <select  v-model="load_pat.select.picked">
                              <option style="" v-for="(build) in load_pat.select.variants" v-bind:key=build >
                                {{build}}
                              </option>
                          </select>
                          <div v-if="top.loadpat_error.show">
                            <div v-if="top.loadpat_error.text == ''">
                            <b style="color:red">
                              Ошибка загрузки шаблона
                            </b>
                            </div>
                            <div v-if="top.loadpat_error.text != ''">
                            <b style="color:red">
                                {{top.loadpat_error.text}}
                            </b>
                            </div>
                          </div>
      
                        </div>
                        <div class="pat_buttons">
                          <button class="pat_btn"  @click="import_from_server()">
                            ОК
                          </button>
                          <button class="pat_btn" @click="Change_check_loadpat(top.name)">
                            Отмена
                          </button>
                        </div>
                      </div>
                    </div>
                    <div class="btn_savepat">
                      <button class="btn_pat" @click="Change_check_savepat(top.name)">
                        Сохранить шаблон
                      </button> 
                      <div class="show_pat" v-if="top.check_savepat">
                        <div class="content"> 
                          <b>Название шаблона</b>
                          <input class="inp_pat" type="text" v-model="functions[0].input[0][2]">
                          <br>
                          <div>
                            <input type="radio" value="Загружать полностью" v-model="savepat.radio.picked">
                            <label>Сохранять полностью</label>
                          </div>
                          <div>
                            <input type="radio" value="Сохранять только из выбранных расчётов" v-model="savepat.radio.picked">
                            <label>Сохранять только из выбранных расчётов</label>
                          </div>
                          <br>
                            <div v-if="top.savepat_error.show">
                                <div v-if="top.savepat_error.text == ''">
                                <b style="color:red"> Ошибка сохранения шаблона </b>
                                </div>
                                <div v-if="top.savepat_error.text != ''">
                                <b style="color:red">
                                    {{top.savepat_error.text}}
                                </b>
                            </div>
                          </div>
                        </div>
                        <div class="pat_buttons">
                          <button class="pat_btn"  @click="export_to_server()">
                            ОК
                          </button>
                          <button class="pat_btn" @click="Change_check_savepat(top.name)">
                            Отмена
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                  -->
                  <button v-show="top.name!=='general'" class="btn_calc" v-on:click="cacl_result(result.id)">
                    <div style="margin: 2%;"> Расчёт </div>
                  </button>
                </div>
              </div>
            </div>
          </div>   
        </div>
        </div>
      </div>  
    </div>
    </div>
  </div>
  <dialogbox-login v-model:show=login_reg_check[0]>
  </dialogbox-login> 
  <dialogbox-reg v-model:show=login_reg_check>
  </dialogbox-reg> 
  <dialog-buttons v-model:show=dialog_buttons_check>
  </dialog-buttons>     
</div>
</template>

<script>

export default{
  data(){
    return {
      dialog_buttons_check: true,
      login_reg_check:[false,false],
      menu_check: '20',
      global_check_savepat: false,
      global_check_loadpat: false,
      load_pat: {
        select: {
          variants: [],
          ids: [],
          picked: ''
        }
      },
      savepat: {
        radio: {
          picked: 'Сохранять только из выбранных расчётов'
        }
      },
      topics_name:
      [
        {name: 'Общая характеристика здания'},
        {name: 'Надежность'},
        {name: 'Теплопотери'},
        {name: 'Теплопритоки'}
      ],
      topics:
      [
        {  topic:'Общая характеристика здания', name: 'general', title: 'Общая характеристика здания', check: 'true', main_block_width: '100', check_savepat: false, check_loadpat: false, loadpat_error: {show: false, text: ''}, savepat_error: {show: false, text: ''}},
        {  topic:'Надежность', name: 'reliability', title: 'Надежность', check: 'false', main_block_width: '69'},
        {  topic:'Теплопотери', name: 'heat_los_win', title: 'Расчет тепловых потерь через окна', check: 'false', main_block_width: '69'},
        {  topic:'Теплопотери', name: 'inf_win', title: 'Расчет инфильтрации через окна', check: 'false', main_block_width: '69'},
        {  topic:'Теплопотери', name: 'heat_los_inpgr', title: 'Расчет тепловых потерь через входную группу', check: 'false', main_block_width: '69'},
        {  topic:'Теплопотери', name: 'inf_inpgr', title: 'Расчет инфильтрации через двери', check: 'false', main_block_width: '69'},
        {  topic:'Теплопотери',  name: 'heat_los_heatcond_benv', title: 'Определение теплопотерь посредством теплопроводности через ограждающие конструкции', check: 'false', main_block_width: '69'},
        {  topic:'Теплопотери', name: 'heat_los_heatcond_roof', title: 'Определение теплопотерь посредством теплопроводности через кровлю', check: 'false', main_block_width: '69'},
        {  topic:'Теплопотери', name: 'heat_los_floor', title: 'Расчет теплопотерь через пол', check: 'false', main_block_width: '69'},
        {  topic:'Теплопотери', name: 'heat_los_vent', title: 'Расчет теплопотерь, связанных с вентиляцией', check: 'false', main_block_width: '69'},
        {  topic:'Теплопотери', name: 'add_heatcosts', title: 'Дополнительные затраты теплоты на повторный прогрев внутренних перегородок и интерьеров', check: 'false', main_block_width: '69'},
        {  topic:'Теплопритоки', name: 'heat_gains_people', title: 'Определение теплопритоков от людей', check: 'false', main_block_width: '69'},
        {  topic:'Теплопритоки', name: 'heat_gains_washstands', title: 'Определение затрат тепловой энергии на ГВС для рукомойников', check: 'false', main_block_width: '69'},
        {  topic:'Теплопритоки', name: 'heat_gains_showers', title: 'Определение затрат тепловой энергии на ГВС для душевых', check: 'false', main_block_width: '69'},
        {  topic:'Теплопритоки', name: 'heat_gains_electriclighting', title: 'Определение теплопритока от систем электроосвещения и силового электроснабжения', check: 'false', main_block_width: '69'},
        {  topic:'Теплопритоки', name: 'heat_gains_GVS', title: 'Определение теплопритока от неизолированных трубопроводов ГВС', check: 'false', main_block_width: '69'},
        {  topic:'Теплопритоки', name: 'heat_gains_pipelines', title: 'Определение теплопритока от неизолированных трубопроводов отопления', check: 'false', main_block_width: '69'},
      ],
      results: [
        {id: 'general', val: ''},
        {id: 'reliability', val: ''},
        {id: 'heat_los_win', val: ''},
        {id: 'inf_win', val: ''},
        {id: 'heat_los_inpgr', val: ''},
        {id: 'inf_inpgr', val: ''},
        {id: 'heat_los_heatcond_benv', val: ''},
        {id: 'heat_los_heatcond_roof', val: ''},
        {id: 'heat_los_floor', val: ''},
        {id: 'heat_los_vent', val: ''},
        {id: 'add_heatcosts', val: ''},
        {id: 'heat_gains_people', val: ''},
        {id: 'heat_gains_washstands', val: ''},
        {id: 'heat_gains_showers', val: ''},
        {id: 'heat_gains_electriclighting', val: ''},
        {id: 'heat_gains_GVS', val: ''},
        {id: 'heat_gains_pipelines', val: ''},
      ],
      functions:
      [
        {
          id: 'general', 
          title:'Общая характеристика здания', 
          //input[2] - значение, input[3] - тип, input[4] - валидность
          input:[['Название','', '', 'str', true],['Этажность','', '', 'uint', true],['Длина здания','м', '', 'uint', true],['Ширина здания','м', '', 'uint', true],['Длина стен на одном этаже','м', '', 'uint', true],['Высота стен','м', '', 'uint', true],['Температура внутреннего воздуха','°C',  '', 'int', true],['Температура наружного воздуха','°C',  '', 'int', true]],
          r_btn:null,
          //теперь  select[2] - массив id, select[3] - выбранный вариант
          select:[['Тип запорной арматуры',['Запорный вентиль шаровой','Запорный вентиль винтовой']],['Тип трубы',['Труба металлическая','Труба металлопластиковая','Труба пластиковая','Труба металлическая с утеплителем','Труба металлопластиковая с утеплителем','Труба пластиковая с утеплителем']],['Тип радиатора',['Радиатор чугунный','Радиатор биметаллический']],['Тип крана',['Кран типа \'елочка\'','Кран шаровой с аэратором','Кран сенсорный с аэратором']]],
          c_box:null,
          date:[['Дата постройки','2011-12-15']]
        },
        {
          id: 'reliability', 
          title:'Тепловой пункт', 
          input:null,
          r_btn:['Элеватор',  'ИТП'], 
          select:[['Тип насоса',['Насос ЧРП','Насос 1']],['Тип теплообменика',['Теплообменик пластинчатый','Теплообменник \'труба в трубе\'']]],
          c_box:['Наличие подогрева приточного воздуха'],
          date:null, 
          radio_elem: 'Элеватор', // хранит выбранный r_btn
          check_elem: [false]     // хранит значения чекбоксов
        }, 
        {
          id: 'reliability', 
          title:'Система подогрева приточного воздуха', 
          input:[['Число установок','ед.', '', 'uint', true],['Длина трубы от ТП до установки №1','м', '', 'uint', true],['Длина трубы от ТП до установки №2','м',  '', 'uint', true]],
          r_btn:null, 
          select:null,
          c_box:null,
          date:null,
          render: false //отвечает за отрисовку
        }, 
        {
          id: 'reliability', 
          title:'Система ГВС с рециркуляцией', 
          input:[['Число подъёмов ГВС','ед.', '', 'uint', true],['Число опусков ГВС','ед.', '', 'uint', true],['Число кранов на каждом этаже (от одного опуска)','ед.', '', 'uint', true]],
          r_btn:null, 
          select:null,
          c_box:null,
          date:null
        },   
        {
          id: 'reliability', 
          title:'Система отопления здания', 
          input:[['Число подъёмов от ТП до чердака','ед.', '', 'uint', true],['Число опусков от чердака до ТП','ед.', '', 'uint', true],['Число отопительных приборов на этаже (от одного опуска)','ед.', '', 'uint', true]],
          r_btn:null, 
          select:null,
          c_box:null,
          date:null
        },
        {
          id: 'heat_los_win', 
          title:'Характеристика окон', 
          input:[['Число окон','',  '', 'uint', true],['Температура внутреннего воздуха','',  '', 'uint', true],['Расчётная температура наружного воздуха','',  '', 'uint', true],['Длина типового окна','',  '', 'uint', true],['Высота типового окна','',  '', 'uint', true]],
          r_btn:null, 
          select:[['Тип окон',['Деревянные окна с двойным остеклением','Стеклопакет 24мм (4-16-4) в корпусе ПВХ','Стеклопакет 24мм (4-16-4) в корпусе ПВХ, низкоэмиссионное покрытие','Стеклопакет 36мм (4-10-4-14-4) в корпусе ПВХ','Стеклопакет 44мм (4-12-4-20-4) в корпусе ПВХ','Стеклопакет 44мм (4-12-4-20-4) в корпусе ПВХ, низкоэмиссионное покрытие']]],
          c_box:null,
          date: [['Дата установки окон','2011-11-12']]
        },
        {
          id: 'inf_win', 
          title:'Характеристика окон', 
          input:[['Число окон','',  '', 'uint', true],['Температура внутреннего воздуха','',  '', 'int', true],['Расчётная температура наружного воздуха','',  '', 'int', true],['Длина типового окна','',  '', 'uint', true],['Высота типового окна','',  '', 'uint', true]],
          r_btn:null, 
          select:[['Тип окон',['Деревянные окна с двойным остеклением','Стеклопакет 24мм (4-16-4) в корпусе ПВХ','Стеклопакет 24мм (4-16-4) в корпусе ПВХ, низкоэмиссионное покрытие','Стеклопакет 36мм (4-10-4-14-4) в корпусе ПВХ','Стеклопакет 44мм (4-12-4-20-4) в корпусе ПВХ','Стеклопакет 44мм (4-12-4-20-4) в корпусе ПВХ, низкоэмиссионное покрытие']]],
          c_box:null,
          date: [['Дата установки окон','2011-11-12']]
        },
        {
          id: 'heat_los_inpgr', 
          title:'Характеристика входной группы', 
          input:[['Число дверей','',  '', 'uint', true],['Температура внутреннего воздуха','',  '', 'int', true],['Расчётная температура наружного воздуха','',  '', 'int', true],['Длина типовой входной двери','',  '', 'uint', true],['Высота типовой входной двери','',  '', 'uint', true]],
          r_btn:null, 
          select:[['Тип двери',['Двери одинарные деревянные без тамбура','Двери одинарные деревянные с тамбуром между ними','Двери двойные (распашные) деревянные без тамбура','Двери двойные (распашные) деревянные с тамбуром между ними','Двери одинарные ПВХ без тамбура','Двери одинарные ПВХ с тамбуром между ними','Двери двойные (распашные) ПВХ без тамбура','Двери двойные (распашные) ПВХ с тамбуром между ними','Двери одинарные алюминиевые без тамбура','Двери одинарные алюминиевые с тамбуром между ними','Двери двойные (распашные) алюминиевые без тамбура','Двери двойные (распашные) алюминиевые с тамбуром между ними']]],
          c_box:null,
          date: [['Дата установки дверей','2011-11-12']]
        },
        {
          id: 'inf_inpgr',
          title:'Характеристика входной группы', 
          input:[['Число дверей','',  '', 'uint', true],['Температура внутреннего воздуха','',  '', 'int', true],['Расчётная температура наружного воздуха','',  '', 'int', true],['Длина типовой входной двери','',  '', 'uint', true],['Высота типовой входной двери','',  '', 'uint', true]],
          r_btn:null, 
          select:[['Тип двери',['Двери одинарные деревянные без тамбура','Двери одинарные деревянные с тамбуром между ними','Двери двойные (распашные) деревянные без тамбура','Двери двойные (распашные) деревянные с тамбуром между ними','Двери одинарные ПВХ без тамбура','Двери одинарные ПВХ с тамбуром между ними','Двери двойные (распашные) ПВХ без тамбура','Двери двойные (распашные) ПВХ с тамбуром между ними','Двери одинарные алюминиевые без тамбура','Двери одинарные алюминиевые с тамбуром между ними','Двери двойные (распашные) алюминиевые без тамбура','Двери двойные (распашные) алюминиевые с тамбуром между ними']]],
          c_box:null,
          date: [['Дата установки дверей','2011-11-12']]
        },
        {
          id: 'heat_los_heatcond_benv',
          title:'Характеристика класса энергетической эффективности', 
          input:null,
          r_btn:null, 
          select:[['Класс энергетической эффективности ограждающих конструкций',['A++ (очень высокий)','A+ (очень высокий)','A (очень высокий)','B+ (высокий)','B (высокий)','C+ (нормальный)','C (нормальный)','C- (нормальный)','D (пониженный)','E (низкий)']]],
          c_box:null,
          date:null,
        },
        {
          id: 'heat_los_heatcond_roof',
          title:'Характеристика класса энергетической эффективности', 
          input:null,
          r_btn:null, 
          select:[['Класс энергетической эффективности кровли',['A++ (очень высокий)','A+ (очень высокий)','A (очень высокий)','B+ (высокий)','B (высокий)','C+ (нормальный)','C (нормальный)','C- (нормальный)','D (пониженный)','E (низкий)']]],
          c_box:null,
          date:null,
        },
        {
          id: 'heat_los_floor', 
          title:'Тепловые потери через пол', 
          input:[['Высота подвала','м',  '', 'uint', true],['Температура внутреннего воздуха','°C',  '', 'int', true],['Температура наружного воздуха','°C',  '', 'int', true]],
          r_btn:null, 
          select:null,
          c_box:null,
          date:null
        },
        {
          id: 'heat_los_vent', 
          title:'Тепловые потери от вентиляции', 
          input:[['Температура внутреннего воздуха','°C',  '', 'int', true],['Температура наружного воздуха','°C',  '', 'int', true]],
          r_btn:null, 
          select:null,
          c_box:null,
          date:null
        },
        {
          id: 'add_heatcosts', 
          title:'Характеристика интерьера и перегородок', 
          input:[['Число дверей','',  '', 'uint', true],['Число шкафов','',  '', 'uint', true],['Число диванов','',  '', 'uint', true],['Число столов','',  '', 'uint', true],['Число навесных шкафчиков','',  '', 'uint', true]],
          r_btn:null, 
          select:null,
          c_box:null,
          date:null
        },
        {
          id: 'add_heatcosts', 
          title:'В здании многофункционального назначения реализовано энергосберегающее мероприятие – введение в нерабочее время пониженного (до +12°С) температурного графика отопления', 
          input:null,
          r_btn:null, 
          select:[['Периодичность',['Еженочно в рабочие дни и все выходные дни','Еженочно каждый календарный день','Раз в неделю (режим загородного дома)','Без понижения температуры']]],
          c_box:null,
          date:null
        },
        {
          id: 'add_heatcosts', 
          title:'Выберите варианты состава основных объектов', 
          input:null,
          r_btn:null, 
          select:[['Стены',['Штукатурка + ротбанд + кирпич глиняный пустотелый + ротбанд + штукатурка','Штукатурка + ротбанд + газобетон + ротбанд + штукатурка','Штукатурка + ротбанд + Пенобетон + ротбанд + штукатурка','Штукатурка + ротбанд + железобетон + ротбанд + штукатурка','Штукатурка + ротбанд + дерево (сосна) + ротбанд + штукатурка','Штукатурка + ротбанд + Пенополистерол + ротбанд + штукатурка','Штукатурка + ротбанд + Керамзитобетон + ротбанд + штукатурка']],['Полы',['Линолеум','Плиточный клей + плитка','Фанера (сосна) + паркет (сосна)','Фанера (сосна) + паркет (пихта)','Фанера (сосна) + паркет (клён)','Фанера (сосна) + паркет (липа)','Фанера (сосна) + паркет (дуб)']],['Двери',['Сосна','Пихта','Клён','Липа','Дуб','ДСП']],['Твёрдокорпусная мебель',['Сосна','Пихта','Клён','Липа','Дуб','ДСП']],['Диван/кровать',['Сосна','Пихта','Клён','Липа','Дуб','ДСП']]],
          c_box:null,
          date:null
        },
        {
          id: 'heat_gains_people', 
          title:'Определение теплопритоков людей', 
          input:[['Число посетителей/жильцов мужчин','', '', 'uint', true],['Число посетителей/жильцов женщин','', '', 'uint', true],['Среднее время пребывания посетителей/жильцов в сутки','', '', 'uint', true]],
          r_btn:null, 
          select:null,
          c_box:null,
          date:null
        },
        {
          id: 'heat_gains_washstands', 
          title:'Определение затрат тепловой энергии на ГВС для рукомойников', 
          input:[['Число посетителей/жильцов мужчин','', '', 'uint', true],['Число посетителей/жильцов женщин','', '', 'uint', true]],
          r_btn:null, 
          select:null,
          c_box:null,
          date:null
        },
        {
          id: 'heat_gains_showers', 
          title:'Определение затрат тепловой энергии на ГВС для душевых', 
          input:[['Число посетителей/жильцов мужчин','', '', 'uint', true],['Число посетителей/жильцов женщин','', '', 'uint', true]],
          r_btn:null, 
          select:null,
          c_box:null,
          date:null
        },
        {
          id: 'heat_gains_electriclighting', 
          title:'Определение теплопритока от систем электроосвещения и силового электроснабжения', 
          input:[['Объём потребления электрической энергии зданием за отопительный период','кВт · ч / отоп. период', '', 'uint', true]],
          r_btn:null, 
          select:null,
          c_box:['Определить расчётным способом'],
          date:null,
          check_elem: [false]     // хранит значения чекбоксов
        },
        {
          id: 'heat_gains_GVS', 
          title:'Определение теплопритока от неизолированных трубопроводов ГВС', 
          input:null,
          r_btn:null, 
          select:[['Тип трубы',['Труба металлическая','Труба металлопластиковая','Труба пластиковая','Труба металлическая с утеплителем','Труба металлопластиковая с утеплителем','Труба пластиковая с утеплителем']]],
          c_box:null,
          date:null,
        },
        {
          id: 'heat_gains_pipelines', 
          title:'Определение теплопритока от неизолированных трубопроводов отопления', 
          input:null,
          r_btn:null, 
          select:[['Тип трубы',['Труба металлическая','Труба металлопластиковая','Труба пластиковая','Труба металлическая с утеплителем','Труба металлопластиковая с утеплителем','Труба пластиковая с утеплителем']]],
          c_box:null,
          date:null,
        },
      ]
    }
  },
  methods:{
    Change_check_loadpat(name){
      this.topics.forEach(function(item){
        if(item.name === name){
          item.check_loadpat = !item.check_loadpat;
        }
      })
      return name;
    },
    Change_check_savepat(name){
      this.topics.forEach(function(item){
        if(item.name === name){
          item.check_savepat = !item.check_savepat;
        }
      })
      return name;
    },
  },
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box; 
  font-family: 'Montserrat', sans-serif;
  font-size: 14px;
}
input[type="date"]{
  background-color: #e5e5dc; 
  border: 2px solid #435d6b;
  border-radius: 4px; 
  padding-left: 1%;
  padding-right: 1%;
  margin: 0.25%;
  width: 20%;
}
input[type="checkbox"]{
  background-color: #e5e5dc; 
  border: 2px solid #435d6b;
  border-radius: 4px; 
  transform:scale(1.2);
}
select{
  background-color: #e5e5dc; 
  border: 2px solid #435d6b;
  border-radius: 4px; 
  padding-left: 1%;
  padding-right: 1%;
  margin: 0.25%;
}
label{
  margin-left: 2%;
  cursor: inherit;
  user-select: inherit;
  width: 100%;
}
.app{
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
}
.header{
  position: fixed;
  display: flex;
  flex-direction: row;
  margin: auto;
  width: 100%;
  height: 50px;
  background: #26495c;
  
  color: #e5e5dc;
}
.header_title{
  font-size: 36px;
}
.cb_transparent{
  opacity: 0;
}
.header_registration{
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;

  height: 100%;
}
.reg_label{
  display: flex;
  align-items: center;
  user-select: none; 
  height: 100%;
}
.header_menu{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 50px;
}
.strip{
  width: 35px;
  height: 6px;
  background-color: #e5e5dc;
  margin-top: 4px;
  margin-bottom: 4px;
}
.header_text{
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-left: 10px;
  padding-right: 10px;
  width: 100%;
}
.main{
  margin-top: 50px;
  width: 100%;
  height: 80%;
  display: flex;
  flex-direction: row;
}
.function{
  position: fixed;
  background: #e28a16;
  top: 50px;
  bottom: 0px;
  width: 20%;
  overflow-y: scroll;
  min-width: 170px;
}
.solution{ 
  display: flex;
  flex-direction: column;
  width: 100%;
}
.treecb1{
  display: flex;
  flex-direction: row;
  align-items: center;
  margin: 1%;
  color: #e5e5dc;
  font-size: 16px;
}
.treecb2{
  display: flex;
  flex-direction: row;
  align-items: center;
  min-height: 35px;
  margin: 2%;
  color: #0b161d;
  border-bottom: 2px dotted rgb(0, 0, 0);
  user-select: none;
}
.treecb2:hover{
  background-color: #e0886563;
  cursor: pointer;
  transition: background-color 300 ms;
}
.topics{
  display: flex; 
  flex-direction: column;
  margin: 1%;
}
.mega_block_borders{
  display: flex; 
  flex-direction: column;
  justify-content: right;
  background-color: #e5e5dc; 
  border: 2px solid #e28a16;
  border-radius: 10px; 
  box-shadow: 0 0 10px #cf7b0c;
  margin: 1% 1% 0%;
}
.mega_block_title{
  font-size: 20px;
}
.btn_div{
  display: flex;
  align-items: center;
  justify-content: right;
  margin: 1%;
}
.btn_div_global{
  display: flex;
  align-items: start;
  justify-content: space-between;
  margin: 2%;
}
.btn_patterns{
  display: flex;
  align-items: start;
  justify-content: space-between;
  width: 69%;
}
.show_pat{
  display: flex;
  flex-direction: column;
  align-items: left;
  justify-content: center;

  background: #e5e5dc;
  border: 2px solid #e28a16;
  border-radius: 4px; 
  padding: 2%;
  margin: 1%;
}
.content{
  display: flex;
  flex-direction: column;
  margin: 1%;
}
.inp_pat{
  background-color: #e5e5dc; 
  border: 2px solid #435d6b;
  border-radius: 4px; 
  padding-left: 1%;
  padding-right: 1%;
  margin: 0.25%;
}
.pat_buttons{
  display: flex;
  justify-content: space-between;
}
.pat_btn{
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
.pat_btn:hover{
  display: flex;
  justify-content: center;
  width: 30%; 
  background-color: #26495c; 
  border: 2px solid #234455;
  border-radius: 10px; 
  box-shadow: 0 0 5px #1e3a49 inset;
  color: #e5e5dc;
}
.btn_savepat{
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 48%; 
}
.btn_loadpat{
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 48%;
}
.btn_pat{
  width: 100%; 
  background-color: #e28a16; 
  border: 2px solid #d47e0c;
  border-radius: 10px; 
  box-shadow: 0 0 10px #b96e0b;
  color: #e5e5dc;
  transition: box-shadow 300ms ease-in-out, color 300ms ease-in-out;
}
.btn_pat:hover{
  width: 100%; 
  background-color: #e28a16; 
  border: 2px solid #d47e0c;
  border-radius: 10px; 
  box-shadow: 0 0 5px #b96e0b inset;
  color: #e5e5dc;
}
.btn_calc{
  display: flex;
  justify-content: center;
  width: 29%; 
  background-color: #26495c; 
  border: 2px solid #234455;
  border-radius: 10px; 
  box-shadow: 0 0 10px #1e3a49;
  color: #e5e5dc;
  transition: box-shadow 300ms ease-in-out, color 300ms ease-in-out;
}
.btn_calc:hover{
  display: flex;
  justify-content: center;
  width: 29%; 
  background-color: #26495c; 
  border: 2px solid #234455;
  border-radius: 10px; 
  box-shadow: 0 0 5px #1e3a49 inset;
  color: #e5e5dc;
}
.mega_block{
  display: flex;
  flex-direction: row;
}
.main_block{
  margin: 1% 1% 0%;
}
.res_block{
  width: 29%;
  margin: 1% 1% 0%;
}
.block{
  background-color: #e5e5dc; 
  border: 2px solid #e28a16;
  border-radius: 10px; 
  box-shadow: 0 0 10px #cf7b0c;
  width: 100%;
  margin-bottom: 1%;
  padding: 20px 40px;
}
.block_title{
  font-size: 16px;
  color:#1e3a49;
}
.block_inp{
  background-color: #e5e5dc; 
  border: 2px solid #435d6b;
  border-radius: 4px; 
  padding-left: 1%;
  padding-right: 1%;
  margin: 0.25%;
  width: 25%;
}
.block_r{
  background-color: #e5e5dc; 
  border: 2px solid #26495c;
  border-radius: 10px; 
  box-shadow: 0 0 10px #1e3a49;
  width: 100%;
  height: 99%;
  margin-bottom: 1%;
  padding: 20px 40px;
}
</style>

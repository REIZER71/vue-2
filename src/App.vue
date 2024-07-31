<script setup>
import {ref} from 'vue'
const ItemList = ref([
{id: 1, 
  label: "помыть посуду", 
  done: false,
  HighPriority: false},
{id: 2,
  label: "помыть пол",
  done: true,
  HighPriority: true}
]);
const NewItem = ref(" "); 
const isHighPriority = ref(false);
const saveItem = ()=> {
  ItemList.value.unshift(
    {id: ItemList.value.length + 1, 
    label: NewItem.value,
    done: false, 
    HighPriority: isHighPriority.value
    });
  NewItem.value = "";
  isHighPriority.value = false;
}
const isEditing = ref(false);
const edit =(e) =>
{
  isEditing.value  = e; 
  NewItem.value = ""; 
}
const switcher= (item) =>
{
item.done = !item.done;
}
const charCount =() =>
{
  return NewItem.value.length;
}
</script>

<template>
<div class="header"> 
<h1>{{"Список задач"}} </h1>
<button v-if = "isEditing"  @click="edit(false)"  class = "btn">  отмена </button>
<button v-else @click="edit(true)" class = "btn button-input"> Начать </button>
</div>
<form
    v-if = "isEditing"
     class = "add-item-form" 
     v-on:submit.prevent=saveItem()
    >
  <input type = "text" v-model = NewItem placeholder = "введите задачу"> 

  <label>
    <input type="checkbox" v-model="isHighPriority">
    Важная задача
  </label>
  <button 
    class = "btn button-input"
    v-bind:disabled="NewItem.length < 5">
    Добавить
  </button>
</form>
<p class = "counter">{{charCount()}}/200</p>
<ul>
  <li v-for= "curItem in ItemList"
    :key="curItem.id"
    @click="switcher(curItem)"
    class="static-class"
    :class=" {strikeout:curItem.done, priority: curItem.HighPriority}"
  >
  {{curItem.label}}  
    </li>
  </ul>
  <p v-if = "!ItemList.length"> Задач не найдено</p>
</template>

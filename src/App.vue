<script setup>
import { computed, ref } from 'vue';
import ItemList from './ItemList.vue';

// Olulisus: mida suurem arv, seda olulisem
const importanceLevels = [
  {value: 3, label: 'Kõrge'},
  {value: 2, label: 'Keskmine'},
  {value: 1, label: 'Madal'},
];

let message = ref('');
let importance = ref(2);
let i = 1;
let items = ref([
    {id: i++, text: 'Piim', isDone: false, importance: 3},
    {id: i++, text: 'Viin', isDone: true, importance: 1},
    {id: i++, text: 'Õlu', isDone: false, importance: 2},
    {id: i++, text: 'Sai', isDone: true, importance: 3},
    {id: i++, text: 'Juust', isDone: false, importance: 1},
    {id: i++, text: 'Munad', isDone: true, importance: 2},
]);

function add(){
  if(message.value.trim() !== '') {
    items.value.push({id: i++, text: message.value.trim(), isDone: false, importance: importance.value});
  }
  message.value = '';
}

// Kõige olulisem ees. [...] teeb koopia, et sort ei muudaks originaalmassiivi.
function sortByImportance(list) {
  return [...list].sort((a, b) => b.importance - a.importance);
}

let sortedItems = computed(() => sortByImportance(items.value));
let doneItems = computed(() => sortByImportance(items.value.filter(item => item.isDone)));
let toDoItems = computed(() => sortByImportance(items.value.filter(item => !item.isDone)));
</script>

<template>
    <div class="container content mt-2">
        <div class="field has-addons">
            <div class="control is-expanded">
                <input @keydown.enter="add" v-model="message" class="input" type="text" placeholder="Add item">
            </div>
            <div class="control">
                <div class="select">
                    <select v-model="importance">
                        <option v-for="level in importanceLevels" :key="level.value" :value="level.value">
                            {{ level.label }}
                        </option>
                    </select>
                </div>
            </div>
            <div class="control">
                <button @click="add" class="button is-info">
                    Add
                </button>
            </div>
        </div>
        
        <ItemList :items="sortedItems" title="All items"></ItemList>
        <ItemList :items="doneItems" title="Done items"></ItemList>
        <ItemList :items="toDoItems" title="ToDo items"></ItemList>
    </div>
</template>

<style></style>
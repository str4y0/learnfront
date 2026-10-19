<script setup>
defineProps(['items', 'title']);

// Olulisus -> Bulma värviklass (3 = punane, 2 = kollane, 1 = roheline)
function importanceClass(importance) {
    if (importance >= 3) return 'has-text-danger';
    if (importance === 2) return 'has-text-warning-dark';
    return 'has-text-success';
}
</script>
<template>
    <h1>{{ title }}</h1>
    <ul>
        <li v-for="item in items" :key="item.id" :class="importanceClass(item.importance)">
            {{ item.text }}
            <input type="checkbox" v-model="item.isDone">
        </li>
    </ul>
</template>
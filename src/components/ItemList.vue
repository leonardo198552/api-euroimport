<template>
    <div>
      <ul>
        <li v-for="item in items" :key="item.id">
          {{ item.title }}
          <button @click="editItem(item.id)">Editar</button>
          <button @click="deleteItem(item.id)">Excluir</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      items: Array,
    },
    methods: {
      editItem(itemId) {
        this.$emit("editItem", itemId);
      },
      deleteItem(itemId) {
        this.$emit("deleteItem", itemId);
      },
    },
  };
  </script>
  
  <style scoped>
  ul {
    list-style-type: none;
    padding: 0;
  }
  
  li {
    margin-bottom: 10px;
  }
  
  button {
    margin-left: 5px;
  }
  </style>
  
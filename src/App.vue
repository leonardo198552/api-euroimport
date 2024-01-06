<template>
  <div id="app">
    <h1>Vue CRUD</h1>
    <form @submit.prevent="addItem" v-if="!editingItemId">
      <label>
        Novo Item:
        <input v-model="newItem" />
      </label>
      <button type="submit">Adicionar</button>
    </form>
    <form @submit.prevent="updateItem" v-if="editingItemId">
      <label>
        Editar Item:
        <input v-model="editedItemTitle" />
      </label>
      <button type="submit">Atualizar</button>
    </form>
    <ItemList
      :items="items"
      @editItem="startEditing"
      @deleteItem="deleteItem"
    />
  </div>
</template>

<script>
import ItemList from "@/components/ItemList";

export default {
  data() {
    return {
      newItem: "",
      editedItemTitle: "",
      editingItemId: null,
      items: [],
    };
  },
  mounted() {
    this.fetchItems();
  },
  methods: {
    fetchItems() {
      // Simula uma chamada à API usando JSONPlaceholder
      // Substitua isso pelo seu próprio backend
      fetch("https://jsonplaceholder.typicode.com/todos")
        .then((response) => response.json())
        .then((data) => {
          this.items = data.slice(0, 5); // Limita a 5 itens para exemplo
        })
        .catch((error) => {
          console.error("Erro ao buscar itens:", error);
        });
    },
    addItem() {
      // Adiciona um novo item
      this.items.push({ id: Date.now(), title: this.newItem });
      this.newItem = "";
    },
    startEditing(itemId) {
      // Inicia a edição de um item
      this.editingItemId = itemId;
      const editedItem = this.items.find((item) => item.id === itemId);
      this.editedItemTitle = editedItem ? editedItem.title : "";
    },
    updateItem() {
      // Atualiza o item editado
      const editedItemIndex = this.items.findIndex(
        (item) => item.id === this.editingItemId
      );

      if (editedItemIndex !== -1) {
        this.items[editedItemIndex].title = this.editedItemTitle;
      }

      // Reinicia as variáveis de edição
      this.editingItemId = null;
      this.editedItemTitle = "";
    },
    deleteItem(itemId) {
      // Implemente a exclusão de itens aqui
      this.items = this.items.filter((item) => item.id !== itemId);
    },
  },
  components: {
    ItemList,
  },
};
</script>

<style>
#app {
  font-family: 'Arial', sans-serif;
  text-align: center;
  color: #333;
  margin-top: 60px;
}

h1 {
  font-size: 2em;
  margin-bottom: 20px;
}

form {
  margin-bottom: 20px;
}

label {
  font-size: 1.2em;
  margin-right: 10px;
}

input {
  font-size: 1em;
  padding: 5px;
}

button {
  font-size: 1em;
  padding: 5px 10px;
  background-color: #3498db;
  color: #fff;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #2980b9;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  background-color: #ecf0f1;
  margin: 10px 0;
  padding: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

li button {
  background-color: #e74c3c;
  color: #fff;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
}

li button:hover {
  background-color: #c0392b;
}
</style>

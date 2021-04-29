<template>
  <div id="app">
    <h1>Todo List</h1>
    <div class="new">
      <div class="name">
        <input type="text" name="name" id="name" v-model="newName" />
      </div>
      <div class="email">
        <input type="email" name="email" id="email" v-model="newEmail" />
      </div>
      <button @click="insertContact">新規作成</button>
    </div>
    <div class="table">
      <h2>連絡先リスト</h2>
      <table>
        <tr>
          <th>NAME</th>
          <th>EMAIL</th>
          <th>UPDATE</th>
          <th>DELETE</th>
        </tr>
        <tr v-for="item in contactLists" :key="item.id">
          <td><input type="text" v-model="item.name" /></td>
          <td><input type="email" v-model="item.email" /></td>
          <td>
            <button @click="updateContact(item.id, item.name, item.email)">
              更新
            </button>
          </td>
          <td>
            <button @click="deleteContact(item.id)">削除</button>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      newName: "",
      newEmail: "",
      contactLists: [],
    };
  },
  methods: {
    async getContact() {
      const resData = await axios.get("https://todolist429.herokuapp.com/api/contact/");
      this.contactLists = resData.data.data;
    },
    async insertContact() {
      const sendData = {
        name: this.newName,
        email: this.newEmail,
      };
      await axios.post("https://todolist429.herokuapp.com/api/contact/", sendData);
      await this.getContact();
    },
    async updateContact(id, name, email) {
      const sendData = {
        name: name,
        email: email,
      };
      await axios.put("https://todolist429.herokuapp.com/api/contact/" + id, sendData);
      await this.getContact();
    },
    async deleteContact(id) {
      await axios.delete("https://todolist429.herokuapp.com/api/contact/" + id);
      await this.getContact();
    },
  },
  created() {
    this.getContact();
  },
};
</script>

<style>
table,
td,
th {
  border: 1px solid #000;
  border-collapse: collapse;
  text-align: center;
}
td,
th {
  padding: 5px;
}
th {
  background: #f0e6cc;
}
</style>
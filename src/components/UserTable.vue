<template>
  <div id="user-table">
    <h3>Click a field to edit.</h3>
    <el-table :data="users" height="250" ref="userTable" class="user-table">
      <el-table-column label="Name" min-width="180">
        <editable-cell :show-input="row.editMode" slot-scope="{row}" v-model="row.name">
          <span slot="content">{{row.name}}</span>
        </editable-cell>
      </el-table-column>
      <el-table-column label="Username" min-width="180">
        <editable-cell :show-input="row.editMode" slot-scope="{row}" v-model="row.username">
          <span slot="content">{{row.username}}</span>
        </editable-cell>
      </el-table-column>
      <el-table-column label="Email" min-width="180">
        <editable-cell :show-input="row.editMode" slot-scope="{row}" v-model="row.email">
          <span slot="content">{{row.email}}</span>
        </editable-cell>
      </el-table-column>
      <el-table-column fixed="right" label="" width="120">
        <template slot-scope="scope">
          <el-button @click="deleteRow(scope.$index, users)" type="text" size="small">Remove</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import axios from "axios";
import EditableCell from "./EditableCell.vue";

export default {
  name: "UserTable",
  components: {
    EditableCell
  },
  data() {
    return {
      users: null
    };
  },
  methods: {
    deleteRow(index, rows) {
      rows.splice(index, 1);
    }
  },
  mounted() {
    axios.get("https://jsonplaceholder.typicode.com/users").then(res => {
      this.users = res.data.map(row => {
        return {
          name: row.name,
          username: row.username,
          email: row.email,
          editMode: false
        };
      });
    });
  }
};
</script>

<style>
.edit-cell {
  min-height: 35px;
  cursor: pointer;
}
.user-table {
  width: 90%;
  margin: auto;
}
</style>

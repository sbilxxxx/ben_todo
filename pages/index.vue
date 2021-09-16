<template>
  <div class="bodyContainer">
    <img src="image/profile_image.jpg" class="profileImg">
    <div class="addArea">
      <input type="text" v-model="inputStr">
      <button class="addButton" @click="doAdd">追加</button>
      <div>番号 タスク</div>
      <ul>
        <li v-for="(item, index) in list" v-bind:key="item.id">
          {{item.id}} {{item.todo}}
          <input type="checkbox" v-model="item.state" />
          <button @click="doRemove(index)">タスクを消去</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        inputStr: 'タスクを入力してください',
        // タスクインスタンス(stateは完了・未完了をboolで表現)
        list: [
          {id: 1, todo: "掃除", state: false},
          {id: 2, todo: "洗濯", state: true},
          {id: 3, todo: "炊飯", state: false},
        ]
      }
    },
    methods: {
      doAdd: function() {
        var max = this.list.reduce(function (a, b) {
          return a > b.id ? a : b.id
        }, 0);
        this.list.push({
          "id": max + 1,
          "todo": this.inputStr,
        })
      },
      doRemove: function(index) {
        this.list.splice(index, 1)
      }
    }
  };
</script>

<style scoped>
.bodyContainer{
  height: 576px;
  width: 100%;
  justify-content: center;
}
.profileImg{
  height: 120px;
  width: 120px;
}
</style>

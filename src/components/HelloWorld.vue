<template>
  <div class="row">
    <div class="col-3">
      <h3>Draggable 1</h3>
      <draggable
        id="left"
        :sort="false"
        class="dragArea list-group"
        :list="list1"
        :group="{ name: 'people', pull: 'clone', put: false }"
        @unchoose="unchoose"
        :move="move"
      >
        <div
          class="list-group-item"
          v-for="element in list1"
          :key="element.name"
        >
        <div class="item">
          <button :disabled="checkDisable(element)" @click="plusClick(element)">+</button>
          {{ element.name }}
        </div>
        </div>
      </draggable>
    </div>

    <div class="col-3">
      <h3>Draggable 2</h3>
      <draggable
        id="right"
        :sort="false"
        class="dragArea list-group"
        :list="list2"
        group="people"
        @add="add"
        @unchoose="unchoose"
        :move="move"
      >
        <div
          class="list-group-item"
          v-for="element in list2"
          :key="element.name"
        >
        <div class="item">
          <button @click="minusClick(element)">-</button>
          {{ element.name }}
        </div>
        </div>
      </draggable>
    </div>
    <button @click="click">click</button>

  </div>
</template>

<script>
import draggable from "vuedraggable";
import _ from 'underscore';

export default {
  name: "clone-test",
  display: "Clone",
  order: 2,
  components: {
    draggable
  },
  data() {
    return {
      list1: [
        { name: "John", id: 1 },
        { name: "Joao", id: 2 },
        { name: "Jean", id: 3 },
        { name: "Gerard", id: 4 }
      ],
      list2: [
        { name: "John", id: 1 },
      ]
    };
  },
  methods: {
    checkDisable(element) {
      const find = _.find(this.list2, (item) => item.id === element.id);

      return find !== undefined;
    },
    plusClick(item) {
      console.log(item);
      this.list2.push(item);
    },
    minusClick(item) {
      this.list2 = _.without(this.list2, _.findWhere(this.list2, {id: item.id}));
    },
    add(evt) {// eslint-disable-line no-unused-vars
      console.log('add');
      this.list2 = _.sortBy(this.list2, "id");
    },
    unchoose(evt) { // eslint-disable-line no-unused-vars
      console.log('unchoose');
      const element = evt.originalEvent.target.closest('#right');
      console.log(element);
      if (evt.from.id === 'right' && element === null) {
        console.log('제거요청')
        console.log(evt.item._underlying_vm_.id, ' !!!!!!!!!');
        this.list2 = _.without(this.list2, _.findWhere(this.list2, {id: evt.item._underlying_vm_.id}));
      }
    },
    move(evt, originalEvent) { // eslint-disable-line no-unused-vars
      const find = _.find(this.list2, (item) => item.id === evt.draggedContext.element.id);
      return find === undefined;
    },
    click() {
      console.log("list1: ",this.list1);
      console.log("list2: ",this.list2);
    },
    findElement(element) {
      element.closest('div');
    }
  }
};
</script>
<style type="scss" scoped>
.row {
  font-size: 20px;
  display: flex;

  .dragArea {
    height: 100%;
    background-color: yellow;
  }

  .col-3 {
    width: 300px;
    margin: 5px;
    border: 1px solid green;
  }

  .item {
    border: 1px solid black;
    padding: 3px;
    margin: 5px;
  }
}
</style>
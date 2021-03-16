<template>
  <div class="drag">
    <span class="attention-text">Перетащите направо желаемых друзей!</span>
    <span class="attention-subtext">P.S. А если передумали, то обратно :)</span>
    <div class="drag-wrapper">
      <div class="column">
        <draggable :list="list" class="list-group" draggable=".item" group="a">
          <div
            class="list-group-item item friend-item"
            v-for="element in list"
            :key="element.id"
          >
            <img class="friend-img" :src="element.photo_50" alt="Фото друга" />
            <span>{{ element.first_name }} {{ element.last_name }}</span>
          </div>

          <div
            slot="header"
            class="btn-group list-group-item"
            role="group"
            aria-label="Basic example"
          ></div>
        </draggable>
      </div>

      <div class="column">
        <draggable :list="list2" class="list-group" draggable=".item" group="a">
          <div
            class="list-group-item item friend-item"
            v-for="element in list2"
            :key="element.id"
          >
            <img class="friend-img" :src="element.photo_50" alt="Фото друга" />
            <span>{{ element.first_name }} {{ element.last_name }}</span>
          </div>

          <div
            slot="header"
            class="btn-group list-group-item"
            role="group"
            aria-label="Basic example"
          ></div>
        </draggable>
        <b-button @click="choosenFriendsToConsole" variant="outline-success"
          >Показать друзей в консоли</b-button
        >
      </div>
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";
import jsonp from "jsonp";
import { config } from "../../config.json";

export default {
  name: "Drag",
  display: "Two list header slot",
  order: 14,
  components: {
    draggable,
  },
  props: ["api_key"],
  data() {
    return {
      list: [],
      list2: [],
    };
  },
  mounted() {
    jsonp(
      `https://api.vk.com/method/friends.search?count=8&fields=photo_50&access_token=${this.api_key}&v=${config.api_version}`,
      null,
      (err, data) => {
        if (err) {
          console.error(err.message);
        } else {
          console.log(data);
          this.list = data.response.items;
        }
      }
    );
  },
  methods: {
    choosenFriendsToConsole() {
      console.log(this.list2);
    },
  },
};
</script>

<style>
.drag {
  padding-top: 3%;
  display: flex;
  flex-direction: column;
}

.attention-text {
  margin-bottom: 5px;
  color: #fff;
  font-size: 20px;
}

.attention-subtext {
  margin-bottom: 20px;
  font-size: 14px;
  color: #fff;
  font-style: italic;
}

.drag-wrapper {
  width: 80%;
  margin: 0 auto;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}

.column {
  width: 40%;
  padding: 15px;
  background-color: rgb(51, 51, 54);
  border-radius: 8px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
}

.list-group {
  width: 80%;
  margin-bottom: 15px !important;
}

.friend-img {
  margin-right: 10px;
  border-radius: 50%;
}

.friend-item {
  cursor: pointer;
  color: #fff;
  font-size: 13px;
  font-weight: bold;
  background-color: rgb(51, 51, 54) !important;
  border: 1px solid rgb(68, 68, 70) !important;
  display: flex !important;
  justify-content: flex-start;
  align-items: center;
  flex-wrap: wrap;
}

.btn-group {
  background-color: rgb(51, 51, 54) !important;
  border: 1px solid rgb(68, 68, 70) !important;
}

.friend-item:hover {
  background-color: rgb(68, 68, 70) !important;
}
</style>

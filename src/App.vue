<template>
  <div class="container">
    <div
      class="h-100 w-full flex items-center justify-center bg-teal-lightest font-sans"
    >
      <div class="bg-white rounded shadow p-6 m-4 w-full lg:w-3/4 lg:max-w-lg">
        <div class="mb-4">
          <h1 class="text-grey-darkest" style="font-size: 27px">
            📙 My simple TO-DO lists
          </h1>
          <div class="flex mt-4">
            <input
              class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker"
              v-model="title"
              style="outline: none"
              placeholder="Add Todo 😃"
            />
            <button
              @click="add"
              style="outline: none"
              class="flex-no-shrink p-2 border-2 rounded text-teal border-teal hover:text-white hover:bg-teal"
            >
              ✏️ Add
            </button>
          </div>
        </div>
        <div class="">
          📝 task(s): {{ tasks }} &nbsp; &nbsp;✔️ done(s): {{ dones }} &nbsp;
          &nbsp;🔥 undone(s): {{ tasks - dones }}
        </div>
        <div>
          <div
            class="flex mb-4 items-center"
            v-for="(list, index) in lists"
            :key="list.id"
          >
            <p
              :class="`w-full text-grey-darkest ${
                list.done ? 'line-through ' : ''
              }`"
            >
              <span :style="list.done ? 'color: green' : ''">
                <b>#{{ index + 1 }} &nbsp; {{ list.title }} </b></span
              ><br />
              <span style="color: #777">{{ list.fromNow }}</span>
            </p>
            <button
              v-if="!list.done"
              @click="done(list)"
              style="outline: none"
              class="flex-no-shrink p-2 ml-4 mr-2 border-2 rounded hover:text-white text-green border-green hover:bg-green"
            >
              ✔️ Done
            </button>
            <button
              v-else
              @click="done(list)"
              style="outline: none"
              class="flex-no-shrink p-2 ml-4 mr-2 border-2 rounded hover:text-white text-green border-green hover:bg-green"
            >
              🔥 Undone
            </button>
            <button
              @click="remove(list)"
              style="outline: none"
              class="flex-no-shrink p-2 ml-2 border-2 rounded text-red border-red hover:text-white hover:bg-red"
            >
              🔨 Remove
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/**
 * Author: Sirawit Pratoomsuwan
 * Created at: September 19, 2020, 7:48:39 pm
 */
import { nanoid } from "nanoid";
import moment from "moment";

export default {
  name: "App",
  data() {
    return {
      title: "",
      lists: [],
    };
  },
  methods: {
    add() {
      this.lists.push({
        id: nanoid(),
        title: this.title,
        done: false,
        isa: Date.now(),
        fromNow: "a few seconds ago",
      });
      localStorage.setItem("store", window.btoa(JSON.stringify(this.lists)));
      this.title = "";
    },
    done(element) {
      element.done = !element.done;
    },
    remove(element) {
      this.lists = this.lists.filter((e) => e.id !== element.id);
    },
    time(isa) {
      return moment(isa).fromNow();
    },
  },
  computed: {
    dones() {
      return this.lists.filter((el) => el.done).length;
    },
    tasks() {
      return this.lists.length;
    },
  },
  mounted() {
    if (localStorage.getItem("store")) {
      this.lists = JSON.parse(window.atob(localStorage.getItem("store")));
    }
    setInterval(() => {
      for (var i = 0; i < this.lists.length; i++) {
        this.lists[i].fromNow = moment(this.lists[i].isa).fromNow();
      }
    }, 1000);
  },
};
</script>

<style scoped>
button {
  transition: all 0.25s ease-in-out;
  outline: none;
}

.container {
  width: 720px;
  display: block;
  margin: 0 auto;
}
</style>

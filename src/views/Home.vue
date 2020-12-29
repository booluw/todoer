<template>
  <section class="page">
    <div class="page__top">
      <h1 class="heading heading--main">Todo</h1>
      <img
        src="@/assets/images/icon-moon.svg"
        v-if="lights"
        @click="lights = !lights"
      />
      <img
        src="@/assets/images/icon-sun.svg"
        v-else
        @click="lights = !lights"
      />
    </div>
    <div class="page__bottom">
      <div class="input-group">
        <label for="input" class="input-group__label"></label>
        <input
          type="text"
          class="input-group__input"
          id="input"
          v-model="newTask.title"
          @keypress.enter="addToList()"
        />
      </div>
      <div class="lists">
        <div
          class="list"
          :class="{ 'list--active': list.done }"
          v-for="(list, index) in todos"
          :key="index"
        >
          <div>
            <div
              class="list__check"
              :class="{ 'list__check--active': list.done }"
            >
              <img src="@/assets/images/icon-check.svg" alt="Check this" />
            </div>
            <p class="list__text">
              {{ list.title }}
            </p>
          </div>
          <img
            src="@/assets/images/icon-cross.svg"
            class="line__cancel"
            alt="Delete this"
          />
        </div>
        <div class="list">
          <p class="list__text list__text--faint list__text--small">
            {{ itemsLeft }} items left
          </p>
          <a
            class="list__text list__text--faint list__btn"
            href="#"
            @click.prevent="clearCompleted()"
          >
            Clear Completed
          </a>
        </div>
      </div>
      <div class="panel">
        <a class="panel__link panel__link--active" href="#" @click.prevent="showAll()">All</a>
        <a class="panel__link" href="#" @click.prevent="showActive()">Active</a>
        <a class="panel__link" href="#" @click.prevent="showCompleted()">Completed</a>
      </div>
      <p class="page__text">
        Drag and drop to reorder
      </p>
    </div>
  </section>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Home",
  computed: {
    itemsLeft: function() {
      let result = [];

      this.todos.forEach(todo => {
        if (todo.done !== true) {
          result.push(todo);
        }
      });

      return result.length;
    }
  },
  data() {
    return {
      newTask: {
        done: false,
        title: ""
      },
      lights: true,
      todos: [
        { done: true, title: "Complete online JavaScript course" },
        { done: false, title: "Jog around the park 3x" },
        { done: false, title: "10 minutes meditation" },
        { done: false, title: "Read for 1 hour" },
        { done: false, title: "Pick up groceries" },
        { done: false, title: "Complete Todo App on Frontend Mentor" }
      ]
    };
  },
  methods: {
    addTodo: function() {},
  },
  watch: {
    lights() { 
        let body = document.getElementsByTagName('body').[0]
        body.classList.toggle('dark')
     }
  }
};
</script>

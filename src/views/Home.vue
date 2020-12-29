<template>
  <section class="page">
    <div class="page__top">
      <h1 class="heading heading--main">Todo</h1>
      <img
        src="@/assets/images/icon-moon.svg"
        v-if="lights"
        @click="lights = !lights"
        class="lights"
      />
      <img
        src="@/assets/images/icon-sun.svg"
        v-else
        @click="lights = !lights"
        class="lights"
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
          v-for="(list, index) in tasks"
          :key="index"
        >
          <div class="list__details" @click="markAsDone(list)">
            <div
              class="list__check"
              :class="{ 'list__check--active': list.done }"
            >
              <div class="list__check-child">
                <img src="@/assets/images/icon-check.svg" alt="Check this" />
              </div>
            </div>
            <p class="list__text">
              {{ list.title }}
            </p>
          </div>
          <img
            src="@/assets/images/icon-cross.svg"
            class="list__cancel"
            alt="Delete this"
            @click="deleteTask(list)"
          />
        </div>
        <div class="list list--last">
          <p class="list__text list__text--faint list__text--small">
            {{ itemsLeft }} items left
          </p>
          <div class="hidden--mobile">
            <a
              class="panel__link"
              :class="[sorted == 'all' ? 'panel__link--active' : '']"
              href="#"
              @click.prevent="sorted = 'all'"
            >
              All
            </a>
            <a
              class="panel__link"
              :class="[sorted == false ? 'panel__link--active' : '']"
              href="#"
              @click.prevent="sorted = false"
            >
              Active
            </a>
            <a
              class="panel__link"
              :class="[sorted == true ? 'panel__link--active' : '']"
              href="#"
              @click.prevent="sorted = true"
            >
              Completed
            </a>
          </div>
          <a
            class="list__text list__text--faint list__btn"
            href="#"
            @click.prevent="clearCompleted()"
          >
            Clear Completed
          </a>
        </div>
      </div>
      <div class="panel hidden--desktop">
        <a
          class="panel__link"
          :class="[sorted == 'all' ? 'panel__link--active' : '']"
          href="#"
          @click.prevent="sorted = 'all'"
        >
          All
        </a>
        <a
          class="panel__link"
          :class="[sorted == false ? 'panel__link--active' : '']"
          href="#"
          @click.prevent="sorted = false"
        >
          Active
        </a>
        <a
          class="panel__link"
          :class="[sorted == true ? 'panel__link--active' : '']"
          href="#"
          @click.prevent="sorted = true"
        >
          Completed
        </a>
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
    },
    tasks: function() {
      let condition = this.sorted;

      let active = this.todos.filter(todo=>{
        return todo.done == condition
      })

      if (condition === 'all') {
        return this.todos
      } else {
        return active
      }
    }
  },
  data() {
    return {
      newTask: {
        done: false,
        title: ""
      },
      sorted: 'all',
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
    addToList: function() {
      this.todos.push(this.newTask)
      this.newTask = {
        done: false,
        title: ""
      }
    },
    markAsDone: function(list) {
      this.todos.forEach(todo => {
        if (list.title === todo.title) {
          list.done = !list.done
        }
      })
    },
    deleteTask: function(list) {
      this.todos.forEach(todo => {
        if (list.title === todo.title) {
          let index = this.todos.indexOf(list);
          if (index !== -1) {
            this.todos.splice(index, 1);
          }
        }
      })
    },
    clearCompleted: function() {
      let sorted = [];
      this.todos.forEach ( todo => {
        if (todo.done !== true) {
          sorted.push(todo)
        }
      })

      this.todos = sorted
    }
  },
  watch: {
    lights() {
        let body = document.getElementsByTagName('body').[0]
        body.classList.toggle('dark')
     }
  }
};
</script>

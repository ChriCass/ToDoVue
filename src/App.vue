<script>
import { ref, onMounted, watch } from 'vue';

export default {
  name: 'TodoApp',
  setup() {
    const todos = ref([]);
    const name = ref('');

    const input_content = ref('');
    const input_category = ref(null);

    const addTodo = () => {
      if (input_content.value.trim() === '' || input_category.value === null) {
        return;
      }

      todos.value.push({
        content: input_content.value,
        category: input_category.value,
        done: false,
        createdAt: new Date().getTime(),
      });

      input_content.value = '';
      input_category.value = null;
    };

    const deleteTodo = (todo) => {
      const index = todos.value.indexOf(todo);
      if (index !== -1) {
        todos.value.splice(index, 1);
      }
    };

    watch(todos, (newVal) => {
      localStorage.setItem('todos', JSON.stringify(newVal));
    }, { deep: true });

    watch(name, (newVal) => {
      localStorage.setItem('name', newVal);
    });

    onMounted(() => {
      name.value = localStorage.getItem('name') || '';
      todos.value = JSON.parse(localStorage.getItem('todos')) || [];
    });

    return {
      todos,
      name,
      input_content,
      input_category,
      addTodo,
      deleteTodo,
    };
  },
};
</script>

<template>
  <main class="app m-5 ">
    <section class="container shadow-lg pb-5 bg-white rounded ">
      <div class="row justify-content-center">
        <section class="col-12">
          <h2 class="text-secondary fw-bold">
            hey!
            <input type="text" class="inputt" style="border: none; outline: none;" placeholder="Your name :)" v-model="name" />
          </h2>
        </section>

        <section class="col-12 col-md-5 text-center">
          <h3 class="">Let's make a ToDo!</h3>
          <form @submit.prevent="addTodo">
            <h4 class="fw-light">writte someting!</h4>
            <div class="mb-3">
              <input
                type="text"
                class="form-control "
                v-model="input_content"
                placeholder="e.g. do your tech test for Ludik ;)"
              />
            </div>
            <div class="">
              <h4 class="col-12">Job or home?</h4>
              <label>
                <div class="form-check  rounded  text-danger">
                  <input class="form-check-input"  type="radio" name="category" value="job" v-model="input_category" />
                  <label class="form-check-label" style="margin-right: 1rem;">
                    job
                  </label>
                </div>
              </label>
              <label>
                <div class="form-check   rounded  text-info">
                  <input class="form-check-input" type="radio" name="category" value="home" v-model="input_category" />
                  <label class="form-check-label">
                    home
                  </label>
                </div>
              </label>
            </div>
            <button type="submit" class="btn mt-4 btn-info w-100 text-light">add!</button>
          </form>
        </section>

        <section class="mt-2 mx-3 col-12 col-md-6 bg-white mt-5 shadow-sm">
  <h4 class="text-center mb-3 ">check your list:</h4>
  <hr>
  <ul>
    <li v-for="todo in todos" :key="todo.createdAt" class="d-flex justify-content-between">
      <label class="form-check-label text-sm " :class="{ 'text-danger': todo.category === 'job', 'text-info': todo.category === 'home', 'text-decoration-line-through': todo.done }">
        <input type="checkbox" v-model="todo.done" />
      {{ todo.content }}
      </label>
      <button @click="deleteTodo(todo)" class="btn btn-sm btn-danger ms-auto mb-3 ">Delete</button>
    </li>
  </ul>
</section>


      </div>
    </section>
  </main>
</template>


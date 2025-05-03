<template>
  <div class="page__content">
      <header class="header">
        <h1 class="header__title">Список дел</h1>
      </header>
      <main class="content">
        <section class="todos">
          <div class="todo-form-container">
            <form name="todo-form" class="todos__form">
              <input
                type="text"
                name="todo"
                class="todo-form__input"
                placeholder="Следующее дело"
                v-model.trim="todoTitle"
                @keypress.enter.prevent.stop="addTodo"
              />
              <span class="todo-form__error" v-if="validationError">Допускаются только русские и английские буквы и цифры!</span>
              <button 
                type="submit" 
                class="todo-form__submit-btn" 
                @click.prevent.stop="addTodo"
              >
                Добавить
              </button>
            </form>
          </div>
          <ul class="todos__list">
            <li class="todo-item" v-for="todo in todos" v-if="todos.length > 0">
              <span class="todo-item__text">{{ todo.title }}</span>
              <button class="todo-item__del" @click="removeTodo(todo.id)">Готово</button>
            </li>
            <li class="todo-item" v-else>
              <h3 class="todo-item__complete-text">Ты сделал все дела - молодец!</h3>
            </li>
          </ul>
        </section>
      </main>
      <footer class="footer">
        <div class="footer__title">&copy; Andrey</div>
      </footer>
    </div>

    <div class="popup">
      <div class="popup__container">
        <button class="popup__close"></button>
        <div class="popup__content"></div>
      </div>
    </div>
</template>

<script>
import { ref } from 'vue'

export default {
  data() {
    return {
      todos: [{id: '945', title: 'Полить цветы'}, {id: '9d5', title: 'Помыть кота'}, {id: '925', title: 'Сделать дела'}, {id: '245', title: 'Умыться'}],
      todoTitle: '',
      regExp: /^(?=.*[A-Za-zА-Яа-яЁё])[\s\-\dA-Za-zА-Яа-яЁё]+$/,
      validationError: false,
    }
  },
  methods: {
    addTodo() {
      this.validation();
      if (!this.validationError) {
        this.todos.splice(0, 0, {id: String(Date.now()), title: this.todoTitle});
        this.todoTitle = '';
      }
    },
    removeTodo(idToDelete) {
      this.todos = this.todos.filter(todo => todo.id !== idToDelete)
    },
    validation() {
      if (this.regExp.test(this.todoTitle)) {
        this.validationError = false;
      } else {
        this.validationError = true;
      }
    }
  }
}
</script>

<style>
  @import url('./normalize.css');
  @import url('./style.css');

</style>
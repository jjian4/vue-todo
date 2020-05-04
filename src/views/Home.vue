<template>
    <div id="app">
        <AddTodo v-on:add-todo="addTodo" />
        <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
    </div>
</template>

<script>
import Todos from '../components/Todos';
import AddTodo from '../components/AddTodo';
import axios from 'axios';

export default {
    name: 'Home',
    components: { Todos, AddTodo },
    data() {
        return {
            todos: [
                // {
                //     id: 1,
                //     title: 'todo one',
                //     completed: false,
                // },
                // {
                //     id: 2,
                //     title: 'todo two',
                //     completed: true,
                // },
                // {
                //     id: 3,
                //     title: 'todo three',
                //     completed: false,
                // },
            ],
        };
    },
    methods: {
        deleteTodo(id) {
            axios
                .delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
                .then(
                    () =>
                        (this.todos = this.todos.filter(
                            (todo) => todo.id !== id
                        ))
                )
                .catch((err) => console.log(err));
        },
        addTodo(newTodo) {
            const { title, completed } = newTodo;

            axios
                .post('https://jsonplaceholder.typicode.com/todos', {
                    title,
                    completed,
                })
                .then((res) => (this.todos = [...this.todos, res.data]))
                .catch((err) => console.log(err));
        },
    },
    // Similar to componentDidMount in React
    created() {
        axios
            .get('https://jsonplaceholder.typicode.com/todos?_limit=5')
            .then((res) => (this.todos = res.data))
            .catch((err) => console.log(err));
    },
};
</script>

<style scoped></style>

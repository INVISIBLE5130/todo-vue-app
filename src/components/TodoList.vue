<template>
    <div class="container">
        <div class="row">
            <div class="col-12 py-5">
                <h1>{{ listName }}</h1>
            </div>
        </div>
        <div class="row mb-3">
            <create-todo @on-new-todo="addTodo($event)" />
        </div>
        <div class="row">
            <div class="col-12 col-sm-10 col-lg-6">
                <ul class="list-group">
                    <todo
                            v-for="(todo, index) in todos"
                            :key="index"
                            :description="todo.description"
                            :completed="todo.completed"
                            @on-toggle="toggleTodo(todo)"
                            @on-delete="deleteTodo(todo)"
                            @on-edit="editTodo(todo, $event)"
                    />
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
    import Todo from "./Todo.vue";
    import CreateTodo from "./CreateTodo.vue";

    export default {
        props: {
            listName: String,
        },
        data() {
            return {
                todos: [
                    { description: "Learn Vue.js", completed: false },
                    { description: "Create todo on Vue", completed: false }
                ],
            };
        },
        mounted() {
            this.init()
        },
        methods: {
            addTodo(newTodo) {
                this.todos.push({ description: newTodo, completed: false });
                this.saveTodos()
            },
            toggleTodo(todo) {
                todo.completed = !todo.completed;
                this.saveTodos()
            },
            deleteTodo(deletedTodo) {
                this.todos = this.todos.filter(todo => todo !== deletedTodo);
                this.saveTodos()
            },
            editTodo(todo, newTodoDescription) {
                todo.description = newTodoDescription;
                this.saveTodos()
            },
            saveTodos() {
                const parsed = JSON.stringify(this.todos);
                localStorage.setItem('todos', parsed);
            },
            init() {
                if (localStorage.getItem('todos')) {
                    try {
                        this.todos = JSON.parse(localStorage.getItem('todos'));
                    } catch(e) {
                        localStorage.removeItem('todos');
                    }
                }
            }
        },
        components: { Todo, CreateTodo },
    };
</script>
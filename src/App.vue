<template>
    <todo-list list-name="ToDo List" />
</template>

<script>
    import TodoList from "./components/TodoList.vue";
    export default {
        name: "App",
        components: {
            TodoList,
        },
    };
</script>
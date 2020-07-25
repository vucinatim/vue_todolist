<template>
    <div>
        <br>
        <div class="extra-tools-container">
            <p class="text-light">Active tasks: {{ remaining }}</p>
            <small class="text-btn" @click="checkAllTodos">Check All</small>
        </div>
        
        <transition-group name="active-todo">
            <div class="active-todo" v-bind:key="todo.id+'active'" v-for="todo in todos">
                <TodoItem v-if="todo.completed == false" v-bind:todo="todo" v-on:del-todo="$emit('del-todo', todo.id)" />
            </div>
        </transition-group>
    
        <br>
        <div class="extra-tools-container">
            <p class="text-light">Completed tasks: {{ todos.length - remaining }}</p>
            <small class="text-btn" @click="uncheckAllTodos">Uncheck All</small>
        </div>
        <div class="completed-todo" v-bind:key="todo.id+'completed'" v-for="todo in todos">
            <TodoItem v-if="todo.completed == true" v-bind:todo="todo" v-on:del-todo="$emit('del-todo', todo.id)" />
        </div>
    </div>
</template>

<script>
import TodoItem from './TodoItem.vue';

export default {
    name: "Todos",
    components: {
        TodoItem
    },
    props: ["todos"],
    computed: {
        remaining() {
            return this.todos.filter(todo => !todo.completed).length
        }
    },
    methods: {
        checkAllTodos() {
            this.todos.forEach(todo => {
                if (!todo.completed)
                    todo.completed = true;
            });
        },
        uncheckAllTodos() {
            this.todos.forEach(todo => {
                if (todo.completed)
                    todo.completed = false;
            });
        }
    }
}

</script>

<style lang="scss" scoped>

    .completed-todo {
        color: rgb(190, 190, 190);
    }

    .text-light {
        color: rgb(124, 124, 124);
    }

    .extra-tools-container {
        display: flex;
        justify-content: space-between;
    }

    .text-btn {
        color: #33b5e5;
        cursor: pointer;

        &:hover {
            color: darken($color: #33b5e5, $amount: 10%);
        }
    }

.active-todo {
  transition: all 0.3s;
}

.active-todo-enter, .active-todo-leave-to {
  transform: translateY(10px);
  opacity: 0;
}

.active-todo-leave-active {
  transition: all .4s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}

</style>
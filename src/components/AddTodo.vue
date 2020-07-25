<template>
    <div>
        <form @submit="addTodo">
            <input type="text" v-model="title" name="title" placeholder="Kaj je novega za opraviti?">
            <input type="submit" value="Submit" class="btn">
        </form>
    </div>
</template>

<script>
import { v4 as uuidv4 } from 'uuid';

export default {
    name: "AddTodo",
    data() {
        return {
            title: ''
        }
    },
    methods: {
        addTodo(e) {
            e.preventDefault();

            const newTodo = {
                id: uuidv4(),
                title: this.title,
                description: this.title,
                completed: false,
                deadline: new Date(),
                user: 'user1'
            }
            // Send new todo up to parent
            this.$emit('add-todo', newTodo);

            // Clears the input field
            this.title = '';
        }
    }
}
</script>

<style scoped>
    form {
        display: flex;
    }

    input[type="text"] {
        flex: 10;
        padding: 5px;
    }

    input[type=submit] {
        flex: 2;
    }
</style>
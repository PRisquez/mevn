<template>
    <div class="col-md-4 offset-md-4">
            
        <form @submit.prevent="handleUpdate()" class="card card-body mb-3">
            <h1 class="text-center h3 mb-3">Task detail</h1>
            <input class="form-control mb-3" type="text" v-model="currentTask.title">
            <textarea class="form-control mb-3" rows="3" v-model="currentTask.description"></textarea>
            <button class="btn btn-primary">Update</button>
        </form>
        <div class="text-center">
            <button class="btn btn-danger" @click="handleDelete()">Delete</button>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from "@vue/runtime-core";
import { Task } from "../interfaces/Task";
import { deleteTask, getTask, updateTask } from "../services/TaskService";

export default defineComponent({
    data() {
        return {
            currentTask: {} as Task
        }
    },
    methods: {
        async loadTask(id: string) {
            const res = await getTask(id);
            this.currentTask = res.data;
        },
        async handleUpdate(){
            if (typeof this.$route.params.id === "string"){
                const res = await updateTask(this.$route.params.id, this.currentTask);
                this.$router.push("/");
            }
        },
        async handleDelete(){
            if (typeof this.$route.params.id === "string"){
                const res = await deleteTask(this.$route.params.id);
                this.$router.push("/");
            }
        }
    },
    mounted() {
        if (typeof this.$route.params.id === "string"){
            this.loadTask(this.$route.params.id)
        }
    },
});
</script>
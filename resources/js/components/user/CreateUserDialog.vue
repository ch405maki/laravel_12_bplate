    <!-- components/CreateUserDialog.vue -->
<template>
    <Dialog v-model:open="isOpen">
        <DialogTrigger as-child>
            <Button @click="openDialog">
                <UserRoundPlus class="w-4 h-4 mr-2" /> Create
            </Button>
        </DialogTrigger>
        <DialogContent>
            <DialogHeader>
                <DialogTitle>Create New User</DialogTitle>
                <DialogDescription>Fill in the details to add a new user.</DialogDescription>
            </DialogHeader>

            <form @submit.prevent="createUser">
                <div class="grid gap-4">
                    <input v-model="formData.name" type="text" placeholder="Name" class="form-input" required />
                    <input v-model="formData.email" type="email" placeholder="Email" class="form-input" required />
                    <input v-model="formData.password" type="password" placeholder="Password" class="form-input" required />
                    <select v-model="formData.role" class="form-input" required>
                        <option value="admin">Admin</option>
                        <option value="user">User</option>
                    </select>
                    <select v-model="formData.status" class="form-input" required>
                        <option value="active">Active</option>
                        <option value="inactive">Inactive</option>
                    </select>
                </div>

                <DialogFooter class="mt-4">
                    <Button variant="outline" @click="closeDialog">Cancel</Button>
                    <Button type="submit" :disabled="loading">
                        <span v-if="loading">Creating...</span>
                        <span v-else>Create</span>
                    </Button>
                </DialogFooter>
            </form>
        </DialogContent>
    </Dialog>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { Button } from "@/components/ui/button";
import { Dialog, DialogContent, DialogDescription, DialogFooter, DialogHeader, DialogTitle, DialogTrigger } from "@/components/ui/dialog";
import { UserRoundPlus } from "lucide-vue-next";
import axios from "axios";
import { useToast } from "vue-toastification";

const toast = useToast();
const isOpen = ref(false);
const loading = ref(false);
const formData = ref({ name: "", email: "", password: "", role: "user", status: "active" });

const openDialog = () => (isOpen.value = true);
const closeDialog = () => (isOpen.value = false);

const createUser = async () => {
    loading.value = true;
    try {
        await axios.post("/api/users", formData.value);
        toast.success("User created successfully!");
        setTimeout(() => location.reload(), 2000);
    } catch (error) {
        toast.error("Failed to create user");
    } finally {
        loading.value = false;
        closeDialog();
    }
};
</script>

<style scoped>
.form-input {
    @apply border p-2 rounded w-full bg-gray-100 dark:bg-gray-700 border-gray-300 dark:border-gray-600 text-gray-900 dark:text-gray-200;
}
</style>

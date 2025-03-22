<template>
    <Head title="Users Management" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div class="flex h-full flex-1 flex-col gap-4 rounded-xl p-4">
            <div class="relative min-h-[100vh] flex-1 rounded-xl border border-sidebar-border/70 dark:border-sidebar-border md:min-h-min">
                <Table>
                    <TableHeader>
                        <TableRow>
                            <TableHead>User Name</TableHead>
                            <TableHead>Email</TableHead>
                            <TableHead>Role</TableHead>
                            <TableHead class="text-right">Action</TableHead>
                        </TableRow>
                    </TableHeader>
                    <TableBody>
                        <TableRow v-for="user in users" :key="user.id">
                            <TableCell class="font-medium">{{ user.name }}</TableCell>
                            <TableCell>{{ user.email }}</TableCell>
                            <TableCell>Administrator</TableCell>
                            <TableCell class="text-right">
                                <Dialog v-model:open="deleteDialogOpen[user.id]">
                                    <DialogTrigger as-child>
                                        <button @click="openDeleteDialog(user.id)">
                                            <Trash class="w-4 h-4 mr-2 text-red-600 hover:text-red-700" />
                                        </button>
                                    </DialogTrigger>
                                    <DialogContent>
                                        <DialogHeader>
                                            <DialogTitle>Confirm Delete</DialogTitle>
                                            <DialogDescription>
                                                Are you sure you want to delete <strong>{{ user.name }}</strong>? This action cannot be undone.
                                            </DialogDescription>
                                        </DialogHeader>
                                        <DialogFooter>
                                            <Button variant="outline" @click="closeDeleteDialog(user.id)">Cancel</Button>
                                            <Button variant="destructive" :disabled="loading" @click="confirmDelete(user.id)">
                                                <span v-if="loading">Deleting...</span>
                                                <span v-else>Confirm</span>
                                            </Button>
                                        </DialogFooter>
                                    </DialogContent>
                                </Dialog>
                            </TableCell>
                        </TableRow>
                    </TableBody>
                </Table>
            </div>
        </div>
    </AppLayout>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';
import AppLayout from '@/layouts/AppLayout.vue';
import { type BreadcrumbItem } from '@/types';
import { Head } from '@inertiajs/vue3';
import { Button } from '@/components/ui/button';
import { Trash } from 'lucide-vue-next';
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from '@/components/ui/table';
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from '@/components/ui/dialog';
import { useToast } from 'vue-toastification';

const toast = useToast();

const breadcrumbs: BreadcrumbItem[] = [
    { title: 'Users Management', href: '/users' },
];

const props = defineProps({
    users: {
        type: Array,
        required: true,
    },
});

const deleteDialogOpen = ref<Record<number, boolean>>({});
const loading = ref(false);

const openDeleteDialog = (userId: number) => {
    deleteDialogOpen.value[userId] = true;
};

const closeDeleteDialog = (userId: number) => {
    deleteDialogOpen.value[userId] = false;
};

const confirmDelete = async (userId: number) => {
    loading.value = true;
    try {
        await axios.delete(`/api/users/${userId}`);
        
        // Show toast notification
        toast.success('User deleted successfully!', {
            timeout: 3000,
        });

        // Refresh page after 3 seconds
        setTimeout(() => {
            location.reload();
        }, 3000);

    } catch (error) {
        console.error('Error deleting user:', error);

        // Show error toast notification
        toast.error('An error occurred!', {
            timeout: 3000,
        });

    } finally {
        loading.value = false;
        closeDeleteDialog(userId);
    }
};

</script>

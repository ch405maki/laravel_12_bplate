<template>
    <Head title="Users Management" />
  
    <AppLayout :breadcrumbs="breadcrumbs">
      <div class="flex h-full flex-1 flex-col gap-4 rounded-xl p-4">
        <div class="flex items-center justify-end">
          <CreateUserDialog />
        </div>
        <div class="relative min-h-[100vh] flex-1 rounded-xl border">
          <UsersTable :users="users" />
        </div>
      </div>
    </AppLayout>
  </template>
  
  <script setup lang="ts">
  import { Head } from "@inertiajs/vue3";
  import AppLayout from "@/layouts/AppLayout.vue";
  import CreateUserDialog from "@/components/user/CreateUserDialog.vue";
  import UsersTable from "@/components/user/UsersTable.vue";
  
  // Define the User type
  interface User {
    id: number;
    name: string;
    email: string;
    role: string;
    status: string;
  }
  
  // Define props with explicit type
  defineProps<{
    users: User[];
  }>();
  
  const breadcrumbs = [{ title: "Users Management", href: "/users" }];
  </script>
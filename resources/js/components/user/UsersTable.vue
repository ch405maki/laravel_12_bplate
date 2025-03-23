<template>
    <Table>
      <TableHeader>
        <TableRow>
          <TableHead>User Name</TableHead>
          <TableHead>Email</TableHead>
          <TableHead>Role</TableHead>
          <TableHead>Status</TableHead>
          <TableHead class="text-right">Action</TableHead>
        </TableRow>
      </TableHeader>
      <TableBody>
        <TableRow v-for="user in users" :key="user.id">
          <TableCell class="font-medium">{{ user.name }}</TableCell>
          <TableCell>{{ user.email }}</TableCell>
          <TableCell>{{ user.role }}</TableCell>
          <TableCell>{{ user.status }}</TableCell>
          <TableCell class="text-right">
            <DeleteUserDialog :user="user" />
          </TableCell>
        </TableRow>
      </TableBody>
    </Table>
  </template>
  
  <script setup lang="ts">
  import { Table, TableBody, TableCell, TableHead, TableHeader, TableRow } from "@/components/ui/table";
  import DeleteUserDialog from "@/components/user/DeleteUserDialog.vue";
  
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
  </script>
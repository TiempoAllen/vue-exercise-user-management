<script setup>
import { ref } from "vue";

const formData = ref({
  fullName: "",
  username: "",
  email: "",
  phone: "",
});

const users = ref([]);
const selectedUser = ref(null);

const selectUser = (user) => {
  selectedUser.value = { ...user };
  formData.value = { ...user };
};

const addUser = () => {
  if (
    !formData.value.fullName ||
    !formData.value.username ||
    !formData.value.email ||
    !formData.value.phone
  ) {
    alert("Please fill in all fields");
    return;
  }

  if (!validateUser(formData.value)) {
    return;
  }

  users.value.push({
    id: users.value.length + 1,
    ...formData.value,
  });

  resetForm();
};

const updateUser = () => {
  if (!selectedUser.value) {
    alert("No user selected for update");
    return;
  }

  if (!validateUser(formData.value)) {
    return;
  }

  const index = users.value.findIndex((user) => user.id === formData.value.id);
  if (index !== -1) {
    users.value[index] = { ...formData.value };
  }

  resetForm();
};

const removeUser = (user) => {
  if (confirm(`Are you sure you want to delete ${user.fullName}?`)) {
    const index = users.value.findIndex((u) => u.id === user.id);
    if (index !== -1) {
      users.value.splice(index, 1);
    }
    resetForm();
  }
};

const validateUser = (newUser) => {
  const index = users.value.findIndex(
    (user) =>
      (user.username === newUser.username || user.email === newUser.email) &&
      user.id !== newUser.id
  );

  if (index !== -1) {
    alert("User already exists!");
    return false;
  } else {
    return true;
  }
};

const resetForm = () => {
  formData.value = {
    fullName: "",
    username: "",
    email: "",
    phone: "",
  };
  selectedUser.value = null;
};
</script>

<template>
  <div class="d-flex justify-content-center container pt-5">
    <div class="w-75">
      <h1 class="text-center">User Management</h1>
      <form
        @submit.prevent="!selectedUser ? addUser() : updateUser()"
        class="mb-5 border p-4 rounded shadow"
      >
        <div class="mb-3">
          <div class="d-flex flex-column gap-3">
            <div class="d-flex flex-column gap-2">
              <div class="d-flex flex-column gap-1">
                <label for="fullName"> Full Name </label>
                <input
                  v-model="formData.fullName"
                  type="text"
                  class="form-control"
                  id="fullName"
                  placeholder="eg. John Doe"
                />
              </div>

              <div class="d-flex flex-column gap-1">
                <label for="username"> Username </label>
                <input
                  v-model="formData.username"
                  type="text"
                  class="form-control"
                  id="username"
                  placeholder="eg. johndoe"
                />
              </div>

              <div class="d-flex flex-column gap-1">
                <label for="email"> Email Address </label>
                <input
                  v-model="formData.email"
                  type="text"
                  class="form-control"
                  id="email"
                  placeholder="eg. johndoe@gmail.com"
                />
              </div>

              <div class="d-flex flex-column gap-1">
                <label for="phone"> Phone Number </label>
                <input
                  v-model="formData.phone"
                  type="text"
                  class="form-control"
                  id="phone"
                  placeholder="eg. 09221234567"
                />
              </div>
            </div>

            <div class="d-flex justify-content-end">
              <button v-if="!selectedUser" class="btn btn-primary ms-3">
                Add User
              </button>
              <button v-if="selectedUser" class="btn btn-primary ms-3">
                Update User
              </button>
            </div>
          </div>
        </div>
      </form>

      <table class="table border shadow">
        <thead>
          <tr>
            <th scope="col">ID</th>
            <th scope="col">Full Name</th>
            <th scope="col">Username</th>
            <th scope="col">Email Address</th>
            <th scope="col">Phone Number</th>
            <th scope="col">Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in users" :key="user.id">
            <th scope="row">{{ user.id }}</th>
            <td>{{ user.fullName }}</td>
            <td>{{ user.username }}</td>
            <td>{{ user.email }}</td>
            <td>{{ user.phone }}</td>
            <td>
              <button
                @click.stop="selectUser(user)"
                class="btn btn-outline-secondary btn-sm"
              >
                Edit
              </button>
              <button
                @click.stop="removeUser(user)"
                class="btn btn-danger btn-sm ms-1"
              >
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

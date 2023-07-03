<script setup>
import CreateCourse from "@/components/Forms/CreateCourse.vue";
import Modal from "@/components/UI/Modal.vue";

const updateCourse = ref({
    id: 0,
    name: '',
    category: ''
});

const isModalVisible = ref(false);

const closeModal = () => {
    isModalVisible.value = false;
}

const { data: courses, error, pending, refresh } = await useFetch('https://localhost:7155/api/Courses', {
    key: "course-list",
    server: false,
    headers: {
        "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoiZGF2aXplaXJhIiwicm9sZSI6ImVtcGxveWVlIiwibmJmIjoxNjg4MjUwMzg5LCJleHAiOjE2ODgyNTc1ODksImlhdCI6MTY4ODI1MDM4OX0.5Z9Gl7zpQpFpYEJV4304XkWPbnOQB_is8E9373EN8dA"
    }
})

const onRemove = async (id) => {
    await useFetch(`https://127.0.0.1:7155/api/Courses/${id}`, {
        method: "DELETE",
        headers: {
            "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoiZGF2aXplaXJhIiwicm9sZSI6ImVtcGxveWVlIiwibmJmIjoxNjg4MjUwMzg5LCJleHAiOjE2ODgyNTc1ODksImlhdCI6MTY4ODI1MDM4OX0.5Z9Gl7zpQpFpYEJV4304XkWPbnOQB_is8E9373EN8dA"
        }
    });

    refresh();
}

const onUpdateCourse = async () => {
    if (updateCourse.value.id == 0) return;

    await useFetch(`https://localhost:7155/api/Courses/${updateCourse.value.id}`, {
        method: "PUT",
        body: {
            id: updateCourse.value.id,
            name: updateCourse.value.name,
            category: updateCourse.value.category
        },
        headers: {
            "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoiZGF2aXplaXJhIiwicm9sZSI6ImVtcGxveWVlIiwibmJmIjoxNjg4MjUwMzg5LCJleHAiOjE2ODgyNTc1ODksImlhdCI6MTY4ODI1MDM4OX0.5Z9Gl7zpQpFpYEJV4304XkWPbnOQB_is8E9373EN8dA"
        }
    });

    updateCourse.value.category = '';
    updateCourse.value.id = 0;
    updateCourse.value.name = '';

    isModalVisible.value = false;
    refresh();
}

const handleOpenEdit = (course) => {
    isModalVisible.value = true;
    updateCourse.value.id = course.id;
    updateCourse.value.name = course.name;
    updateCourse.value.category = course.category;
}

</script>

<template>
    <div class="bg-slate-900 h-screen p-4 flex flex-col">
        <h2 class="text-gray-200 text-4xl font-bold">Courses</h2>

        <CreateCourse />
        <Modal v-show="isModalVisible" @close="closeModal">
            <template #header>
                <h2 class="text-xl text-cyan-700 font-bold uppercase">Atualizar Curso</h2>
            </template>
            <template #body>
                <section class="bg-slate-600 rounded-lg my-4 p-4">
                    <div class="grid gap-6 mt-4 mb-6 md:grid-cols-2">
                        <div class="col-span-2">
                            <label for="first_name" class="block mb-2 text-sm font-medium text-white">Course name</label>
                            <input type="text" id="first_name" v-model="updateCourse.name"
                                class="border text-sm rounded-lg block w-full p-2.5 bg-gray-700 border-gray-600 placeholder-gray-400 text-white focus:ring-blue-500 focus:border-blue-500"
                                placeholder="Create a CRUD with Nuxt 3" required>
                        </div>
                        <div class="col-span-2">
                            <label for="countries" class="block mb-2 text-sm font-medium text-white">Categoria</label>
                            <select id="countries" v-model="updateCourse.category"
                                class="border text-sm rounded-lg block w-full p-2.5 bg-gray-700 border-gray-600 placeholder-gray-400 text-white focus:ring-blue-500 focus:border-blue-500">
                                <option selected disabled>Categoria</option>
                                <option value="front-end">Front-end</option>
                                <option value="back-end">Back-end</option>
                                <option value="fullstack">Fullstack</option>
                                <option value="tests">Q&A</option>
                            </select>
                        </div>
                        <div class="flex justify-end col-span-2">
                            <button type="button" @click="onUpdateCourse"
                                class="border focus:ring-4 focus:outline-none font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2 border-purple-400 text-purple-400 hover:text-white hover:bg-purple-500 focus:ring-purple-900">
                                Salvar
                            </button>
                        </div>
                    </div>
                </section>
            </template>
        </Modal>
        <div v-if="error" class="text-lg text-white">Error {{ error }}</div>
        <div v-else-if="pending" class="text-lg text-blue-500">Loading....</div>
        <section v-else>
            <div class="relative overflow-x-auto shadow-md sm:rounded-lg">
                <table class="w-full text-sm text-left text-gray-400">
                    <thead class="text-xs text-gray-400 uppercase bg-gray-700">
                        <tr>
                            <th scope="col" class="px-6 py-3">Id: </th>
                            <th scope="col" class="px-6 py-3">Name: </th>
                            <th scope="col" class="px-6 py-3">Category: </th>
                            <th scope="col" class="px-6 py-3">
                                <span class="sr-only">Edit</span>
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(course, idx) in courses" :class="idx % 2 == 0 ? 'bg-gray-800' : 'bg-gray-700'"
                            class="border-b  border-gray-700 hover:bg-gray-600" :key="course._id">
                            <td class="px-6 py-4">{{ course.id }}</td>
                            <td scope="col" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">{{
                                course.name }}</td>
                            <td class="px-6 py-4">{{ course.category }}</td>
                            <td class="px-6 py-4 text-right">
                                <button type="button" @click="handleOpenEdit(course)"
                                    class="font-medium text-blue-500 hover:underline">
                                    Edit
                                </button>
                                |
                                <button @click="onRemove(course.id)" class="font-medium text-red-500 hover:underline">
                                    Remover
                                </button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </section>
    </div>
</template>

<style>
.btn {
    color: white;
    background: #4AAE9B;
    border: 1px solid #4AAE9B;
    border-radius: 2px;
}
</style>
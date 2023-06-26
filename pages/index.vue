<script setup>
import CreateCourse from "@/components/Forms/CreateCourse.vue";

const { data: courses, pending, error, refresh } = await useFetch(() => "http://localhost:8080/api/courses", {
    key: "course-list"
});

const onRemove = async (id) => {
    await useFetch(`http://localhost:8080/api/courses/${id}`, {
        method: "DELETE",

    });

    refresh();
}

</script>

<template>
    <div class="bg-slate-900 h-screen p-4 flex flex-col">
        <h2 class="text-gray-200 text-4xl font-bold">Courses</h2>

        <CreateCourse />

        <section>
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
                        <tr class="border-b bg-gray-800 border-gray-700 hover:bg-gray-600" v-for="course in courses"
                            :key="course._id">
                            <td class="px-6 py-4">{{ course._id }}</td>
                            <td scope="col" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">{{
                                course.name }}</td>
                            <td class="px-6 py-4">{{ course.category }}</td>
                            <td class="px-6 py-4 text-right">
                                <a href="#" class="font-medium text-blue-500 hover:underline">Edit</a>
                                |
                                <button @click="onRemove(course._id)" class="font-medium text-red-500 hover:underline">
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
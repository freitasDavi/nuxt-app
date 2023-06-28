<script setup>
const newCourse = ref({
    name: '',
    category: ''
});
// const newCourseName = useState('newCourseName', () => '');
// const newCourseCategory = useState('newCoursCategory', () => '');

const onClear = () => {

    newCourse.value.name = '';
    newCourse.value.category = '';
}

const onCreateNewCourse = async () => {
    await useFetch('https://127.0.0.1:7155/api/Courses', {
        method: "POST",
        server: true,
        body: {
            name: newCourse.value.name,
            category: newCourse.value.category
        },
    });

    refreshNuxtData('course-list');

    onClear();
}

</script>

<template>
    <section class="bg-slate-600 rounded-lg my-4 p-4">
        <h2 class="text-xl text-gray-200 font-bold">Create new Course</h2>
        <div class="grid gap-6 mt-4 mb-6 md:grid-cols-2">
            <div>
                <label for="first_name" class="block mb-2 text-sm font-medium text-white">Course name</label>
                <input type="text" id="first_name" v-model="newCourse.name"
                    class="border text-sm rounded-lg block w-full p-2.5 bg-gray-700 border-gray-600 placeholder-gray-400 text-white focus:ring-blue-500 focus:border-blue-500"
                    placeholder="Create a CRUD with Nuxt 3" required>
            </div>
            <div>
                <label for="countries" class="block mb-2 text-sm font-medium text-white">Categoria</label>
                <select id="countries" v-model="newCourse.category"
                    class="border text-sm rounded-lg block w-full p-2.5 bg-gray-700 border-gray-600 placeholder-gray-400 text-white focus:ring-blue-500 focus:border-blue-500">
                    <option selected disabled>Categoria</option>
                    <option value="front-end">Front-end</option>
                    <option value="back-end">Back-end</option>
                    <option value="fullstack">Fullstack</option>
                    <option value="tests">Q&A</option>
                </select>
            </div>
            <div class="flex justify-end col-span-2">
                <button type="button" @click="onClear"
                    class="text-red-700 hover:text-white border border-red-700 hover:bg-red-800 focus:ring-4 focus:outline-none focus:ring-red-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2 dark:border-red-500 dark:text-red-500 dark:hover:text-white dark:hover:bg-red-600 dark:focus:ring-red-900">
                    Apagar
                </button>
                <button type="button" @click="onCreateNewCourse"
                    class="border focus:ring-4 focus:outline-none font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2 border-purple-400 text-purple-400 hover:text-white hover:bg-purple-500 focus:ring-purple-900">
                    Salvar
                </button>
            </div>
        </div>
    </section>
</template>
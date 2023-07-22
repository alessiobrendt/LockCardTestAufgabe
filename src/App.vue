<script>
import axios from "axios";
import Modal from "./components/Modal.vue";
import { useToast } from "vue-toastification";
export default {
  setup() {
    const toast = useToast();
    return { toast };
  },
  name: "App",
  components: {
    Modal,
  },

  data() {
    return {
      form: {
        title: "",
        content: "",
        author: "",
        time: "",
      },
      blogs: [],
    };
  },
  methods: {
    async loadContent() {
      await axios
        .get("http://localhost:3000/posts")
        .then(
          (response) => (
            (this.blogs = response.data), console.log(response.data)
          )
        );
    },
    async loadEdit(title, content, author, time) {
      this.form.title = title;
      this.form.content = content;
      this.form.author = author;
      this.form.time = time;
    },
    async editBlog(id) {
      await axios
        .put("http://localhost:3000/posts/" + id, this.form)
        .then((response) => console.log(response.data));
      this.toast.success("Der Post wurde erfolgreich bearbeitet!");
      this.loadContent();
    },
    async deleteBlog(id) {
      await axios
        .delete("http://localhost:3000/posts/" + id)
        .then((response) => console.log(response.data));
      this.toast.success("Der Post wurde erfolgreich gelöscht!");
      this.loadContent();
    },
    async addBlog() {
      this.form.time = new Date().toLocaleString();
      await axios
        .post("http://localhost:3000/posts", this.form)
        .then((response) => console.log(response.data));
      this.toast.success("Der Post wurde erfolgreich hinzugefügt!");
      this.loadContent();
    },
  },
  mounted() {
    this.loadContent();
  },
};
</script>

<template>
  <div>
    <div class="bg-gray-100">
      <p class="py-1 text-sm font-bold text-center text-black">
        TESTAUFGABE ALESSIO BRENDT
      </p>
    </div>
    <div class="px-[15%] py-8 flex flex-row align-middle">
      <div><img src="lockcard.avif" /></div>
      <p class="text-black font-bold py-4 px-[1rem] text-xl">HOME</p>
      <p class="font-bold py-4 text-xl text-red-500 px-[1rem]">BLOG</p>
    </div>

    <div
      class="bg-cover bg-no-repeat bg-center h-[50%] shadow-lg"
      style="background-image: url('cow.jpg')"
    >
      <div class="h-[40rem] flex flex-row algin-middle items-center pl-[20rem]">
        <div class="items-center justify-center algin-middle">
          <h1 class="py-8 text-4xl font-bold text-white">
            DER BLOG MIT DEM<br />EXTRA MUH!
          </h1>
          <a href="#blog"
            ><button
              class="px-12 py-3 text-xl font-bold text-center text-black bg-white rounded-full shadow-lg hover:bg-gray-200"
            >
              Zum Blog
            </button></a
          >
        </div>
      </div>
    </div>

    <div class="py-12" id="blog">
      <h1 class="text-5xl font-medium text-center">BLOGS</h1>

      <div class="justify-center text-center align-middle">
        <Modal name="fa-solid fa-plus" color="text-blue-600">
          <p>Edit Blog</p>
          <br />

          <label class="block text-sm font-medium text-gray-700">Title</label>
          <div class="mt-1">
            <input
              type="text"
              v-model="form.title"
              class="block w-full px-2 py-1 bg-gray-100 border-gray-700 rounded-md shadow-sm focus:border-sky-500 focus:ring-sky-500 sm:text-sm"
            />
          </div>

          <label class="block text-sm font-medium text-gray-700">Author</label>
          <div class="mt-1">
            <input
              type="text"
              v-model="form.author"
              class="block w-full px-2 py-1 bg-gray-200 border-gray-700 rounded-md shadow-sm w-ful focus:border-sky-500 focus:ring-sky-500 sm:text-sm"
            />
          </div>

          <label class="block text-sm font-medium text-gray-700">Content</label>
          <div class="mt-1">
            <textarea
              rows="20"
              v-model="form.content"
              class="block w-full px-2 py-1 bg-gray-200 border-gray-700 rounded-md shadow-sm focus:border-sky-500 focus:ring-sky-500 sm:text-sm"
            />
          </div>
          <div class="mt-5 sm:mt-6">
            <button
              type="button"
              v-on:click="addBlog()"
              class="inline-flex justify-center w-full px-4 py-2 text-base font-medium text-white border border-transparent rounded-md shadow-sm bg-sky-600 hover:bg-sky-700 focus:outline-none focus:ring-2 focus:ring-sky-500 focus:ring-offset-2 sm:text-sm"
            >
              Create
            </button>
          </div>
        </Modal>
      </div>

      <div
        class="flex flex-col items-center justify-center py-12 algin-middle"
        v-for="blog in this.blogs"
        :key="blog.id"
      >
        <div
          class="bg-white shadow-[0_35px_60px_-15px_rgba(0,0,0,0.3)] rounded-xl w-[50%] py-6 px-6"
        >
          <div class="flex flex-row justify-end">
            <div class="flex items-center justify-center align-middle">
              <Modal
                name="fa-solid fa-pen"
                color="text-blue-600"
                @isModalOpen="
                  loadEdit(blog.title, blog.content, blog.author, blog.time)
                "
              >
                <p>Edit Blog</p>
                <br />
                <label class="block text-sm font-medium text-gray-700"
                  >Title</label
                >
                <div class="mt-1">
                  <input
                    type="text"
                    v-model="form.title"
                    class="block w-full px-2 py-1 bg-gray-100 border-gray-700 rounded-md shadow-sm focus:border-sky-500 focus:ring-sky-500 sm:text-sm"
                  />
                </div>

                <label class="block text-sm font-medium text-gray-700"
                  >Author</label
                >
                <div class="mt-1">
                  <input
                    type="text"
                    v-model="form.author"
                    class="block w-full px-2 py-1 bg-gray-200 border-gray-700 rounded-md shadow-sm w-ful focus:border-sky-500 focus:ring-sky-500 sm:text-sm"
                  />
                </div>

                <label class="block text-sm font-medium text-gray-700"
                  >Content</label
                >
                <div class="mt-1">
                  <textarea
                    rows="20"
                    v-model="form.content"
                    class="block w-full px-2 py-1 bg-gray-200 border-gray-700 rounded-md shadow-sm focus:border-sky-500 focus:ring-sky-500 sm:text-sm"
                  />
                </div>

                <div class="mt-5 sm:mt-6">
                  <button
                    type="button"
                    v-on:click="editBlog(blog.id)"
                    class="inline-flex justify-center w-full px-4 py-2 text-base font-medium text-white border border-transparent rounded-md shadow-sm bg-sky-600 hover:bg-sky-700 focus:outline-none focus:ring-2 focus:ring-sky-500 focus:ring-offset-2 sm:text-sm"
                  >
                    Save
                  </button>
                </div>
              </Modal>

              <Modal name="fa-solid fa-trash" color="text-red-600">
                <p>Delete Blog</p>
                <div class="mt-5 sm:mt-6">
                  <button
                    type="button"
                    v-on:click="deleteBlog(blog.id)"
                    class="inline-flex justify-center w-full px-4 py-2 text-base font-medium text-white bg-red-600 border border-transparent rounded-md shadow-sm hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-red-500 focus:ring-offset-2 sm:text-sm"
                  >
                    Delete
                  </button>
                </div>
              </Modal>
            </div>
          </div>
          <div class="flex">
            <h1 class="flex-1 w-64 text-2xl">{{ blog.title }}</h1>

            <div class="flex flex-row justify-end">
              <div class="flex items-center justify-center align-middle">
                <p class="px-4 text-sm">{{ blog.time }}</p>
              </div>
            </div>
          </div>

          <div class="py-4">
            <p class="text">{{ blog.content }}</p>
          </div>

          <div class="flex flex-row justify-end">
            <div class="flex items-center justify-center align-middle">
              <p class="px-4">Von: {{ blog.author }}</p>
              <img src="pb.png" class="w-12 h-12 rounded-full" />
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="py-12 bg-gray-100"></div>
  </div>
</template>

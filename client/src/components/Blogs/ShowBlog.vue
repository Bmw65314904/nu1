<template>
    <div class="blog-container">
        <h1 class="centered-title">Show Blog</h1>
        <template v-if="isLoading">
            <p>Loading...</p>
        </template>
        <template v-else-if="error">
            <p>{{ errorMessage }}</p>
            <button v-on:click="navigateTo('/blogs')">กลับ</button>
        </template>
        <template v-else-if="blog">
            <p>id: {{ blog.id }}</p>
            <p>title: {{ blog.title }}</p>
            <p>content: <span v-html="blog.content"></span></p>
            <p>category: {{ blog.category }}</p>
            <p>status: {{ blog.status }}</p>
            <img v-if="blog.image" :src="blog.image" alt="Blog Image" class="blog-image" />
            <p>
                <button v-on:click="navigateTo('/blog/edit/' + blog.id)">แก้ไข blog</button>
                <button v-on:click="navigateTo('/blogs')">กลับ</button>
            </p>
        </template>
        <template v-else>
            <p>Blog not found.</p>
            <button v-on:click="navigateTo('/blogs')">กลับ</button>
        </template>
    </div>
</template>

<script>
import BlogsService from '@/services/BlogsService'

export default {
    data() {
        return {
            blog: null,
            isLoading: true,
            error: false,
            errorMessage: ''
        }
    },
    async created() {
        try {
            let blogId = this.$route.params.blogId
            const response = await BlogsService.show(blogId)
            this.blog = response.data
            console.log(this.blog) // ตรวจสอบข้อมูลที่ได้
        } catch (error) {
            this.error = true
            this.errorMessage = "ไม่สามารถโหลดข้อมูลบล็อกได้"
            console.log(error)
        } finally {
            this.isLoading = false
        }
    },
    methods: {
        navigateTo(route) {
            this.$router.push(route)
        },
    }
}
</script>

<style scoped>
.blog-container {
    padding: 20px;
    margin: 20px;
    border: 1px solid #62ff00;
    border-radius: 5px;
    max-width: 800px;
    background-color: #fffdfd;
}

.centered-title {
    text-align: center; /* จัดกลาง */
    margin-bottom: 20px; /* เพิ่มระยะห่างด้านล่าง */
}

.blog-image {
    max-width: 100%;
    height: auto;
    margin-top: 10px;
}

button {
    margin-top: 10px;
}
</style>

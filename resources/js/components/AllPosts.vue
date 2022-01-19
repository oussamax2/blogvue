<template>
    <div>
        <h3 class="text-center">All Posts</h3><br/>

        <table class="table table-bordered">
            <thead>
            <tr>
                <th>ID</th>
                <th>Title</th>
                <th>Description</th>
                <th>Created At</th>
                <th>Updated At</th>
                <th>Actions</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="post in posts.data" :key="post.id">
                <td>{{ post.id }}</td>
                <td>{{ post.title }}</td>
                <td>{{ post.description }}</td>
                <td>{{ post.created_at }}</td>
                <td>{{ post.updated_at }}</td>
                <td>
                    <div class="btn-group" role="group">
                        <router-link :to="{name: 'edit', params: { id: post.id }}" class="btn btn-primary">Edit
                        </router-link>
                        <button class="btn btn-danger"  @click="deletePost(post.id)">Delete</button>
                    </div>
                </td>
            </tr>
            </tbody>
        </table>


  <pagination align="center" :data="posts" @pagination-change-page="getResults"></pagination>
    </div>



</template>

<script>
    export default {
        data() {
            return {
                posts: [],

            }
        },
        created() {
     this.getResults();
        },
        methods: {
  getResults(page) {
            if (typeof page === "undefined") {
        page = 1;
      }
      this.axios
        .get("http://localhost:8000/api/posts?page=" + page)
        .then(response => {
          this.posts = response.data;
          console.log(posts);
        });
        },
            deletePost(id){

           this.$swal.fire({
                    title: "Delete this comment?",
                    text: "Are you sure? You won't be able to revert this!",
                    type: "error",
                    icon: 'error',
                    showCancelButton: true,
                    confirmButtonColor: "#3085d6",
                    confirmButtonText: "Yes, Delete it!",
                    closeOnConfirm: true
                }
                ).then((result) => {
  if (result.isConfirmed) {
    this.axios
                    .delete(`http://localhost:8000/api/post/delete/${id}`)
                    .then(response => {
                        let i = this.posts.map(item => item.id).indexOf(id); // find index of your object
                        this.posts.splice(i, 1)
                    });
  }
});
        }
        },
    computed: {
      rows() {
        return this.posts.length
      }
    }
    }
</script>

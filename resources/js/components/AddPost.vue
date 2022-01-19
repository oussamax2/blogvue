<template>
    <div>
        <h3 class="text-center">Add Post</h3>
        <div class="row">
            <div class="col-md-6">
                <form @submit.prevent="addPost">
                    <div class="form-group">
                        <label>Title</label>
                        <input type="text" class="form-control" v-model="post.title">
                    </div>
                    <div class="form-group">
                        <label>Description</label>

                         <select name="category_id" @change="onChange($event)" class="form-control" v-model="post.description">
                            <option>--- Select Category ---</option>
                            <option value="1">USA</option>
                            <option value="2">India</option>
                            <option value="3">Bangladesh</option>
                            <option value="4">England</option>

                        </select>
                    </div>
                    <button type="submit" class="btn btn-primary">Add Post</button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                post: {}
            }
        },
        methods: {
            addPost() {

                this.axios
                    .post('http://localhost:8000/api/post/add', this.post)
                    .then(response => (
                        this.$router.push({name: 'home'})
                        // console.log(response.data)
                    ))
                    .catch(error => console.log(error))

               .finally(() => this.loading = false)
            }
            ,
         onChange(event) {
              var optionValue = event.target.value;
              var optionText = event.target.options[event.target.options.selectedIndex].text;
          //  this.post.description = optionText;
              console.log(optionText);
              console.log(optionValue);}

        }

    }
</script>

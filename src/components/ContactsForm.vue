<script>

import axios from 'axios';

export default {
    name: "ContactsForm",
    data() {
        return {
            name: '',
            email: '',
            message: '',
            baseUrl: 'http://127.0.0.1:8000',
            success: false,
            errors: {}
        }
    },
    methods: {
        sendForm() {

            const data = {
                name: this.name,
                email: this.email,
                message: this.message
            }

            axios.post(`${this.baseUrl}/api/contacts`, data)
                .then(res => {

                    this.success = res.data.success

                    // console.log(this.success)

                    if (this.success) {
                        // SE è TRUE
                        this.name = '';
                        this.email = '';
                        this.message = '';
                    } else {
                        this.errors = res.data.errors
                    }
                })
        }
    },
}
</script>

<template>
    <h1 class="text-center mt-3 text-uppercase">Contact Form</h1>
    <div>
        <form @submit.prevent="sendForm()">
            <div class="mb-3">
                <label for="" class="form-label">Name</label>
                <input v-model="name" type="text" class="form-control" name="name" id="" aria-describedby="helpId"
                    placeholder="Enter your Name">
            </div>
            <div class="mb-3">
                <label for="" class="form-label">Email</label>
                <input v-model="email" type="email" class="form-control" name="email" id="" aria-describedby="helpId"
                    placeholder="Enter Email">
            </div>
            <div class="mb-3">
                <label for="" class="form-label">Message</label>
                <textarea v-model="message" class="form-control" name="message" id="" cols="30" rows="10"></textarea>
            </div>
            <button type="submit" class="btn btn-primary">Send</button>
        </form>
    </div>
</template>

<style lang="scss" scoped></style>

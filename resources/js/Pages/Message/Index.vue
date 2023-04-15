<template>
    <div class="w-1/2 mx-auto py-6">
        <div class="flex mb-4 w-full">
            <div class="w-full">
                <input v-model="body"
                       class="w-full rounded-full border border-teal-300 focus:border focus:border-teal-200"
                       type="text" placeholder="Your message"
                >
            </div>
            <div class="ml-1">
                <a href="#" @click.prevent="store"
                   class="flex text-center items-center px-2 h-full text-teal-300 rounded-lg border border-teal-300
                   hover:bg-teal-300 hover:text-white transition duration-200"
                > Send
                </a>
            </div>
        </div>
       <div v-if="messages.length > 0" class="mb-4">
           <h3>Messages</h3>
           <div class="pt-4">
               <div class="text-sm pb-4 mb-4 border-b border-gray-300" v-for="message in messages">
                    <p>{{ message.id }}</p>
                    <p>{{ message.body }}</p>
                    <p class="text-right">{{ message.time }}</p>
               </div>
           </div>
       </div>
    </div>
</template>

<script>
export default {
    name: "Index",

    data() {
        return {
            body: "",
        }
    },

    props: [
        "messages"
    ],

    methods: {
        store() {
            axios.post('/messages', {body: this.body})
                .then(res => {
                    this.messages.unshift(res.data)
                    this.body = ""
                })
        }
    },

    created() {
        window.Echo.channel("store_message")
            .listen(".store_message", res => {
                console.log(res);
            })
    }
}
</script>

<style scoped>

</style>

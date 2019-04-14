<template>
    <div>
        <button @click="sendmsg">我是弟弟</button>
        <p></p>
    </div>

</template>

<script>
    import bus from '@/components/bus.vue'
    export default {
        name: "child1",
        methods:{
            sendmsg(){
                bus.$emit('msg','弟弟叫你写作业')

            },

        }
    }
</script>

<style scoped>

</style>

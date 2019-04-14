<template>
    <div>
        <button>我是哥哥</button>
        <p>{{recMsg}}</p>
    </div>

</template>

<script>
    import bus from '@/components/bus.vue'
    export default {
        name: "child2",
        methods:{

        },
        mounted() {
            let that = this;
            bus.$on('msg',function (msg) {
                that.recMsg = msg;
                console.log(that.recMsg)
            });

        },
        data(){
            return{
                recMsg:''
            }
        }
    }
</script>

<style scoped>

</style>

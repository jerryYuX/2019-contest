<template>
    <div id="home">
        <div class="main_box">




        </div>
        <span>游戏状态：{{state}}</span><br/>
        <button @click="next_game" v-if="chapter == 1">下一关</button>
        <button @click="last_game" v-if="chapter == 2">上一关</button>
        <br/>
        <span>难度选择：<button @click="change_difficult(1)" >简单</button>
        <button @click="change_difficult(2)" >中等</button>
        <button @click="change_difficult(3)" >困难</button></span>
        <br />
        <span>当前难度：{{diffc}}</span>
        <br/>
        <span>当前步数：{{steps}}</span>
        <span>限制步数：{{limit_steps}}</span>

    </div>

</template>

<script>
    // @ is an alias to /src
    // import HelloWorld from '@/components/HelloWorld.vue'


    export default {
        name: 'home',
        components: {},
        data() {
            return {
                gameArr: [
                    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 3, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 2, 0, 0, 0, 0, 0],
                    [0, 3, 4, 2, 4, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 1, 4, 2, 4, 3, 0],
                    [0, 0, 0, 0, 4, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 2, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 3, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
                ],
                gameArr1:[
                    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 3, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 2, 0, 0, 0, 0, 0],
                    [0, 3, 4, 2, 4, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 1, 4, 2, 4, 3, 0],
                    [0, 0, 0, 0, 4, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 2, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 3, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
                ],
                gameArr2:[
                    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 4, 4, 4, 4, 4, 4, 4, 4, 0],
                    [0, 4, 3, 0, 4, 2, 4, 4, 4, 0],
                    [0, 4, 4, 4, 4, 4, 4, 4, 4, 0],
                    [0, 4, 4, 4, 1, 4, 2, 4, 4, 0],
                    [0, 0, 0, 0, 4, 4, 4, 4, 4, 0],
                    [0, 4, 4, 4, 2, 4, 4, 4, 4, 0],
                    [0, 4, 3, 4, 4, 4, 4, 0, 0, 0],
                    [0, 4, 4, 4, 4, 4, 4, 4, 3, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
                ],
                wall: 0,
                people: 1,
                box: 2,
                aim: 3,
                blank:4,
                people_loc:{
                    x:4,
                    y:4
                },
                state:'进行中',
                chapter:1,
                steps: 0,
                limit_steps:40,
                diffc:'简单'
            }
        },
        methods:{
            up(node){
                this.steps++;
                if(this.gameArr[node.x-1][node.y] == 4){
                    this.gameArr[node.x-1][node.y] = 1;
                    this.gameArr[node.x][node.y] = 4;
                    this.people_loc.x = this.people_loc.x - 1;


                }else
                if(this.gameArr[node.x-1][node.y] == 3){

                }else
                if(this.gameArr[node.x-1][node.y] == 2){
                    if(this.gameArr[node.x-2][node.y] == 4||this.gameArr[node.x-2][node.y] == 3){
                        this.gameArr[node.x-2][node.y] = 2;
                        this.gameArr[node.x-1][node.y] = 1;
                        this.gameArr[node.x][node.y] = 4;
                        this.people_loc.x = this.people_loc.x - 1;

                    }

                }else
                if(this.gameArr[node.x-1][node.y] == 0){

                }
                if(this.isFail()){
                    this.$message.error('失败！');
                    this.init();
                }

                if(this.isSuccess()){
                    this.$message({
                        message: '成功通过当前关卡',
                        type: 'success'
                    });
                    if(this.chapter == 2){
                        this.$message({
                            message:'后续关卡待添加！'
                        })
                        this.init();
                    }else{
                        this.next_game();
                    }
                }
                this.draw(this.gameArr);


            },
            down(node){
                this.steps++;
                if(this.gameArr[node.x+1][node.y] == 4){
                    this.gameArr[node.x+1][node.y] = 1;
                    this.gameArr[node.x][node.y] = 4;
                    this.people_loc.x = this.people_loc.x + 1;


                }else
                if(this.gameArr[node.x+1][node.y] == 3){

                }else
                if(this.gameArr[node.x+1][node.y] == 2){
                    if(this.gameArr[node.x+2][node.y] == 4||this.gameArr[node.x+2][node.y] == 3){
                        this.gameArr[node.x+2][node.y] = 2;
                        this.gameArr[node.x+1][node.y] = 1;
                        this.gameArr[node.x][node.y] = 4;
                        this.people_loc.x = this.people_loc.x + 1;

                    }

                }else
                if(this.gameArr[node.x+1][node.y] == 0){

                }
                if(this.isFail()){
                    this.$message.error('失败！')
                    this.init();
                }

                if(this.isSuccess()){
                    this.$message({
                        message: '成功通过当前关卡',
                        type: 'success'
                    });
                    if(this.chapter == 2){
                        this.$message({
                            message:'后续关卡待添加！'
                        })
                        this.init();
                    }else{
                        this.next_game();
                    }
                }
                this.draw(this.gameArr);

            },
            left(node){
                this.steps++;
                if(this.gameArr[node.x][node.y-1] == 4){
                    this.gameArr[node.x][node.y-1] = 1;
                    this.gameArr[node.x][node.y] = 4;
                    this.people_loc.y = this.people_loc.y - 1;


                }else
                if(this.gameArr[node.x][node.y-1] == 3){

                }else
                if(this.gameArr[node.x][node.y-1] == 2){
                    if(this.gameArr[node.x][node.y -2] == 4||this.gameArr[node.x][node.y - 2] == 3){
                        this.gameArr[node.x][node.y - 2] = 2;
                        this.gameArr[node.x][node.y - 1] = 1;
                        this.gameArr[node.x][node.y] = 4;
                        this.people_loc.y = this.people_loc.y - 1;

                    }

                }else
                if(this.gameArr[node.x][node.y-1] == 0){

                }
                if(this.isFail()){
                    this.$message.error('失败！')
                    this.init();
                }

                if(this.isSuccess()){
                    this.$message({
                        message: '成功通过当前关卡',
                        type: 'success'
                    });
                    if(this.chapter == 2){
                        this.$message({
                            message:'后续关卡待添加！'
                        })
                        this.init();
                    }else{
                        this.next_game();
                    }
                }
                this.draw(this.gameArr);

            },
            right(node){
                this.steps++;
                if(this.gameArr[node.x][node.y+1] == 4){
                    this.gameArr[node.x][node.y+1] = 1;
                    this.gameArr[node.x][node.y] = 4;
                    this.people_loc.y = this.people_loc.y + 1;


                }else
                if(this.gameArr[node.x][node.y+1] == 3){

                }else
                if(this.gameArr[node.x][node.y+1] == 2){
                    if(this.gameArr[node.x][node.y +2] == 4||this.gameArr[node.x][node.y + 2] == 3){
                        this.gameArr[node.x][node.y + 2] = 2;
                        this.gameArr[node.x][node.y + 1] = 1;
                        this.gameArr[node.x][node.y] = 4;
                        this.people_loc.y = this.people_loc.y + 1;

                    }

                }else
                if(this.gameArr[node.x][node.y+1] == 0){

                }
                if(this.isFail()){
                    this.$message.error('失败！')
                    this.init();
                }

                if(this.isSuccess()){
                    this.$message({
                        message: '成功通过当前关卡',
                        type: 'success'
                    });
                    if(this.chapter == 2){
                        this.$message({
                            message:'后续关卡待添加！'
                        })
                        this.init();
                    }else{
                        this.next_game();
                    }

                }
                this.draw(this.gameArr);

            },
            isSuccess(){

                for(let i = 0;i<this.gameArr.length;i++){
                    if(this.gameArr[i].indexOf(3) != -1){
                        return false
                    }
                }
                return true;
            },
            isFail(){
                if(this.steps > this.limit_steps){
                    return true;
                }
            },
            creat_el(name){
                var div = document.createElement('div');
                div.className = name;
                return div;

            },

            draw(arr){
                document.querySelector('.main_box').innerHTML = null;
                for(let i = 0;i<arr.length;i++){
                    document.querySelector('.main_box').append(this.creat_el('span'));
                    for(let j = 0;j<arr[i].length;j++){
                        if(arr[i][j] ==0){
                            document.querySelectorAll('.span')[i].append(this.creat_el('wall'));
                        }else if(arr[i][j] ==1){
                            document.querySelectorAll('.span')[i].append(this.creat_el('people'));
                        }else if(arr[i][j] ==2){
                            document.querySelectorAll('.span')[i].append(this.creat_el('box'));
                        }else if(arr[i][j] ==3){
                            document.querySelectorAll('.span')[i].append(this.creat_el('aim'));
                        }else if(arr[i][j] ==4){
                            document.querySelectorAll('.span')[i].append(this.creat_el('blank'));
                        }


                    }
                }
            },
            next_game(){

                this.people_loc={
                    x:4,
                    y:4
                }
                this.steps = 0;
                console.log(this.gameArr);
                this.gameArr = this.gameArr2.concat();
                this.state = '进行中';
                this.$nextTick(function () {
                    this.draw(this.gameArr);
                })


                this.chapter =2;


            },
            last_game(){
                this.steps = 0;
                this.gameArr = this.gameArr1.concat();
                this.state = '进行中'
                this.$nextTick(function () {
                    this.draw(this.gameArr);
                })
                this.people_loc={
                    x:4,
                    y:4
                }
                this.chapter =1;


            },
            init(){
                this.steps = 0;
                this.gameArr = this.gameArr1.concat();
                this.state = '进行中'
                this.$nextTick(function () {
                    this.draw(this.gameArr);
                })
                this.people_loc={
                    x:4,
                    y:4
                }
                this.chapter =1;
                this.$router.go(0);
            },
            change_difficult(num){
                if(num == 1){
                    this.diffc = '简单';
                    this.limit_steps = 40;
                }
                if(num == 2){
                    this.diffc = '中等';
                    this.limit_steps = 25;

                }
                if(num == 3){
                    this.diffc = '困难';
                    this.limit_steps = 15;

                }

            }

        },
        mounted() {
            this.draw(this.gameArr);
            let that = this;
            let people_loc = this.people_loc;
            document.onkeydown = function (e) {
                let keyNum = window.event ? e.keyCode : e.which;  //获取被按下的键值
                // alert(keyNum);
                let num = Number(keyNum);
                switch (num) {
                    case 37: that.left(that.people_loc);
                        break;
                    case 38: that.up(that.people_loc);
                        break;
                    case 39:   that.right(that.people_loc);
                        break;
                    case 40: that.down(that.people_loc);
                        break;

                }

            };

        }
    }
</script>
<style>
    .main_box{
        width: 200px;
        height: 200px;
        padding: 0;
        margin: 0 auto;
    }
    .span{
        width: 200px;
        height: 20px;
        padding: 0;
        margin: 0;
    }
    .span div{
        width: 20px;
        height: 20px;
        margin: 0;
        padding: 0;
        float: left;
    }
    .wall{
        background-color: #2c3e50;
        background-image: url("../assets/wall.png");
        background-size: 100%;

    }
    .people{
        background-color: antiquewhite;
        background-image: url("../assets/people.png");
        background-size: 100%;
    }
    .box{
        background-color: burlywood;
        background-image: url("../assets/box.png");
        background-size: 100%;
    }
    .aim{
        background-color: #42b983;
        background-image: url("../assets/aim.png");
        background-size: 100%;
    }
    .blank{
        background-color: aliceblue;
        background-image: url("../assets/blank.png");
    }

</style>

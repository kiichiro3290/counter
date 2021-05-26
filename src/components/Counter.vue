<template>
    <div id="counter">
        <p class="cnt">Counter</p>
        <div class="number">{{num}}</div>
        <button class="plus" @click="addOne">＋</button>
        <button class="minus" @click="substractOne">－</button>
        <p ><button class="reset" @click="reset">reset</button></p>
    </div>
</template>

<script>
    export default{
        name: "Counter",
        data(){
            return{
                num:0
            }
        },
        methods: {
            addOne: function(){
                this.num = this.num +1;
            },
            substractOne: function(){
                this.num = this.num -1;
            },
            reset: function(){
                this.num = 0;
            }
        }
    }
</script>

<style scoped>

.cnt{
    font-size:30px;
}

button{
    display:inline-block;
    font-size:50px;
    color: white;
}

.number{
    font-size:150px;
    margin:10px;
}

.plus{
    background-color: red;
    padding:20px 50px;
    margin: 10px;
}

.minus{
    background-color: blue;
    padding:20px 50px;
}

.reset{
    background-color: black;
    margin: 10px;
    padding:10px 30px;
}

</style>

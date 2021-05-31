<template>
    <div class = "get-api">
        <div>
            <p>センサーID（半角数字）を入力してください</p>
            <input type="text" value="" id="sensor">
            <input type="button" value="表示" @click="getSensorId">
            <p class="sensor-value">センサー<span id="result">{{sensorValue}}</span></p>
            <p class="result-value">{{resultValue}}</p>
        </div>
        <div class="table">    
            <div class="table-line" v-for="(info, name) in infos" :key="info">
                <p class="table-column">{{name}}</p>
                <p>{{info}}</p>
            </div>
        </div>
    </div>
</template>



<script>
    
    import axios from 'axios'

    let edgeId = 0;

    export default{
        name:"GetApi",
        data(){
            return{
                infos:"",
                sensorValue:"",
                resultValue:"",
            }
        },
        methods: {
            getSensorId: function(){
                this.infos = "";
                this.resultValue="";
                console.log(this.resultValue);
                const sensor = document.getElementById("sensor").value;
                this.sensorValue = sensor;
                edgeId = Number(sensor);
                axios
                .get(`https://platform.coi.kyushu-u.ac.jp/raspi/area/${edgeId}?start={unixtime}&end={unixtime}`)
                .then(response => {
                    console.log(response);
                    console.log(response.data);
                    let datas = response["data"];
                    if(datas.result=="false2")
                    {
                        this.resultValue="センサーIDが間違っています";
                        console.log(this.resultValue);
                    }
                    else{    
                        this.infos = JSON.parse(datas);
                        console.log(datas);
                        let dateTime = new Date(parseInt(this.infos.time));
                        console.log(dateTime.toString());
                        this.infos.time = dateTime;
                    }
                })
            },
        }
    }

</script>

<style scoped>

.get-api{
    color:black;
    font-size:10px;
}


.sensor-value{
    font-size: 4em;

}


.result-value{
    color: red;
    font-size: 18px;
}

.table p{
    width:14em;
    display: inline-block;
    
}

.table-column{
    border-right: black 2px solid;
}

.table-line{
    border: black 1px solid;
    width: 30em;
    margin: 0 auto;
}

.table{
    font-size: 16px;
}


</style>
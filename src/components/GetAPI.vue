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
            <div class="table-column">
                
                <div  v-for="(info, num) in infos" :key="info" class="column-value">
                    <div v-for="(i,name) in info" :key="i">
                        <p v-if="num==0">{{name}}</p>
                        <p>{{i}}</p>
                    </div>
                </div>
            </div>
            <!-- <div class="table-line">
                <p class="tapple-value" v-for= "info in infos" :key="info">{{info}}</p>
            </div> -->
        </div>
    </div>
</template>



<script>
    
    //API用のフレームワークaxios
    import axios from 'axios'

    //センサーのIDを格納する変数
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
                //値をリセット
                this.infos = "";
                this.resultValue="";

                //センサーIDをhtmlに表示
                const sensor = document.getElementById("sensor").value;
                this.sensorValue = sensor;
                edgeId = Number(sensor);
                
                //日付を取得
                let DateTime = new Date();
                console.log(Math.floor(DateTime.getTime()/1000)-1000);
                let yesterdayTime = Math.floor(DateTime.getTime()/1000)-600;
                let nowTime = Math.floor(DateTime.getTime()/1000);

                //APIにデータを貰う
                axios
                .get(`https://platform.coi.kyushu-u.ac.jp/raspi/area/${edgeId}?start=${yesterdayTime}&end=${nowTime}`)
                 .then(response => {
                    console.log(response);
                    
                    //
                    let datas = response["data"];
                    if(datas.result=="false2")
                    {
                        //存在しないセンサーID
                        this.resultValue="センサーIDが間違っています";
                        console.log(this.resultValue);
                    }
                    else{    
                        //時間を文字列に変換
                        this.infos = JSON.parse(datas);
                        for(let i = 0; i <= this.infos.length; i++){
                            let resultTime = new Date(parseInt(this.infos[i]["time"]));
                            this.infos[i].time = resultTime.toString();
                        }
                        console.log(this.infos[0]);
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




</style>
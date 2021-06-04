<template>

<div class="main">

<h1>Covid Update</h1>
</div><div class="main2">
<h3>Search your state</h3>
<input type="text" id="input_date" v-model="ipt_date" placeholder="Enter state">
<button @click="ondatechange(input_date)">Click</button>
</div>
<div v-show="show_time">
<table >
<tr >
<th style="text-align: left; width: 30rem;"><b>Province</b></th>
<th style="text-align: left; width: 15rem;"><b>Active</b></th>
<th style="text-align: left; width: 15rem;"><b>Confirmed</b></th>
<th style="text-align: left; width: 15rem;"><b>Deaths</b></th>

</tr>
<tr  v-for="item in list" v-bind:key="item.id">
<td style="text-align: left; width: 10rem;" v-if="item.region.province!='Unknown'" >{{item.region.province}}</td>
<td  style="text-align: left; width: 10rem;" v-if="item.active!=0">{{item.active}}</td>
<td  style="text-align: left; width: 10rem;" v-if="item.confirmed!=0">{{item.confirmed}}</td>
<td  style="text-align: left; width: 10rem;" v-if="item.deaths!=0">{{item.deaths}}</td>

</tr>
</table>

</div>
</template>
<script>

import axios from 'axios'
import VueAxios from 'vue-axios'

export default{
    name:"covid",
    data(){
        return{
            list:undefined,
            area:[],
            deaths:[],
            obj1:{},
            show_time:false
            
        }
    },
    methods:{
    ondatechange(ipt_date){

        setTimeout(()=>{this.show_time=true},2000)
        const options = {
        method: 'GET',
        url: 'https://covid-19-statistics.p.rapidapi.com/reports',
        params: {
            date: this.ipt_date,
            
            region_name: 'India',
            iso: 'IND',
            
            
        },
        headers: {
            'x-rapidapi-key': '23d383e42emshc7b2987b0244768p1a81d8jsn18978a41d98e',
            'x-rapidapi-host': 'covid-19-statistics.p.rapidapi.com'
        }
        };

        axios.request(options).then( (response)=> {
            this.list=response.data.data;
            
        }).then((list)=>{  
            for(const item of this.list){ 
                this.area.push(item.region.province)
                this.deaths.push(item.deaths )
            }
        }).then((area,deaths)=>{
            this.area.forEach((value,index)=>
            this.obj[this.deaths[index]]=value)
            console.log(this.obj)
        }).catch((error)=> {
            console.error(error);
        });
    }
    }
}
</script>
<style>
#app {
font-family: 'Times New Roman'
, Times
, serif;
color: #0d0b1c;
width: 500px;
}
body{
    background-image:url("../covid.jpg" );
    background-repeat: no-repeat;
    background-size: cover;
}
button{
    border-radius: 22px;
    cursor: pointer;
    padding: 12px 20px;
    margin: 0 8px 0;
}
.main{
    height: 50px;
    width: 1350px;
    text-align:center;
    align-items:center;
    color:White;
    font-family: "Lucida Console", "Courier New", monospace;

}
.main2{
    font-family: Garamond, serif;
    text-align:center;
    align-items:center;
    color:White;
}
table {
  border-collapse: collapse;
  width: 100%;
  color:White;
  margin:2px 0;
}

th, td {
  text-align: left;
  padding: 8px;
}

tr:hover {background-color: #ffffff;
color:Black;
}
input[type=text] {
  width: 130px;
  box-sizing: border-box;
  border: 2px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
  background-color: white;
  background-position: 10px 10px; 
  background-repeat: no-repeat;
  padding: 12px 20px 12px 20px;
  transition: width 0.4s ease-in-out;
}

input[type=text]:focus {
  width: 40%;
}
</style>
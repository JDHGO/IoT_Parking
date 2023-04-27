<template>
  <div class="home" style="display:flex;flex-direction:column;align-items:center;justify-contents: center;">
    <div class="info" style="display:flex; flex-direction:row; align-items:center;justify-contents: center;margin-bottom:10px;">
      <div style="">
        <h1 style="margin: 5px;margin-right:100px; font-size:40px;">현재시간 : {{ times }}</h1>
      </div>
      <div>
        <h1 style="margin: 5px;margin-lefg:100px; font-size:40px;">주차현황 : {{ count }} / 4</h1>
      </div>
    </div>
    
    <div class="status" style="display:flex; flex-direction:row;margin-top:10px;">
      <div>
        <img v-if="place1==1" :src="require(`@/assets/red_1.png`)" style="border-radius:10px;margin:5px 10px;height:250px;width:200px;">
        <img v-else :src="require(`@/assets/green_1.png`)" style="border-radius:10px;margin:5px 10px;height:250px;width:200px;">
      </div>
      <div>
        <img v-if="place2==1" :src="require(`@/assets/red_2.png`)" style="border-radius:10px;margin:5px 10px;height:250px;width:200px;">
        <img v-else :src="require(`@/assets/green_2.png`)" style="border-radius:10px;margin:5px 10px;height:250px;width:200px;">
      </div>
      <div>
        <img v-if="place3==1" :src="require(`@/assets/red_3.png`)" style="border-radius:10px;margin:5px 10px;height:250px;width:200px;">
        <img v-else :src="require(`@/assets/green_3.png`)" style="border-radius:10px;margin:5px 10px;height:250px;width:200px;">
      </div>
      <div>
        <img v-if="place4==1" :src="require(`@/assets/red_4.png`)" style="border-radius:10px; margin:5px 10px;height:250px;width:200px;">
        <img v-else :src="require(`@/assets/green_4.png`)" style="border-radius:10px; margin:5px 10px;height:250px;width:200px;">
      </div>
    </div>
    <div class="description">
      <h1 style="font-size:30px;">안내</h1>
      <div style="display:flex;flex-direction:row;">
        <div style="display:flex;flex-direction:column;">
          <img :src="require(`@/assets/green.png`)" style="border-radius:10px;margin:5px 10px;height:200px;width:150px;">
          <h1 style="font-size:20px;">주차 공간 있음</h1>
        </div>
        <div style="display=flex;flex-direction:column;">
          <img :src="require(`@/assets/red.png`)" style="border-radius:10px;margin:5px 10px;height:200px;width:150px;">
          <h1 style="font-size:20px;">주차 공간 없음</h1>
        </div>
      </div>
    </div>
    
  </div>
</template>

<script>
import io from 'socket.io-client'
import moment from 'moment'

export default {
  name: 'Home',
  data() {
    return {
      socket: '',
      times:'',
      place1: 0,
      place2: 0,
      place3: 0,
      place4: 0,
      count: 0,
    }
  },
  created() {    
    this.socket = io("http://localhost:3000"); 
    this.socket.on("parkinglotstatus", (arg)=> {
      console.log(arg);
      this.times = moment(arg[0].time).format("HH:mm:ss");
      this.place1 = arg[0].place1;
      this.place2 = arg[0].place2;
      this.place3 = arg[0].place3;
      this.place4 = arg[0].place4;
      this.count=arg[0].place1+arg[0].place2+arg[0].place3+arg[0].place4;
    });

    this.socket.emit("sendstatus", "finsh");
  }
}
</script>

<style scoped>
</style>
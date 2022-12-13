<template>
<div>
  <h1 class="text-4xl font-bold bg-gray-800 text-white text-center">NBA Score!</h1>
  <h1 class="text-2xl font-bold  bg-red-600 text-white text-center">To Day:{{date}}</h1>

  <div class=" bg-slate-400 w-full text-white">
    <date-picker v-model="date_today" :inline="true" :minimumView="'week'" :maximumView="'year'" class=" text-black "/>
    <p>{{date_today}}</p>
  </div>

  <div class="flex justify-around mt-5">
    
    <div class="">
      <div class="flex flex-col justify-center items-center text-2xl text-center h-80 border-t-4 mb-5" v-for="(away,index) in awayTeam" :key="index">
        <img class=" h-4/5  w-4/5" :src="getAwayPic(index)">
        {{away}}<br>
        {{awayTeamwin[index]}}-{{awayTeamlose[index]}}
      </div>
    </div>

    <div class=""><div class="flex flex-col justify-center  text-2xl h-80 border-t-4 mb-5" v-for="score in awayTeamScore" :key="score">{{ score }}</div></div>

    <div class=" text-center ">
      <div class="flex flex-col justify-center  text-2xl h-80  border-t-4 mb-5" v-for="(value,index) in status" :key="index">
        <div v-if=" status[index] === 3" class="bg-slate-400 text-black font-bold w-full">
          {{statusText[index]}}
        </div>
        <div v-if=" status[index] === 2" class=" font-bold w-full">
          <p class="bg-red-500 text-white">Live</p>
          {{statusText[index]}}
        </div>
        <div v-if=" status[index] === 1" class=" text-black font-bold w-full">
          {{statusText[index]}}
        </div>
      </div>
    </div>

    <div class=""><div class="flex flex-col justify-center  text-2xl h-80  border-t-4 mb-5" v-for="score in homeTeamScore" :key="score">{{ score }}</div></div>
    
    <div class="">
      <div class="flex flex-col justify-center items-center text-2xl text-center h-80 border-t-4 mb-5" v-for="(home,index) in homeTeam" :key="index">
      <img class="h-4/5  w-4/5" :src="getHomePic(index)">
      {{home}}<br>
      {{homeTeamwin[index]}}-{{homeTeamlose[index]}}
      </div>
    </div>
  </div>
  
</div>
</template>

<script>
export default {
  data (){
    return {
      date_today:new Date()
    }
  },
  methods: {
    getAwayPic(index) {
      return 'https://cdn.nba.com/logos/nba/' + this.awayTeamId[index] + '/global/L/logo.svg';
    },
    getHomePic(index) {
      return 'https://cdn.nba.com/logos/nba/' + this.homeTeamId[index] + '/global/L/logo.svg';
    }
  },
  async asyncData({ $axios }) {
    const corsURL = 'https://cors-anywhere.herokuapp.com/'; // use cors-anywhere to fetch api data
    const apiURL = 'https://cdn.nba.com/static/json/liveData/scoreboard/todaysScoreboard_00.json';
    try{
        const toDay = await $axios.$get(`${corsURL}${apiURL}`);
        let toDayGames = toDay.scoreboard.games
        let awayTeam = toDayGames.map(a => a.awayTeam.teamName);
        let awayTeamScore = toDayGames.map(a => a.awayTeam.score);
        let awayTeamwin = toDayGames.map(a => a.awayTeam.wins);
        let awayTeamlose = toDayGames.map(a => a.awayTeam.losses);
        let awayTeamId = toDayGames.map(a => a.awayTeam.teamId);
        let homeTeam = toDayGames.map(a => a.homeTeam.teamName);
        let homeTeamScore = toDayGames.map(a => a.homeTeam.score);
        let homeTeamwin = toDayGames.map(a => a.homeTeam.wins);
        let homeTeamlose = toDayGames.map(a => a.homeTeam.losses);
        let homeTeamId = toDayGames.map(a => a.homeTeam.teamId);
        let status = toDayGames.map(a => a.gameStatus);
        let statusText = toDayGames.map(a => a.gameStatusText);
        let date = toDay.scoreboard.gameDate

        console.log(awayTeamId)
        console.log(statusText[3])
        return {
          awayTeam,
          awayTeamScore,
          awayTeamwin,
          awayTeamlose,
          awayTeamId,
          homeTeam,
          homeTeamScore,
          homeTeamwin,
          homeTeamlose,
          homeTeamId,
          status,
          statusText,
          date,
        }
    }catch(err){
        console.log(err);
    }
  }
}
</script>

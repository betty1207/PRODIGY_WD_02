<template>
  <div class="h-screen flex flex-col space-y-8 items-center justify-center">
   <div class="bg-zinc-800 p-6 w-[75vh] h-[75vh] ring-4 ring-orange-500 rounded-full flex items-center justify-center text-white">
      <p> <span class="text-5xl bg-gradient-to-r from-pink-500 via-purple-500 to-blue-500 text-transparent bg-clip-text">
    {{formattedTime}}
  </span>   
 <!-- Adding Laps-->
 <div v-if="laps.length" class="  w-full">
   <h2 class="text-xl font-semibold text-center py-3 text-orange-500">Laps</h2>
   <ul class="overflow-y-auto max-h-48 space-y-2  rounded-md ">
     <li v-for="(lap, index) in laps" :key="index" class="text-sm bg-white bg-opacity-5 px-10 rounded-sm flex justify-between">
      {{ laps.length - index }}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{ lap }}
     </li>
   </ul>
 </div>
 </p>
   </div>
 <div class="flex  items-center">
   <button @click="toggleStartStop" class="ring-1 ring-orange-900 bg-gradient-to-r from-pink-500 to-blue-500 text-white px-20 py-4 ml-10 rounded-3xl hover:scale-105 hover:from-pink-600 hover:to-blue-600 duration-300 focus:outline-none">
     {{ isRunning ? 'Stop' : paused ? 'Resume' : 'Start' }}
   </button>
   
   <button @click="isRunning ? addLap() : reset()" class="ring-1 ring-orange-900 bg-gradient-to-r from-pink-500 to bg-blue-500 hover:scale-105 hover:from-pink-600 hover:to-blue-600 text-white px-20 py-4 ml-10 rounded-3xl transition duration-300 focus:outline-none">
     {{ isRunning ? 'Lap' : 'Reset' }}
   </button>
 </div>
</div>
</template>

<script>
export default {
 data() {
   return {
     time: 0, // time in milliseconds
     interval: null,
     isRunning: false, // tells if the stopwatch is running
     paused: false,
     laps: [], // Stores lap times
     maxLaps: 5, // Max number of laps to show
   };
 },
 computed: {
   formattedTime() {
     const minutes = Math.floor(this.time / 60000).toString().padStart(2, "0");
     const seconds = Math.floor((this.time % 60000) / 1000).toString().padStart(2, "0");
     const milliseconds = ((this.time % 1000) / 10).toString().padStart(2, "0");
     return `${minutes}:${seconds}.${milliseconds}`;
   },
   
 },
 methods: {
   toggleStartStop() {
     if (this.isRunning) {
       this.stop();
     } else if (this.paused) {
       this.resume();
     } else {
       this.start();
     }
   },
   start() {
     this.isRunning = true;
     this.paused = false;
     this.interval = setInterval(() => {
       this.time += 10;
     }, 10);
   },
   stop() {
     clearInterval(this.interval);
     this.interval = null;
     this.isRunning = false; // Stop the stopwatc
     this.paused = true;
   },
   resume() {
     this.isRunning = true;
     this.paused = false;
     this.interval = setInterval(() => {
       this.time += 10;
     }, 10);
   },
   addLap() {
     this.laps.push(this.formattedTime);
   },
   reset() {
     this.stop();
     this.time = 0;
     this.laps = [];
     this.paused = false;
   }
 },
 beforeDestroy() {
   this.stop();
 }
};
</script>
<style scoped>
.overflow-y-auto {
  max-height: 12rem; /* Limits the height to show approximately 5 laps */
  overflow-y: auto; /* Enables scrolling when content overflows */
}

.overflow-y-auto::-webkit-scrollbar {
  width: 8px; /* Scrollbar width */
  background-color: rgba(181, 151, 17, 0.4);
  border-radius: 4px;

}

.overflow-y-auto::-webkit-scrollbar-thumb {
  background-color: rgba(255, 165, 0, 0.5); /* Semi-transparent orange */
  border-radius: 4px; /* Rounded scrollbar thumb */
}

.overflow-y-auto::-webkit-scrollbar-track {
  background-color: transparent; /* Transparent track for the scrollbar */
}


</style>
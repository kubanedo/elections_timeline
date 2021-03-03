<template>
<div>
  <div v-if="dataLoaded" class="timeline">
      <div class="timeline-inner">
        <TimelineItem 
        v-for="timelineItem in timelineItems" 
        :key="timelineItem._id" 
        :itemData="timelineItem" />
      </div>
  </div>
  <div v-else class="timeline-loader">
        <div class="ball"></div>
        <p>Loading timeline</p>
  </div> 
</div>  
</template>

<script>
import axios from 'axios';
import TimelineItem from './TimelineItem';

export default {
    components: {
        TimelineItem
    },
    data() {
        return {
            timelineItems: [],
            dataLoaded: false
        }
    },
    methods: {
        sortByDate(array, dateProperty) {
            return array.sort(function(a,b){
                return new Date(a[dateProperty]) - new Date(b[dateProperty]);
            });            
        },
        fakeDelay(fn) {
            return setTimeout(fn, 3000);
        }
    },
    mounted() {
        axios.get('timeline.json')
        .then(res => {
            this.timelineItems = this.sortByDate(res.data, 'registered');
            this.fakeDelay(() => this.dataLoaded = true);
        })
        .catch((err) => console.error(err))
    }
}
</script>

<style lang="scss" scoped>
$primary-color: #095184;

.timeline {
    position: relative;
    width: 100%;
    max-width: 800px;
    margin: 0 auto;

    &::after {
        content: '';
        position: absolute;
        width: 5px;
        background: $primary-color;
        top: 0;
        bottom: 0;
        left: 0;
        margin-left: 16px;

        @media screen and (min-width: 539px) {
            left: 50%;
            margin-left: -5px;    
        }        
    }
}
.timeline-inner {
    position: relative;
    padding: 2rem 0; 

    @media screen and (min-width: 539px) {
        padding: 4rem 0; 
    } 

    &::before, &::after {
        content: '';
        position: absolute;
        width: 26px;
        height: 26px;
        top: -1px;
        left: 4px;
        background: $primary-color;
        border: 5px solid $primary-color;
        border-radius: 50%;
        z-index: 1;
        box-sizing: border-box;

        @media screen and (min-width: 539px) {
            left: calc(50% - 16px);   
        }          
    }   
    &::after {
        top: unset;
        bottom: 0;
    }   
}
.timeline-loader {
    width: 100px;
    height: 50px;
    position: absolute;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    margin: auto;
    text-align: center;
  
    .ball {
        width: 20px;
        height: 20px;
        background-color: $primary-color;
        border-radius: 50%;
        display: inline-block;
        animation: motion 3s cubic-bezier(0.77, 0, 0.175, 1) infinite;
    }  
        
    p {
        color: $primary-color;
        margin-top: 5px;
        font-family: sans-serif;
        letter-spacing: 3px;
        font-size: 10px;
        text-transform: uppercase;
    }    
    @keyframes motion {
        0% {
            transform: translateX(0) scale(1)
        } 
        25% {
            transform: translateX(-50px) scale(0.3)
        }
        50% {
            transform: translateX(0) scale(1)
        }
        75% {
            transform: translateX(50px) scale(0.3)
        }
        100% {
            transform: translateX(0) scale(1)
        }
    }    
}
</style>
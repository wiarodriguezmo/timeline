<template>
    <div class="month" :class="[borderColor, status[month]]">
        <span v-if="month%2==0" class="month-name">
            {{ cutMonth(months[month]) }}
        </span>
            
        <span :style="{ margin: '0px ' + margin + '%' }" :class="[event.interest, event.status]" v-for="(event, key) in events" :key="key">
        </span>

        <span style="float: right" v-if="month%2==1" class="month-name">
             {{ cutMonth(months[month]) }}
        </span>
            
    </div>
</template>

<script>
var red = "red", blue = "blue", yellow = "yellow", green = "green", grey = "grey";

var calculateBorderColor = ((month, events) => {
    let borderColor;
    switch (month) {
        case 0:
            borderColor = blue;
            break;
        case 1:
            borderColor = blue;
            break;
        case 2:
            borderColor = blue;
            break;
        case 3:
            borderColor = red;            
            break;
        case 4:
            borderColor = red;            
            break;
        case 5:
            borderColor = red;            
            break;
        case 6:
            borderColor = red;    
            break;
        case 7:
            borderColor = green;    
            break;
        case 8:
            borderColor = green;    
            break;
        case 9:
            borderColor = yellow;    
            break;
        case 10:
            borderColor = yellow;    
            break;
        case 11:
            borderColor = grey;    
            break;
    }
    return borderColor;
});


export default {
  name: 'Event',
  methods: {
    cutMonth: ((text) => {
          return text.substring(0,3);
      })
  }, 
  props: ['events', 'month', 'months', 'status'],
  data: function () {
    return {
      margin: (40 / this.events.length),
      borderColor: calculateBorderColor(this.month, this.events)
    }
  }   
}
</script>

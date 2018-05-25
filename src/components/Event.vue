<template>
    <div class="month" :class="[borderColor, status[month]]">
        <span v-if="month%2==0" class="month-name">
            {{ cutMonth(months[month]) }}
        </span>
            
        <div :style="{ margin: '0px ' + margin + '%' }" v-for="(event, key) in events" :key="key"
            :class="[event.interest, event.status, {'show-tultip': event.tooltip}]"  class="tultip" v-bind:id="key" @click="tooltip(event)">
            <div class="square-tooltip">
                <span>
                    {{ date(event.date) }}
                </span><br>
                <span>
                    {{ event.name }}
                </span><br>
                <a v-if="event.url" :href="event.url" target="_blank">Ver más</a>
            </div>
        </div>

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
      }),
    date: ((date) => {
        let event = new Date(date);
        event.setMinutes(event.getMinutes() + event.getTimezoneOffset());
        var options = { weekday: 'short', month: 'short', day: 'numeric' };
        return event.toLocaleDateString('es-ES', options);
      }),
    tooltip: ((event) => {
        event.tooltip = !event.tooltip;
    })
  }, 
  props: ['events', 'month', 'months', 'status'],
  data: function () {
    return {
      margin: (35 / this.events.length),
      borderColor: calculateBorderColor(this.month, this.events)
    }
  }   
}
</script>

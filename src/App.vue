<template>
  <div id="app">
    {{monthsStatus}}
    <div style="margin-bottom: 2em">
      <select v-model="selected.front" v-on:click="setColorByFilter(selected, 'front')">
        <option value="">Por frente</option>
        <option>Momento 1: Innovación educativa con TIC</option>
        <option>Momento 2: Innovaciones en pregrado</option>
        <option>Momento 3: Innovaciones en posgrado</option>
        <option>Momento 4: Innovaciones en extensión</option>
      </select>
      <select v-model="selected.month" v-on:click="setColorByMonth(selected.month)">
        <option value="">Por mes</option>
        <option v-for="(month, num) in months" :key="num" :value="num">{{month}}</option>
      </select>
      <select v-model="selected.interest" v-on:click="setColorByFilter(selected, 'interest')">
        <option value="">Por interés</option>
        <option value="one">&#x25B3; Compartir con otros</option>
        <option value="two">&#x25C7; Atención personalizada</option>
        <option value="three">&#x25CB; Explorar posibilidades pedagógicas y tecnológicas</option>
      </select>
      <select v-model="selected.activity" v-on:click="setColorByFilter(selected, 'activity')">
        <option value="">Por actividad</option>
        <option>Conversatorios</option>
        <option>Talleres</option>
        <option>Citas con Conecta-TE</option>
      </select>
    </div>
    {{selected.front}} {{selected.activity}} <br><br><br><br>
    <Event v-for="(events, key) in groupedEvents" :events="events" :key="key" :month="key" :months="months" :status="monthsStatus"/>

  </div>
</template>

<script>
import Event from './components/Event'
var months = [
  "Enero", "Febrero", "Marzo",
  "Abril", "Mayo", "Junio",
  "Julio", "Agosto", "Septiembre",
  "Octubre", "Noviembre", "Diciembre"
]
var events = [
  {
      "name": "Citas con conecta-TE",
      "date": "2018-04-25",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "status": "hidden"
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-04-27",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "status": "visible"
  },
  {
      "name": "Conversatorio #1",
      "date": "2018-03-20",
      "hour": "",
      "front": "",
      "interest": "one",
      "activity": "Conversatorios",
      "status": "visible"
  },
  {
      "name": "Conversatorio #2",
      "date": "2018-05-09",
      "hour": "",
      "front": "",
      "interest": "one",
      "activity": "Conversatorios",
      "status": "visible"
  },
  {
      "name": "Conversatorio #3",
      "date": "2018-07-24",
      "hour": "",
      "front": "",
      "interest": "one",
      "activity": "Conversatorios",
      "status": "visible"
  },
  {
      "name": "Conversatorio #4",
      "date": "2018-09-10",
      "hour": "",
      "front": "",
      "interest": "one",
      "activity": "Conversatorios",
      "status": "visible"
  },
  {
      "name": "Conversatorio #5",
      "date": "2018-11-22",
      "hour": "",
      "front": "",
      "interest": "one",
      "activity": "Conversatorios",
      "status": "visible"
  },
  {
      "name": "Conecta-TE Explorando #1",
      "date": "2018-04-12",
      "hour": "",
      "front": "",
      "interest": "three",
      "activity": "Conecta-TE Explorando",
      "status": "visible"
  },
  {
      "name": "Conecta-TE Explorando #2",
      "date": "2018-06-14",
      "hour": "",
      "front": "",
      "interest": "three",
      "activity": "Conecta-TE Explorando",
      "status": "visible"
  },
  {
      "name": "Conecta-TE Explorando #3",
      "date": "2018-08-23",
      "hour": "",
      "front": "",
      "interest": "three",
      "activity": "Conecta-TE Explorando",
      "status": "visible"
  },
  {
      "name": "Conecta-TE Explorando #4",
      "date": "2018-10-11",
      "hour": "",
      "front": "",
      "interest": "three",
      "activity": "Conecta-TE Explorando",
      "status": "visible"
  }
]

var monthsStatus =  ["visible", "visible", "visible", "visible", "visible", "visible", "visible", "visible", "visible", "visible", "visible", "visible"];

var containOnlyVisibleChilds = (events) => {
  return events.every((event) => event.status == "visible")
};
var a = (b) => {
  for(var i = 0; i<b.length; i++) {
    monthsStatus[i] = containOnlyVisibleChilds(b[i])?"visible":"hidden";
  }
};
var groupByMonth = (list) => {
  let temp = [];
  for(var i = 0; i<12; i++) {
    temp[i] = list.filter((x) => {
      let d = new Date(x.date);
      d.setMinutes(d.getMinutes() + d.getTimezoneOffset());
      if(d.getMonth() == i){
        return x;
      }
    });
  }
  return temp;
}
export default {
  name: 'App',
  methods: {
    setColorByFilter: ((options, actual) => {
      events.forEach((elem) => {
        if(options[actual]=="" || elem[actual] == options[actual])
          elem.status = "visible";
        else elem.status = "hidden";
      });
      a(groupByMonth(events));
    }),
    setColorByMonth: ((actual) => {
      actual = actual + "";
      events.forEach((elem) => {
        let d = new Date(elem.date);
        d.setMinutes(d.getMinutes() + d.getTimezoneOffset());
        if(actual=="" || d.getMonth() == actual)
          elem.status = "visible";
        else elem.status = "hidden";
      })
    }),
  },
  components: {
    Event
  },
  data: function () {
    return {
      selected: {
        front: '',
        month: '',
        interest: '',
        activity: ''
      },
      groupedEvents: groupByMonth(events),
      months,
      monthsStatus
    }
  }
}
</script>

<style  lang="scss">
  $blue: #45ACD7;
  $red: #E13815;
  $green: #8BB822;
  $yellow: #ED8C00;
  $grey: #666666;
  $disabled: #B3B3B3;

  @mixin set-color($color) {
    border: 0.8px solid $color;
    .one::before {
        border-bottom-color: $color;
    }
    .two, .three{
        border-color: $color;
    }
    .month-name {
      background-color: $color;
    }
  }

  * {
    font-family: 'Roboto', sans-serif;
  }

  body {
    margin: 6em;    
  }
  .month {
    position: relative;
    height: 70px;
    border-bottom: none !important;

    &:nth-child(odd) {
      border-right: none;
    }
    &:nth-child(even) {
      border-left: none;
    }
    &:nth-last-child(1) {
      border-left: none;
    }
    .month-name{
        padding: 5px 15px;
        margin: 0 10px;
        text-transform: uppercase;
        font-size: 11px;
        font-weight: 900;
        color: white;
        letter-spacing: 2px;   
    }
  }
  
  .one, .two, .three {
    cursor: pointer;
  }
  
  .one {
    position: absolute;
    
    &::before, &::after {
        content: '';
        display: block;
        position: absolute;
        width: 0;
        height: 0;
        border-style: solid;
    }
    &::before {
        bottom: -7px;
        border-color: transparent;
        border-width: 15px 8px;
    }
    &.hidden::before {
      border-bottom-color: $disabled !important;
    }

        
    &::after {
        bottom: -4px;
        border-color: transparent transparent #fff transparent;
        border-width: 8px 6px;
        margin-left: 2px;
    }
  }
  .two {
      background-color: #fff;
      border: 1px solid;
      display: inline-block;
      height: 8px;
      width: 8px;
      position: relative;
      top: -9px;
      transform: rotate(45deg);
      &.hidden {
        border-color: $disabled !important;
      }
  }
  .three {
      height: 10px;
      width: 10px;
      background-color: #fff;
      border-radius: 50%;
      display: inline-block;
      position: relative;
      top: -8px;
      cursor: pointer;
      border: 1px solid;

    &.hidden {
      border-color: $disabled !important;
    }
  } 

  .blue {
      @include set-color($blue);
  }
  .red {
      @include set-color($red);
  }
  .green {
      @include set-color($green);
  }
  .yellow {
      @include set-color($yellow);
  }
  .grey {
    @include set-color($grey);
  }
  .hidden {
    border: 0.8px solid $disabled;
    .month-name {
      background-color: $disabled;
    }
  }



</style>

<template>
  <div id="app">
    <div id="header">
      <div id="compass">
      </div>
      <div id="filters">
        <select v-model="selected.front" v-on:click="setColorByFilter(selected, 'front')" v-on:change="resetSelects(selected, 'front')">
          <option value="">Por frente</option>
          <option value="one">Momento 1: Innovación educativa con TIC</option>
          <option value="two">Momento 2: Innovaciones en pregrado</option>
          <option value="three">Momento 3: Innovaciones en posgrado</option>
          <option value="four">Momento 4: Innovaciones en extensión</option>
        </select>
        <select v-model="selected.month" v-on:click="setColorByMonth(selected.month)" v-on:change="resetSelects(selected, 'month')">
          <option value="">Por mes</option>
          <option v-for="(month, num) in months" :key="num" :value="num">{{month}}</option>
        </select>
        <select v-model="selected.interest" v-on:click="setColorByFilter(selected, 'interest')" v-on:change="resetSelects(selected, 'interest')">
          <option value="">Por interés</option>
          <option value="one">&#x25B3; Compartir con otros</option>
          <option value="two">&#x25C7; Atención personalizada</option>
          <option value="three">&#x25CB; Explorar posibilidades pedagógicas y tecnológicas</option>
        </select>
        <select v-model="selected.activity" v-on:click="setColorByFilter(selected, 'activity')" v-on:change="resetSelects(selected, 'activity')">
          <option value="">Por actividad</option>
          <option>Conversatorios</option>
          <option>Conecta-TE Explorando</option>
          <option>Citas con Conecta-TE</option>
        </select>
      </div>
      <div class="clear"></div>
    </div>
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
      "name": "Conversatorio #1: Innovación con TIC en educación superior: retos y oportunidades",
      "date": "2018-03-20",
      "hour": "",
      "front": "one",
      "interest": "one",
      "activity": "Conversatorios",
      "url": "https://conectate.uniandes.edu.co/index.php/eventos/516-conversatorio-1-innovacion-con-tic-en-educacion-superior",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Conversatorio #2: Dispositivos pedagógicos apoyados en tecnología digital",
      "date": "2018-05-09",
      "hour": "",
      "front": "two",
      "interest": "one",
      "activity": "Conversatorios",
      "url" : "https://conectate.uniandes.edu.co/index.php/eventos/542-conversatorio-2",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Conversatorio #3",
      "date": "2018-07-24",
      "hour": "",
      "front": "two",
      "interest": "one",
      "activity": "Conversatorios",
      "status": "visible",
      "tooltip": false,
      "url": "https://conectate.uniandes.edu.co/index.php/eventos/555-conversatorio-3-aprendizaje-conjetural"
  },
  {
      "name": "Conversatorio #4",
      "date": "2018-09-26",
      "hour": "",
      "front": "three",
      "interest": "one",
      "activity": "Conversatorios",
      "status": "visible",
      "tooltip": false,
      "url": "https://conectate.uniandes.edu.co/index.php/eventos/572-conversatorio-4-blended-learning-en-educacion-superior"
  },
  {
      "name": "Conversatorio #5",
      "date": "2018-11-22",
      "hour": "",
      "front": "four",
      "interest": "one",
      "activity": "Conversatorios",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Conecta-TE Explorando #1",
      "date": "2018-04-12",
      "hour": "",
      "front": "two",
      "interest": "three",
      "activity": "Conecta-TE Explorando",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Conecta-TE Explorando #2",
      "date": "2018-06-14",
      "hour": "",
      "front": "two",
      "interest": "three",
      "activity": "Conecta-TE Explorando",
      "status": "visible",
      "tooltip": false,
      "url": "https://conectate.uniandes.edu.co/index.php/eventos/554-conecta-te-explorando-diseno-elaboracion-y-comunicacion-de-contenidos-con-videos-e-infografias"
  },
  {
      "name": "Conecta-TE Explorando #3",
      "date": "2018-08-23",
      "hour": "",
      "front": "three",
      "interest": "three",
      "activity": "Conecta-TE Explorando",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Conecta-TE Explorando #4",
      "date": "2018-10-11",
      "hour": "",
      "front": "four",
      "interest": "three",
      "activity": "Conecta-TE Explorando",
      "status": "visible",
      "tooltip": false
  },

  // Todos los miércoles de 8:00 a.m. a 10:00 a.m. - Citas con conecta-TE
  {
      "name": "Citas con conecta-TE",
      "date": "2018-02-07",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "one",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-02-14",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "one",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-02-21",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "one",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-02-28",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "one",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-03-07",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "one",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-03-14",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "one",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-03-21",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "one",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-03-28",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "one",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-04-04",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://conectate.uniandes.edu.co/index.php/eventos/533-participacion-en-ambientes-de-aprendizaje",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-04-11",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-04-18",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-04-25",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-05-02",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-05-09",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-05-16",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-05-23",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-05-30",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-06-06",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-06-13",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-06-20",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-06-27",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-07-04",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-07-11",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-07-18",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-07-25",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-08-01",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-08-08",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-08-15",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-08-22",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://conectate.uniandes.edu.co/index.php/eventos/570-aprendizaje-colaborativo",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-08-29",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-09-05",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-09-12",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-09-19",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-09-26",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-10-03",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-10-10",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-10-17",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-10-24",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-10-31",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-11-07",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-11-14",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-11-21",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-11-28",
      "hour": "8:00 a.m. a 10:00 a.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },

  // Todos los viernes de 2:00 p.m. a 4:00 p.m. - Citas con conecta-TE
  {
      "name": "Citas con conecta-TE",
      "date": "2018-02-02",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "one",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-02-09",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "one",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-02-16",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "one",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-02-23",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "one",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-03-02",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "one",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-03-09",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "one",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-03-16",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "one",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-03-23",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "one",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-03-30",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "one",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },




  {
      "name": "Citas con conecta-TE",
      "date": "2018-04-06",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-04-13",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-04-20",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-04-27",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-05-04",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-05-11",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-05-18",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-05-25",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-06-01",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-06-08",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-06-15",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-06-22",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-06-29",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-07-06",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-07-13",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-07-20",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-07-27",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "two",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },



  {
      "name": "Citas con conecta-TE",
      "date": "2018-08-03",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-08-10",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-08-17",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-08-24",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-08-31",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-09-07",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-09-14",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-09-21",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-09-28",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "three",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-10-05",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-10-12",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-10-19",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-10-26",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-11-02",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-11-9",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-11-16",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-11-23",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  },
  {
      "name": "Citas con conecta-TE",
      "date": "2018-11-30",
      "hour": "2:00 p.m. a 4:00 p.m",
      "front": "four",
      "interest": "two",
      "activity": "Citas con Conecta-TE",
      "url": "https://outlook.office365.com/owa/calendar/UniversidaddelosAndes@uniandes.onmicrosoft.com/bookings/",
      "status": "visible",
      "tooltip": false
  }
]

var monthsStatus =  [];

var containOnlyVisibleChilds = (events) => {
  if(events.length == 0)return "hidden";
  else return events.every((event) => event.status == "visible")
};
var paintLinesMonths = (b) => {
  for(var i = 0; i<b.length; i++) {
    let paint = b[i].length != 0 && containOnlyVisibleChilds(b[i]);
    monthsStatus[i] = paint?"visible":"hidden";
  }
};
var groupByMonth = (list) => {
  let temp = [];
  list.sort(function (a, b) {
    return new Date(a.date) - new Date(b.date);
  });
  
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
var setColorByMonth = (actual) => {
    actual = actual + "";
      events.forEach((elem) => {
        let d = new Date(elem.date);
        d.setMinutes(d.getMinutes() + d.getTimezoneOffset());
        if(actual=="" || d.getMonth() == actual)
          elem.status = "visible";
        else elem.status = "hidden";
      })
      paintLinesMonths(groupByMonth(events));
}
paintLinesMonths(groupByMonth(events));
export default {
  name: 'App',
  created: function () {
    events.sort(function (a, b) {
        return new Date(a.date) - new Date(b.date);
    });
    // let index = events.findIndex((event) => {
    //     return new Date(event.date) > new Date() && event.interest != "two";
    // });
    // events[index].tooltip = true;

    // forEach((item)=> {
    //     if(new Date(item.date).getMonth() == new Date().getMonth())
    //         item.tooltip = true;
    // });

    setColorByMonth(new Date().getMonth()+"")
  },
  methods: {
    resetSelects: ((options, actual) => {
        options.front = actual=="front"?options.front:"";
        options.interest = actual=="interest"?options.interest:"";
        options.activity = actual=="activity"?options.activity:"";
        options.month = actual=="month"?options.month:"";
        events.forEach((elem) => { 
            elem.tooltip = false;
        });
    }),
    setColorByFilter: ((options, actual) => {
      events.forEach((elem) => {
        if(options[actual]=="" || elem[actual] == options[actual])
          elem.status = "visible";
        else elem.status = "hidden";
      });
      paintLinesMonths(groupByMonth(events));
    }),
    setColorByMonth: setColorByMonth
  },
  components: {
    Event
  },
  data: function () {
    return {
      selected: {
        front: '',
        month: 4,
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
    .two::after {
        border-color: $color;
    }
    .three{
        border-color: $color;
    }
    .month-name {
      background-color: $color;
    }
    .square-tooltip {
      background: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACoAAAAYCAYAAACMcW/9AAAACXBIWXMAAAsSAAALEgHS3X78AAAB+ElEQVRIx81Xy3HDIBB99rgA0oFKUAmUoBJIB0oHKkGnnNVBKEHnnChBl9zVwebgx8yGQbaIcJw3w7BCgB77Y3USEVSEAzDhAThVJDoBaAAYAAvHeiUfUsC5IkkAsABGACuAQA3XQAMROdKMiMwi4pLxVq6wB/ePbTiiUQNgpjZTvwwAXqndn5q5tmKci00AeAADSZqCtS3XzJTLUGgCJyKdiATK0cTtDtOvIjKISE/ZPNL0hmZu2VsAb+ydSlEdTW8517O3ao5Xz1XTU/TJUfmkSfzTsHk+fwJ4V3NmpivtAoOav4XhkiHj2IdkA6N8cuI7rwKmoeyZngyAF8oLSS1sTZJ7i4MpBgoAfCR5cKFJRx6iV2Z2NGM8SEcCX5w/cr3eb1SHKw4mHRgDc2Qud44i4hksy0YwpeOBgWSStbuCKR1YRGTiB1bKuYWRiL8xZ0reOZINXJe7KHYTjQSEG91KIfEwOW1atjV517EV30xHrzabIevpGrlDVLtCewbKrCL1HjyDxKkbaGUfapVmOj0NjEKrPjSRwNYHY141XNco8gtqgqpt6OQ5Pww7KqjYT4VXY7HpB5odmSooMC9uYVW9U89VcU4qmxzGO0T/BGd1tW0h/LaGfIRG7Y3a0uIf4EKNLUlVlP4PTc8mGss8XTXlUtD8ZJ72Gx0gAOYwtk55AAAAAElFTkSuQmCC') center 5px no-repeat, $color;
    }
  }

  #app {
    margin: 1% 5%;    
    font-family: 'Roboto', sans-serif;      

    .clear {
      clear: both;
    }

    #header {
      margin-bottom: 1em;
      line-height: 80px;
    }  
    #compass {
      float: left;
      width: 120px;
      margin: 10px;
      height: 100px;
      background: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA2NC42NyA2NC42NyI+PGRlZnM+PHN0eWxlPi5jbHMtMSwuY2xzLTR7ZmlsbDpub25lO3N0cm9rZS1saW5lY2FwOnJvdW5kO3N0cm9rZS1saW5lam9pbjpyb3VuZDt9LmNscy0xe3N0cm9rZTojYjNiM2IzO30uY2xzLTJ7ZmlsbDojNTA5ZDk2O30uY2xzLTN7ZmlsbDojYjNiM2IzO30uY2xzLTR7c3Ryb2tlOiM1MDlkOTY7fTwvc3R5bGU+PC9kZWZzPjx0aXRsZT5SZWN1cnNvIDM8L3RpdGxlPjxnIGlkPSJDYXBhXzIiIGRhdGEtbmFtZT0iQ2FwYSAyIj48ZyBpZD0iQ2FwYV8xLTIiIGRhdGEtbmFtZT0iQ2FwYSAxIj48Y2lyY2xlIGNsYXNzPSJjbHMtMSIgY3g9IjMyLjM0IiBjeT0iMzIuMzQiIHI9IjMxLjgzIiB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMTMuMzkgMzIuMzQpIHJvdGF0ZSgtNDUpIi8+PHBhdGggY2xhc3M9ImNscy0xIiBkPSJNMTcuODQsMTYuMzFhMjEuNiwyMS42LDAsMCwxLDI5LDAiLz48cGF0aCBjbGFzcz0iY2xzLTEiIGQ9Ik0xNi4zMSw0Ni44M2EyMS42LDIxLjYsMCwwLDEsMC0yOSIvPjxwYXRoIGNsYXNzPSJjbHMtMSIgZD0iTTQ2Ljc0LDQ4LjQ0YTIxLjYsMjEuNiwwLDAsMS0yOC45LS4wOCIvPjxwYXRoIGNsYXNzPSJjbHMtMSIgZD0iTTQ4LjQzLDQ2Ljc1YTIxLjYxLDIxLjYxLDAsMCwwLC4zMi0yOC40NmwtLjQxLS40MiIvPjxwYXRoIGNsYXNzPSJjbHMtMSIgZD0iTTQyLjk0LDI0LjUzYTEzLjEyLDEzLjEyLDAsMCwxLDIuNTMsNyIvPjxwYXRoIGNsYXNzPSJjbHMtMSIgZD0iTTMzLjM5LDE5LjIxYTEzLjEsMTMuMSwwLDAsMSw2Ljc1LDIuNTIiLz48cGF0aCBjbGFzcz0iY2xzLTEiIGQ9Ik0yNC41MywyMS43M2ExMy4xMSwxMy4xMSwwLDAsMSw2LjgyLTIuNTIiLz48cGF0aCBjbGFzcz0iY2xzLTEiIGQ9Ik0xOS4yLDMxLjM5YTEzLjEyLDEzLjEyLDAsMCwxLDIuNTItNi44NiIvPjxwYXRoIGNsYXNzPSJjbHMtMSIgZD0iTTIxLjU3LDM5LjkzYTEzLjExLDEzLjExLDAsMCwxLTIuMzctNi42NSIvPjxwYXRoIGNsYXNzPSJjbHMtMSIgZD0iTTMxLjIzLDQ1LjQ2YTEzLjExLDEzLjExLDAsMCwxLTYuNy0yLjUxIi8+PHBhdGggY2xhc3M9ImNscy0xIiBkPSJNNDAsNDMuMDdhMTMuMTEsMTMuMTEsMCwwLDEtNi42OSwyLjQiLz48cGF0aCBjbGFzcz0iY2xzLTEiIGQ9Ik00NS40OCwzMy4yQTEzLjEyLDEzLjEyLDAsMCwxLDQzLjA3LDQwIi8+PHBvbHlnb24gY2xhc3M9ImNscy0yIiBwb2ludHM9IjI5Ljk2IDI5Ljk2IDMyLjM0IDMyLjM0IDkuODIgNTQuODUgMjkuOTYgMjkuOTYiLz48cG9seWdvbiBjbGFzcz0iY2xzLTEiIHBvaW50cz0iMzQuNzEgMzQuNzEgMjkuOTYgMjkuOTYgNTQuODUgOS44MiAzNC43MSAzNC43MSIvPjxwb2x5bGluZSBjbGFzcz0iY2xzLTEiIHBvaW50cz0iMzYuODMgMzIuMDkgNTQuODUgNTQuODUgMzIuMTQgMzYuODkiLz48cG9seWxpbmUgY2xhc3M9ImNscy0xIiBwb2ludHM9IjI4LjA0IDMyLjM0IDkuODIgOS44MiAzMi4zNCAyOC4wNCIvPjxwb2x5bGluZSBjbGFzcz0iY2xzLTEiIHBvaW50cz0iMzQuMDkgMzguNDQgMzIuMzQgNTMuNTUgMzAuNTMgMzguMDkiLz48cG9seWxpbmUgY2xhc3M9ImNscy0xIiBwb2ludHM9IjMwLjY5IDI2LjcxIDMyLjM0IDExLjEyIDMzLjk4IDI2LjcxIi8+PHBvbHlsaW5lIGNsYXNzPSJjbHMtMSIgcG9pbnRzPSIyNi41OCAzMC41NCAxMS4xMiAzMi4zNCAyNi41OCAzNC4xNCIvPjxwb2x5bGluZSBjbGFzcz0iY2xzLTEiIHBvaW50cz0iMzguNDUgMzMuOTYgNTMuNTUgMzIuMzQgMzcuOTYgMzAuNjkiLz48cG9seWdvbiBjbGFzcz0iY2xzLTMiIHBvaW50cz0iMzQuNzEgMzQuNzEgMzIuMzQgMzIuMzQgNTQuODUgOS44MiAzNC43MSAzNC43MSIvPjxsaW5lIGNsYXNzPSJjbHMtMSIgeDE9IjMwLjMiIHkxPSIzOC4yOCIgeDI9IjI4LjA3IiB5Mj0iNDQuNzkiLz48bGluZSBjbGFzcz0iY2xzLTEiIHgxPSIzNi4xOCIgeTE9IjQ0LjkzIiB4Mj0iMzQuMSIgeTI9IjM4LjQ0Ii8+PGxpbmUgY2xhc3M9ImNscy0xIiB4MT0iNDUiIHkxPSIzNS45NiIgeDI9IjM4LjQ1IiB5Mj0iMzMuOTYiLz48bGluZSBjbGFzcz0iY2xzLTEiIHgxPSI0NC44MiIgeTE9IjI4LjE0IiB4Mj0iMzguMDkiIHkyPSIzMC41MyIvPjxsaW5lIGNsYXNzPSJjbHMtMSIgeDE9IjM2LjYxIiB5MT0iMTkuODgiIHgyPSIzNC4xNCIgeTI9IjI2LjU4Ii8+PGxpbmUgY2xhc3M9ImNscy0xIiB4MT0iMzAuNjIiIHkxPSIyNi41MyIgeDI9IjI4LjU3IiB5Mj0iMTkuNzIiLz48bGluZSBjbGFzcz0iY2xzLTEiIHgxPSIyNi41OCIgeTE9IjMwLjUzIiB4Mj0iMTkuODEiIHkyPSIyOC4yOCIvPjxsaW5lIGNsYXNzPSJjbHMtMSIgeDE9IjI2LjQ2IiB5MT0iMzQuMjkiIHgyPSIxOS45MiIgeTI9IjM2LjczIi8+PHBvbHlnb24gY2xhc3M9ImNscy00IiBwb2ludHM9IjI5Ljk2IDI5Ljk2IDM0LjcxIDM0LjcxIDkuODIgNTQuODUgMjkuOTYgMjkuOTYiLz48L2c+PC9nPjwvc3ZnPg==) no-repeat;
    }
    select {
      width: 140px;
      font-size: 16px;
      padding: 5px 0 5px 20px;
      margin: 0 10px;
    }
  }
  .month {
    position: relative;
    height: 140px;
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
    > .square-tooltip {
      visibility: hidden;
      position: absolute;
      width: 60px;
      height: 70px;
      font-size: 9px;
    //   background: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACMAAAAVCAYAAADM+lfpAAAACXBIWXMAAAsSAAALEgHS3X78AAABl0lEQVRIx71Wy42EMAx9g6aAlJDTHvZECSlhTnumBEpICZSQElICJVBCSqCDt5cXyULMDr/FkpXEgP3G9nPmQRInxGstuECeJ75tAWTtR609gPmgv645CeQlnY39eJZJ7tWWZNFabZ7kTDIc8Fc17gXR/hE0kBzNuSM5/AeYSDIJSEdyIunMcydba4CMJPMOQLHZwZqinijSHoCTdrI500MOwCR7vIpNHkBQkGICWTbNevYN4EfnQWuvd5JAvxWbGScHWcGtvQhMUoBs6AxlwAH4MvZifKUtTLNgBn04yokzgWxJghy3svfm3bpPAm999x9rYBpoFCOcqLtkTKVvZZRfeVabelDDd/KVtd/MpiiHk/Qdvf+idjbnJD9h4/yJa8HChnljqV2pvhyEu4ee7Zmg2sYPVKyMiuoPrz57qb8OS2PulEEOg6HmmsyL+6myZTp9bStFy4kK1T9svCJwgcZGv3Bcufr7T0NK2ZhwkTQqTX7TGx43SmPG+5q4O8E81XxZoKp6ZSzeCeah/8DtYnzXMqUbsYRf9J9PP9uFv0gAAAAASUVORK5CYII=');
      color: white;
      padding: 35px 6px 0px 6px;
      z-index: 1;
      border-radius: 3px;
      
      span:nth-child(1) {
        font-weight: 900;
        text-transform: uppercase;
      }
      
      span:nth-child(2) {
        font-weight: 500;
      }
    }
    &.show-tultip > .square-tooltip {
      visibility: visible;
      a {
          color: white;
      }
    }
    &.visible > .square-tooltip {
        
      visibility: visible;
    }
  }
  
  .one {
    display: inline-block;
    
    &::before, &::after {
        content: '';
        display: block;
        position: absolute;
        width: 0;
        height: 0;
        border-style: solid;
    }
    &::before {
        top: -32px;
        border-color: transparent;
        border-width: 20px 11px;
    }
    &.hidden::before {
      border-bottom-color: $disabled !important;
    }
    &::after {
        top: -22px;
        border-color: transparent transparent #fff transparent;
        border-width: 14px 7px;
        margin-left: 4px;
    }
    .square-tooltip {
      bottom: 155px;
      margin-left: -8px;
      font-size: 8px;
    }
      
        
  }
  .two {
      display: inline-block;
      height: 15px;
      width: 15px;
      position: relative;
      bottom: 12px;
      &::after {
        content:'';
        position:absolute;
        top:0;
        left:0;
        right:0;
        bottom:0;
        -webkit-transform: rotate(45deg);
        -moz-transform: rotate(45deg);
        -o-transform: rotate(45deg);
        -ms-transform: rotate(45deg);
        transform: rotate(45deg);
        background-color: #fff;
        border: 2px solid;
    }
    .square-tooltip {
      bottom: 18px;
    }
  }
  .three {
      height: 15px;
      width: 15px;
      background-color: #fff;
      border-radius: 50%;
      display: inline-block;
      position: relative;
      bottom: 10px;
      cursor: pointer;
      border: 2px solid;

    &.hidden {
      border-color: $disabled !important;
    }
    .square-tooltip {
      bottom: 18px;
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
    border-color: $disabled;
    .month-name {
      background-color: $disabled;
    }
    &.one {
      &::after {
        border-width: 14px 8px;
        margin-left: 2px;
      }
      &::before {
        border-width: 17px 10px;
        top: -27px;
        
      }
    }
    &.two::after {
      border: 0.8px solid $disabled;
    }
    &.three {
      border: 0.8px solid $disabled;
      height: 17px;
      width: 17px;
    }
  }



</style>

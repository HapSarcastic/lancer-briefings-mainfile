<template>
  <Header :header="this.header" />
  <div class="content-container">
    <section class="section-container" id="missions" style="width:435px; height:714px;">
      <div class="section-header clipped-medium-backward">
        <img src="/icons/mission-icon.svg" />
        <h1>Mission Log</h1>
      </div>
      <div class="section-content-container">
        <h3>Current Assignment</h3>
        <Markdown :source="current_md" class="markdown" />
        <h3>Mission List</h3>
        <div class="mission-list-container">
          <Mission
            v-for="item in this.missions"
            :key="item.slug"
            :mission="item"
            :selected="this.mission_slug"
            @click="selectMission(item)"
          />
        </div>
      </div>
    </section>
    <section class="section-container" id="events" style="width:435px; height:714px;">
      <div class="section-header clipped-medium-backward">
        <img src="/icons/events-icon.svg" />
        <h1>Briefing</h1>
      </div>
      <div class="section-content-container">
        <Markdown :source="events" class="markdown" />
      </div>
    </section>
    <section class="section-container" id="pilots" style="width:894px; height:714px;">
      <div style="height:52px; overflow:hidden;">
        <div class="section-header clipped-medium-backward-pilot">
          <img src="/icons/pilot-icon.svg" />
          <h1>Pilot Roster</h1>
        </div>
        <div class="rhombus-back">&nbsp;</div>
      </div>
      <div class="section-content-container">
        <div class="pilot-list-container">
          <Pilot v-for="item in this.pilots" :key="item.slug" :pilot="item" />
        </div>
      </div>
    </section>
  </div>
  <svg
    style="visibility: hidden; position: absolute;"
    width="0"
    height="0"
    xmlns="http://www.w3.org/2000/svg"
    version="1.1"
  >
    <defs>
      <filter id="round">
        <feGaussianBlur in="SourceGraphic" stdDeviation="5" result="blur" />
        <feColorMatrix
          in="blur"
          mode="matrix"
          values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 19 -5"
          result="goo"
        />
        <feComposite in="SourceGraphic" in2="goo" operator="atop" />
      </filter>
    </defs>
  </svg>
  <audio autoplay>
    <source src="/startup.ogg" type="audio/ogg" />
  </audio>
  <Footer/>
</template>

<script>
import Header from './components/layout/Header.vue';
import Footer from './components/layout/Footer.vue';
import Mission from './components/Mission.vue';
import Pilot from './components/Pilot.vue';
import Markdown from 'vue3-markdown-it';

export default {
  components: {
    Header,
    Footer,
    Mission,
    Pilot,
    Markdown
  },

  data() {
    return {
      "mission_slug": "003-a",
      "current_md": "",
      "events": "",
      "missions": [
        {
          "slug": "001-a",
          "name": "Trapdoor Spider",
          "status": "success"
        },
        {
          "slug": "001-b",
          "name": "The Horde",
          "status": "partial-success"
        },
        {
          "slug": "002-a",
          "name": "Night Strike",
          "status": "success"
        },
        {
          "slug": "002-b",
          "name": "Reactor Defense",
          "status": "success"
        },
        {
          "slug": "002-c",
          "name": "REGRET",
          "status": "success"
        },
        {
          "slug": "003-a",
          "name": "Sepulcher",
          "status": "success"
        },
        { "slug": "003-b",
          "name": "London Bridge",
          "status": "success"
        },
        { "slug": "003-c",
          "name": "Operation: Rematch",
          "status": "success"
        },
        { "slug": "003-d",
          "name": "Roadblock",
          "status": "success"
        },
        { "slug": "004-a",
          "name": "Annelida",
          "status": "success"
        },
        { "slug": "004-b",
          "name": "Death Valley",
          "status": "success"
        },
        { "slug": "004-c",
          "name": "Abaddon",
          "status": "success"
        },
        { "slug": "005-a",
          "name": "Tactical Logic",
          "status": "success"
        },
        { "slug": "005-b",
          "name": "Tonight There's Gonna be a Prison Break",
          "status": "success"
        },
        { "slug": "005-c",
          "name": "Quiet Night",
          "status": "success"
        },
        { "slug": "005-d",
          "name": "Hitchhikers and Stays",
          "status": "success"
        },
        { "slug": "005-e",
          "name": "Attic of the World",
          "status": "success"
        },
        { "slug": "005-f",
          "name": "Split-Knuckle Haymaker",
          "status": "success"
        },
      ],    
      "pilots": [
        {
          "callsign": "Echo",
          "alias": "Charlie",
          "code": "5sQq40-ZNf50-KOU-uZm-XXFSe///MSMC-A-LOCAL-TEAM-EPSILON//4f89-bcd9-501464e960da",
          "corpro": "HA",
          "frame": "M1 Genghis-451b",
          "mech": "Against the Dying of Light"
        },
        {
          "callsign": "Lizard",
          "alias": "Ulysses de Goya",
          "code": "Union RM-4 IDENT Record: 3365dcd6-81d7-4b0b-877c-0588fc826346       iSOMETRICS: OHM C//5024-08-21T20:42:31.158Z",
          "corpro": "SSC",
          "frame": "Death's Head",
          "mech": "Wrong About Physics"
        },
        {
          "callsign": "Jack",
          "alias": "Slader Volstok",
          "code": "H3kLBd-Xgkmv-XLl-wmW-6DthU///MSMC-C-LOCAL-TEAM-EPSILON///4f89-bcd9-501464e960da",
          "corpro": "SSC",
          "frame": "Mourning Cloak",
          "mech": "Ronin"
        },
      ],
      "header": {
        "planet": "Hercynia",
        "year": "5014u",
        "system": "Ardennes-3",
        "gate": "Atlas-Quanokrim",
        "ring": "Atlas-Line",
        "headerTitle": "Mirrorsmoke",
        "headerSubtitle": "Mercenary Company",
        "subheaderTitle": "Contracted - Wonder Inc",
        "subheaderSubtitle": "Field Team Epsilon",
      },
      "options":{
        "eventsMarkdownPerMission": true
      }
    }
  },

  created() {
    this.loadMissionMarkdown()
    this.loadEventsMarkdown()
  },

  computed: {

  },

  methods: {
    selectMission(mission) {
      this.mission_slug = mission.slug;
      this.loadMissionMarkdown()
      if(this.options.eventsMarkdownPerMission){
        this.loadEventsMarkdown();
      }
    },
    loadMissionMarkdown() {
      let self = this;
      let md = `/missions/${self.mission_slug}.md`
      var client = new XMLHttpRequest();
      client.open('GET', md);
      client.onreadystatechange = function () {
        self.current_md = client.responseText;
      }
      client.send();
    },
    loadEventsMarkdown() {
      let self = this;
      let md = "";

      if(self.options.eventsMarkdownPerMission){
        md = `/events/${self.mission_slug}.md`
      }
      else {
        md = "/events.md"
      }

      var client = new XMLHttpRequest();
      client.open('GET', md);
      client.onreadystatechange = function () {
        self.events = client.responseText;
      }
      client.send();
    }
  }

}
</script>


<style lang="scss">
#app {
  width: 1902px;
  height: 910px;
  overflow: hidden;
}
</style>

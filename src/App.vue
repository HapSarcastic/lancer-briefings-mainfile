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
      "mission_slug": "001a",
      "current_md": "",
      "events": "",
      "missions": [
        {
          "slug": "001a",
          "name": "Trapdoor Spider",
          "status": "success"
        },
      ],
      "mission_slug": "001b",
      "current_md": "",
      "events": "",
      "missions": [
        {
          "slug": "001-b",
          "name": "The Horde",
          "status": "partial-success"
        },
      ],
      "mission_slug": "002a",
      "current_md": "",
      "events": "",
      "missions": [
        {
          "slug": "002a",
          "name": "Night Strike",
          "status": "start"
        },
      ],      
      "pilots": [
        {
          "callsign": "Echo",
          "alias": "Charlie",
          "code": "5sQq40-ZNf50-KOU-uZm-XXFSe///MSMC-A-LOCAL-TEAM-EPSILON//4f89-bcd9-501464e960da",
          "corpro": "GMS",
          "frame": "Everest-XV86",
          "mech": "Gory Gory"
        },
        {
          "callsign": "Lizard",
          "alias": "Ulysses de Goya",
          "code": "Union RM-4 IDENT Record: 3365dcd6-81d7-4b0b-877c-0588fc826346       iSOMETRICS: OHM C//5024-08-21T20:42:31.158Z",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "Hovel"
        },
        {
          "callsign": "Jack",
          "alias": "Slader Volstok",
          "code": "H3kLBd-Xgkmv-XLl-wmW-6DthU///MSMC-C-LOCAL-TEAM-EPSILON///4f89-bcd9-501464e960da",
          "corpro": "GMS",
          "frame": "Everest-7274",
          "mech": "74"
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

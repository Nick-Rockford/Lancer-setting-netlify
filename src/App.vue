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
        <h1>Events Log</h1>
      </div>
      <div class="section-content-container">
        <Markdown :source="events" class="markdown" />
      </div>
    </section>
    <section class="section-container" id="pilots" style="width:894px; height:714px;">
      <div style="height:52px; overflow:hidden;">
        <div class="section-header clipped-medium-backward-pilot">
          <img src="/icons/pilot-icon.svg" />
          <h1>Important individuals</h1>
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
      "mission_slug": "000",
      "current_md": "",
      "events": "",
      "missions": [
      {
          "slug": "002",
          "name": "Impending Gravity",
          "status": "start"
      },
      {
          "slug": "001",
          "name": "Vigilant Gaze",
          "status": "success"
      },
      ],
      "pilots": [
        {
        // callsign is actually Name
          "callsign": "captian brigid farris",
          "alias": "Captian, UNS Rio Grande",
          "code": "4e7a9650-8f89-4a8e-a1b3-3d9d5c7b2f9a//UNION-NCL Academy//2d795a4c-8efb-4e17-af7c-76b87a93d1f3",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "no-mech"
        },
        {
          "callsign": "1st lieutenant alex kim",
          "alias": "First Officer, UNS Rio Grande",
          "code": "7cd700cc-c990-e5468de724c4//UNION-NCL Academy//a98c3e28-bcd9-501464e960da",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "no-mech"
        },
        {
          "callsign": "RIO",
          "alias": "NHP, UNS Rio Grande",
          "code": "4069-b6c9-76437d4be455//Aether-Vault//056940c6-8d55-4190-8e85-57caa043cb1a",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "no-mech"
        },
        {
          "callsign": "SSgt Omari Garcia",
          "alias": "Staff Sargent, UNS Rio Grande",
          "code": "98ca9616-044e-b89b-aae4eb3387ec///NDL-C//6f572259-41bf-931a-e0543709e892",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "Mayfly"
        },
        {
          "callsign": "UNION Ambassador Nilan Bannerjee",
          "alias": 'Union Ambassador',
          "code": "d1fdf62e-d81e-4e10-97c8-df3bc4860117///NDL-C-DEEP-STATION//5a4254aa-9fa2-42ca-a077-8f5bfd1e1ad3",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "no-mech"
        },
      ],
      "header": {
        "planet": "Cressidium",
        "year": "5016u",
        "vessel": "UNS-CV Rio Grande",
        "captian": "Brigid Farris",
        "nhp": "Rio",
        "headerTitle": "Strikeforce",
        "headerSubtitle": ": Seraph",
        "subheaderTitle": "Crisis Response",
        "subheaderSubtitle": "'Bayonet' Protocol active",
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

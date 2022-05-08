<template>
  <div class="list row">
    <div class="col-md-6">
      <h4> Mission Commanders </h4>
      <ul class="list-group">
        <li class="list-group-item"
          :class="{ active: index == currentIndex }"
          v-for="(mc, index) in mcs"
          :key="index"
          @click="setActiveMC(mc, index)"
        >
          {{ mc.username }}
        </li>

      </ul>
    </div>
    <div class="col-md-6">
      <div v-if="currentMC">
        <h4>Mission Commander</h4>
        <div>
          <br><label><strong>Nombre:</strong></label> {{ currentMC.name}}
          <br><label><strong>Username:</strong></label> {{ currentMC.username}}
          <br><label><strong>Main Stack:</strong></label> {{ currentMC.mainStack}}
        </div>
        <router-link :to="'/missionCommander/' + currentMC.id" class="btn btn-info"> Editar</router-link>
      </div>
      <div v-else>
        <br />
        <p> Selecciona un MC.</p>
      </div>
    </div>
  </div>
</template>
<script>

import MissionCommanderService from "../services/MissionCommanderService";

export default {
  name: "missionCommanders-list",
  data() {
    return {
      tutorials: [],
      mcs: [],
      currentTutorial: null,
      currentMC: null,
      currentIndex: -1,
      title: "",
      mcId: ""
    };
  },
  methods: {
    getAllMC(){
      MissionCommanderService.getAll()
        .then(response => {
          this.mcs = response.data;
        })
        .catch(e => {
          console.log(e);
        });
    },
    refreshList() {
      this.retrieveTutorials();
      this.currentTutorial = null;
      this.currentIndex = -1;
    },
    setActiveTutorial(tutorial, index) {
      this.currentTutorial = tutorial;
      this.currentIndex = tutorial ? index : -1;
    },
    setActiveMC(mc, index) {
      this.currentMC= mc;
      this.currentIndex = mc? index : -1;
    },
    searchTitle() {
      MissionCommanderService.findByTitle(this.title)
        .then(response => {
          this.tutorials = response.data;
          this.setActiveTutorial(null);
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  mounted() {
    this.getAllMC();
  }
};
</script>


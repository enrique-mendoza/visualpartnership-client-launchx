<template>
<h4>Mission Commander</h4>
  <div v-if="currentMC" class="edit-form">
    <form>
      <div class="form-group">
        <label for="title">Nombre</label>
        <input type="text" disabled class="form-control" id="title"
          v-model="currentMC.name"
        />
      </div>
      <div class="form-group">
        <label for="title">Username</label>
        <input type="text" disabled class="form-control" id="title"
          v-model="currentMC.username"
        />
      </div>
      <div class="form-group">
        <label for="title">Main Stack</label>
        <input type="text" class="form-control" id="title"
          v-model="currentMC.mainStack"
        />
      </div>
    </form>
    <button class="btn btn-danger mr-2" @click="deleteMC">
      Eliminar  
    </button>
    <button type="submit" class="btn btn-info mr-2" @click="updateMC">
      Actualizar
    </button>
    <p>{{ message }}</p>
  </div>
  <div v-else>
    <br />
    <p> Selecciona un Mission Commander. </p>
  </div>
</template>
<script>
import MissionCommanderService from "../services/MissionCommanderService";
export default {
  name: "missionCommander",
  data() {
    return {
      currentMC: null,
      message: ''
    };
  },
  methods: {
    getMC(id) {
      MissionCommanderService.get(id)
        .then(response => {
          this.currentMC = response.data;
        })
        .catch(e => {
          console.log(e);
        });
    },
    deleteMC() {
      MissionCommanderService.delete(this.currentMC.id)
      .then(response => {
        console.log(response.data);
        this.$router.push({ name: "missionCommanders" });
      })
      .catch(e => {
          console.log(e);
      });
    },
    updateMC() {
      MissionCommanderService.update(this.currentMC.id, this.currentMC)
        .then(response => {
          console.log(response.data);
          this.message = 'Se actualizó correctamente';
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  mounted() {
    this.message = '';
    this.getMC(this.$route.params.id);
  }
};
</script>

<style>
.edit-form {
  max-width: 300px;
  margin: auto;
}
</style>


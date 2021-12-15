<template>

  <div id="Agregarcita" class="agregarcita">
    <div class="container_agregarcita">
      <h2>Pedir cita</h2>

      <form v-on:submit.prevent="processAgregarcita">
        <input
          type="text"
          v-model="createAgregarcita.paciente"
          placeholder="paciente"
        />
        <br />
        <input
          type="date"
          v-model="createAgregarcita.fechaCita"
          placeholder="fechaCita"
        />
        <br />
        <input
          type="number"
          v-model="createAgregarcita.horaCita"
          placeholder="horaCita"
        />
        <br />
        <input
          type="text"
          v-model="createAgregarcita.medico"
          placeholder="medico"
        />
        <br />
        <input
          type="text"
          v-model="createAgregarcita.especialidad"
          placeholder="especialidad"
        />
        <br />
        <button type="submit">Crear cita </button>
      </form>
    </div>
  </div>

</template>


<script>
import gql from "graphql-tag";

export default {
  name: "Agregarcita",
  //paciente: localStorage.getItem("username"),
  data: function() {
    return {
      createAgregarcita: {
        paciente: localStorage.getItem("username"),
        fechaCita: "",
        horaCita: "",
        medico: "",
        especialidad: "",
      },
    };
  },

  methods: {
    processAgregarcita: async function() {
      
      if (localStorage.getItem("token_access")  === null ||
          localStorage.getItem("token_refresh") === null ) {
        this.$emit("logOut");
        return;
      }

      localStorage.setItem("token_access", "");

      await this.$apollo
        .mutate({
          mutation: gql`
            mutation ($refresh: String!) {
              refreshToken(refresh: $refresh) {
                access
              }
            }
          `,
          variables: {
            refresh: localStorage.getItem("token_refresh"),
          },
        })
        .then((result) => {
          localStorage.setItem("token_access", result.data.refreshToken.access);
        })
        .catch((error) => {
          this.$emit("logOut");
          return;
        });
      
      await this.$apollo
        .mutate({
          mutation: gql`
            mutation Mutation($cita: CitaInput!) {
              createCita(cita: $cita) {
                paciente
                fechaCita
                horaCita
                medico
                especialidad
              }
            }
          `,
          variables: {
            cita: this.createAgregarcita,
          },
        })
        .then((result) => {
          alert("Cita agendada, Revise Historial de citas");
        })
        /*.catch((error) => {
          alert("No hay medico disponible para esta fecha");
        });*/
    },
  },
};
</script>


<style>

.agregarcita {
  margin: 0;
  padding: 0%;
  height: 100%;
  width: 100%;

  display: flex;
  justify-content: center;
  align-items: center;
}

.container_agregarcita {
  border: 3px solid #283747;
  border-radius: 10px;
  width: 25%;
  height: 60%;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.agregarcita h2 {
  color: #283747;
}

.agregarcita form {
  width: 50%;
}

.agregarcita input {
  height: 40px;
  width: 100%;

  box-sizing: border-box;
  padding: 10px 20px;
  margin: 5px 0;

  border: 1px solid #283747;
}

.agregarcita button {
  width: 100%;
  height: 40px;

  color: #e5e7e9;
  background: #283747;
  border: 1px solid #e5e7e9;

  border-radius: 5px;
  padding: 10px 25px;
  margin: 5px 0;
}

.agregarcita button:hover {
  color: #e5e7e9;
  background: crimson;
  border: 1px solid #283747;
}

</style>

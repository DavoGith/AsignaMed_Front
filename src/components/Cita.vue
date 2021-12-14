<template>

  <div id="Citas">

    <div class="container">
      <h2>
        Citas:
        <span>{{ username }}</span>
      </h2>
    </div>

    <h2>Citas</h2>     
    <div class="container-table">
        <table>
            <tr>
                <th>Paciente</th>
                <th>Fecha Cita</th>
                <th>Hora cita</th>
                <th>Médico</th>
                <th>Especialidad</th>
            </tr>

            <tr v-for="cita in citaByPaciente" :key="cita.id">
                <td>{{ cita.paciente }}</td>
                <td>{{ cita.fechaCita.date }}</td>
                <td>{{ cita.horaCita }}</td>
                <td>{{ cita.medico }}</td>
                <td>{{ cita.especialidad }} </td>
            </tr>
        </table>
    </div>
  </div>

</template>


<script>
import gql from "graphql-tag";
export default {
  name: "Cita",

  data: function () {
    return {
      username: localStorage.getItem("username") || "none",
      citaByPaciente: []
    };
  },
    apollo: {
    citaByPaciente: {
      query: gql`
        query Query($username: String!) {
          citaByPaciente(username: $username) {
            paciente
            fechaCita
            horaCita
            medico
            especialidad
            id
          }
        }
      `,
      variables() {
        return {
          username: this.username,
        };
      },
    },
  },

  created: function () {
    this.$apollo.queries.citaByPaciente.refetch();
  }
};

</script>


<style>
#Citas {
  width: 100%;

  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-direction: column;
}

#Citas .container-table{
    width:50%;
    
    max-height: 250px;
    overflow-y: scroll;
    overflow-x: hidden;
}

#Citas table {
  width: 100%;
  border-collapse: collapse;
  border: 1px solid rgba(0, 0, 0, 0.3);
  
}

#Citas table td,
#Citas table th {
  border: 1px solid #ddd;
  padding: 8px;
}

#Citas table tr:nth-child(even) {
  background-color: #f2f2f2;
}

#Citas table tr:hover {
  background-color: #ddd;
}

#Citas table th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: crimson;
  color: white;
}

#Citas > h2 {
  color: #283747;
  font-size: 25px;
}

#Citas .container {
  padding: 30px;
  border: 3px solid rgba(0, 0, 0, 0.3);
  border-radius: 20px;
  margin: 5% 0 1% 0;
}

#Citas  .container h2 {
  font-size: 25px;
  color: #283747;
}
#Citas .container span {
  color: crimson;
  font-weight: bold;
}

</style>
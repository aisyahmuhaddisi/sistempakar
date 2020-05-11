<template>
  <div id = "symptomp" class="container">
    <v-layout column>
      <v-flex xs6 m-4>
        <v-simple-table>
        <template v-slot:default>
          <thead>
            <tr>
              <th class="text-left">Gejala</th>
              <th>
                <input class="form-check-input" type="checkbox"/>
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="symptomp in symptomps" :key="symptomp.name">
              <td>{{ symptomp.name }}</td>
              <td>
                <input class="form-check-input" type="checkbox" v-model="evidence" :value="symptomp.evidence" id="defaultCheck1">
              </td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
      </v-flex>
      <div class="container m-4">
        Gejala Yang Dirasakan:
        <div v-for="(s,id) in evidence" :key="id">
          {{s}}
        </div>
      </div>
      <span class="error" v-if="error">{{error}}</span><br/><br/>
      <v-btn dark style="margin: 4px;" @click="submit">submit</v-btn>
      <v-col cols="12" justify-center >
          <v-card
            color="#385F73"
            dark
            style="margin: 4px;"
          >
            <v-card-title class="headline center">Hasil</v-card-title>

            <div class="card" v-for="(i,id) in result.probability" :key="id">
              <p>
                {{i.name}} - {{i.probability}}
              </p>
            </div>
          </v-card>
        </v-col>
    </v-layout>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',
  data () {
      return {
        symptomps: [
          {
            evidence: 'coughing_blood',
            name: 'Batuk Berdarah',
          },
          {
            evidence: 'weight_loss',
            name: 'Penurunan Berat Badan',
          },
          {
            evidence: 'lack_of_appetite',
            name: 'Tidak Nafsu Makan',
          },
          {
            evidence: 'shortness_of_breath',
            name: 'Nafas Terengah-engah',
          },
          {
            evidence: 'chest_pain',
            name: 'Nyeri Dada',
          },
          {
            evidence: 'fever',
            name: 'Demam',
          },
          {
            evidence: 'headache',
            name: 'Sakit Kepala',
          },
          {
            evidence: 'cough',
            name: 'Batuk',
          },
          {
            evidence: 'persistent_cough',
            name: 'Batuk Terus Menerus',
          },
          {
            evidence: 'night_sweats',
            name: 'Keringat Dingin',
          },
          {
            evidence: 'shaking_chills',
            name: 'Menggigil',
          },
          {
            evidence: 'fatigue',
            name: 'Kelelahan',
          },
          {
            evidence: 'digital_clubbing',
            name: 'Muntah',
          }
        ],
        evidence: [
        ],
        result: [],
        error: null,
        userEvidence: ['']
      };
    },
    methods: {
    async submit() {
      try {
        console.log(this.evidence)
        let hasil = await axios({
          url: 'http://128.199.77.28:3000/bayes',
          method: 'GET',
          params: {
            "lung_cancer_probability": 0.40,
            "tuberculosis_probability": 0.35,
            "pneumonia_probability": 0.25,
            "evidence": this.evidence
          }
        })
        console.log(hasil)
        this.result = hasil.data
      } catch (error) {
        this.error = error.response.data.error
      }
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.form /deep/ .theme--light{
  left: 0px !important;
  right: auto !important;
}
.error{
  color: red;
}
</style>

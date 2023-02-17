
<script setup lang="ts">
import { autofill } from '@mapbox/search-js-web';
import mapboxgl, { mapboxSdk } from "mapbox-gl";
// defineProps<{
//   msg: string
// }>()
</script>

<script lang="ts">
import { defineComponent } from 'vue'


export default defineComponent({
  data() {
    return {
      map: {},
      longitudeLatitudeEdit: [],
      accessToken: 'pk.eyJ1IjoiYXVyZWxpZW4wMSIsImEiOiJjbGU3Mm11Z3EwMWVzM3BuNzNnMGpvMTQzIn0.oGvJ6_6fexbc1gVZSF6KXA',
      firstname: "Jean",
      age: 30,
      annee: 2022,
      buttonVisible: false,
      firstnameEdit: '',
      lastnameEdit: '',
      ageEdit: 0,
      addressEdit: '',
      phoneNumberEdit: '',
      family: [{
        firstname: "Jojo",
        lastname: "Bernard",
        age: 25,
        address: "azer",
        phoneNumber: "tyui"
      },
      {
        firstname: "Marie",
        lastname: "Blachère",
        age: 29,
        address: "opqs",
        phoneNumber: "dfgh"
      },
      {
        firstname: "Jean",
        lastname: "Bernard",
        age: 20,
        address: "jklm",
        phoneNumber: "wxcv"
      },
      {
        firstname: "Paul",
        lastname: "Pogba",
        age: 17,
        address: "aqzs",
        phoneNumber: "edrf"
      }]
    }
  },
  methods: {
    setAge() {
      this.age++
      this.annee++
    },
    changeName() {
      if (this.firstname == "Patrick") {
        this.firstname = "Jean"
      } else {
        this.firstname = "Patrick"
      }
    },
    addMember() {
      if (this.ageEdit >= 10) {
        var memberAdd = {
          firstname: this.firstnameEdit,
          lastname: this.lastnameEdit,
          age: this.ageEdit,
          address: this.addressEdit,
          phoneNumber: this.phoneNumberEdit
        }
        this.family.push(memberAdd)
        const popup = new mapboxgl.Popup({ offset: 50 }).setText(
          '<p style="color:"black"">Nom : ' + this.firstnameEdit + ' Age : ' + this.ageEdit + '</p>'
        );
        new mapboxgl.Marker({
          color: "#FFFFFF",
          draggable: false,

        }).setLngLat(this.longitudeLatitudeEdit)
          .setPopup(popup)
          .addTo(this.map);
        this.firstnameEdit = ''
        this.lastnameEdit = ''
        this.ageEdit = 0
        this.addressEdit = ''
        this.phoneNumberEdit = ''
        this.longitudeLatitudeEdit = []
      }
    },
    deleteMember(item: any) {
      const i = this.family.indexOf(item)
      if (i > -1) {
        this.family.splice(i, 1)
      }
    }
  },
  mounted() {

    setInterval(this.setAge, 3000)
    const collection = autofill({
      accessToken: 'pk.eyJ1IjoiYXVyZWxpZW4wMSIsImEiOiJjbGU3Mm11Z3EwMWVzM3BuNzNnMGpvMTQzIn0.oGvJ6_6fexbc1gVZSF6KXA'
    }).addEventListener('retrieve', (event) => {
      const featureCollection = event.detail;
      this.longitudeLatitudeEdit = featureCollection.features[0].geometry.coordinates
    });

    mapboxgl.accessToken = this.accessToken;
    this.map = new mapboxgl.Map({
      container: "mapContainer",
      style: "mapbox://styles/mapbox/streets-v11",
      center: [103.811279, 1.345399],
      zoom: 12
    });
  }
})
</script>

<template>
  <div class="center-black">
    <form action="">
      <input v-model="firstnameEdit" placeholder="firstname" />
      <input v-model="lastnameEdit" placeholder="lastname" />
      <input type="number" required min="10" v-model="ageEdit" placeholder="age" />
      <input v-model="addressEdit" id="addressInput" type="text" autocomplete="address-line1" />
      <input type="tel" pattern="[0-9]{10}" v-model="phoneNumberEdit" required placeholder="phone number" />
    </form>
    <div id='mapContainer' style='width: 100%; height: 300px;'></div>
    <div id="presentation">
      <p>Bonjour je m'appelle {{ firstname }}</p>
    </div>
    <div id="age">
      <p>En {{ annee }}, {{ firstname }} aura {{ age }} ans</p>
    </div>
    <button v-if="buttonVisible == true" @click="changeName" id="crazy-button">
      Crazy Button
    </button>
    <div id="family" v-for="member in family.sort((p1, p2) => (p1.age < p2.age) ? -1 : (p1.age > p2.age) ? 1 : 0)">
      {{ member.firstname }} - {{ member.lastname }} - {{ member.age }} - {{ member.address }} - {{ member.phoneNumber }}
      <button id="delete" @click="deleteMember(member)"> Suppr.</button>
    </div>
    <button @click="addMember" id="add-family-member">
      Add member
    </button>
</div>
</template>

<style scoped>
.center-black {
  background-color: black;
  color: white;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {

  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>

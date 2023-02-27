<script setup lang="ts">
import { autofill } from "@mapbox/search-js-web";
import mapboxgl, { Map, type LngLatLike } from "mapbox-gl";
// defineProps<{
//   msg: string
// }>()
</script>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  data() {
    return {
      map: {} as Map,
      longitudeLatitudeEdit: {} as LngLatLike,
      accessToken:
        "pk.eyJ1IjoiYXVyZWxpZW4wMSIsImEiOiJjbGU3Mm11Z3EwMWVzM3BuNzNnMGpvMTQzIn0.oGvJ6_6fexbc1gVZSF6KXA",
      firstname: "Jean",
      age: 30,
      annee: 2022,
      buttonVisible: false,
      firstnameEdit: "",
      lastnameEdit: "",
      ageEdit: 0,
      addressEdit: "",
      phoneNumberEdit: "",
      family: [
        {
          firstname: "Jojo",
          lastname: "Bernard",
          age: 25,
          address: "azer",
          phoneNumber: "tyui",
        },
        {
          firstname: "Marie",
          lastname: "Blachère",
          age: 29,
          address: "opqs",
          phoneNumber: "dfgh",
        },
        {
          firstname: "Jean",
          lastname: "Bernard",
          age: 20,
          address: "jklm",
          phoneNumber: "wxcv",
        },
        {
          firstname: "Paul",
          lastname: "Pogba",
          age: 17,
          address: "aqzs",
          phoneNumber: "edrf",
        },
      ],
    };
  },
  methods: {
    setAge() {
      this.age++;
      this.annee++;
    },
    changeName() {
      if (this.firstname == "Patrick") {
        this.firstname = "Jean";
      } else {
        this.firstname = "Patrick";
      }
    },
    addMember() {
      if (this.ageEdit >= 10) {
        var memberAdd = {
          firstname: this.firstnameEdit,
          lastname: this.lastnameEdit,
          age: this.ageEdit,
          address: this.addressEdit,
          phoneNumber: this.phoneNumberEdit,
        };
        this.family.push(memberAdd);
        const popup = new mapboxgl.Popup({ offset: 50 }).setHTML(
          "<p style=" +
            "color:" +
            "black" +
            ">Prénom : " +
            this.firstnameEdit +
            " Age : " +
            this.ageEdit +
            "</p>"
        );
        new mapboxgl.Marker({
          color: "#FFFFFF",
          draggable: false,
        })
          .setLngLat(this.longitudeLatitudeEdit)
          .setPopup(popup)
          .addTo(this.map);
        this.firstnameEdit = "";
        this.lastnameEdit = "";
        this.ageEdit = 0;
        this.addressEdit = "";
        this.phoneNumberEdit = "";
        this.longitudeLatitudeEdit = {} as LngLatLike;
      }
    },
    deleteMember(item: any) {
      const i = this.family.indexOf(item);
      if (i > -1) {
        this.family.splice(i, 1);
      }
    },
  },
  mounted() {
    setInterval(this.setAge, 3000);
    autofill({
      accessToken:
        "pk.eyJ1IjoiYXVyZWxpZW4wMSIsImEiOiJjbGU3Mm11Z3EwMWVzM3BuNzNnMGpvMTQzIn0.oGvJ6_6fexbc1gVZSF6KXA",
    }).addEventListener("retrieve", (event) => {
      const featureCollection = event.detail;
      this.longitudeLatitudeEdit = {
        lng: featureCollection.features[0].geometry.coordinates[0],
        lat: featureCollection.features[0].geometry.coordinates[1],
      };
    });

    mapboxgl.accessToken = this.accessToken;
    this.map = new mapboxgl.Map({
      accessToken:
        "pk.eyJ1IjoiYXVyZWxpZW4wMSIsImEiOiJjbGU3Mm11Z3EwMWVzM3BuNzNnMGpvMTQzIn0.oGvJ6_6fexbc1gVZSF6KXA",
      container: "mapContainer",
      style: "mapbox://styles/mapbox/dark-v11",
      center: [103.811279, 1.345399],
      zoom: 1,
    });
  },
});
</script>

<template>
  <div class="center-black">
    <form>
      <div class="mb-6">
        <label for="firstname" class="form-label">Firstname</label>
        <input id="firstname" class="form-control" v-model="firstnameEdit" />
      </div>
      <div class="mb-6">
        <label for="lastname" class="form-label">Lastname</label>
        <input id="lastname" class="form-control" v-model="lastnameEdit" />
      </div>
      <div class="mb-6">
        <label for="age" class="form-label">Firstname</label>
        <input
          type="number"
          required
          min="10"
          id="age"
          class="form-control"
          v-model="ageEdit"
        />
      </div>
      <div class="mb-6">
        <label for="addressInput" class="form-label">Address</label>
        <input
          id="addressInput"
          class="form-control"
          v-model="addressEdit"
          autocomplete="address-line1"
        />
      </div>
      <div class="mb-6">
        <label for="phonenumber" class="form-label">Phone</label>
        <input
          id="phonenumber"
          class="form-control"
          v-model="phoneNumberEdit"
        />
      </div>
      <button @click="addMember" type="submit" class="btn btn-primary">
        Add member
      </button>
    </form>
    <div id="mapContainer" style="width: 100%; height: 300px"></div>
    <div id="presentation">
      <p>Bonjour je m'appelle {{ firstname }}</p>
    </div>
    <div id="age">
      <p>En {{ annee }}, {{ firstname }} aura {{ age }} ans</p>
    </div>
    <button v-if="buttonVisible == true" @click="changeName" id="crazy-button">
      Crazy Button
    </button>
    <div
      id="family"
      v-for="member in family.sort((p1, p2) =>
        p1.age < p2.age ? -1 : p1.age > p2.age ? 1 : 0
      )"
      :key="member.phoneNumber"
    >
      {{ member.firstname }} - {{ member.lastname }} - {{ member.age }} -
      {{ member.address }} - {{ member.phoneNumber }}
      <button id="delete" @click="deleteMember(member)">Suppr.</button>
    </div>
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

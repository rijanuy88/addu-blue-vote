<template>
  <div>
    <div class="row">
      <div class="col">
        <div class="mb-3">
          <label for="name" class="form-label">Name</label>
          <input
            v-model="poi.name"
            type="text"
            class="form-control"
            :class="{
              'is-invalid': poi.errors.name,
            }"
            id="name"
            placeholder="Name"
          />
          <div class="invalid-feedback">{{ poi.errors.name }}</div>
        </div>
        <div class="mb-3">
          <label for="description" class="form-label">Description</label>
          <textarea
            id="description"
            cols="30"
            rows="4"
            class="form-control"
            :class="{
              'is-invalid': poi.errors.description,
            }"
            v-model="poi.description"
            placeholder="Description"
          ></textarea>
          <div class="invalid-feedback">{{ poi.errors.description }}</div>
        </div>
        <div class="mb-3">
          <label for="image" class="form-label">Image Preview</label>
          <input
            id="image"
            @change="handleImage"
            class="form-control"
            :class="{
              'is-invalid': poi.errors.image,
            }"
            type="file"
          />
          <div class="invalid-feedback">{{ poi.errors.image }}</div>
        </div>
      </div>
      <div class="col">
        <div class="mb-3">
          <label for="point_of_interest_type_id" class="form-label">Type</label>
          <select
            class="form-select"
            :class="{
              'is-invalid': poi.errors.point_of_interest_type_id,
            }"
            id="point_of_interest_type_id"
            aria-label="PoI Type Select"
            v-model="poi.point_of_interest_type_id"
          >
            <option selected disabled :value="null">Select a type</option>
            <option v-for="type in poi_types" :key="type.id" :value="type.id">
              {{ type.type }}
            </option>
          </select>
          <div class="invalid-feedback">
            {{ poi.errors.point_of_interest_type_id }}
          </div>
        </div>
        <div class="mb-3">
          <label for="location_id" class="form-label">Locations</label>
          <n-tree-select
            id="location_id"
            v-model:value="poi.location_id"
            :class="{ 'is-invalid': poi.location_id }"
            :options="locationOptions"
          ></n-tree-select>
          <div class="invalid-feedback">{{ poi.errors.location_id }}</div>
        </div>
        <div class="row">
          <div class="col">
            <div class="mb-3">
              <label for="longitude" class="form-label">Longitude</label>
              <input
                v-model="poi.longitude"
                type="text"
                class="form-control bg-white"
                :class="{
                  'is-invalid': poi.errors.longitude,
                }"
                id="longitude"
                readonly
                placeholder="Longitude"
              />
              <div class="invalid-feedback">{{ poi.errors.longitude }}</div>
            </div>
          </div>
          <div class="col">
            <div class="mb-3">
              <label for="latitude" class="form-label">Latitude</label>
              <input
                v-model="poi.latitude"
                type="text"
                class="form-control bg-white"
                readonly
                :class="{
                  'is-invalid': poi.errors.latitude,
                }"
                id="latitude"
                placeholder="Latitude"
              />
              <div class="invalid-feedback">{{ poi.errors.latitude }}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="row">
      <label for="mapPicker" class="form-label">Pick a location</label>
      <MapPicker
        v-model:longitude="poi.longitude"
        v-model:latitude="poi.latitude"
        id="mapPicker"
      ></MapPicker>
    </div>
  </div>
</template>

<script>
import MapPicker from "../Map/MapPicker.vue";
import { NTreeSelect } from "naive-ui";

export default {
  components: { MapPicker, NTreeSelect },
  props: {
    poi: {
      type: Object,
      default: () => ({
        name: null,
        description: null,
        longitude: 125.61315274255003,
        latitude: 7.071250196247246,
        district_id: null,
        point_of_interest_type_id: null,
        image: null,
        errors: {
          name: "",
          description: "",
          longitude: "",
          latitude: "",
          district_id: "",
          point_of_interest_type_id: "",
          image: "",
        },
      }),
    },
    locations: {
      type: Array,
      default: [],
    },
    poi_types: {
      type: Array,
      default: [],
    },
  },
  methods: {
    handleImage(event) {
      this.$emit("update:image", event.target.files[0]);
    },
  },
  computed: {
    locationOptions() {
      return this.locations.map((region) => ({
        key: region.id,
        label: region.name,
        children: [
          {
            key: -1,
            label: "Provinces",
            disabled: true,
          },
          ...region.children.map((province) => ({
            key: province.id,
            label: province.name,
            children: [
              {
                key: -1,
                label: "Districts",
                disabled: true,
              },
              ...province.children.map((district) => ({
                key: district.id,
                label: district.name,
                children: [
                  {
                    key: -1,
                    label: "Cities/Municipalities",
                    disabled: true,
                  },
                  ...district.children.map((city) => ({
                    key: city.id,
                    label: city.name,
                  })),
                ],
              })),
            ],
          })),
        ],
      }));
    },
  },
};
</script>

<style></style>

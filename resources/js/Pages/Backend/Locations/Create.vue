<template>
  <Head title="Create Location"></Head>

  <DashboardLayout>
    <div class="rounded container" style="background-color: #ccdfff">
      <div class="d-flex flex-column">
        <form @submit.prevent="location.post(route('locations.store'))">
          <h1 class="p-3">Create Location</h1>

          <div class="p-3">
            <LocationForm
              v-model:location="location"
              :locations="locations"
              :types="types"
            ></LocationForm>
          </div>
          <div class="btn-group mx-auto w-100" role="group">
            <div class="p-3">
              <button class="btn btn-primary">Save</button>
            </div>
            <div class="p-3">
              <Link :href="route('locations.index')" class="btn btn-danger"
                >Cancel</Link
              >
            </div>
          </div>
        </form>
      </div>
    </div>
  </DashboardLayout>
</template>

<script>
import { Link, Head } from "@inertiajs/inertia-vue3";
import DashboardLayout from "@/Layouts/DashboardLayout.vue";
import LocationForm from "@/Components/Locations/LocationForm.vue";
import { useForm } from "@inertiajs/inertia-vue3";
import AdminLayout from "@/Layouts/admin";

export default {
  props: {
    locations: Object,
    types: Array,
  },
  components: { Link, Head, DashboardLayout, LocationForm, AdminLayout },
  data: () => ({
    location: useForm({
      name: "",
      slug: "",
      description: "",
      color: "#ffffff",
      longitude: null,
      latitude: null,
      location_type_id: null,
      parent_location_id: null,
      media: null,
    }),
  }),
};
</script>

<style></style>

<template>
  <div class="flex h-screen overflow-hidden">
    <!-- Sidebar -->
    <Sidebar :sidebar-open="sidebarOpen" @close-sidebar="sidebarOpen = false" />

    <!-- Content area -->
    <div
      class="relative flex flex-col flex-1 overflow-y-auto overflow-x-hidden"
    >
      <!-- Site header -->
      <Header
        :sidebar-open="sidebarOpen"
        @toggle-sidebar="sidebarOpen = !sidebarOpen"
      />

      <main>
        <Banner v-if="updated" type="success" class="mb-4" :open="true">
          User information updated successfully.
        </Banner>

        <Banner v-if="deleted" type="success" class="mb-4" :open="true">
          User deleted successfully.
        </Banner>

        <Banner v-if="!user.isValide" type="warning" :open="!user.isValide">
          This user is not yet validated
        </Banner>
        <div
          class="px-4 sm:px-6 lg:px-8 py-4 w-full max-w-9xl mx-auto"
          style="background-color: #f1f5f9"
        >
          <!-- Page header -->
          <div class="mb-5 flex justify-between">
            <!-- Title -->
            <h1 class="text-2xl md:text-3xl text-slate-800 font-bold">
              Account Settings ✨
            </h1>
            <button
              class="btn border-slate-200 hover:border-slate-300 text-rose-500"
              @click="modaDeletelOpen = true"
            >
              <svg class="w-4 h-4 fill-current shrink-0" viewBox="0 0 16 16">
                <path
                  d="M5 7h2v6H5V7zm4 0h2v6H9V7zm3-6v2h4v2h-1v10c0 .6-.4 1-1 1H2c-.6 0-1-.4-1-1V5H0V3h4V1c0-.6.4-1 1-1h6c.6 0 1 .4 1 1zM6 2v1h4V2H6zm7 3H3v9h10V5z"
                />
              </svg>
              <span class="ml-2">Delete</span>
            </button>
          </div>

          <!-- Content -->
          <div class="bg-white shadow-lg rounded-sm mb-5">
            <div class="flex flex-col md:flex-row md:-mr-px">
              <SettingsSidebar />

              <div class="grow">
                <!-- Panel body -->
                <div class="p-6 space-y-6">
                  <!-- Picture -->
                  <section>
                    <div class="flex items-center">
                      <div class="mr-4">
                        <img
                          class="w-20 h-20 rounded-full"
                          src="@/images/useravatar.png"
                          width="80"
                          height="80"
                          alt="User upload"
                        />
                      </div>

                      <div class="mr-4">
                        <p class="">
                          Name : {{ user.firstname }} {{ user.lastname }}
                        </p>
                        <span class="">Email : {{ user.email }}</span>
                      </div>
                    </div>
                  </section>

                  <form>
                    <div class="space-y-2">
                      <!-- 1st row -->
                      <div class="md:flex space-y-1 md:space-y-0 md:space-x-4">
                        <div class="flex-1">
                          <label
                            class="block text-sm font-medium mb-1"
                            for="firstname"
                            >First Name</label
                          >
                          <input
                            id="firstname"
                            v-model.trim="user.firstname"
                            class="form-input w-full"
                            type="text"
                          />
                          <div
                            v-if="errors.name"
                            class="text-xs mt-1 text-rose-500"
                          >
                            {{ errors.name }}
                          </div>
                        </div>
                        <div class="flex-1">
                          <label
                            class="block text-sm font-medium mb-1"
                            for="lastname"
                            >Last Name</label
                          >
                          <input
                            id="lastname"
                            v-model.trim="user.lastname"
                            class="form-input w-full"
                            type="text"
                          />

                          <div
                            v-if="errors.familyname"
                            class="text-xs mt-1 text-rose-500"
                          >
                            {{ errors.familyname }}
                          </div>
                        </div>

                        <div class="flex-1">
                          <label
                            class="block text-sm font-medium mb-1"
                            for="address"
                            >E-mail:</label
                          >
                          <input
                            id="address"
                            v-model.trim="user.email"
                            class="form-input w-full"
                            type="text"
                          />

                          <div
                            v-if="emailExist"
                            class="text-xs mt-1 text-rose-500"
                          >
                            this mail already exists
                          </div>
                        </div>
                      </div>
                      <!-- 2nd row -->
                      <div class="md:flex space-y-4 md:space-y-0 md:space-x-4">
                        <div class="flex-1">
                          <label
                            class="block text-sm font-medium mb-1"
                            for="card-city"
                            >Phone Number</label
                          >
                          <input
                            id="card-city"
                            v-model.trim="user.phoneNumber"
                            class="form-input w-full"
                            type="text"
                          />
                          <div v-if="error" class="text-xs mt-1 text-rose-500">
                            {{ error }}
                          </div>
                        </div>
                      </div>
                      <!-- 3rd row -->

                      <!-- Divider -->
                      <hr class="my-2 border-t border-slate-200" />

                      <!-- Street Address -->
                      <div>
                        <label
                          class="block text-sm font-medium mb-1"
                          for="street"
                          >Edit Address:
                          <span class="text-rose-500">*</span></label
                        >
                        <input
                          id="street"
                          v-model.trim="street"
                          autoComplete="none"
                          class="form-input w-full"
                          type="text"
                          @input="searchStreet($event)"
                        />

                        <div v-if="isAddressLoading" class="">
                          <svg
                            class="animate-spin w-4 h-4 fill-current shrink-0"
                            viewBox="0 0 16 16"
                          >
                            <path
                              d="M8 16a7.928 7.928 0 01-3.428-.77l.857-1.807A6.006 6.006 0 0014 8c0-3.309-2.691-6-6-6a6.006 6.006 0 00-5.422 8.572l-1.806.859A7.929 7.929 0 010 8c0-4.411 3.589-8 8-8s8 3.589 8 8-3.589 8-8 8z"
                            />
                          </svg>
                        </div>

                        <div
                          v-for="searchedAddress in searchedAddresses"
                          v-else-if="searchedAddresses"
                          :key="searchedAddress.properties.id"
                        >
                          <div
                            class="text-gray-900 bg-white border border-gray-200 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                          >
                            <button
                              type="button"
                              class="relative inline-flex items-center w-full px-4 py-2 text-sm font-medium border-b border-gray-200 rounded-t-lg hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-2 focus:ring-blue-700 focus:text-blue-700 dark:border-gray-600 dark:hover:bg-gray-600 dark:hover:text-white dark:focus:ring-gray-500 dark:focus:text-white"
                              @click="setAddress(searchedAddress)"
                            >
                              {{
                                searchedAddress.properties.label.substring(
                                  0,
                                  30
                                ) + "..."
                              }}
                            </button>
                          </div>
                        </div>

                        <div
                          v-if="errors.street"
                          class="text-xs mt-1 text-rose-500"
                        >
                          {{ errors.street }}
                        </div>
                      </div>
                      <!-- 4th row -->
                      <div class="md:flex space-y-4 md:space-y-0 md:space-x-4">
                        <div class="flex-1">
                          <label
                            class="block text-sm font-medium mb-1"
                            for="card-vat"
                            >Street Address:</label
                          >
                          <input
                            id="card-vat"
                            v-model="user.adresse"
                            class="form-input w-full cursor-not-allowed"
                            type="text"
                            disabled
                          />
                        </div>
                        <div class="flex-1">
                          <label
                            class="block text-sm font-medium mb-1"
                            for="card-vat"
                            >City</label
                          >
                          <input
                            id="card-vat"
                            v-model="user.city"
                            class="form-input w-full cursor-not-allowed"
                            type="text"
                            disabled
                          />
                        </div>
                        <div class="flex-1">
                          <label
                            class="block text-sm font-medium mb-1"
                            for="card-postcode"
                            >Postcode</label
                          >

                          <input
                            id="card-postcode"
                            v-model="user.codeCity"
                            class="form-input w-full cursor-not-allowed"
                            type="text"
                            disabled
                          />
                        </div>
                      </div>
                    </div>

                    <div
                      class="flex flex-col px-6 py-5 border-t border-slate-200"
                    >
                      <div class="flex self-end">
                        <button
                          class="btn bg-indigo-500 hover:bg-indigo-600 text-white ml-3"
                          @click.prevent="updateUser"
                        >
                          Save Changes
                        </button>
                      </div>
                    </div>
                  </form>
                </div>
              </div>
            </div>
          </div>
        </div>
        <ModalBasic id="danger-modal" :modal-open="modaVlidateOpen">
          <div class="p-5 flex w-full space-x-4">
            <!-- Icon -->
            <div
              class="w-10 h-10 rounded-full flex items-center justify-center shrink-0 bg-rose-100"
            >
              <svg
                class="w-4 h-4 shrink-0 fill-current text-rose-500"
                viewBox="0 0 16 16"
              >
                <path
                  d="M8 0C3.6 0 0 3.6 0 8s3.6 8 8 8 8-3.6 8-8-3.6-8-8-8zm0 12c-.6 0-1-.4-1-1s.4-1 1-1 1 .4 1 1-.4 1-1 1zm1-3H7V4h2v5z"
                />
              </svg>
            </div>
            <!-- Content -->
            <div>
              <!-- Modal header -->
              <div class="mb-2">
                <div class="text-lg font-semibold text-slate-800">
                  Validate the User?
                </div>
              </div>
              <!-- Modal content -->
              <div class="text-sm mb-10">
                <div class="">
                  <p>Are you sure you want to valid this User?</p>
                </div>
              </div>
              <!-- Modal footer -->
            </div>
          </div>

          <div class="flex flex-wrap justify-end space-x-2 m-6">
            <button
              class="btn-sm border-slate-200 hover:border-slate-300 text-slate-600"
              @click.stop="modaVlidateOpen = false"
            >
              Cancel
            </button>
            <button
              class="btn-sm bg-rose-500 hover:bg-rose-600 text-white"
              @click="validuser"
            >
              Yes, valid the user
            </button>
          </div>
        </ModalBasic>

        <ModalBasic id="danger-modal" :modal-open="modaDeletelOpen">
          <div class="p-5 flex w-full space-x-4">
            <!-- Icon -->
            <div
              class="w-10 h-10 rounded-full flex items-center justify-center shrink-0 bg-rose-100"
            >
              <svg
                class="w-4 h-4 shrink-0 fill-current text-rose-500"
                viewBox="0 0 16 16"
              >
                <path
                  d="M8 0C3.6 0 0 3.6 0 8s3.6 8 8 8 8-3.6 8-8-3.6-8-8-8zm0 12c-.6 0-1-.4-1-1s.4-1 1-1 1 .4 1 1-.4 1-1 1zm1-3H7V4h2v5z"
                />
              </svg>
            </div>
            <!-- Content -->
            <div>
              <!-- Modal header -->
              <div class="mb-2">
                <div class="text-lg font-semibold text-slate-800">
                  Delete User?
                </div>
              </div>
              <!-- Modal content -->
              <div class="text-sm mb-10">
                <div class="">
                  <p>Are you sure you want to delete this User?</p>
                </div>
              </div>
              <!-- Modal footer -->
            </div>
          </div>

          <div class="flex flex-wrap justify-end space-x-2 m-6">
            <button
              class="btn-sm border-slate-200 hover:border-slate-300 text-slate-600"
              @click.stop="modaDeletelOpen = false"
            >
              Cancel
            </button>
            <button
              class="btn-sm bg-rose-500 hover:bg-rose-600 text-white"
              @click="deleteItem"
            >
              Yes, Delete it
            </button>
          </div>
        </ModalBasic>
      </main>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import Sidebar from "@/partials/Sidebar.vue";
import Header from "@/partials/Header.vue";
import SettingsSidebar from "@/partials/settings/SettingsSidebarAdmin.vue";
import Banner from "@/components/Banner.vue";
import ModalBasic from "@/components/Modal.vue";
import axios from "axios";

import {
  phoneValidation,
  passwordValidation,
  emailValidation,
} from "@/utils/utils-common-function";

export default {
  name: "Account",
  components: {
    Sidebar,
    Header,
    SettingsSidebar,
    Banner,
    ModalBasic,
  },
  setup() {
    const sidebarOpen = ref(false);

    return {
      sidebarOpen,
    };
  },
  data() {
    return {
      searchedAddresses: [],
      isAddressLoading: false,
      modaVlidateOpen: false,
      modaDeletelOpen: false,
      user: {
        firstname: "",
        lastname: "",
        email: "",
        adresse: "",
        city: "",
        codeCity: "",
        phoneNumber: "",
        street: "",
      },

      errors: {
        name: "",
        familyname: "",
        phoneNumber: "",
        email: "",
        street: "",
      },

      error: null,
      emailExist: false,

      updated: false,
    };
  },
  async created() {
    if (!this.$store.getters["auth/isAuthenticated"]) {
      this.$router.push("/");
    }
    const id = document.URL.substring(document.URL.lastIndexOf("/") + 1);

    const token = this.$store.getters["auth/token"];

    // const response = await axios.get(`${import.meta.env.VITE_API_URL}/users?page=${page.value}`, {
    const response = await axios.get(
      `${import.meta.env.VITE_API_URL}/getoneuser/${id}`,
      {
        headers: {
          Authorization: `Bearer ${token}`,
        },
      }
    );

    /*if(response.data["hydra:member"]){
          customers.value = await response.data["hydra:member"];
        }*/

    if (response.data) {
      this.user = response.data;
    }
  },

  methods: {
    async validuser() {
      const token = this.$store.getters["auth/token"];

      /*
        
        try {const response = await axios.delete(`${import.meta.env.VITE_API_URL}/insurance/${this.contract['_id']}` , {
              headers: {
                  'Authorization': `Bearer ${token}`
              }
          })
        this.modaDeletelOpen=false
        this.deleted=true
        }
        catch(e){
         this. modaDeletelOpen=false
        this.deleted=false
        }
        */

      this.modaVlidateOpen = false;
    },

    setAddress(address) {
      this.city = address.properties.city;
      this.postalCode = Number(address.properties.postcode);
      this.street = address.properties.name;
      this.searchedAddresses = [];
    },

    async searchStreet(event) {
      this.isAddressLoading = true;
      try {
        const street = event.target.value;
        if (street.length <= 3) {
          return;
        }
        const response = await fetch(
          `https://api-adresse.data.gouv.fr/search/?q=` +
            new URLSearchParams(street)
        );

        const data = await response.json();
        if (data.features.length) {
          this.searchedAddresses = data.features;
        }
        if (data.features.length == 0) {
          this.errors.address = "There is no address !";
          this.isAddressLoading = false;
          return;
        }
      } catch (error) {
        this.error = error.message || "Failed to search for the given street";
      }
      this.isAddressLoading = false;
    },

    async deleteItem() {
      const token = this.$store.getters["auth/token"];

      const id = document.URL.substring(document.URL.lastIndexOf("/") + 1);

      try {
        const response = await axios.delete(
          `${import.meta.env.VITE_API_URL}/auth/delete-user/${id}`,
          {
            headers: {
              Authorization: `Bearer ${token}`,
            },
          }
        );

        if (response.ok) {
          const redirectUrl =
            "/" + (this.$route.query.redirect || "dashboard/users");
          this.$router.replace(redirectUrl);
        }

        this.modaDeletelOpen = false;
        this.deleted = true;
      } catch (e) {
        this.modaDeletelOpen = false;
        this.deleted = false;
      }
    },

    async updateUser() {
      try {
        // Get the form data from the inputs

        if (!this.user.firstname) {
          this.errors.name = "Veuillez revérifier votre nom";
          return;
        }
        if (!this.user.lastname) {
          this.errors.familyname = "Veuillez revérifier votre prénom";
          return;
        }

        if (!emailValidation(this.user.email)) {
          this.errors.email = "Veuillez revérifier votre email s'il est valide";
          return;
        }

        if (!phoneValidation(this.user.phoneNumber)) {
          this.error =
            "Veuillez revérifier votre numéro de téléphone s'il est valide";
          return;
        }

        const id = this.$store.getters["auth/id"];

        const data = {
          id: id,
          email: this.user.email,
          firstname: this.user.firstname,
          lastname: this.user.lastname,
          phoneNumber: this.user.phoneNumber,
        };

        try {
          const token = this.$store.getters["auth/token"];
          const response = await axios.put(
            `${import.meta.env.VITE_API_URL}/auth/update-user/`,
            data,
            {
              headers: {
                Authorization: `Bearer ${token}`,
              },
            }
          );
        } catch (error) {
          console.log(error);
          if ((error.message = "Request failed with status code 500"))
            this.emailExist = true;
          return;
        }
      } catch (error) {
        console.log(error);
      }

      this.updated = true;
    },
  },
};
</script>

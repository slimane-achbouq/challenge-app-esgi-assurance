<template>
  <div
    class="flex flex-nowrap overflow-x-scroll no-scrollbar md:block md:overflow-auto px-3 py-6 border-b md:border-b-0 md:border-r border-slate-200 min-w-60 md:space-y-3"
  >
    <!-- Group 1 -->
    <div>
      <div class="text-xs font-semibold text-slate-400 uppercase mb-3">
        All pages
      </div>
      <ul class="flex flex-nowrap md:block mr-3 md:mr-0">
        <li class="mr-0.5 md:mr-0 md:mb-0.5">
          <a
            class="flex items-center px-2.5 py-2 rounded whitespace-nowrap"
            :class="isExactActive && 'bg-indigo-50'"
            :href="href"
            @click="navigate"
          >
            <svg
              class="w-4 h-4 shrink-0 fill-current text-slate-400 mr-2"
              :class="isExactActive && 'text-indigo-400'"
              viewBox="0 0 16 16"
            >
              <path
                d="M12.311 9.527c-1.161-.393-1.85-.825-2.143-1.175A3.991 3.991 0 0012 5V4c0-2.206-1.794-4-4-4S4 1.794 4 4v1c0 1.406.732 2.639 1.832 3.352-.292.35-.981.782-2.142 1.175A3.942 3.942 0 001 13.26V16h14v-2.74c0-1.69-1.081-3.19-2.689-3.733zM6 4c0-1.103.897-2 2-2s2 .897 2 2v1c0 1.103-.897 2-2 2s-2-.897-2-2V4zm7 10H3v-.74c0-.831.534-1.569 1.33-1.838 1.845-.624 3-1.436 3.452-2.422h.436c.452.986 1.607 1.798 3.453 2.422A1.943 1.943 0 0113 13.26V14z"
              />
            </svg>
            <router-link class="block" to="/profile">
              <span
                class="text-sm font-medium text-slate-600"
                :class="
                  isExactActive ? 'text-indigo-500' : 'hover:text-slate-700'
                "
                >Profile</span
              >
            </router-link>
          </a>
        </li>

        <li class="mr-0.5 md:mr-0 md:mb-0.5">
          <a
            class="flex items-center px-2.5 py-2 rounded whitespace-nowrap"
            :class="isExactActive && 'bg-indigo-50'"
            :href="href"
            @click="navigate"
          >
            <svg
              class="w-4 h-4 shrink-0 fill-current text-slate-400 mr-2"
              :class="isExactActive && 'text-indigo-400'"
              viewBox="0 0 16 16"
            >
              <path
                d="M14.3.3c.4-.4 1-.4 1.4 0 .4.4.4 1 0 1.4l-8 8c-.2.2-.4.3-.7.3-.3 0-.5-.1-.7-.3-.4-.4-.4-1 0-1.4l8-8zM15 7c.6 0 1 .4 1 1 0 4.4-3.6 8-8 8s-8-3.6-8-8 3.6-8 8-8c.6 0 1 .4 1 1s-.4 1-1 1C4.7 2 2 4.7 2 8s2.7 6 6 6 6-2.7 6-6c0-.6.4-1 1-1z"
              />
            </svg>
            <router-link class="block" to="/claims">
              <span
                class="text-sm font-medium text-slate-600"
                :class="
                  isExactActive ? 'text-indigo-500' : 'hover:text-slate-700'
                "
                >My claims</span
              >
            </router-link>
          </a>
        </li>
        <li class="mr-0.5 md:mr-0 md:mb-0.5">
          <a
            class="flex items-center px-2.5 py-2 rounded whitespace-nowrap"
            :class="isExactActive && 'bg-indigo-50'"
            :href="href"
            @click="navigate"
          >
            <svg
              class="w-4 h-4 shrink-0 fill-current text-slate-400 mr-2"
              :class="isExactActive && 'text-indigo-400'"
              viewBox="0 0 16 16"
            >
              <path
                d="M3.414 2L9 7.586V16H7V8.414l-5-5V6H0V1a1 1 0 011-1h5v2H3.414zM15 0a1 1 0 011 1v5h-2V3.414l-3.172 3.172-1.414-1.414L12.586 2H10V0h5z"
              />
            </svg>
            <router-link class="block" to="/user-quotes">
              <span
                class="text-sm font-medium text-slate-600"
                :class="
                  isExactActive ? 'text-indigo-500' : 'hover:text-slate-700'
                "
                >My quotes</span
              >
            </router-link>
          </a>
        </li>
        <li class="mr-0.5 md:mr-0 md:mb-0.5">
          <a
            class="flex items-center px-2.5 py-2 rounded whitespace-nowrap"
            :class="isExactActive && 'bg-indigo-50'"
            :href="href"
            @click="navigate"
          >
            <svg
              class="w-4 h-4 shrink-0 fill-current text-slate-400 mr-2"
              :class="isExactActive && 'text-indigo-400'"
              viewBox="0 0 16 16"
            >
              <path
                d="M5 9h11v2H5V9zM0 9h3v2H0V9zm5 4h6v2H5v-2zm-5 0h3v2H0v-2zm5-8h7v2H5V5zM0 5h3v2H0V5zm5-4h11v2H5V1zM0 1h3v2H0V1z"
              />
            </svg>
            <router-link class="block" to="/user-contracts">
              <span
                class="text-sm font-medium text-slate-600"
                :class="
                  isExactActive ? 'text-indigo-500' : 'hover:text-slate-700'
                "
                >My contracts</span
              >
            </router-link>
          </a>
        </li>
        <li v-if="role == 'User'" class="mr-0.5 md:mr-0 md:mb-0.5">
          <a
            class="flex items-center px-2.5 py-2 rounded whitespace-nowrap"
            :class="isExactActive && 'bg-indigo-50'"
            :href="href"
            @click="navigate"
          >
            <svg
              class="w-4 h-4 shrink-0 fill-current text-slate-400 mr-2"
              :class="isExactActive && 'text-indigo-400'"
              viewBox="0 0 16 16"
            >
              <path
                d="M15 4c.6 0 1 .4 1 1v10c0 .6-.4 1-1 1H3c-1.7 0-3-1.3-3-3V3c0-1.7 1.3-3 3-3h7c.6 0 1 .4 1 1v3h4zM2 3v1h7V2H3c-.6 0-1 .4-1 1zm12 11V6H2v7c0 .6.4 1 1 1h11zm-3-5h2v2h-2V9z"
              />
            </svg>
            <router-link class="block" to="/settings/paiments">
              <span
                class="text-sm font-medium text-slate-600"
                :class="
                  isExactActive ? 'text-indigo-500' : 'hover:text-slate-700'
                "
                >My Card</span
              >
            </router-link>
          </a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { useStore } from "vuex";
import { ref } from "vue";

export default {
  name: "SettingsSidebar",
  setup() {
    const store = useStore();
    const role = ref(null);

    role.value = store.getters["auth/roles"];

    return {
      role,
    };
  },
};
</script>

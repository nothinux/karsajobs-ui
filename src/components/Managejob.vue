<template>
    <div class="manage-job">
    <notificationGroup group="success">
    <div class="fixed inset-0 flex px-4 py-6 pointer-events-none p-6 items-start justify-end">
    <div class="max-w-sm w-full">
      <notification v-slot="{notifications}">
        <div
          class="flex max-w-sm w-full mx-auto bg-white shadow-md rounded-lg overflow-hidden mt-4"
          v-for="notification in notifications"
          :key="notification.id"
        >
          <div class="flex justify-center items-center w-12 bg-green-500">
            <svg
              class="h-6 w-6 fill-current text-white"
              viewBox="0 0 40 40"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M20 3.33331C10.8 3.33331 3.33337 10.8 3.33337 20C3.33337 29.2 10.8 36.6666 20 36.6666C29.2 36.6666 36.6667 29.2 36.6667 20C36.6667 10.8 29.2 3.33331 20 3.33331ZM16.6667 28.3333L8.33337 20L10.6834 17.65L16.6667 23.6166L29.3167 10.9666L31.6667 13.3333L16.6667 28.3333Z"
              />
            </svg>
          </div>

          <div class="-mx-3 py-2 px-4">
            <div class="mx-3">
              <span class="text-green-500 font-semibold">{{notification.title}}</span>
              <p class="text-gray-600 text-sm">{{notification.text}}</p>
            </div>
          </div>
        </div>
     </notification>
   </div>
   </div>
   </notificationGroup>
   <notificationGroup group="error">
    <div
        class="fixed inset-0 flex px-4 py-6 pointer-events-none p-6 items-start justify-end"
    >
        <div class="max-w-sm w-full">
        <notification v-slot="{notifications}">
            <div
            class="flex max-w-sm w-full mx-auto bg-white shadow-md rounded-lg overflow-hidden mt-4"
            v-for="notification in notifications"
            :key="notification.id"
            >
            <div class="flex justify-center items-center w-12 bg-red-500">
                <svg class="h-6 w-6 fill-current text-white" viewBox="0 0 40 40" xmlns="http://www.w3.org/2000/svg">
                    <path d="M20 3.36667C10.8167 3.36667 3.3667 10.8167 3.3667 20C3.3667 29.1833 10.8167 36.6333 20 36.6333C29.1834 36.6333 36.6334 29.1833 36.6334 20C36.6334 10.8167 29.1834 3.36667 20 3.36667ZM19.1334 33.3333V22.9H13.3334L21.6667 6.66667V17.1H27.25L19.1334 33.3333Z"/>
                </svg>
            </div>

            <div class="-mx-3 py-2 px-4">
                <div class="mx-3">
                    <span class="text-red-500 font-semibold">{{notification.title}}</span>
                    <p class="text-gray-600 text-sm">{{notification.text}}</p>
                </div>
            </div>
            </div>
        </notification>
        </div>
    </div>
   </notificationGroup>
    <div class="flex space-x-2">
        <div class="w-3/12">
        <ul class="list-none flex flex-col justify-between">
            <li>
                <router-link class="flex items-center py-2 px-2 mb-2 bg-gray-50 rounded-md font-medium" to="/manage" exact>
                <svg class="w-5 mr-2" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z" />
                </svg>
                    New Job
                </router-link>
            </li>
            <li>
                <router-link class="flex items-center py-2 px-2 mb-2 bg-gray-50 rounded-md font-medium" to="/manage-job" exact>
                <svg class="w-5 mr-2" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 13.255A23.931 23.931 0 0112 15c-3.183 0-6.22-.62-9-1.745M16 6V4a2 2 0 00-2-2h-4a2 2 0 00-2 2v2m4 6h.01M5 20h14a2 2 0 002-2V8a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
                </svg>
                    Jobs List
                </router-link>
            </li>
        </ul>
        </div>
        <div class="w-9/12">
        <h1 class="text-gray-500 text-xl w-full bg-gray-100 rounded-md px-3 py-1.5 font-bold">Manage Job</h1>
        <div class="jobs-list">
        <p class="text-center text-gray-500" v-if="status.status === 0">can't connect to backend server.<br/>{{ status.url }} <b class="text-red-500">{{status.statusText}}</b></p>
        <div class="flex border-b border-gray-300 items-center py-4" v-for="job in jobs" :key="job.ID">
            <div class="w-3/4">
            <p class="text-gray-600 text-lg font-bold pl-2"><router-link v-bind:to="'/job/' + job.ID">{{ job.Role }}</router-link></p>
            <span class="text-gray-500 pl-2">{{ job.Company }}</span><small class="text-gray-600 text-sm"> @ {{ getHumanDate(job.PublishedAt) }}</small>
            </div>
            <div class="w-1/3 text-right">
            <button @click="deleteJob(job.ID)" class="bg-red-500 text-white rounded-sm px-4 py-1">Delete</button>
            </div>
        </div>
        </div>
        </div>
    </div>
</div>
</template>

<script>
import moment from "moment";
export default {
  data() {
    return {
      status: "",
      jobs: [],
    };
  },
  methods: {
    getHumanDate: function (date) {
      return moment(date, "YYYY-MM-DD").fromNow();
    },
    deleteJob(ID) {
      this.$http.delete(process.env.VUE_APP_BACKEND + '/job/' + ID).then(
        (res) => {
          console.log(res);
          this.$notify(
            {
              group: "success",
              title: "Success",
              text: "Job has been deleted!",
            },
            2000
          );
        },
        (error) => {
          console.log(error);
          this.$notify(
            {
              group: "error",
              title: "Failed",
              text: "Error when deleting job!",
            },
            2000
          );
        }
      );
    },
  },
  created() {
    this.$http.get(process.env.VUE_APP_BACKEND + '/jobs').then(
      function (data) {
        this.jobs = data.body;
      },
      (error) => {
        this.status = error;
      }
    );
  },
};
</script>

<style scoped>
.router-link-active {
  --tw-text-opacity: 1;
  color: white;
  background-color: rgba(248, 113, 113, var(--tw-bg-opacity));
}
</style>
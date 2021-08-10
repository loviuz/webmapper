<template>
  <card class="card" :title="title">
    <div>
      <ul class="list-unstyled team-members">
        <li>
          <div class="row" v-for="host in hosts" :key="host.ip_address">
            <div class="col-3">
              <div class="avatar">
                <img :src="host.name" alt="Circle Image" class="rounded img-fluid">
              </div>
            </div>
            <div class="col-6">
              {{host.name}}
              <br>
              <span :class="getStatusClass(host.status)">
                <small>{{host.status}}</small>
              </span>
            </div>

            <div class="col-3">
              <p-button type="success" outline icon>
                <i class="fa fa-envelope"></i>
              </p-button>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </card>
</template>
<script>
export default {
  props: ['hosts'],
  data() {
    return {
      title: "Discovered Hosts"
    };
  },
  methods: {
    getStatusClass(status) {
      switch (status) {
        case "Offline":
          return "text-muted";
        case "Available":
          return "text-success";
        case "Busy":
          return "text-danger";
        default:
          return "text-success";
      }
    }
  }
};
</script>
<style>
</style>

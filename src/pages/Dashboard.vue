<template>
  <div>

    <!--Stats cards-->
    <div class="row">
      <div class="col-md-6 col-xl-3" v-for="stats in statsCards" :key="stats.name">
        <stats-card>
          <div class="icon-big text-center" :class="`icon-${stats.type}`" slot="header">
            <i :class="stats.icon"></i>
          </div>
          <div class="numbers" slot="content">
            <p>{{stats.title}}</p>
            {{stats.value}}
          </div>
          <div class="stats" slot="footer">
            <i :class="stats.footerIcon"></i> {{stats.footerText}}
          </div>
        </stats-card>
      </div>
    </div>

    <DiscoveredHosts v-bind:hosts="hosts"></DiscoveredHosts>

  </div>
</template>
<script>
import axios from "axios";
import { StatsCard } from "@/components/index";
import DiscoveredHosts from "@/components/Dashboard/DiscoveredHosts.vue";


export default {
  components: {
    DiscoveredHosts,
    StatsCard
  },
  data() {
    return {
      statsCards: [
        {
          name: 'discovered-hosts',
          type: "success",
          icon: "ti-server",
          title: "Discovered hosts",
          value: 0,
          footerText: "Updated now",
          footerIcon: "ti-time",
        },
        {
          type: "warning",
          icon: "ti-target",
          title: "Open ports",
          value: 0,
          footerText: "Last day",
          footerIcon: "ti-calendar"
        },
        {
          name: "vulnerabilities",
          type: "danger",
          icon: "ti-pulse",
          title: "Vulnerabilities",
          value: "0",
          footerText: "In the last hour",
          footerIcon: "ti-timer"
        },
        {
          name: "brute-force-success",
          type: "danger",
          icon: "ti-unlock",
          title: "Brute force",
          value: "0",
          footerText: "Updated now",
          footerIcon: "ti-reload"
        }
      ],
      hosts: []
    };
  },
  created () {
    console.log( this.components );
    this.fetchHosts();
    this.timer = setInterval(this.fetchHosts, 5000);
  },
  methods: {
      fetchHosts () {
        axios.get(this.$crud_url + "/records/hosts")
          .then(response => {
            this.hosts = response.data.records;
            this.statsCards[0].value = this.hosts.length;
          });
      },
      cancelAutoUpdate () {
          clearInterval(this.timer);
      }
  },
  beforeDestroy () {
    this.cancelAutoUpdate();
  }
};

</script>
<style>
</style>

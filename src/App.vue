<template>
  <iss-tracker @notify="n"></iss-tracker>
  <notification-bar ref="notifier"></notification-bar>

</template>

<script>
import IssTracker from "@/components/IssTracker";
import NotificationBar from "@/components/NotificationBar";

export default {
  name: 'App',
  components: {
    NotificationBar,
    IssTracker
  },
  methods: {
    n(d) {
      this.$refs.notifier.notify(d.text, d.color);
    }
  }
}
</script>

<style>
</style>

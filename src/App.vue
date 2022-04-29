<template>
  <iss-tracker></iss-tracker>
  <notification-bar></notification-bar>

</template>

<script>
import IssTracker from "@/components/IssTracker";
import NotificationBar from "@/components/NotificationBar";

export default {
  name: 'App',
  components: {
    NotificationBar,
    IssTracker
  }
}
</script>

<style>

</style>

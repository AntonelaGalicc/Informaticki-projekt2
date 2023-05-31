<template>
  <div>
    <FormComp></FormComp>
  </div>
</template>

<script>
import FormComp from "../components/FormComp.vue";

export default {
  components: {
    FormComp
  },
  data() {
    return {
      addDialog: false,
      successAlert: false
    };
  }
}
</script>



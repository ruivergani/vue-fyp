<template>
  <button class="button" @click="onClick">{{ title }}</button>
  <!-- @click is a shorthand syntax for v-on = used to attach event listeners -->
  <!-- @click="onClick" means that when the button is clicked the method named onClick will be invoked -->
</template>
<script>
  // Defines the JavaScript logic for the component
  export default {
    components: {
    },
    props: {
      title: {
        type: String,
        required: true,
      },
      onClick: {
        type: Function,
        required: true,
      },
    },
  };
</script>
<!-- CSS -->
<style src="./Button.css" scoped> </style>
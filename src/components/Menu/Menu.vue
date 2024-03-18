<!-- 
  The <template> section contains the structure of the component, 
  where :class is used to dynamically bind the class based on the styles.menuContainer variable. 
-->
<template>
  <div class="menuContainer">
    <Button @click="() => onButtonClick('Delete')" title="Delete All Data" />
    <Button @click="() => onButtonClick('Read')" title="Read All Data" />
    <Button @click="() => onButtonClick('Update')" title="Update Data" />
    <Button @click="() => onButtonClick('Create')" title="Create 1 row" />
    <Button @click="() => onButtonClick('Create1000')" title="Create 1,000 rows" />
    <Button @click="() => onButtonClick('Create10000')" title="Create 10,000 rows*" />
  </div>
</template>

<script>
  import Button from '../Button/Button.vue';

  export default {
    components: {
      Button,
    },
    props: {
      onButtonClick: {
        type: Function,
        required: true,
      },
    },
  };
</script>

<style src="./Menu.css" scoped></style>
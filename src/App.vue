<template>
  <div id="app">
    <!-- Render the Header component and pass a callback to handle button clicks -->
    <Header :onButtonClick="handleButtonClick" />
    <main>
      <div class="content">
        <!-- Data Used -->
        <div class="sectionData">
          <h2>Data Table</h2>
          <table class="tableData">
            <thead>
              <tr>
                <th>ID</th>
                <th>Value</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in itemsData" :key="item.id">
                <td>{{ item.id }}</td>
                <td>{{ item.value }}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <!-- Algorithm Benchmark Results -->
        <div class="sectionResults">
          <h2>Algorithm Benchmark Results</h2>
          <table class="tableData">
            <thead>
              <tr>
                <th>Operation</th>
                <th>Duration (millisecond)</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>Create</td>
                <td>{{ benchmarkResults.create }} ms</td>
              </tr>
              <tr>
                <td>Read</td>
                <td>{{ benchmarkResults.read }} ms</td>
              </tr>
              <tr>
                <td>Update</td>
                <td>{{ benchmarkResults.update }} ms</td>
              </tr>
              <tr>
                <td>Delete</td>
                <td>{{ benchmarkResults.delete }} ms</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
  import Header from './components/Header/Header.vue';
  import Menu from './components/Menu/Menu.vue';
  import Button from './components/Button/Button.vue';
  import buildData from './data.js';

  export default {
    components: {
      Header,
      Menu,
      Button,
    },
    methods: {
      initializeData() {
        const itemsData = buildData(this.numItems);
        this.itemsData = itemsData;
      },
      createOperation() {
        const startTime = performance.now();
        const newItem = {
          id: this.numItems,
          value: buildData(1)[0].value,
          recentlyUpdated: false,
        };
        this.itemsData.push(newItem);
        const endTime = performance.now();
        const createDuration = endTime - startTime;
        this.updateBenchmarkResult('create', createDuration);
        this.numItems++;
      },
      createOperation1000() {
        const startTime = performance.now();
        const newItems = Array.from({ length: 1000 }, (_, index) => ({
          id: this.itemsData.length + index + 1,
          value: buildData(1)[0].value,
          recentlyUpdated: false,
        }));
        this.itemsData.push(...newItems);
        const endTime = performance.now();
        const createDuration = endTime - startTime;
        this.updateBenchmarkResult('create', createDuration);
      },
      createOperation10000() {
        const startTime = performance.now();
        const newItems = Array.from({ length: 10000 }, (_, index) => ({
          id: this.itemsData.length + index + 1,
          value: buildData(1)[0].value,
          recentlyUpdated: false,
        }));
        this.itemsData.push(...newItems);
        const endTime = performance.now();
        const createDuration = endTime - startTime;
        this.updateBenchmarkResult('create', createDuration);
      },
      readOperation() {
        const startTime = performance.now();
        if (this.itemsData.length > 0) {
          this.itemsData.forEach((item) => {
            const value = item.value;
            console.log(`Item read: ${value}`);
          });
        }
        const endTime = performance.now();
        const readDuration = endTime - startTime;
        this.updateBenchmarkResult('read', readDuration);
      },
      updateOperation() {
        const startTime = performance.now();
        // Ensure there are items to Update
        if (this.itemsData.length > 0) {
          // Create an array of available, non-updated indices
          const availableIndices = this.itemsData
            .map((item, index) => ({ index, recentlyUpdated: item.recentlyUpdated }))
            .filter((item) => !item.recentlyUpdated)
            .map((item) => item.index);
          if (availableIndices.length > 0) {
            // Randomly select an index from the available indices
            const randomIndex = availableIndices[Math.floor(Math.random() * availableIndices.length)];
            // Mark the item at the random index as recently updated
            const updatedItems = [...this.itemsData];
            updatedItems[randomIndex].recentlyUpdated = true;
            // Create an updatedItem object by copying the original item located at the random index and changing its value
            const updatedItem = {
              ...updatedItems[randomIndex],
              value: `Updated Item ${randomIndex}`,
            };
            updatedItems[randomIndex] = updatedItem;
            console.log(`Updated item:`, updatedItem);
            // Update the component's state with the updated itemsData array
            this.itemsData = updatedItems;
            const endTime = performance.now();
            const updateDuration = endTime - startTime;
            this.updateBenchmarkResult('update', updateDuration);
            console.log(`Updated duration: ${updateDuration} ms`);
          } else {
            console.log('All items have been updated. Please refresh.');
          }
        }
      },
      deleteOneItem() {
        const startTime = performance.now();
        // Ensure there are items to Delete
        if (this.itemsData.length > 0) {
          // Generating a random index within the valid range
          const randomIndex = Math.floor(Math.random() * this.itemsData.length);
          // Create a copy of the itemsData array with the item at randomIndex removed
          const updatedItems = [...this.itemsData];
          updatedItems.splice(randomIndex, 1); // removes one element from the item at random index using splice
          // Update the component's state with the updated itemsData array
          this.$set(this, 'itemsData', updatedItems);
          const endTime = performance.now();
          const deleteDuration = endTime - startTime;
          this.updateBenchmarkResult('delete', deleteDuration);
        }
      },
      deleteOperation() {
        const startTime = performance.now();
        // Ensure there are items to Delete
        if (this.itemsData.length > 0) {
          // Remove all items from the itemsData array
          this.itemsData.splice(0, this.itemsData.length);
          const endTime = performance.now();
          const deleteDuration = endTime - startTime;
          this.updateBenchmarkResult('delete', deleteDuration);
          // Trigger a re-render after state is updated (if needed)
          this.$forceUpdate();
        }
      },
      updateBenchmarkResult(operation, duration) {
        this.benchmarkResults = {
          ...this.benchmarkResults,
          [operation]: duration.toFixed(4),
        };
      },
      handleButtonClick(operation) {
        switch (operation) {
          case 'Create':
            this.createOperation();
            break;
          case 'Create1000':
            this.createOperation1000();
            break;
          case 'Create10000':
            this.createOperation10000();
            break;
          case 'Read':
            this.readOperation();
            break;
          case 'Update':
            this.updateOperation();
            break;
          case 'Delete':
            this.deleteOperation();
            break;
          default:
            console.error('Invalid operation:', operation);
        }
      },
    },
    data() {
      return {
        itemsData: [],
        benchmarkResults: {
          create: 0,
          read: 0,
          update: 0,
          delete: 0,
        },
        numItems: 10,
        timingInterval: 1,
      };
    },
    mounted() {
      this.initializeData();
    },
  };
</script>

<style scoped> </style>
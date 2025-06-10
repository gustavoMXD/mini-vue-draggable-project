<script setup>
import { reactive, computed, ref } from 'vue';
import draggable from 'vuedraggable';

const message = ref('Hello World');
const counter = reactive({count: 0});

const incrementCounter = () => counter.count++;

// The original list of available meals
const availableMeals = ref([
  { id: 1, name: 'Pizza' },
  { id: 2, name: 'Hamburger' },
  { id: 3, name: 'Spaghetti' },
  { id: 4, name: 'Tacos' },
  { id: 5, name: 'Sushi' },
  { id: 6, name: 'Curry' }
]);

// The new list where dragged meals will go
const myMeals = ref([]);

// Dummy drag state (optional, for visual feedback)
const drag = ref(false);

// Function to reset lists (optional, for demonstration)
const resetMeals = () => {
  availableMeals.value = [
    { id: 1, name: 'Pizza' },
    { id: 2, name: 'Hamburger' },
    { id: 3, name: 'Spaghetti' },
    { id: 4, name: 'Tacos' },
    { id: 5, name: 'Sushi' },
    { id: 6, name: 'Curry' }
  ];
  myMeals.value = [];
};

const sendMeals = () => {
  // Option 1: Directly log the object (console will allow you to expand it)
  console.log('--- My Selected Meals (Object View) ---');
  console.log(myMeals.value);

  // Option 2: Log as a formatted JSON string (good for seeing exact structure)
  console.log('--- My Selected Meals (JSON String View) ---');
  console.log(JSON.stringify(myMeals.value, null, 2)); // `null, 2` for pretty-printing

  console.log('-------------------------');
}

</script>

<template>
  <div>
    <div id="item-1">
      <h2>{{ message }}</h2>
      <p>Count is: {{ counter.count }}</p>
      <button @click="incrementCounter">Increment counter</button>
      <button @click="resetMeals" style="margin-left: 10px;">Reset Meals</button>
      <button @click="sendMeals">Send meals</button>
    </div>

    <div class="meals-container">
      <div id="favourite-foods-list" class="meals-column">
        <h2>Available Foods</h2>
        <draggable
          v-model="availableMeals"
          item-key="id"
          group="shared-meals"
          @start="drag=true"
          @end="sendMeals"
          tag="ul"
          class="meals-list"
        >
          <template #item="{ element: meal }">
            <li class="item">
              {{ meal.name }}
            </li>
          </template>
        </draggable>
        <p v-if="availableMeals.length === 0" class="empty-message">No more available foods to drag.</p>
      </div>

      <div id="my-meals-list" class="meals-column">
        <h2>My Selected Meals</h2>
        <draggable
          v-model="myMeals"
          item-key="id"
          group="shared-meals"
          @start="drag=true"
          @end="sendMeals"
          tag="ul"
          class="meals-list selected-meals-list"
        >
          <template #item="{ element: meal }">
            <li class="item selected-item">
              {{ meal.name }}
            </li>
          </template>
        </draggable>
        <p v-if="myMeals.length === 0" class="empty-message">Drag meals here!</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* General styling for containers */
#item-1 {
  border: 1px solid #ccc;
  padding: 20px;
  margin-bottom: 20px;
  border-radius: 8px;
  background-color: #f9f9f9;
}

h2 {
  color: #333;
  margin-top: 0;
}

/* Flexbox container for the two meal lists */
.meals-container {
  display: flex;
  gap: 20px; /* Space between the two columns */
  margin-top: 20px;
}

/* Styling for each meal list column */
.meals-column {
  flex: 1; /* Each column takes equal width */
  border: 1px solid #ddd;
  padding: 15px;
  border-radius: 8px;
  background-color: #ffffff;
  box-shadow: 0 2px 5px rgba(0,0,0,0.05);
}

/* Styling for the draggable lists themselves */
.meals-list {
  list-style: none;
  padding: 0;
  margin: 0;
  min-height: 100px; /* Give the lists some height even if empty */
  border: 1px dashed #eee;
  padding: 10px;
  border-radius: 5px;
  background-color: #fdfdfd;
}

/* Styling for individual draggable items */
.item {
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 8px;
  cursor: grab;
  border-radius: 4px;
  display: flex;
  align-items: center;
  box-shadow: 0 1px 3px rgba(0,0,0,0.05);
  transition: background-color 0.2s ease, box-shadow 0.2s ease;
}

.item:hover {
  background-color: #e5e5e5;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

/* Specific styling for items in the "My Selected Meals" list */
.selected-item {
  background-color: #e6ffe6; /* Lighter green for selected items */
  border-color: #aaddaa;
}

.selected-item:hover {
  background-color: #d0ffcf;
}

.empty-message {
  text-align: center;
  color: #888;
  padding: 20px;
  font-style: italic;
}
</style>
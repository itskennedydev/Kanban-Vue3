<script>
import { ref } from 'vue';

export default {
  setup() {
    const items = ref([
      // unique ID to identify each item - Title to render on screen - list it belongs to
      { id: 0, title: 'Style registration', list: 1 },
      { id: 1, title: 'Help with designs', list: 1 },
      { id: 3, title: 'Test dashboard', list: 1 },
      { id: 4, title: 'Finish Kanban Board', list: 1 },
    ])

    const getList = (list) => {
      // filters list - only returns item if their list matches argument
      return items.value.filter((item) => item.list == list)
    }

    const startDrag = (event, item) => {
      console.log(item)
      // Controls visual feedback user gets when dragging & dropping 
      event.dataTransfer.dropEffect = 'move'
      //  Tells the drag & drop api to move original item not copy 
      event.dataTransfer.effectAllowed = 'move'
      event.dataTransfer.setData('itemID', item.id)
    }

    const onDrop = (event, list) => {
      // Accesses the ID that is stored in data transfer object
      const itemID = event.dataTransfer.getData('itemID')
      // Finds item associated with ID
      const item = items.value.find((item) => item.id == itemID)
      item.list = list
    }

    // Allows access to template
    return {
      getList,
      onDrop,
      startDrag,
    }
  }
}
</script>

<template>

  <header>
    <h1>
      Kanban Board
    </h1>
  </header>

  <!-- Board Start -->
  <div class="kanban-row">
    <div class="kanban-board-base" id="Red" @drop="onDrop($event, 1)" @dragenter.prevent @dragover.prevent>
      <div>
        <h2>Backlog</h2>
      </div>
      <div>
        <div v-for="item in getList(1)" :key="item.id" class="drag-el" draggable="true"
          @dragstart="startDrag($event, item)">
          {{ item.title }}
        </div>
      </div>
    </div>
    <!-- Board End -->

    <!-- Board Start -->
    <div class="kanban-board-base" id="Yellow" @drop="onDrop($event, 2)" @dragenter.prevent @dragover.prevent>
      <div>
        <h2>In Progress</h2>
      </div>
      <div>
        <div v-for="item in getList(2)" :key="item.id" class="drag-el" draggable="true"
          @dragstart="startDrag($event, item)">
          {{ item.title }}
        </div>
      </div>
    </div>
    <!-- Board End -->

    <!-- Board Start -->
    <div class="kanban-board-base" id="Green" @drop="onDrop($event, 3)" @dragenter.prevent @dragover.prevent>
      <div>
        <h2>Tested</h2>
      </div>
      <div>
        <div v-for="item in getList(3)" :key="item.id" class="drag-el" draggable="true"
          @dragstart="startDrag($event, item)">
          {{ item.title }}
        </div>
      </div>
    </div>
    <!-- Board End -->

    <!-- Board Start -->
    <div class="kanban-board-base" id="Blue" @drop="onDrop($event, 4)" @dragenter.prevent @dragover.prevent>
      <div>
        <h2>Done</h2>
      </div>
      <div>
        <div v-for="item in getList(4)" :key="item.id" class="drag-el" draggable="true"
          @dragstart="startDrag($event, item)">
          {{ item.title }}
        </div>
      </div>
    </div>
    <!-- Board End -->
  </div>
</template>

<style>
/* global styles */
* {
  font-family: Helvetica, sans-serif;
  text-align: center;
  font-weight: bold;
}

/* styles for kanban board */
.kanban-board {
  background-color: #ecf0f1;
  min-height: 10px;
}

.kanban-row {
  display: flex;
  justify-content: space-evenly;
  gap: 20px;
}

#Red {
  background-color: #f45a5f;
}

#Yellow {
  background-color: #fea126;
}

#Green {
  background-color: #1eba46;
}

#Blue {
  background-color: #5caff1;
}

.kanban-board-base {
  border-radius: 15px;
  padding: 10px;
  min-height: 340px;
}

.drag-el {
  opacity: 60%;
  background-color: white;
  padding: 5px;
  margin-bottom: 10px;
  border-radius: 5px;
  color: black;
}

;

.drag-el:nth-last-of-type(1) {
  margin-bottom: 0;
}
</style>

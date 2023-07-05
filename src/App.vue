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
    ]);

    const newBoardName = ref('');

    const getBoardId = (boardId) => {
      return `board-${boardId}`;
    };

    const addBoard = () => {
      const boardName = newBoardName.value.trim();
      if (boardName) {
        const newBoard = {
          id: Date.now(),
          name: boardName,
        };
        boards.value.push(newBoard);
        newBoardName.value = '';
      }
    };

    const boards = ref([
      { id: 1, name: 'Backlog' },
      { id: 2, name: 'In Progress' },
      { id: 3, name: 'Tested' },
      { id: 4, name: 'Done' },
    ]);

    const newTaskTitle = ref('');

    const getList = (list) => {
      // filters list - only returns item if their list matches argument
      return items.value.filter((item) => item.list == list);
    };

    const startDrag = (event, item) => {
      console.log(item)
      // Controls visual feedback user gets when dragging & dropping 
      event.dataTransfer.dropEffect = 'move';
      //  Tells the drag & drop api to move original item not copy 
      event.dataTransfer.effectAllowed = 'move';
      event.dataTransfer.setData('itemID', item.id);
    }

    const onDrop = (event, list, board) => {
      // Accesses the ID that is stored in data transfer object
      const itemID = event.dataTransfer.getData('itemID');
      // Finds item associated with ID
      const item = items.value.find((item) => item.id == itemID);
      item.list = list;
    }

    // Handles new Tasks 
    const addTask = () => {
      const taskTitle = newTaskTitle.value.trim();
      if (taskTitle) {
        const newTask = {
          // Unique ID
          id: Date.now(),
          title: taskTitle,
          // adds Task to the backlog list by default 
          list: 1,
        };
        items.value.push(newTask);
        // Empty field for input
        newTaskTitle.value = '';
      }
    };

    const getBoardStyle = (boardName) => {
      if (boardName === 'Backlog') {
        return { backgroundColor: '#f45a5f', minHeight: '340px' };
      } else if (boardName === 'In Progress') {
        return { backgroundColor: '#fea126', minHeight: '340px' };
      } else if (boardName === 'Tested') {
        return { backgroundColor: '#1eba46', minHeight: '340px' };
      } else if (boardName === 'Done') {
        return { backgroundColor: '#5caff1', minHeight: '340px' };
      }
    };

    // Allows access to template
    return {
      items,
      getList,
      onDrop,
      startDrag,
      newTaskTitle,
      addTask,
      boards,
      newBoardName,
      getBoardId,
      addBoard,
      getBoardStyle,
    }
  }
};

</script>

<template>
  <header>
    <h1>
      Kanban Board
    </h1>
  </header>

  <!-- New Board -->
  <div class="board-form">
    <input type="text" v-model="newBoardName" placeholder="Add a new Board">
    <button @click="addBoard">Add Board</button>
  </div>

  <!-- New Task -->
  <div class="task-form">
    <input type="text" v-model="newTaskTitle" placeholder="Enter a new Task">
    <button @click="addTask">Add Task</button>
  </div>

  <!-- Boards -->
  <div class="kanban-row">
    <div v-for="board in boards" :key="board.id" :id="getBoardId(board.id)" @drop="onDrop($event, board.id)"
      @dragenter.prevent @dragover.prevent class="kanban-board-base" :style="getBoardStyle(board.name)">
      <div>
        <h2>{{ board.name }}</h2>
      </div>
      <div>
        <div v-for="item in getList(board.id)" :key="item.id" class="drag-el" draggable="true"
          @dragstart="startDrag($event, item)">{{ item.title }}</div>
      </div>
    </div>
  </div>
</template>

<style>
/* global styles */
* {
  font-family: Helvetica, sans-serif;
  text-align: center;
  font-weight: bold;
}

.task-form {
  margin-bottom: 20px;
  padding-top: 20px;
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

<script lang="ts" context="module">
  export interface Task {
    name: string
    description: string
    isImportant: boolean
    assignee: string
    createdAt: Date
    updatedAt: Date
  }
</script>

<script lang="ts">
  import pencil from '../assets/pencil.svg'
  import trash from '../assets/trash.svg'
  import { createEventDispatcher } from 'svelte'

  export let task: Task
  export let taskID: number
  export let deleteTask: (id: number) => void

  $: color = task.isImportant
    ? 'text-blue-100 bg-blue-900/80 hover:bg-blue-900'
    : 'text-gray-200 bg-gray-700/80 hover:bg-gray-700'

  const dispatch = createEventDispatcher()

  let editTaskName = () => {
    dispatch('editTask', {
      task: task,
      taskID: taskID,
    })
  }
</script>

<style>

</style>

<div
  class="flex justify-between items-center p-2 text-base font-normal rounded-lg {color}"
  draggable="true"
  on:dragstart>

  <p class:font-semibold={task.isImportant} class="mr-4">{task.name}</p>

  <div class="flex items-center gap-2">
    <button on:click={editTaskName}>
      <img src={pencil} alt="edit" class="h-6 w-6" />
    </button>
    <button on:click={() => deleteTask(taskID)}>
      <img src={trash} alt="edit" class="h-6 w-6" />
    </button>
  </div>

</div>

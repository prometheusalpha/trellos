<script lang="ts">
  import type { Task as ITask } from './Task.svelte'
  import Input from '../lib/Input.svelte'

  export let task: ITask
  export let saveTask: () => void
</script>

<div
  class="w-screen h-screen bg-gray-500/60 absolute top-0 left-0 flex
  justify-center items-center">
  <div class="absolute inset-0 z-0" on:click={saveTask} />
  <div
    class="block p-6 max-w-sm bg-white rounded-lg border border-gray-200
    shadow-md dark:bg-gray-800 dark:border-gray-700 z-10 flex flex-col gap-4
    min-w-[350px]">
    <p>Name</p>
    <Input bind:value={task.name} />
    <p>Description</p>
    <Input bind:value={task.description} />
    <div class="flex items-center">
      <input
        id="important"
        type="checkbox"
        class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 rounded
        focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800
        focus:ring-2 dark:bg-gray-700 dark:border-gray-600"
        bind:checked={task.isImportant} />
      <label for="important" class="ml-2">Important</label>
    </div>
    <button
      type="button"
      class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4
      focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2
      dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none
      dark:focus:ring-blue-800"
      on:click={saveTask}>
      Save
    </button>
  </div>
</div>

<script lang="ts" context="module">
  export interface Board {
    name: string
    tasks: ITask[]
  }
</script>

<script lang="ts">
  import done from '../assets/done.svg'
  import pencil from '../assets/pencil.svg'
  import AddButton from '../lib/AddButton.svelte'
  import Input from '../lib/Input.svelte'
  import type { Task as ITask } from './Task.svelte'
  import Task from './Task.svelte'
  import { createEventDispatcher } from 'svelte'

  export let board: Board
  export let boardID: number
  export let dragStart: (
    event: any,
    deptBoardID: number,
    taskID: number,
  ) => void

  const dispatch = createEventDispatcher()
  let editable: boolean = false

  let addTask = () => {
    board.tasks = [
      ...board.tasks,
      {
        name: 'newTask',
        description: 'new',
        isImportant: false,
        assignee: 'new',
        createdAt: new Date(),
        updatedAt: new Date(),
      },
    ]
  }

  let toggleEditable = () => {
    editable = !editable
  }

  let forwardTask = (event: any) => {
    dispatch('editTask', {
      task: event.detail.task,
      taskID: event.detail.taskID,
      boardID: boardID,
    })
  }

  let deleteTask = (id: number) => {
    board.tasks = board.tasks.filter((_: ITask, taskID: number) => {
      return taskID !== id
    })
  }
</script>

<div
  class="block p-6 max-w-sm bg-white rounded-lg border border-gray-200 shadow-md
  dark:bg-gray-800 dark:border-gray-700 min-w-[250px]"
  on:drop>
  <div class="flex items-center justify-between">
    {#if !editable}
      <h1 class="text-3xl font-semibold mr-5">{board.name}</h1>
    {:else}
      <Input bind:value={board.name} />
    {/if}

    <button on:click={toggleEditable} class="ml-4">
      <img src={editable ? done : pencil} alt="edit" class="h-6 w-6" />
    </button>
  </div>

  <div class="flex flex-col gap-4 my-4">
    {#each board.tasks as task, taskID}
      <Task
        {task}
        {taskID}
        on:editTask={forwardTask}
        {deleteTask}
        on:dragstart={(event) => dragStart(event, boardID, taskID)} />
    {/each}
  </div>
  <AddButton content="Add Task" on:click={addTask} />
</div>

<script lang="ts">
  import './index.css'
  import Board from './components/Board.svelte'
  import type { Board as IBoard } from './components/Board.svelte'
  import type { Task as ITask } from './components/Task.svelte'
  import AddButton from './lib/AddButton.svelte'
  import Modal from './components/Modal.svelte'

  interface ITaskCoo {
    task: ITask
    taskID: number
    boardID: number
  }

  let isModalVisible: boolean = false
  let editingTask: ITaskCoo

  let sampleTask = {
    name: 'newTask',
    description: 'new',
    isImportant: false,
    assignee: 'new',
    createdAt: new Date(),
    updatedAt: new Date(),
  }

  let workspace: IBoard[] = [
    {
      name: 'New Table',
      tasks: [sampleTask],
    },
  ]

  function dragStart(event, deptBoardID: number, taskID: number) {
    // console.log('dragstart')
    const data = { deptBoardID, taskID }
    event.dataTransfer.setData('text/plain', JSON.stringify(data))
  }

  let moveTask = (deptBoardID, destBoardID, taskID) => {
    const [task] = workspace[deptBoardID].tasks.splice(taskID, 1)
    workspace[destBoardID].tasks.push(task)
  }

  function dragEnd(event, destBoardID: number) {
    // console.log('drop')
    event.preventDefault()
    const json = event.dataTransfer.getData('text/plain')
    const data = JSON.parse(json)

    moveTask(data.deptBoardID, destBoardID, data.taskID)

    forceRefresh()
  }

  let addBoard = () => {
    workspace = [
      ...workspace,
      {
        name: 'New Board',
        tasks: [],
      },
    ]
  }

  let updateTask = (newTask: ITaskCoo) => {
    workspace[newTask.boardID].tasks[newTask.taskID] = newTask.task
    forceRefresh()
  }

  let forceRefresh = () => {
    let a = [...workspace]
    workspace = a
  }

  let showModal = (event) => {
    isModalVisible = true
    editingTask = {
      task: event.detail.task,
      taskID: event.detail.taskID,
      boardID: event.detail.boardID,
    }
  }

  let hideModal = () => {
    isModalVisible = false
    updateTask(editingTask)
  }
</script>

<main class="font-['SF_Pro_Text'] bg-gray-900 min-h-screen text-white">
  <div class="flex p-10 flex-wrap gap-5 items-start">
    {#each workspace as board, boardID}
      <Board
        {board}
        {boardID}
        on:editTask={showModal}
        on:drop={(event) => dragEnd(event, boardID)}
        {dragStart} />
    {/each}
    <AddButton on:click={addBoard} content="Add new board" />
  </div>
  {#if isModalVisible}
    <Modal task={editingTask.task} saveTask={hideModal} />
  {/if}
</main>

<script lang="ts">
  import plus from '../assets/plus.svg'
  export let content: string = ''
</script>

<button
  class="inline-flex gap-2 items-center py-2 px-3 text-sm font-medium
  text-center text-white bg-blue-700 rounded-lg hover:bg-blue-800 focus:ring-4
  focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700
  dark:focus:ring-blue-800"
  on:click>
  <img src={plus} alt="plus" class="h-6 w-6" />
  {content}
</button>

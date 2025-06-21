<script lang="ts">
  import { onMount } from "svelte";
  export let bento: boolean = false;

  interface Student {
    id: string;
    nombre: string;
    apellido: string;
    carrera: string;
  }

  let students: Student[] = [];
  let loading = false;
  let error = "";

  const backendUrl = "https://rust-backend-veyq.shuttle.app";

  onMount(async () => {
    await fetchStudents();
  });

  async function fetchStudents() {
    loading = true;
    error = "";
    try {
      const response = await fetch(`${backendUrl}/get_students`);
      if (response.ok) {
        students = await response.json();
      } else {
        error = "No se pudieron obtener los estudiantes";
      }
    } catch (err) {
      error = "Error de conexión. Intenta nuevamente.";
    } finally {
      loading = false;
    }
  }
</script>

<div class={bento ? "h-full w-full flex flex-col justify-between rounded-2xl border border-zinc-800 bg-zinc-900/30 p-8" : "h-full w-3/4 p-10 grid grid-cols-3 grid-rows-2 gap-10 text-white"}>
  <div class="flex items-center gap-4 mb-4">
    <div class="p-3 rounded-xl bg-zinc-800/60 text-zinc-400">
      <svg viewBox="0 0 24 24" class="w-8 h-8" fill="none" stroke="currentColor" stroke-width="1.5">
        <path d="M15.75 6a3.75 3.75 0 1 1-7.5 0 3.75 3.75 0 0 1 7.5 0ZM4.501 20.118a7.5 7.5 0 0 1 14.998 0A17.933 17.933 0 0 1 12 21.75c-2.676 0-5.216-.584-7.499-1.632Z" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </div>
    <h1 class="text-2xl font-bold text-zinc-100">Estudiantes</h1>
  </div>

  {#if error}
    <div class="mb-3 p-3 rounded-xl bg-red-900/20 border border-red-800/50 text-red-400 flex items-center gap-2 text-sm">
      <svg class="w-4 h-4 flex-shrink-0" viewBox="0 0 24 24" fill="currentColor">
        <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-2 15l-5-5 1.41-1.41L10 14.17l7.59-7.59L19 8l-9 9z"/>
      </svg>
      {error}
    </div>
  {/if}

  {#if loading}
    <div class="flex items-center gap-2 text-zinc-400">
      <svg class="animate-spin h-4 w-4" viewBox="0 0 24 24">
        <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4" fill="none" />
        <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"/>
      </svg>
      Cargando...
    </div>
  {:else if students.length === 0}
    <div class="text-zinc-400 text-base mt-4 flex flex-col items-center">
      <svg class="mb-2 w-8 h-8 text-zinc-700" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.5">
        <path d="M15.75 6a3.75 3.75 0 1 1-7.5 0 3.75 3.75 0 0 1 7.5 0ZM4.501 20.118a7.5 7.5 0 0 1 14.998 0A17.933 17.933 0 0 1 12 21.75c-2.676 0-5.216-.584-7.499-1.632Z" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
      No hay estudiantes registrados.
    </div>
  {/if}
  {#if students.length > 0}
    <div class="flex-1 overflow-auto">
      <table class="w-full text-left border-collapse">
        <thead>
          <tr>
            <th class="px-3 py-2 text-zinc-300 font-medium">Nombre</th>
            <th class="px-3 py-2 text-zinc-300 font-medium">Apellido</th>
            <th class="px-3 py-2 text-zinc-300 font-medium">Carrera</th>
          </tr>
        </thead>
        <tbody>
          {#each students.slice(0, 6) as student}
            <tr class="border-b border-zinc-800 hover:bg-zinc-800/50 transition-all text-white/75">
              <td class="px-3 py-2">{student.nombre}</td>
              <td class="px-3 py-2">{student.apellido}</td>
              <td class="px-3 py-2">{student.carrera}</td>
            </tr>
          {/each}
        </tbody>
      </table>
      {#if students.length > 6}
        <div class="mt-2 text-xs text-zinc-500 text-right">
          y {students.length - 6} estudiante(s) más...
        </div>
      {/if}
    </div>
  {/if}
</div>

<style>
  /* Scrollbar minimal para el listado */
  .overflow-auto::-webkit-scrollbar {
    width: 6px;
    background: transparent;
  }
  .overflow-auto::-webkit-scrollbar-thumb {
    background: #52525b;
    border-radius: 8px;
  }
</style>
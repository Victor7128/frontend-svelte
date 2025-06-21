<script lang="ts">
  import { onMount } from "svelte";

  interface Student {
    id: string;
    nombre: string;
    apellido: string;
    edad: number;
    carrera: string;
    created_at?: string;
    updated_at?: string;
  }

  let students: Student[] = [];
  let loading = false;
  let error = "";

  // Puedes ajustar la URL según tu entorno
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

  function formatDate(dateString?: string) {
    if (!dateString) return "No disponible";
    const date = new Date(dateString);
    return date.toLocaleDateString("es-ES", {
      year: "numeric",
      month: "long",
      day: "numeric",
      hour: "2-digit",
      minute: "2-digit",
    });
  }
</script>

<div class="h-full w-3/4 p-10 grid grid-cols-3 grid-rows-2 gap-10 text-white">
  <!-- Listado principal -->
  <section class="col-span-2 row-span-2 rounded-2xl border border-zinc-800 bg-zinc-900/30 p-8">
    <div class="flex items-center gap-4 mb-6">
      <div class="p-3 rounded-xl bg-zinc-800/60 text-zinc-400">
        <svg viewBox="0 0 24 24" class="w-8 h-8" fill="none" stroke="currentColor" stroke-width="1.5">
          <path d="M15.75 6a3.75 3.75 0 1 1-7.5 0 3.75 3.75 0 0 1 7.5 0ZM4.501 20.118a7.5 7.5 0 0 1 14.998 0A17.933 17.933 0 0 1 12 21.75c-2.676 0-5.216-.584-7.499-1.632Z" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </div>
      <h1 class="text-3xl font-bold text-zinc-100">Estudiantes Registrados</h1>
    </div>

    {#if error}
      <div class="mb-6 p-4 rounded-xl bg-red-900/20 border border-red-800/50 text-red-400 flex items-center gap-3">
        <svg class="w-5 h-5 flex-shrink-0" viewBox="0 0 24 24" fill="currentColor">
          <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-2 15l-5-5 1.41-1.41L10 14.17l7.59-7.59L19 8l-9 9z"/>
        </svg>
        {error}
      </div>
    {/if}

    {#if loading}
      <div class="flex items-center gap-3 text-zinc-400">
        <svg class="animate-spin h-5 w-5" viewBox="0 0 24 24">
          <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4" fill="none" />
          <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"/>
        </svg>
        Cargando estudiantes...
      </div>
    {:else if students.length === 0}
      <div class="text-zinc-400 text-lg mt-8 flex flex-col items-center">
        <svg class="mb-2 w-10 h-10 text-zinc-700" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.5">
          <path d="M15.75 6a3.75 3.75 0 1 1-7.5 0 3.75 3.75 0 0 1 7.5 0ZM4.501 20.118a7.5 7.5 0 0 1 14.998 0A17.933 17.933 0 0 1 12 21.75c-2.676 0-5.216-.584-7.499-1.632Z" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
        No hay estudiantes registrados
      </div>
    {:else}
      <div class="overflow-x-auto">
        <table class="w-full text-left border-collapse">
          <thead>
            <tr>
              <th class="px-4 py-2 text-zinc-300 font-medium">Nombre</th>
              <th class="px-4 py-2 text-zinc-300 font-medium">Apellido</th>
              <th class="px-4 py-2 text-zinc-300 font-medium">Edad</th>
              <th class="px-4 py-2 text-zinc-300 font-medium">Carrera</th>
              <th class="px-4 py-2 text-zinc-300 font-medium">Registrado</th>
            </tr>
          </thead>
          <tbody>
            {#each students as student}
              <tr class="border-b border-zinc-800 hover:bg-zinc-800/40 transition-all">
                <td class="px-4 py-3">{student.nombre}</td>
                <td class="px-4 py-3">{student.apellido}</td>
                <td class="px-4 py-3">{student.edad}</td>
                <td class="px-4 py-3">{student.carrera}</td>
                <td class="px-4 py-3 text-xs">{formatDate(student.created_at)}</td>
              </tr>
            {/each}
          </tbody>
        </table>
      </div>
    {/if}
  </section>

  <!-- Detalles y acciones, similar a Create.svelte -->
  <section class="col-span-1 row-span-2 rounded-2xl border border-zinc-800 bg-zinc-900/30 p-6 flex flex-col justify-between">
    <article>
      <div class="flex items-center gap-3 mb-6">
        <div class="p-2 rounded-lg bg-zinc-800/60 text-zinc-400">
          <svg class="w-6 h-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/>
            <polyline points="14,2 14,8 20,8"/>
            <line x1="16" y1="13" x2="8" y2="13"/>
            <line x1="16" y1="17" x2="8" y2="17"/>
            <polyline points="10,9 9,9 8,9"/>
          </svg>
        </div>
        <h1 class="text-xl font-bold text-zinc-100">Detalles</h1>
      </div>
      <div class="space-y-3">
        <p class="text-zinc-300 text-sm">
          Total de estudiantes: <span class="font-semibold text-zinc-100">{students.length}</span>
        </p>
        <div class="text-xs text-zinc-400">
          Actualizado el: {formatDate(new Date().toISOString())}
        </div>
      </div>
    </article>
    <div class="mt-6 flex flex-col gap-4">
      <a
        href="/"
        class="w-full px-6 py-4 rounded-xl border border-zinc-700 text-zinc-300 font-medium text-lg
        hover:bg-zinc-800/50 hover:border-zinc-600 focus:outline-none focus:ring-2 focus:ring-zinc-600
        transition-all duration-200 disabled:opacity-50 disabled:cursor-not-allowed
        flex items-center justify-center gap-3 text-center"
      >
        <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M19 12H5m7-7-7 7 7 7" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
        Volver al inicio
      </a>
      <div class="p-3 rounded-lg bg-zinc-800/20">
        <p class="text-xs text-zinc-500 text-center">
          Refresca la página para actualizar el listado de estudiantes.
        </p>
      </div>
    </div>
  </section>
</div>

<style>
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px);}
    to   { opacity: 1; transform: translateY(0);}
  }
  @keyframes slideIn {
    from { opacity: 0; transform: translateX(-10px);}
    to   { opacity: 1; transform: translateX(0);}
  }
  a:hover { transform: translateY(-1px);}
</style>
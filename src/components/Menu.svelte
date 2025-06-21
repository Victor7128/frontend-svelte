<script lang="ts">
  import Listar from "./Listar.svelte";

  export let active: string = "";

  export function html(node: HTMLElement, htmlString: string) {
    node.innerHTML = htmlString;
    return {
      update(newHtml: string) {
        node.innerHTML = newHtml;
      },
    };
  }
</script>

<!-- Contenedor principal centrado -->
<div class="relative h-full flex items-center justify-center overflow-hidden">
  <div class="relative w-full h-full max-w-6xl p-5">
    <div class="relative grid grid-cols-4 gap-5 lg:grid-rows-2">
      <!-- Crear estudiante: 2 columnas, 2 filas -->
      <a
        href="/create"
        class="group block rounded-2xl border transition-all duration-200 ease-out p-12 hover:border-zinc-600 hover:shadow-lg hover:shadow-zinc-900/20 lg:col-span-2
          {active === '/create'
          ? 'border-zinc-500 bg-zinc-900/50'
          : 'border-zinc-800 bg-zinc-900/30 hover:bg-zinc-900/50'}"
        aria-current={active === "/create" ? "page" : undefined}
      >
        <div
          class="flex flex-col items-center justify-center text-center h-full"
        >
          <div
            class="flex-shrink-0 p-6 mb-6 rounded-xl transition-all duration-300 overflow-hidden
            {active === '/create'
              ? 'bg-zinc-700/80 text-zinc-300'
              : 'bg-zinc-800/60 text-zinc-400 group-hover:bg-zinc-700/60 group-hover:text-zinc-300'}
          "
          >
            <span
              use:html={`<svg viewBox="0 0 24 24" class="w-8 h-8 transition-transform duration-300 group-hover:scale-110 group-hover:rotate-90" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M12 5v14m7-7H5" stroke-linecap="round" stroke-linejoin="round"/></svg>`}
            ></span>
          </div>

          <div class="text-center">
            <h3
              class="text-3xl lg:text-4xl font-semibold mb-4 transition-all duration-300
              {active === '/create'
                ? 'text-zinc-100'
                : 'text-zinc-200 group-hover:text-zinc-100'}
            "
            >
              Crear estudiante
            </h3>
            <p
              class="text-lg leading-relaxed transition-all duration-300 max-w-md mx-auto
              {active === '/create'
                ? 'text-zinc-400'
                : 'text-zinc-500 group-hover:text-zinc-400'}
            "
            >
              Agrega un nuevo estudiante al sistema de forma rápida y sencilla.
            </p>
          </div>
        </div>
      </a>

      <!-- Ver estudiantes: 1 columna, 2 filas -->
      <div
        class="group block rounded-2xl border transition-all duration-200 ease-out p-8 hover:border-zinc-600 hover:shadow-lg hover:shadow-zinc-900/20 lg:col-span-2 lg:row-span-2"
      >
        <!-- <Listar /> -->
      </div>

      <div
        class="group block rounded-2xl border transition-all duration-200 ease-out p-5 hover:border-zinc-600 hover:shadow-lg hover:shadow-zinc-900/20 lg:col-span-1"
      >
        <div
          class="flex flex-col items-center text-center justify-center h-full"
        >
          <div
            class="flex-shrink-0 p-4 rounded-xl transition-all duration-300 overflow-hidden
            {active === '/list'
              ? 'bg-zinc-700/80 text-zinc-300'
              : 'bg-zinc-800/60 text-zinc-400 group-hover:bg-zinc-700/60 group-hover:text-zinc-300'}
          "
          >
            <span
              use:html={`<svg viewBox="0 0 24 24" class="w-8 h-8 transition-all duration-300 group-hover:scale-110 group-hover:-rotate-12" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M11 4h2v16m-1-8h8" stroke-linecap="round" stroke-linejoin="round"/></svg>`}
            ></span>
          </div>

          <div class="text-center mt-5">
            <h3
              class="text-xl font-semibold mb-4 transition-all duration-300
              {active === '/list'
                ? 'text-zinc-100'
                : 'text-zinc-200 group-hover:text-zinc-100'}
            "
            >
              Actualizar estudiante
            </h3>
            <p
              class="text-sm leading-relaxed transition-all duration-300
              {active === '/list'
                ? 'text-zinc-400'
                : 'text-zinc-500 group-hover:text-zinc-400'}
            "
            >
              Edita y modifica la información de cualquier estudiante existente.
            </p>
          </div>
        </div>
      </div>

      <div
        class="group block rounded-2xl border transition-all duration-200 ease-out p-5 hover:border-zinc-600 hover:shadow-lg hover:shadow-zinc-900/20 lg:col-span-1"
      >
        <div
          class="flex flex-col items-center text-center justify-center h-full"
        >
          <div
            class="flex-shrink-0 p-3 rounded-xl transition-all duration-300 overflow-hidden
            {active === '/delete'
              ? 'bg-zinc-700/80 text-zinc-300'
              : 'bg-zinc-800/60 text-zinc-400 group-hover:bg-zinc-700/60 group-hover:text-zinc-300'}
          "
          >
            <span
              use:html={`<svg viewBox="0 0 24 24" class="w-8 h-8 transition-all duration-300 group-hover:scale-110 group-hover:rotate-12" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M19 7l-.867 12.142A2 2 0 0 1 16.138 21H7.862a2 2 0 0 1-1.995-1.858L5 7m5 4v6m4-6v6M9 7V4a1 1 0 0 1 1-1h4a1 1 0 0 1 1 1v3M4 7h16" stroke-linecap="round" stroke-linejoin="round"/></svg>`}
            ></span>
          </div>

          <div class="text-center mt-5">
            <h3
              class="text-lg lg:text-xl font-semibold mb-2 transition-all duration-300
              {active === '/delete'
                ? 'text-zinc-100'
                : 'text-zinc-200 group-hover:text-zinc-100'}
            "
            >
              Eliminar Estudiante
            </h3>
            <p
              class="text-sm leading-relaxed transition-all duration-300
              {active === '/delete'
                ? 'text-zinc-400'
                : 'text-zinc-500 group-hover:text-zinc-400'}
            "
            >
              Elimina estudiantes del sistema de manera segura y controlada.
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  /* Animación personalizada para el icono de lista */
  :global(.group:hover svg path) {
    animation-duration: 1s;
    animation-iteration-count: infinite;
  }
</style>

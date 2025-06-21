<script lang="ts">
  import ListStudents from "./ListStudents.svelte"; // Renombrado el componente para listar
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
  <div class="relative w-full h-full max-w-7xl p-5">
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
              use:html={`<svg viewBox="0 0 24 24" class="w-8 h-8 transition-transform duration-300 group-hover:scale-110 group-hover:rotate-90" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M12 4.5v15m7.5-7.5h-15" stroke-linecap="round" stroke-linejoin="round"/></svg>`}
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

      <!-- Ver estudiantes: 2 columnas, 2 filas, SOLO un div, sin borde externo, para el bento grid -->
      <div class="lg:col-span-2 lg:row-span-2 flex flex-col h-full">
        <ListStudents bento />
      </div>
    </div>
  </div>
</div>

<style>
  /* Elimina bordes dobles del bento grid */
  .grid > a,
  .grid > div {
    border-width: 1.5px;
  }
  /* Animación personalizada para el icono de lista */
  :global(.group:hover svg path) {
    animation-duration: 1s;
    animation-iteration-count: infinite;
  }
</style>
<script lang="ts">
  export let active: string = "";
  const items = [
    {
      label: "Crear estudiante",
      path: "/create",
      description:
        "Agrega un nuevo estudiante al sistema de forma rápida y sencilla.",
      icon: `<svg viewBox="0 0 24 24" class="w-8 h-8 transition-transform duration-300 group-hover:scale-110 group-hover:rotate-90" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M12 5v14m7-7H5" stroke-linecap="round" stroke-linejoin="round"/></svg>`,
      size: "large",
    },
    {
      label: "Ver estudiantes",
      path: "/list",
      description:
        "Consulta y explora la lista completa de todos los estudiantes registrados.",
      icon: `<svg viewBox="0 0 24 24" class="w-8 h-8 transition-all duration-300 group-hover:scale-110" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M4 6h16M4 12h16M4 18h16" stroke-linecap="round" stroke-linejoin="round"/></svg>`,
      size: "medium",
    },
    {
      label: "Actualizar estudiante",
      path: "/update",
      description:
        "Edita y modifica la información de cualquier estudiante existente.",
      icon: `<svg viewBox="0 0 24 24" class="w-8 h-8 transition-all duration-300 group-hover:scale-110 group-hover:-rotate-12" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M11 4h2v16m-1-8h8" stroke-linecap="round" stroke-linejoin="round"/></svg>`,
      size: "small",
    },
    {
      label: "Eliminar estudiante",
      path: "/delete",
      description:
        "Elimina estudiantes del sistema de manera segura y controlada.",
      icon: `<svg viewBox="0 0 24 24" class="w-8 h-8 transition-all duration-300 group-hover:scale-110 group-hover:rotate-12" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M19 7l-.867 12.142A2 2 0 0 1 16.138 21H7.862a2 2 0 0 1-1.995-1.858L5 7m5 4v6m4-6v6M9 7V4a1 1 0 0 1 1-1h4a1 1 0 0 1 1 1v3M4 7h16" stroke-linecap="round" stroke-linejoin="round"/></svg>`,
      size: "small",
    },
  ];

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
<div class="min-h-screen flex items-center justify-center">
  <div class="w-full max-w-5xl">
    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6 lg:grid-rows-2">
      <!-- Crear estudiante: 2 columnas, 2 filas -->
      <a
        href={items[0].path}
        class="group block rounded-2xl border transition-all duration-200 ease-out p-12 hover:border-zinc-600 hover:shadow-lg hover:shadow-zinc-900/20 lg:col-span-2
          {active === items[0].path
          ? 'border-zinc-500 bg-zinc-900/50'
          : 'border-zinc-800 bg-zinc-900/30 hover:bg-zinc-900/50'}"
        aria-current={active === items[0].path ? "page" : undefined}
      >
        <div
          class="flex flex-col items-center justify-center text-center h-full"
        >
          <div
            class="flex-shrink-0 p-6 mb-6 rounded-xl transition-all duration-300 overflow-hidden
            {active === items[0].path
              ? 'bg-zinc-700/80 text-zinc-300'
              : 'bg-zinc-800/60 text-zinc-400 group-hover:bg-zinc-700/60 group-hover:text-zinc-300'}
          "
          >
            <span use:html={items[0].icon}></span>
          </div>

          <div class="text-center">
            <h3
              class="text-3xl lg:text-4xl font-semibold mb-4 transition-all duration-300
              {active === items[0].path
                ? 'text-zinc-100'
                : 'text-zinc-200 group-hover:text-zinc-100'}
            "
            >
              {items[0].label}
            </h3>
            <p
              class="text-lg leading-relaxed transition-all duration-300 max-w-md mx-auto
              {active === items[0].path
                ? 'text-zinc-400'
                : 'text-zinc-500 group-hover:text-zinc-400'}
            "
            >
              {items[0].description}
            </p>
          </div>
        </div>
      </a>

      <!-- Ver estudiantes: 1 columna, 2 filas -->
      <a
        href={items[1].path}
        class="group block rounded-2xl border transition-all duration-200 ease-out p-8 hover:border-zinc-600 hover:shadow-lg hover:shadow-zinc-900/20 lg:col-span-1 lg:row-span-2
          {active === items[1].path
          ? 'border-zinc-500 bg-zinc-900/50'
          : 'border-zinc-800 bg-zinc-900/30 hover:bg-zinc-900/50'}"
        aria-current={active === items[1].path ? "page" : undefined}
      >
        <div
          class="flex flex-col items-center text-center justify-center h-full"
        >
          <div
            class="flex-shrink-0 p-4 mb-6 rounded-xl transition-all duration-300 overflow-hidden
            {active === items[1].path
              ? 'bg-zinc-700/80 text-zinc-300'
              : 'bg-zinc-800/60 text-zinc-400 group-hover:bg-zinc-700/60 group-hover:text-zinc-300'}
          "
          >
            <span use:html={items[1].icon}></span>
          </div>

          <div class="text-center">
            <h3
              class="text-xl lg:text-2xl font-semibold mb-4 transition-all duration-300
              {active === items[1].path
                ? 'text-zinc-100'
                : 'text-zinc-200 group-hover:text-zinc-100'}
            "
            >
              {items[1].label}
            </h3>
            <p
              class="text-base leading-relaxed transition-all duration-300
              {active === items[1].path
                ? 'text-zinc-400'
                : 'text-zinc-500 group-hover:text-zinc-400'}
            "
            >
              {items[1].description}
            </p>
          </div>
        </div>
      </a>

      <!-- Actualizar estudiante: 1 columna, 1 fila -->
      <a
        href={items[2].path}
        class="group block rounded-2xl border transition-all duration-200 ease-out p-6 hover:border-zinc-600 hover:shadow-lg hover:shadow-zinc-900/20 lg:col-span-1 lg:row-span-1
          {active === items[2].path
          ? 'border-zinc-500 bg-zinc-900/50'
          : 'border-zinc-800 bg-zinc-900/30 hover:bg-zinc-900/50'}"
        aria-current={active === items[2].path ? "page" : undefined}
      >
        <div
          class="flex flex-col items-center text-center justify-center h-full"
        >
          <div
            class="flex-shrink-0 p-3 mb-4 rounded-xl transition-all duration-300 overflow-hidden
            {active === items[2].path
              ? 'bg-zinc-700/80 text-zinc-300'
              : 'bg-zinc-800/60 text-zinc-400 group-hover:bg-zinc-700/60 group-hover:text-zinc-300'}
          "
          >
            <span use:html={items[2].icon}></span>
          </div>

          <div class="text-center">
            <h3
              class="text-lg lg:text-xl font-semibold mb-2 transition-all duration-300
              {active === items[2].path
                ? 'text-zinc-100'
                : 'text-zinc-200 group-hover:text-zinc-100'}
            "
            >
              {items[2].label}
            </h3>
            <p
              class="text-sm leading-relaxed transition-all duration-300
              {active === items[2].path
                ? 'text-zinc-400'
                : 'text-zinc-500 group-hover:text-zinc-400'}
            "
            >
              {items[2].description}
            </p>
          </div>
        </div>
      </a>

      <!-- Eliminar estudiante: 1 columna, 1 fila -->
      <a
        href={items[3].path}
        class="group block rounded-2xl border transition-all duration-200 ease-out p-6 hover:border-zinc-600 hover:shadow-lg hover:shadow-zinc-900/20 lg:col-span-1 lg:row-span-1
          {active === items[3].path
          ? 'border-zinc-500 bg-zinc-900/50'
          : 'border-zinc-800 bg-zinc-900/30 hover:bg-zinc-900/50'}"
        aria-current={active === items[3].path ? "page" : undefined}
      >
        <div
          class="flex flex-col items-center text-center justify-center h-full"
        >
          <div
            class="flex-shrink-0 p-3 mb-4 rounded-xl transition-all duration-300 overflow-hidden
            {active === items[3].path
              ? 'bg-zinc-700/80 text-zinc-300'
              : 'bg-zinc-800/60 text-zinc-400 group-hover:bg-zinc-700/60 group-hover:text-zinc-300'}
          "
          >
            <span use:html={items[3].icon}></span>
          </div>

          <div class="text-center">
            <h3
              class="text-lg lg:text-xl font-semibold mb-2 transition-all duration-300
              {active === items[3].path
                ? 'text-zinc-100'
                : 'text-zinc-200 group-hover:text-zinc-100'}
            "
            >
              {items[3].label}
            </h3>
            <p
              class="text-sm leading-relaxed transition-all duration-300
              {active === items[3].path
                ? 'text-zinc-400'
                : 'text-zinc-500 group-hover:text-zinc-400'}
            "
            >
              {items[3].description}
            </p>
          </div>
        </div>
      </a>
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

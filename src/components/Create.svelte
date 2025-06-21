<script lang="ts">
  let nombre = "";
  let apellido = "";
  let edad: number | null = null;
  let carrera = "";
  let loading = false;
  let error = "";
  let success = false;
  let createdStudent: StudentResponse | null = null; // Nueva variable para almacenar el estudiante creado
  let studentId: string | null = null; // ID del estudiante creado

  interface NewStudent {
    nombre: string;
    apellido: string;
    edad: number;
    carrera: string;
  }

  interface StudentResponse {
    id: string;
    nombre: string;
    apellido: string;
    edad: number;
    carrera: string;
    created_at?: string;
    updated_at?: string;
  }

  async function handleSubmit() {
    // Validación básica
    if (!nombre.trim() || !apellido.trim() || !edad || !carrera.trim()) {
      error = "Todos los campos son obligatorios";
      return;
    }

    if (edad < 6 || edad > 120) {
      error = "La edad debe ser entre 6 y 120 años";
      return;
    }

    loading = true;
    error = "";

    try {
      const newStudent: NewStudent = {
        nombre: nombre.trim(),
        apellido: apellido.trim(),
        edad: edad,
        carrera: carrera.trim(),
      };

      const backendUrl = "https://rust-backend-veyq.shuttle.app";

      const response = await fetch(`${backendUrl}/create_student`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(newStudent),
      });

      if (response.ok) {
        // Obtener la respuesta que debería incluir el ID del estudiante creado
        const createdStudentResponse = await response.json();
        studentId =
          createdStudentResponse.id || createdStudentResponse.student_id;

        // Obtener los datos completos del estudiante recién creado
        if (studentId) {
          await fetchCreatedStudent(studentId);
        } else {
          console.error("Error: studentId is null");
        }
        success = true;
      } else {
        const errorData = await response.text();
        error = `Error al crear estudiante: ${errorData}`;
      }
    } catch (err) {
      error = "Error de conexión. Intenta nuevamente.";
      console.error("Error:", err);
    } finally {
      loading = false;
    }
  }

  async function fetchCreatedStudent(id: string) {
    try {
      const backendUrl =
        import.meta.env.VITE_BACKEND_URL || "http://localhost:8000";
      const response = await fetch(`${backendUrl}/get_student/${id}`);

      if (response.ok) {
        createdStudent = await response.json();
      }
    } catch (err) {
      console.error("Error al obtener datos del estudiante:", err);
    }
  }

  // Función para formatear fecha
  function formatDate(dateString: string) {
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

  function handleApellidoInput(event: Event) {
    const value = event.target ? (event.target as HTMLInputElement).value : "";
    // Permitir solo letras (incluye acentos), espacios y apostrofes
    const cleanValue = value.replace(/[^A-Za-zÀ-ÿ\u00f1\u00d1\s']/g, "");

    // Evitar espacios múltiples
    const finalValue = cleanValue.replace(/\s+/g, " ");

    // Evitar que empiece con espacio
    apellido = finalValue.replace(/^\s/, "");

    // Actualizar el valor del input si fue modificado
    if (event.target && (event.target as HTMLInputElement).value !== apellido) {
      if (event.target) {
        (event.target as HTMLInputElement).value = apellido;
      }
    }
  }

  function handleNombreInput(event: Event) {
    const value = event.target ? (event.target as HTMLInputElement).value : "";
    // Permitir solo letras (incluye acentos), espacios y apostrofes
    const cleanValue = value.replace(/[^A-Za-zÀ-ÿ\u00f1\u00d1\s']/g, "");

    // Evitar espacios múltiples
    const finalValue = cleanValue.replace(/\s+/g, " ");

    // Evitar que empiece con espacio
    nombre = finalValue.replace(/^\s/, "");

    // Actualizar el valor del input si fue modificado
    if (event.target && (event.target as HTMLInputElement).value !== nombre) {
      (event.target as HTMLInputElement).value = nombre;
    }
  }

  // Función para manejar la entrada de carrera
  function handleCarreraInput(event: Event) {
    const value = event.target ? (event.target as HTMLInputElement).value : "";
    const cleanValue = value.replace(/[^A-Za-zÀ-ÿ\u00f1\u00d1\s']/g, "");
    const finalValue = cleanValue.replace(/\s+/g, " ");
    carrera = finalValue.replace(/^\s/, "");
    if (event.target && (event.target as HTMLInputElement).value !== carrera) {
      (event.target as HTMLInputElement).value = carrera;
    }
  }

  function preventNumbers(event: KeyboardEvent) {
    const allowedKeys = [
      "Backspace",
      "Delete",
      "ArrowLeft",
      "ArrowRight",
      "ArrowUp",
      "ArrowDown",
      "Home",
      "End",
      "Tab",
      "Enter",
      " ",
    ];

    // Si es una tecla de control, permitirla
    if (allowedKeys.includes(event.key) || event.ctrlKey || event.metaKey) {
      return;
    }

    // Si no es una letra (incluye acentos) o apostrofe, prevenir
    if (!/^[A-Za-zÀ-ÿ\u00f1\u00d1']$/.test(event.key)) {
      event.preventDefault();
    }
  }

  function handleEdadInput(event: Event) {
    const value = event.target
      ? parseInt((event.target as HTMLInputElement).value)
      : NaN;
    if (value < 1) {
      edad = null;
      if (event.target) {
        (event.target as HTMLInputElement).value = "";
      }
    } else if (value > 80) {
      edad = 80;
      if (event.target) {
        (event.target as HTMLInputElement).value = "80";
      }
    } else if (!isNaN(value)) {
      edad = value;
    }
  }

  function preventNegative(event: KeyboardEvent) {
    // Prevenir el signo menos (-) y el punto (.)
    if (
      event.key === "-" ||
      event.key === "." ||
      event.key === "+" ||
      event.key === "e" ||
      event.key === "E"
    ) {
      event.preventDefault();
    }
  }

  // Función para obtener información de progreso
  $: progressInfo = {
    completedFields: [nombre, apellido, edad, carrera].filter(
      (field) => field !== "" && field !== null,
    ).length,
    totalFields: 4,
    percentage: Math.round(
      ([nombre, apellido, edad, carrera].filter(
        (field) => field !== "" && field !== null,
      ).length /
        4) *
        100,
    ),
  };
</script>

<!-- Contenedor principal centrado -->
<div class="h-full w-3/4 p-10 grid grid-cols-3 grid-rows-2 gap-10 text-white">
  <!-- Sección principal del formulario -->
  <section
    class="col-span-2 row-span-2 rounded-2xl border border-zinc-800 bg-zinc-900/30 p-8"
  >
    <article>
      <div class="flex items-center gap-4 mb-6">
        <div class="p-3 rounded-xl bg-zinc-800/60 text-zinc-400">
          <svg
            viewBox="0 0 24 24"
            class="w-8 h-8"
            fill="none"
            stroke="currentColor"
            stroke-width="1.5"
          >
            <path
              d="M15.75 6a3.75 3.75 0 1 1-7.5 0 3.75 3.75 0 0 1 7.5 0ZM4.501 20.118a7.5 7.5 0 0 1 14.998 0A17.933 17.933 0 0 1 12 21.75c-2.676 0-5.216-.584-7.499-1.632Z"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
        </div>
        <h1 class="text-3xl font-bold text-zinc-100">Datos del Estudiante</h1>
      </div>

      <!-- Mensajes de estado -->
      {#if error}
        <div
          class="mb-6 p-4 rounded-xl bg-red-900/20 border border-red-800/50 text-red-400 flex items-center gap-3"
        >
          <svg
            class="w-5 h-5 flex-shrink-0"
            viewBox="0 0 24 24"
            fill="currentColor"
          >
            <path
              d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-2 15l-5-5 1.41-1.41L10 14.17l7.59-7.59L19 8l-9 9z"
            />
          </svg>
          {error}
        </div>
      {/if}

      {#if success}
        <div
          class="mb-6 p-4 rounded-xl bg-green-900/20 border border-green-800/50 text-green-400 flex items-center gap-3"
        >
          <svg
            class="w-5 h-5 flex-shrink-0"
            viewBox="0 0 24 24"
            fill="currentColor"
          >
            <path d="M9 16.17L4.83 12l-1.42 1.41L9 19 21 7l-1.41-1.41z" />
          </svg>
          ¡Estudiante creado exitosamente!
        </div>
      {/if}

      <form onsubmit={handleSubmit} class="space-y-6">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <!-- Campo Nombre -->
          <div class="space-y-2">
            <label
              for="nombre"
              class="text-sm font-medium text-zinc-300 flex items-center gap-2"
            >
              <svg
                class="w-4 h-4"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2" />
                <circle cx="12" cy="7" r="4" />
              </svg>
              Nombre *
            </label>
            <input
              id="nombre"
              type="text"
              bind:value={nombre}
              disabled={loading}
              placeholder="Ingresa el nombre"
              oninput={(event) => handleNombreInput(event)}
              onkeydown={preventNumbers}
              pattern="[A-Za-zÀ-ÿ\u00f1\u00d1\s']+"
              class="w-full px-4 py-3 rounded-xl border border-zinc-700 bg-zinc-800/50 text-zinc-100 placeholder-zinc-500
           focus:outline-none focus:ring-2 focus:ring-zinc-600 focus:border-transparent
           transition-all duration-200 disabled:opacity-50 disabled:cursor-not-allowed"
            />
            {#if nombre && !/^[A-Za-zÀ-ÿ\u00f1\u00d1\s']+$/.test(nombre)}
              <p class="text-xs text-red-400">
                El nombre solo puede contener letras y espacios
              </p>
            {/if}
          </div>

          <!-- Campo Apellido -->
          <div class="space-y-2">
            <label
              for="apellido"
              class="text-sm font-medium text-zinc-300 flex items-center gap-2"
            >
              <svg
                class="w-4 h-4"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2" />
                <circle cx="12" cy="7" r="4" />
              </svg>
              Apellido *
            </label>
            <input
              id="apellido"
              type="text"
              bind:value={apellido}
              disabled={loading}
              placeholder="Ingresa el apellido"
              oninput={handleApellidoInput}
              onkeydown={preventNumbers}
              pattern="[A-Za-zÀ-ÿ\u00f1\u00d1\s']+"
              class="w-full px-4 py-3 rounded-xl border border-zinc-700 bg-zinc-800/50 text-zinc-100 placeholder-zinc-500
           focus:outline-none focus:ring-2 focus:ring-zinc-600 focus:border-transparent
           transition-all duration-200 disabled:opacity-50 disabled:cursor-not-allowed"
            />
            {#if apellido && !/^[A-Za-zÀ-ÿ\u00f1\u00d1\s']+$/.test(apellido)}
              <p class="text-xs text-red-400">
                El apellido solo puede contener letras y espacios
              </p>
            {/if}
          </div>

          <div class="grid grid-cols-1 gap-6">
            <!-- Campo Edad -->
            <div class="space-y-2">
              <label
                for="edad"
                class="text-sm font-medium text-zinc-300 flex items-center gap-2"
              >
                <svg
                  class="w-4 h-4"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                >
                  <circle cx="12" cy="12" r="10" />
                  <polyline points="12,6 12,12 16,14" />
                </svg>
                Edad *
              </label>
              <input
                id="edad"
                type="number"
                bind:value={edad}
                disabled={loading}
                placeholder="Edad mínima 1 año"
                min="1"
                max="80"
                oninput={handleEdadInput}
                onkeydown={preventNegative}
                class="w-full px-4 py-3 rounded-xl border border-zinc-700 bg-zinc-800/50 text-zinc-100 placeholder-zinc-500
           focus:outline-none focus:ring-2 focus:ring-zinc-600 focus:border-transparent
           transition-all duration-200 disabled:opacity-50 disabled:cursor-not-allowed
           custom-spinner"
              />
            </div>
          </div>
          <div class="grid grid-cols-1 gap-6">
            <!-- Campo Carrera -->
            <div class="space-y-2">
              <label
                for="carrera"
                class="text-sm font-medium text-zinc-300 flex items-center gap-2"
              >
                <svg
                  class="w-4 h-4"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                >
                  <path d="M22 10v6M2 10l10-5 10 5-10 5z" />
                  <path d="M6 12v5c3 3 9 3 12 0v-5" />
                </svg>
                Carrera *
              </label>
              <input
                id="carrera"
                type="text"
                bind:value={carrera}
                disabled={loading}
                placeholder="Ingresa la carrera"
                oninput={handleCarreraInput}
                onkeydown={preventNumbers}
                pattern="[A-Za-zÀ-ÿ\u00f1\u00d1\s']+"
                class="w-full px-4 py-3 rounded-xl border border-zinc-700 bg-zinc-800/50 text-zinc-100 placeholder-zinc-500
           focus:outline-none focus:ring-2 focus:ring-zinc-600 focus:border-transparent
           transition-all duration-200 disabled:opacity-50 disabled:cursor-not-allowed"
              />
              {#if carrera && !/^[A-Za-zÀ-ÿ\u00f1\u00d1\s']+$/.test(carrera)}
                <p class="text-xs text-red-400">
                  La carrera solo puede contener letras y espacios
                </p>
              {/if}
            </div>
          </div>
        </div>
        <!-- Información adicional -->
        <div
          class="mt-8 p-4 rounded-xl bg-zinc-800/30 border border-zinc-700/50"
        >
          <div class="flex items-center justify-center gap-2 py-2">
              <svg
                class="w-4 h-4 text-zinc-500"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <circle cx="12" cy="12" r="10" />
                <path d="m9 12 2 2 4-4" />
              </svg>
              <p class="text-sm text-zinc-400 text-center">
                Completa el formulario para registrar un nuevo estudiante
              </p>
            </div>
        </div>
      </form>
    </article>
  </section>

  <!-- Sección de detalles del proceso -->
  <section
    class="col-span-1 row-span-1 rounded-2xl border border-zinc-800 bg-zinc-900/30 p-6"
  >
    <article>
      <div class="flex items-center gap-3 mb-6">
        <div class="p-2 rounded-lg bg-zinc-800/60 text-zinc-400">
          <svg
            class="w-6 h-6"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"
            />
            <polyline points="14,2 14,8 20,8" />
            <line x1="16" y1="13" x2="8" y2="13" />
            <line x1="16" y1="17" x2="8" y2="17" />
            <polyline points="10,9 9,9 8,9" />
          </svg>
        </div>
        <h1 class="text-xl font-bold text-zinc-100">Detalles del proceso</h1>
      </div>

      <!-- Progreso del formulario -->
      <div class="space-y-3">
        <div class="flex justify-between items-center">
          <span class="text-sm text-zinc-300">Progreso del formulario</span>
          <span class="text-sm font-medium text-zinc-200"
            >{progressInfo.percentage}%</span
          >
        </div>

        <div class="w-full bg-zinc-800 rounded-full h-2">
          <div
            class="bg-gradient-to-r from-zinc-600 to-zinc-500 h-2 rounded-full transition-all duration-300"
            style="width: {progressInfo.percentage}%"
          ></div>
        </div>

        <div class="text-xs text-zinc-400">
          {progressInfo.completedFields} de {progressInfo.totalFields} campos completados
        </div>

        <!-- Estado de campos -->
        <div class="space-y-2">
          <div class="flex items-center gap-2 text-sm">
            <div
              class="w-2 h-2 rounded-full {nombre
                ? 'bg-green-500'
                : 'bg-zinc-600'}"
            ></div>
            <span class="text-zinc-300">Nombre</span>
          </div>
          <div class="flex items-center gap-2 text-sm">
            <div
              class="w-2 h-2 rounded-full {apellido
                ? 'bg-green-500'
                : 'bg-zinc-600'}"
            ></div>
            <span class="text-zinc-300">Apellido</span>
          </div>
          <div class="flex items-center gap-2 text-sm">
            <div
              class="w-2 h-2 rounded-full {edad
                ? 'bg-green-500'
                : 'bg-zinc-600'}"
            ></div>
            <span class="text-zinc-300">Edad</span>
          </div>
          <div class="flex items-center gap-2 text-sm">
            <div
              class="w-2 h-2 rounded-full {carrera
                ? 'bg-green-500'
                : 'bg-zinc-600'}"
            ></div>
            <span class="text-zinc-300">Carrera</span>
          </div>
        </div>
      </div>
    </article>
  </section>

  <!-- Sección de acciones -->
  <section
    class="col-span-1 row-span-1 rounded-2xl border border-zinc-800 bg-zinc-900/30 p-6"
  >
    <article class="h-full flex flex-col justify-center">
      <div class="space-y-4">
        <!-- Botón Registrar -->
        <button
          type="submit"
          onclick={handleSubmit}
          disabled={loading}
          class="w-full px-6 py-4 rounded-xl bg-zinc-700 text-zinc-100 font-medium text-lg
                 hover:bg-zinc-600 focus:outline-none focus:ring-2 focus:ring-zinc-600
                 transition-all duration-200 disabled:opacity-50 disabled:cursor-not-allowed
                 flex items-center justify-center gap-3 cursor-pointer"
        >
          {#if loading}
            <svg class="animate-spin h-5 w-5" viewBox="0 0 24 24">
              <circle
                class="opacity-25"
                cx="12"
                cy="12"
                r="10"
                stroke="currentColor"
                stroke-width="4"
                fill="none"
              ></circle>
              <path
                class="opacity-75"
                fill="currentColor"
                d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
              ></path>
            </svg>
            Registrando...
          {:else}
            <svg
              class="w-5 h-5"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
            >
              <path
                d="M12 4.5v15m7.5-7.5h-15"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
            </svg>
            Registrar Estudiante
          {/if}
        </button>

        <!-- Botón Volver -->
        <a
          href="/"
          class="w-full px-6 py-4 rounded-xl border border-zinc-700 text-zinc-300 font-medium text-lg
                 hover:bg-zinc-800/50 hover:border-zinc-600 focus:outline-none focus:ring-2 focus:ring-zinc-600
                 transition-all duration-200 disabled:opacity-50 disabled:cursor-not-allowed
                 flex items-center justify-center gap-3 text-center"
        >
          <svg
            class="w-5 h-5"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              d="M19 12H5m7-7-7 7 7 7"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
          Volver al inicio
        </a>

        <!-- Información adicional -->
        <div class="mt-6 p-3 rounded-lg bg-zinc-800/20">
          <p class="text-xs text-zinc-500 text-center">
            Los datos se almacenarán de forma segura en la base de datos
          </p>
        </div>
      </div>
    </article>
  </section>
</div>

<style>
  /* Animaciones personalizadas */
  @keyframes fadeIn {
    from {
      opacity: 0;
      transform: translateY(10px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  @keyframes slideIn {
    from {
      opacity: 0;
      transform: translateX(-10px);
    }
    to {
      opacity: 1;
      transform: translateX(0);
    }
  }

  /* Efectos de hover personalizados */
  button:hover:not(:disabled) {
    transform: translateY(-1px);
  }

  a:hover {
    transform: translateY(-1px);
  }
  /* Personalizar spinners */
  .custom-spinner::-webkit-outer-spin-button,
  .custom-spinner::-webkit-inner-spin-button {
    opacity: 0.3;
    background: #52525b;
    border-radius: 4px;
  }

  .custom-spinner::-webkit-outer-spin-button:hover,
  .custom-spinner::-webkit-inner-spin-button:hover {
    opacity: 0.6;
    background: #71717a;
  }
</style>

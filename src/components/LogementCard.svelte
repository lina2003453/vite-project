<script>
  export let titre;
  export let localisation;
  export let prix;
  export let image;
  export let onSupprimer;
</script>

<article class="group bg-white rounded-3xl overflow-hidden shadow-lg border border-slate-200 hover:shadow-2xl hover:-translate-y-2 transition duration-300">
  <div class="relative overflow-hidden">
    <img
      class="w-full h-64 object-cover group-hover:scale-105 transition duration-500"
      src={image}
      alt={titre}
    />

    <div class="absolute inset-0 bg-gradient-to-t from-black/50 to-transparent"></div>

    <span class="absolute top-4 left-4 bg-white/90 text-blue-800 text-sm font-bold px-4 py-2 rounded-full shadow">
      Séjour ski
    </span>
  </div>

  <div class="p-6">
    <h3 class="text-2xl font-extrabold text-slate-900 mb-3">
      {titre}
    </h3>

    <p class="text-slate-500 mb-5">
      📍 {localisation}
    </p>

    <div class="flex items-center justify-between mb-4">
      <div>
        <p class="text-sm text-slate-400">À partir de</p>
        <p class="text-3xl font-extrabold text-blue-700">
          {prix} €
        </p>
      </div>

      <button class="bg-blue-700 text-white px-5 py-3 rounded-xl font-semibold hover:bg-blue-800 transition">
        Voir
      </button>
    </div>

    <button
      on:click={onSupprimer}
      class="w-full bg-red-600 text-white px-5 py-3 rounded-xl font-semibold hover:bg-red-700 transition"
    >
      Supprimer
    </button>
  </div>
</article>
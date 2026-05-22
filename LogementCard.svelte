<script>
  export let titre;
  export let localisation;
  export let prix;
  export let image;

  export let onSupprimer;

  export let lienDetail;
  export let lienModification;
</script>

<article class="group bg-white rounded-3xl overflow-hidden shadow-lg border border-slate-200 hover:shadow-2xl hover:-translate-y-2 transition duration-300">

  <div class="relative overflow-hidden">
    <img
      class="w-full h-64 object-cover group-hover:scale-105 transition duration-500"
      src={image}
      alt={titre}
    />

    <div class="absolute inset-0 bg-gradient-to-t from-black/50 to-transparent"></div>
  </div>

  <div class="p-6">
    <h3 class="text-2xl font-extrabold text-slate-900 mb-3">
      {titre}
    </h3>

    <p class="text-slate-500 mb-5">
      📍 {localisation}
    </p>

    <p class="text-3xl font-extrabold text-blue-700 mb-6">
      {prix} €
    </p>

    <div class="flex flex-col gap-3">

      <a
        href={lienDetail}
        class="bg-blue-700 text-center text-white px-5 py-3 rounded-xl font-semibold hover:bg-blue-800 transition"
      >
        Voir détail
      </a>

      <a
        href={lienModification}
        class="bg-yellow-500 text-center text-white px-5 py-3 rounded-xl font-semibold hover:bg-yellow-600 transition"
      >
        Modifier
      </a>

      <button
        on:click={onSupprimer}
        class="bg-red-600 text-white px-5 py-3 rounded-xl font-semibold hover:bg-red-700 transition"
      >
        Supprimer
      </button>

    </div>
  </div>
</article>
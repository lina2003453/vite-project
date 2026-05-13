<script>
  import './app.css';
  import { onMount } from 'svelte';
  import LogementCard from './lib/LogementCard.svelte';

  let logements = [];
  let loading = true;
  let error = null;

  let nom = '';
  let ville = '';
  let prix = '';

  async function chargerLogements() {
    loading = true;
    error = null;

    try {
      const response = await fetch('/api/logements');

      if (!response.ok) {
        throw new Error("Erreur GET");
      }

      const data = await response.json();
      logements = data;
    } catch (e) {
      console.log(e);
      error = "Impossible de charger les logements";
    }

    loading = false;
  }

  async function ajouterLogement() {
    try {
      const response = await fetch('/api/logements', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          nom: nom,
          ville: ville,
          prix: Number(prix),
          imageUrl: "https://images.pexels.com/photos/754268/pexels-photo-754268.jpeg",
          description: "Logement ajouté depuis SkiWeb",
          capacite: 4,
          proprietaireId: "69f8b01a50b9a786a524fd2d"
        })
      });

      if (!response.ok) {
        const erreur = await response.text();
        console.log(erreur);
        throw new Error("Erreur POST");
      }

      await chargerLogements();

      nom = '';
      ville = '';
      prix = '';
    } catch (e) {
      console.log(e);
      error = "Erreur lors de l'ajout du logement";
      loading = false;
    }
  }

  async function supprimerLogement(id) {
    try {
      const response = await fetch(`/api/logements/${id}`, {
        method: 'DELETE'
      });

      if (!response.ok) {
        throw new Error("Erreur DELETE");
      }

      await chargerLogements();
    } catch (e) {
      console.log(e);
      error = "Erreur lors de la suppression du logement";
      loading = false;
    }
  }

  function choisirImage(index) {
    if (index === 0) {
      return "https://images.pexels.com/photos/754268/pexels-photo-754268.jpeg";
    } else if (index === 1) {
      return "https://images.pexels.com/photos/618833/pexels-photo-618833.jpeg";
    } else if (index === 2) {
      return "https://images.pexels.com/photos/848599/pexels-photo-848599.jpeg";
    } else if (index === 3) {
      return "https://images.pexels.com/photos/773471/pexels-photo-773471.jpeg";
    } else if (index === 4) {
      return "https://images.pexels.com/photos/417074/pexels-photo-417074.jpeg";
    } else {
      return "https://images.pexels.com/photos/338515/pexels-photo-338515.jpeg";
    }
  }

  onMount(() => {
    chargerLogements();
  });
</script>

<header class="bg-gradient-to-br from-slate-950 via-blue-900 to-cyan-700 text-white">
  <div class="max-w-6xl mx-auto px-6 py-20 text-center">
    <p class="uppercase tracking-[0.3em] text-cyan-200 text-sm font-semibold mb-4">
      Séjours à la montagne
    </p>

    <h1 class="text-5xl md:text-7xl font-extrabold mb-6 drop-shadow-lg">
      SkiWeb
    </h1>

    <p class="text-lg md:text-2xl text-blue-100 max-w-3xl mx-auto leading-relaxed">
      Trouvez le logement idéal pour vos vacances au ski : chalets, appartements et séjours confortables en montagne.
    </p>
  </div>
</header>

<main class="bg-gradient-to-b from-slate-50 to-blue-50 min-h-screen">
  <section class="max-w-6xl mx-auto px-4 py-14">
    <div class="text-center mb-12">
      <p class="text-blue-700 font-semibold mb-2">
        Nos meilleures offres
      </p>

      <h2 class="text-3xl md:text-5xl font-extrabold text-slate-900">
        Logements disponibles
      </h2>

      <p class="text-slate-600 mt-4 max-w-2xl mx-auto">
        Une sélection de logements pour profiter pleinement de votre séjour au ski.
      </p>
    </div>

    <form
      on:submit|preventDefault={ajouterLogement}
      class="bg-white p-8 rounded-3xl shadow-lg mb-12 border border-slate-200"
    >
      <h3 class="text-2xl font-bold mb-6 text-slate-800">
        Ajouter un logement
      </h3>

      <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
        <input
          bind:value={nom}
          type="text"
          placeholder="Nom du logement"
          class="border border-slate-300 rounded-xl px-4 py-3"
        />

        <input
          bind:value={ville}
          type="text"
          placeholder="Ville"
          class="border border-slate-300 rounded-xl px-4 py-3"
        />

        <input
          bind:value={prix}
          type="number"
          placeholder="Prix"
          class="border border-slate-300 rounded-xl px-4 py-3"
        />
      </div>

      <button
        class="mt-6 bg-blue-700 text-white px-6 py-3 rounded-xl font-semibold hover:bg-blue-800 transition"
      >
        Ajouter
      </button>
    </form>

    {#if loading}
      <p class="text-center text-slate-600">Chargement des logements...</p>
    {:else if error}
      <p class="text-center text-red-600 font-bold">{error}</p>
    {:else}
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
        {#each logements as logement, index}
          <LogementCard
            titre={logement.nom}
            localisation={logement.ville}
            prix={logement.prix}
            image={choisirImage(index)}
            onSupprimer={() => supprimerLogement(logement.id)}
          />
        {/each}
      </div>
    {/if}
  </section>
</main>
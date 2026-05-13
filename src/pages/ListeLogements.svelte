<script>
  import { onMount } from 'svelte';
  import LogementCard from '../components/LogementCard.svelte';

  let logements = [];
  let loading = true;
  let error = null;

  async function chargerLogements() {
    loading = true;
    error = null;

    try {
      const response = await fetch('/api/logements');

      if (!response.ok) {
        throw new Error('Erreur GET');
      }

      logements = await response.json();

    } catch (e) {
      console.log(e);
      error = 'Impossible de charger les logements';
    }

    loading = false;
  }

  async function supprimerLogement(id) {
    try {
      const response = await fetch(`/api/logements/${id}`, {
        method: 'DELETE'
      });

      if (!response.ok) {
        throw new Error('Erreur DELETE');
      }

      await chargerLogements();

    } catch (e) {
      console.log(e);
      error = 'Erreur suppression';
    }
  }

  function choisirImage(index) {
    if (index === 0) {
      return "https://images.pexels.com/photos/754268/pexels-photo-754268.jpeg";

    } else if (index === 1) {
      return "https://images.pexels.com/photos/618833/pexels-photo-618833.jpeg";

    } else if (index === 2) {
      return "https://images.pexels.com/photos/848599/pexels-photo-848599.jpeg";

    } else {
      return "https://images.pexels.com/photos/338515/pexels-photo-338515.jpeg";
    }
  }

  onMount(() => {
    chargerLogements();
  });
</script>

<section class="max-w-6xl mx-auto px-4 py-14">

  <div class="flex justify-between items-center mb-10">

    <div>
      <h2 class="text-4xl font-extrabold text-slate-900">
        Logements
      </h2>

      <p class="text-slate-600 mt-2">
        Découvrez nos logements disponibles
      </p>
    </div>

    <a
      href="#/logements/add"
      class="bg-blue-700 text-white px-6 py-3 rounded-xl font-semibold hover:bg-blue-800 transition"
    >
      Ajouter un logement
    </a>

  </div>

  {#if loading}

    <p class="text-center text-slate-600">
      Chargement...
    </p>

  {:else if error}

    <p class="text-center text-red-600 font-bold">
      {error}
    </p>

  {:else}

    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">

      {#each logements as logement, index}

        <LogementCard
          titre={logement.nom}
          localisation={logement.ville}
          prix={logement.prix}
          image={choisirImage(index)}
          onSupprimer={() => supprimerLogement(logement.id)}
          lienDetail={`#/logements/${logement.id}`}
          lienModification={`#/logements/edit/${logement.id}`}
        />

      {/each}

    </div>

  {/if}

</section>
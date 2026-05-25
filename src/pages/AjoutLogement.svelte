<script>
  import { push } from 'svelte-spa-router';

  let nom = '';
  let ville = '';
  let prix = '';
  let error = null;

  async function ajouterLogement() {
    error = null;

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
        throw new Error('Erreur POST');
      }

      push('/logements');

    } catch (e) {
      console.log(e);
      error = "Erreur lors de l'ajout du logement";
    }
  }
</script>

<section class="max-w-3xl mx-auto px-4 py-14">

  <div class="bg-white p-8 rounded-3xl shadow-lg border border-slate-200">

    <h2 class="text-4xl font-extrabold text-slate-900 mb-8">
      Ajouter un logement
    </h2>

    {#if error}
      <p class="text-red-600 font-bold mb-4">
        {error}
      </p>
    {/if}

    <form on:submit|preventDefault={ajouterLogement}>

      <div class="mb-4">
        <label class="block font-semibold mb-2">Nom</label>
        <input
          bind:value={nom}
          type="text"
          class="w-full border border-slate-300 rounded-xl px-4 py-3"
        />
      </div>

      <div class="mb-4">
        <label class="block font-semibold mb-2">Ville</label>
        <input
          bind:value={ville}
          type="text"
          class="w-full border border-slate-300 rounded-xl px-4 py-3"
        />
      </div>

      <div class="mb-6">
        <label class="block font-semibold mb-2">Prix</label>
        <input
          bind:value={prix}
          type="number"
          class="w-full border border-slate-300 rounded-xl px-4 py-3"
        />
      </div>

      <div class="flex gap-4">
        <button
          class="bg-blue-700 text-white px-6 py-3 rounded-xl font-semibold hover:bg-blue-800 transition"
        >
          Ajouter
        </button>

        <a
          href="#/logements"
          class="bg-slate-700 text-white px-6 py-3 rounded-xl font-semibold hover:bg-slate-800 transition"
        >
          Annuler
        </a>
      </div>

    </form>

  </div>

</section>

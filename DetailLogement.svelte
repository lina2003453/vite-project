<script>
  import { onMount } from 'svelte';

  export let params;

  let logement = null;
  let loading = true;
  let error = null;

  let afficherModal = false;
  let dateDebut = '';
  let dateFin = '';
  let messageReservation = null;
  let erreurReservation = null;

  async function chargerLogement() {
    loading = true;
    error = null;

    try {
      const response = await fetch(`/api/logements/${params.id}`);

      if (!response.ok) {
        throw new Error('Erreur GET détail');
      }

      logement = await response.json();

    } catch (e) {
      console.log(e);
      error = 'Impossible de charger le logement';
    }

    loading = false;
  }

  async function reserverLogement() {

  messageReservation = null;
  erreurReservation = null;

  try {

    const response = await fetch('/api/reservations', {

      method: 'POST',

      headers: {
        'Content-Type': 'application/json'
      },

      body: JSON.stringify({
        logementId: logement.id,
        dateArrivee: dateDebut,
        dateDepart: dateFin
      })

    });

    if (!response.ok) {

      const erreur = await response.text();
      console.log(erreur);

      throw new Error('Erreur réservation');
    }

    messageReservation = 'Réservation envoyée avec succès';

    afficherModal = false;

    dateDebut = '';
    dateFin = '';

  } catch (e) {

    console.log(e);

    erreurReservation = 'Erreur lors de la réservation';
  }
}
  onMount(() => {
    chargerLogement();
  });
</script>

<section class="max-w-4xl mx-auto px-4 py-14">

  {#if loading}

    <p class="text-center text-slate-600">
      Chargement...
    </p>

  {:else if error}

    <p class="text-center text-red-600 font-bold">
      {error}
    </p>

  {:else}

    <div class="bg-white rounded-3xl shadow-xl overflow-hidden">

      <img
        src="https://images.pexels.com/photos/754268/pexels-photo-754268.jpeg"
        alt={logement.nom}
        class="w-full h-96 object-cover"
      />

      <div class="p-8">

        <h1 class="text-5xl font-extrabold text-slate-900 mb-4">
          {logement.nom}
        </h1>

        <p class="text-xl text-slate-500 mb-6">
          📍 {logement.ville}
        </p>

        <p class="text-4xl font-extrabold text-blue-700 mb-6">
          {logement.prix} €
        </p>

        <p class="text-slate-700 leading-relaxed mb-10">
          {logement.description}
        </p>

        {#if messageReservation}
          <p class="bg-green-100 text-green-700 font-bold p-4 rounded-xl mb-6">
            {messageReservation}
          </p>
        {/if}

        {#if erreurReservation}
          <p class="bg-red-100 text-red-700 font-bold p-4 rounded-xl mb-6">
            {erreurReservation}
          </p>
        {/if}

        <div class="flex gap-4 flex-wrap">

          <a
            href="#/logements"
            class="bg-slate-700 text-white px-6 py-3 rounded-xl font-semibold hover:bg-slate-800 transition"
          >
            Retour
          </a>

          <a
            href={`#/logements/edit/${logement.id}`}
            class="bg-yellow-500 text-white px-6 py-3 rounded-xl font-semibold hover:bg-yellow-600 transition"
          >
            Modifier
          </a>

          <button
            on:click={() => afficherModal = true}
            class="bg-blue-700 text-white px-6 py-3 rounded-xl font-semibold hover:bg-blue-800 transition"
          >
            Réserver
          </button>

        </div>

      </div>
    </div>

  {/if}

</section>

{#if afficherModal}
  <div class="fixed inset-0 bg-black/50 flex items-center justify-center px-4">

    <div class="bg-white rounded-3xl shadow-2xl p-8 w-full max-w-md">

      <h2 class="text-3xl font-extrabold text-slate-900 mb-6">
        Réserver ce logement
      </h2>

      <form on:submit|preventDefault={reserverLogement}>

        <div class="mb-4">
          <label class="block font-semibold mb-2">
            Date de début
          </label>

          <input
            bind:value={dateDebut}
            type="date"
            required
            class="w-full border border-slate-300 rounded-xl px-4 py-3"
          />
        </div>

        <div class="mb-6">
          <label class="block font-semibold mb-2">
            Date de fin
          </label>

          <input
            bind:value={dateFin}
            type="date"
            required
            class="w-full border border-slate-300 rounded-xl px-4 py-3"
          />
        </div>

        <div class="flex gap-4">

          <button
            type="submit"
            class="bg-blue-700 text-white px-6 py-3 rounded-xl font-semibold hover:bg-blue-800 transition"
          >
            Valider
          </button>

          <button
            type="button"
            on:click={() => afficherModal = false}
            class="bg-slate-700 text-white px-6 py-3 rounded-xl font-semibold hover:bg-slate-800 transition"
          >
            Annuler
          </button>

        </div>

      </form>

    </div>

  </div>
{/if}
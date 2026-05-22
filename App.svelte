<script>
  import './app.css';

  import Router from 'svelte-spa-router';

  import ListeLogements from './pages/ListeLogements.svelte';
  import DetailLogement from './pages/DetailLogement.svelte';
  import AjoutLogement from './pages/AjoutLogement.svelte';
  import ModifierLogement from './pages/ModifierLogement.svelte';

  const routes = {
    '/': ListeLogements,
    '/logements': ListeLogements,
    '/logements/add': AjoutLogement,
    '/logements/:id': DetailLogement,
    '/logements/edit/:id': ModifierLogement
  };
</script>

<header class="bg-gradient-to-br from-slate-950 via-blue-900 to-cyan-700 text-white">
  <div class="max-w-6xl mx-auto px-6 py-16 text-center">

    <p class="uppercase tracking-[0.3em] text-cyan-200 text-sm font-semibold mb-4">
      Séjours à la montagne
    </p>

    <h1 class="text-5xl md:text-7xl font-extrabold mb-6 drop-shadow-lg">
      SkiWeb
    </h1>

    <p class="text-lg md:text-2xl text-blue-100 max-w-3xl mx-auto leading-relaxed mb-8">
      Trouvez le logement idéal pour vos vacances au ski.
    </p>

    <nav class="flex justify-center gap-4 flex-wrap">
      

      <a
        href="#/logements"
        class="bg-white/10 px-5 py-3 rounded-xl hover:bg-white/20 transition"
      >
        Logements
      </a>

      <a
        href="#/logements/add"
        class="bg-white/10 px-5 py-3 rounded-xl hover:bg-white/20 transition"
      >
        Ajouter
      </a>
    </nav>

  </div>
</header>

<main class="bg-gradient-to-b from-slate-50 to-blue-50 min-h-screen">
  <Router {routes} />
</main>
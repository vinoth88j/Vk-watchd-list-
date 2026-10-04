/index
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>CineVault • Movies, Anime &amp; Web Series Vault</title>
  
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  
  <!-- Google Fonts: Inter & Plus Jakarta Sans -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=Plus+Jakarta+Sans:wght@700;800;900&display=swap" rel="stylesheet">
  
  <!-- Font Awesome CDN for mobile UI icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            brand: {
              50: '#f0fdf4',
              100: '#dcfce7',
              200: '#bbf7d0',
              300: '#86efac',
              400: '#4ade80',
              500: '#22c55e',
              600: '#16a34a',
              700: '#15803d',
              800: '#166534',
              900: '#14532d',
              950: '#052e16'
            }
          },
          fontFamily: {
            sans: ['Inter', 'sans-serif'],
            display: ['"Plus Jakarta Sans"', 'Inter', 'sans-serif'],
          }
        }
      }
    }
  </script>

  <style>
    body {
      font-family: 'Inter', sans-serif;
      background: #f7faf8;
      color: #0f291e;
      -webkit-tap-highlight-color: transparent;
    }
    
    /* Clean emerald custom scrollbar */
    ::-webkit-scrollbar {
      width: 5px;
      height: 5px;
    }
    ::-webkit-scrollbar-track {
      background: #f0fdf4;
    }
    ::-webkit-scrollbar-thumb {
      background: #22c55e;
      border-radius: 9999px;
    }

    /* Dynamic wallpaper background styling per area */
    #appBackgroundLayer {
      position: fixed;
      inset: 0;
      z-index: -2;
      background-size: cover;
      background-position: center center;
      background-repeat: no-repeat;
      transition: background-image 0.5s ease-in-out, opacity 0.4s ease, filter 0.3s ease;
      pointer-events: none;
    }

    /* Pattern overlay layer for flowers, anime art & series logos */
    #areaPatternOverlay {
      position: fixed;
      inset: 0;
      z-index: -2;
      pointer-events: none;
      opacity: 0.15;
      background-repeat: repeat;
      transition: opacity 0.4s ease;
    }

    #appBackdropTint {
      position: fixed;
      inset: 0;
      z-index: -1;
      pointer-events: none;
      transition: background 0.4s ease;
    }

    /* Poster aspect ratio classes */
    .aspect-poster-classic { aspect-ratio: 2 / 3; }
    .aspect-poster-wide    { aspect-ratio: 3 / 4; }
    .aspect-poster-square  { aspect-ratio: 1 / 1; }
    .aspect-poster-cinema  { aspect-ratio: 16 / 9; }

    /* Active mobile navigation button */
    .bottom-nav-item.active {
      color: #15803d;
    }
    .bottom-nav-item.active .nav-icon-wrap {
      background-color: #dcfce7;
      color: #15803d;
    }
  </style>
</head>

<body class="bg-gradient-to-b from-brand-50 via-white to-brand-100/40 min-h-screen flex flex-col pb-24 sm:pb-12 antialiased select-none relative">
  <!-- Dynamic Custom Background, Themed Pattern & Tint Layers (Works Offline) -->
  <div id="appBackgroundLayer"></div>
  <div id="areaPatternOverlay"></div>
  <div id="appBackdropTint" class="bg-gradient-to-b from-brand-50/90 via-white/85 to-brand-100/80 backdrop-blur-[2px]"></div>

  <!-- Main Top Header -->
  <header class="sticky top-0 z-40 bg-white/95 backdrop-blur-md border-b border-brand-100 shadow-xs">
    <div class="max-w-6xl mx-auto px-4 sm:px-6 py-2.5">
      <div class="flex items-center justify-between gap-2.5">
        
        <!-- Logo -->
        <div class="flex items-center gap-2.5 cursor-pointer" onclick="switchPage('gallery')">
          <div class="w-10 h-10 rounded-2xl bg-gradient-to-tr from-brand-800 to-brand-500 flex items-center justify-center text-white shadow-md shadow-brand-500/25 shrink-0">
            <i class="fa-solid fa-film text-base"></i>
          </div>
          <div>
            <h1 class="font-display font-black text-lg tracking-tight text-slate-900 leading-tight">
              Cine<span class="text-brand-600">Vault</span>
            </h1>
            <p class="text-[9px] text-slate-500 font-bold uppercase tracking-wider">Offline Poster Archive</p>
          </div>
        </div>

        <!-- Available Posters Total Count Badge (Strictly timeless, no dates) -->
        <div class="flex items-center gap-2">
          <!-- Offline / Online Status Pill -->
          <div id="offlineIndicatorBadge" class="hidden sm:flex items-center gap-1.5 px-2.5 py-1 rounded-full text-[10px] font-bold bg-brand-100 text-brand-800 border border-brand-200">
            <span class="w-2 h-2 rounded-full bg-brand-500 animate-pulse"></span>
            <span id="offlineStatusText">Offline Ready</span>
          </div>

          <!-- Total Available Posters Count Badge -->
          <div class="flex items-center gap-2 px-3 py-1.5 rounded-full bg-brand-50 border border-brand-200 text-brand-900 shadow-xs">
            <span class="relative flex h-2.5 w-2.5">
              <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-brand-400 opacity-75"></span>
              <span class="relative inline-flex rounded-full h-2.5 w-2.5 bg-brand-600"></span>
            </span>
            <div class="text-xs font-black flex items-center gap-1.5">
              <span id="headerAvailableCount" class="text-brand-700 text-sm">0</span>
              <span class="text-[11px] font-semibold text-slate-600">Available</span>
            </div>
          </div>
        </div>

        <!-- Desktop Navigation Tabs -->
        <div class="hidden md:flex items-center bg-brand-50/80 p-1 rounded-2xl border border-brand-200 text-xs font-bold gap-1">
          <button onclick="switchPage('gallery')" id="desktopTabGallery" class="px-3 py-1.5 rounded-xl bg-brand-600 text-white shadow-xs transition">Movies</button>
          <button onclick="switchPage('anime')" id="desktopTabAnime" class="px-3 py-1.5 rounded-xl text-slate-600 hover:text-slate-900 transition">Anime</button>
          <button onclick="switchPage('series')" id="desktopTabSeries" class="px-3 py-1.5 rounded-xl text-slate-600 hover:text-slate-900 transition">Web Series</button>
          <button onclick="openAddModalFor('movie')" id="desktopTabAdd" class="px-3 py-1.5 rounded-xl text-slate-600 hover:text-slate-900 transition">+ Add Item</button>
          <button onclick="switchPage('custom')" id="desktopTabCustom" class="px-3 py-1.5 rounded-xl text-slate-600 hover:text-slate-900 transition"><i class="fa-solid fa-sliders"></i> Customize</button>
        </div>

      </div>
    </div>
  </header>

  <main class="flex-1 max-w-6xl w-full mx-auto px-4 sm:px-6 pt-3">

    <!-- Mobile Category Navigation Tabs -->
    <div class="flex items-center gap-1.5 p-1 bg-white rounded-2xl border border-brand-100 shadow-xs mb-3 overflow-x-auto">
      <button onclick="switchPage('gallery')" id="pillBtnMovies" class="flex-1 py-2 px-3 rounded-xl font-bold text-xs bg-brand-600 text-white shadow-xs flex items-center justify-center gap-1.5 shrink-0 transition">
        <i class="fa-solid fa-film text-[11px]"></i>
        <span>Movies</span>
        <span id="pillCountMovies" class="text-[10px] px-1.5 py-0.5 rounded-full bg-white/20">0</span>
      </button>

      <button onclick="switchPage('anime')" id="pillBtnAnime" class="flex-1 py-2 px-3 rounded-xl font-bold text-xs text-slate-600 hover:text-slate-900 flex items-center justify-center gap-1.5 shrink-0 transition">
        <i class="fa-solid fa-dragon text-[11px]"></i>
        <span>Anime Watchlist</span>
        <span id="pillCountAnime" class="text-[10px] px-1.5 py-0.5 rounded-full bg-brand-100 text-brand-800">0</span>
      </button>

      <button onclick="switchPage('series')" id="pillBtnSeries" class="flex-1 py-2 px-3 rounded-xl font-bold text-xs text-slate-600 hover:text-slate-900 flex items-center justify-center gap-1.5 shrink-0 transition">
        <i class="fa-solid fa-tv text-[11px]"></i>
        <span>Web Series</span>
        <span id="pillCountSeries" class="text-[10px] px-1.5 py-0.5 rounded-full bg-brand-100 text-brand-800">0</span>
      </button>
    </div>

    <!-- ================= PAGE 1: MOVIES GALLERY ================= -->
    <section id="galleryPageView" class="space-y-4">

      <!-- Green & White Hero Banner (Available Counter + Offline Ready, strictly without dates) -->
      <div class="rounded-3xl bg-gradient-to-br from-brand-900 via-brand-800 to-emerald-950 text-white p-4 sm:p-5 shadow-lg border border-brand-700/60 relative overflow-hidden">
        <!-- Floating Daisy & Floral Accent in Banner -->
        <div class="absolute -right-4 -bottom-4 text-emerald-300/15 text-8xl pointer-events-none select-none">
          <i class="fa-solid fa-seedling"></i>
        </div>
        <div class="absolute right-12 top-2 text-white/10 text-5xl pointer-events-none select-none">
          <i class="fa-solid fa-spa"></i>
        </div>

        <div class="relative z-10 flex flex-col sm:flex-row sm:items-center justify-between gap-3 sm:gap-4">
          <div>
            <div class="flex items-center gap-2">
              <span class="inline-flex items-center gap-1.5 text-[10px] font-bold uppercase tracking-wider px-2.5 py-0.5 rounded-full bg-brand-700/70 border border-brand-500/40 text-brand-200">
                <i class="fa-solid fa-spa text-emerald-300"></i> White &amp; Green Flower Theme
              </span>
              <span class="inline-flex items-center gap-1 text-[10px] font-semibold text-emerald-300">
                <i class="fa-solid fa-shield-halved"></i> Timeless Archive (No Dates)
              </span>
            </div>
            <h2 class="font-display text-xl sm:text-2xl font-black text-white mt-1">
              Movie Posters Collection
            </h2>
            <p class="text-xs text-brand-100/90 mt-0.5 max-w-md">
              Tap any poster to see one-line pitch, movie time, and launch the user-provided watch URL.
            </p>
          </div>

          <!-- Counters (Available posters count) -->
          <div class="flex gap-2">
            <div class="flex-1 sm:flex-none sm:w-28 bg-white/10 backdrop-blur-md rounded-2xl p-2.5 border border-white/20 text-center">
              <span class="text-[9px] uppercase font-bold text-brand-200 block">Available</span>
              <span id="heroAvailableCount" class="font-display text-2xl font-black text-white">0</span>
              <span class="text-[9px] text-brand-200/80 block">Posters</span>
            </div>

            <div class="flex-1 sm:flex-none sm:w-28 bg-white/10 backdrop-blur-md rounded-2xl p-2.5 border border-white/20 text-center">
              <span class="text-[9px] uppercase font-bold text-brand-200 block">User Links</span>
              <span id="heroUrlsCount" class="font-display text-2xl font-black text-white">0</span>
              <span class="text-[9px] text-brand-200/80 block">Watch URLs</span>
            </div>
          </div>
        </div>
      </div>

      <!-- Search & Filters Toolbar -->
      <div class="bg-white rounded-2xl border border-brand-100 p-3 shadow-xs space-y-2.5">
        <div class="flex items-center gap-2">
          <!-- Search input -->
          <div class="relative flex-1">
            <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-1/2 -translate-y-1/2 text-brand-600 text-xs"></i>
            <input 
              type="text" 
              id="searchInput" 
              placeholder="Search movie title, genre, director, one-liner..." 
              class="w-full pl-9 pr-8 py-2.5 rounded-xl border border-brand-200 focus:border-brand-500 focus:ring-2 focus:ring-brand-200 text-xs outline-none bg-brand-50/20 text-slate-800"
            >
            <button id="clearSearchBtn" class="hidden absolute right-3 top-1/2 -translate-y-1/2 text-slate-400 hover:text-slate-700">
              <i class="fa-solid fa-circle-xmark text-xs"></i>
            </button>
          </div>

          <!-- Quick Add Button -->
          <button onclick="openAddModalFor('movie')" class="px-3.5 py-2.5 rounded-xl bg-brand-600 hover:bg-brand-700 text-white text-xs font-bold shadow-xs active:scale-95 transition shrink-0 flex items-center gap-1.5">
            <i class="fa-solid fa-plus"></i>
            <span class="hidden sm:inline">Add Movie</span>
          </button>
        </div>

        <!-- Filter & Sorting Pills -->
        <div class="flex items-center gap-2">
          <div class="relative flex-1">
            <select id="genreSelect" class="w-full appearance-none bg-white border border-brand-200 text-slate-700 text-xs font-medium rounded-xl px-3 py-2 pr-7 outline-none">
              <option value="ALL">All Genres</option>
            </select>
            <i class="fa-solid fa-chevron-down absolute right-2.5 top-1/2 -translate-y-1/2 text-brand-600 text-[10px] pointer-events-none"></i>
          </div>

          <div class="relative flex-1">
            <select id="sortSelect" class="w-full appearance-none bg-white border border-brand-200 text-slate-700 text-xs font-medium rounded-xl px-3 py-2 pr-7 outline-none">
              <option value="title_asc">Title (A - Z)</option>
              <option value="title_desc">Title (Z - A)</option>
              <option value="rating_desc">Highest Rating</option>
              <option value="time_desc">Longest Runtime</option>
            </select>
            <i class="fa-solid fa-arrow-down-short-wide absolute right-2.5 top-1/2 -translate-y-1/2 text-brand-600 text-[10px] pointer-events-none"></i>
          </div>
        </div>

        <!-- Count Indicator Bar -->
        <div class="pt-2 border-t border-brand-50 flex items-center justify-between text-[11px] text-slate-500 font-medium">
          <span id="filteredStatusText">Showing available posters</span>
          <span class="text-brand-800 font-bold"><span id="galleryCurrentCount">0</span> Available</span>
        </div>
      </div>

      <!-- Poster Cards Grid -->
      <div id="moviePosterGrid" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-3 sm:gap-4"></div>

      <!-- Empty State -->
      <div id="emptyGalleryNotice" class="hidden bg-white border-2 border-dashed border-brand-200 rounded-3xl p-8 text-center max-w-sm mx-auto my-6">
        <div class="w-14 h-14 rounded-2xl bg-brand-50 text-brand-600 flex items-center justify-center text-2xl mx-auto mb-3">
          <i class="fa-solid fa-film"></i>
        </div>
        <h3 class="font-display font-bold text-base text-slate-800 mb-1">No Movie Posters Found</h3>
        <p class="text-xs text-slate-500 mb-4">No movie posters match your current search or list is empty.</p>
        <button onclick="openAddModalFor('movie')" class="px-4 py-2 rounded-xl bg-brand-600 hover:bg-brand-700 text-xs font-bold text-white transition shadow-xs">
          + Add Movie Poster
        </button>
      </div>

    </section>

    <!-- ================= PAGE 2: ANIME WATCHLIST ================= -->
    <section id="animePageView" class="hidden space-y-4">
      
      <!-- Anime Banner -->
      <div class="rounded-3xl bg-gradient-to-br from-emerald-950 via-brand-900 to-teal-950 text-white p-4 sm:p-5 shadow-lg border border-brand-700/60 relative overflow-hidden">
        <!-- Floating Anime Mascot Icons Accent -->
        <div class="absolute -right-4 -bottom-4 text-emerald-400/10 text-8xl pointer-events-none select-none">
          <i class="fa-solid fa-dragon"></i>
        </div>
        <div class="absolute right-20 top-2 text-white/10 text-4xl pointer-events-none select-none">
          <i class="fa-solid fa-mask"></i>
        </div>

        <div class="relative z-10 flex flex-col sm:flex-row sm:items-center justify-between gap-3">
          <div>
            <span class="inline-flex items-center gap-1 text-[10px] font-bold uppercase tracking-wider px-2.5 py-0.5 rounded-full bg-brand-700/70 border border-brand-500/40 text-brand-200">
              <i class="fa-solid fa-user-ninja"></i> Anime Characters Backdrop
            </span>
            <h2 class="font-display text-xl sm:text-2xl font-black text-white mt-1">
              Anime Watchlist
            </h2>
            <p class="text-xs text-brand-100/90 mt-0.5">
              Tracks anime series, films, total episodes count, movie time, and direct watch URLs.
            </p>
          </div>

          <div class="flex items-center gap-2">
            <div class="bg-white/10 backdrop-blur-md rounded-2xl px-4 py-2 border border-white/20 text-center">
              <span class="text-[9px] uppercase font-bold text-brand-200 block">Anime Posters</span>
              <span id="animeAvailableCount" class="font-display text-2xl font-black text-white">0</span>
              <span class="text-[9px] text-brand-200/80 block">Available</span>
            </div>
            <button onclick="openAddModalFor('anime')" class="px-3.5 py-3 rounded-2xl bg-brand-500 hover:bg-brand-600 text-white text-xs font-black shadow-md flex items-center gap-1.5 transition">
              <i class="fa-solid fa-plus"></i>
              <span>Add Anime</span>
            </button>
          </div>
        </div>
      </div>

      <!-- Watch Status Tabs -->
      <div class="flex items-center gap-2 overflow-x-auto pb-1">
        <button onclick="filterWatchStatus('anime', 'ALL')" id="animeStatusAll" class="px-3 py-1.5 rounded-xl text-xs font-bold bg-brand-600 text-white shrink-0">All Anime</button>
        <button onclick="filterWatchStatus('anime', 'Watching')" id="animeStatusWatching" class="px-3 py-1.5 rounded-xl text-xs font-bold bg-white text-slate-700 border border-brand-100 shrink-0">Watching</button>
        <button onclick="filterWatchStatus('anime', 'Plan to Watch')" id="animeStatusPlan" class="px-3 py-1.5 rounded-xl text-xs font-bold bg-white text-slate-700 border border-brand-100 shrink-0">Plan to Watch</button>
        <button onclick="filterWatchStatus('anime', 'Completed')" id="animeStatusCompleted" class="px-3 py-1.5 rounded-xl text-xs font-bold bg-white text-slate-700 border border-brand-100 shrink-0">Completed</button>
      </div>

      <!-- Anime Posters Grid -->
      <div id="animePosterGrid" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-3 sm:gap-4"></div>

      <!-- Anime Empty State -->
      <div id="emptyAnimeNotice" class="hidden bg-white border-2 border-dashed border-brand-200 rounded-3xl p-8 text-center max-w-sm mx-auto my-6">
        <div class="w-14 h-14 rounded-2xl bg-brand-50 text-brand-600 flex items-center justify-center text-2xl mx-auto mb-3">
          <i class="fa-solid fa-dragon"></i>
        </div>
        <h3 class="font-display font-bold text-base text-slate-800 mb-1">No Anime Found</h3>
        <p class="text-xs text-slate-500 mb-4">Add your favorite anime posters, one-line synopsis, episode count, and streaming URLs.</p>
        <button onclick="openAddModalFor('anime')" class="px-4 py-2 rounded-xl bg-brand-600 hover:bg-brand-700 text-xs font-bold text-white transition shadow-xs">
          + Add First Anime
        </button>
      </div>

    </section>

    <!-- ================= PAGE 3: WEB SERIES WATCHLIST ================= -->
    <section id="seriesPageView" class="hidden space-y-4">
      
      <!-- Series Banner -->
      <div class="rounded-3xl bg-gradient-to-br from-brand-950 via-emerald-950 to-green-950 text-white p-4 sm:p-5 shadow-lg border border-brand-700/60 relative overflow-hidden">
        <!-- Series Backdrop Silhouette Accent -->
        <div class="absolute -right-4 -bottom-4 text-emerald-400/10 text-8xl pointer-events-none select-none">
          <i class="fa-solid fa-tv"></i>
        </div>
        <div class="absolute right-20 top-2 text-white/10 text-4xl pointer-events-none select-none">
          <i class="fa-solid fa-play"></i>
        </div>

        <div class="relative z-10 flex flex-col sm:flex-row sm:items-center justify-between gap-3">
          <div>
            <span class="inline-flex items-center gap-1 text-[10px] font-bold uppercase tracking-wider px-2.5 py-0.5 rounded-full bg-brand-700/70 border border-brand-500/40 text-brand-200">
              <i class="fa-solid fa-clapperboard"></i> Series Logos Backdrop
            </span>
            <h2 class="font-display text-xl sm:text-2xl font-black text-white mt-1">
              Web Series Watchlist
            </h2>
            <p class="text-xs text-brand-100/90 mt-0.5">
              Organize OTT and TV shows with seasons, episode count, one-liners &amp; user streaming links.
            </p>
          </div>

          <div class="flex items-center gap-2">
            <div class="bg-white/10 backdrop-blur-md rounded-2xl px-4 py-2 border border-white/20 text-center">
              <span class="text-[9px] uppercase font-bold text-brand-200 block">Series Posters</span>
              <span id="seriesAvailableCount" class="font-display text-2xl font-black text-white">0</span>
              <span class="text-[9px] text-brand-200/80 block">Available</span>
            </div>
            <button onclick="openAddModalFor('series')" class="px-3.5 py-3 rounded-2xl bg-brand-500 hover:bg-brand-600 text-white text-xs font-black shadow-md flex items-center gap-1.5 transition">
              <i class="fa-solid fa-plus"></i>
              <span>Add Series</span>
            </button>
          </div>
        </div>
      </div>

      <!-- Watch Status Tabs -->
      <div class="flex items-center gap-2 overflow-x-auto pb-1">
        <button onclick="filterWatchStatus('series', 'ALL')" id="seriesStatusAll" class="px-3 py-1.5 rounded-xl text-xs font-bold bg-brand-600 text-white shrink-0">All Series</button>
        <button onclick="filterWatchStatus('series', 'Watching')" id="seriesStatusWatching" class="px-3 py-1.5 rounded-xl text-xs font-bold bg-white text-slate-700 border border-brand-100 shrink-0">Watching</button>
        <button onclick="filterWatchStatus('series', 'Plan to Watch')" id="seriesStatusPlan" class="px-3 py-1.5 rounded-xl text-xs font-bold bg-white text-slate-700 border border-brand-100 shrink-0">Plan to Watch</button>
        <button onclick="filterWatchStatus('series', 'Completed')" id="seriesStatusCompleted" class="px-3 py-1.5 rounded-xl text-xs font-bold bg-white text-slate-700 border border-brand-100 shrink-0">Completed</button>
      </div>

      <!-- Series Posters Grid -->
      <div id="seriesPosterGrid" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-3 sm:gap-4"></div>

      <!-- Series Empty State -->
      <div id="emptySeriesNotice" class="hidden bg-white border-2 border-dashed border-brand-200 rounded-3xl p-8 text-center max-w-sm mx-auto my-6">
        <div class="w-14 h-14 rounded-2xl bg-brand-50 text-brand-600 flex items-center justify-center text-2xl mx-auto mb-3">
          <i class="fa-solid fa-tv"></i>
        </div>
        <h3 class="font-display font-bold text-base text-slate-800 mb-1">No Web Series Saved</h3>
        <p class="text-xs text-slate-500 mb-4">Add web series posters, one-line summary, episode count, and streaming URLs.</p>
        <button onclick="openAddModalFor('series')" class="px-4 py-2 rounded-xl bg-brand-600 hover:bg-brand-700 text-xs font-bold text-white transition shadow-xs">
          + Add First Series
        </button>
      </div>

    </section>

    <!-- ================= PAGE 4: ADD / EDIT ITEM FORM ================= -->
    <section id="addMoviePageView" class="hidden max-w-2xl mx-auto space-y-4">
      <div class="bg-white rounded-3xl border border-brand-100 shadow-md overflow-hidden">
        
        <div class="p-4 sm:p-5 bg-gradient-to-r from-brand-800 to-brand-600 text-white flex items-center justify-between">
          <div class="flex items-center gap-3">
            <div class="w-10 h-10 rounded-xl bg-white/20 flex items-center justify-center text-lg">
              <i id="formHeaderIcon" class="fa-solid fa-photo-film"></i>
            </div>
            <div>
              <h2 id="formPageTitle" class="font-display font-black text-lg">Save New Poster</h2>
              <p id="formPageSubtitle" class="text-[11px] text-brand-100">Poster, movie time, episode count &amp; user URL (Stored offline, no dates)</p>
            </div>
          </div>
          <button onclick="returnToPreviousPage()" class="text-xs bg-white/20 hover:bg-white/30 px-3 py-1.5 rounded-xl font-bold transition">
            Close
          </button>
        </div>

        <form id="itemEntryForm" class="p-4 sm:p-6 space-y-4">
          <input type="hidden" id="editItemId">

          <!-- Type Selector: Movie / Anime / Web Series -->
          <div>
            <label class="block text-[11px] font-bold text-slate-700 uppercase tracking-wider mb-1.5">
              Category Type <span class="text-rose-500">*</span>
            </label>
            <div class="grid grid-cols-3 gap-2">
              <label class="border border-brand-200 rounded-xl p-2.5 flex items-center justify-center gap-1.5 cursor-pointer has-[:checked]:bg-brand-50 has-[:checked]:border-brand-600 has-[:checked]:text-brand-900 text-xs font-bold text-slate-600">
                <input type="radio" name="itemCategoryType" value="movie" class="hidden" checked onchange="handleCategoryTypeChange('movie')">
                <i class="fa-solid fa-film"></i> Movie
              </label>
              <label class="border border-brand-200 rounded-xl p-2.5 flex items-center justify-center gap-1.5 cursor-pointer has-[:checked]:bg-brand-50 has-[:checked]:border-brand-600 has-[:checked]:text-brand-900 text-xs font-bold text-slate-600">
                <input type="radio" name="itemCategoryType" value="anime" class="hidden" onchange="handleCategoryTypeChange('anime')">
                <i class="fa-solid fa-dragon"></i> Anime
              </label>
              <label class="border border-brand-200 rounded-xl p-2.5 flex items-center justify-center gap-1.5 cursor-pointer has-[:checked]:bg-brand-50 has-[:checked]:border-brand-600 has-[:checked]:text-brand-900 text-xs font-bold text-slate-600">
                <input type="radio" name="itemCategoryType" value="series" class="hidden" onchange="handleCategoryTypeChange('series')">
                <i class="fa-solid fa-tv"></i> Series
              </label>
            </div>
          </div>

          <!-- Name / Title -->
          <div>
            <label id="inputTitleLabel" class="block text-[11px] font-bold text-slate-700 uppercase tracking-wider mb-1">
              Name / Title <span class="text-rose-500">*</span>
            </label>
            <input 
              type="text" 
              id="inputItemTitle" 
              required
              placeholder="e.g. Inception, Attack on Titan, Stranger Things" 
              class="w-full px-3.5 py-2.5 rounded-xl border border-brand-200 focus:border-brand-500 focus:ring-2 focus:ring-brand-200 text-xs outline-none bg-white text-slate-800"
            >
          </div>

          <!-- One-line Summary / Hook (Required Feature) -->
          <div>
            <label class="block text-[11px] font-bold text-slate-700 uppercase tracking-wider mb-1">
              One-Line Summary / Tagline <span class="text-rose-500">*</span>
            </label>
            <input 
              type="text" 
              id="inputItemOneline" 
              required
              placeholder="e.g. A thief who steals corporate secrets through dream-sharing technology." 
              class="w-full px-3.5 py-2.5 rounded-xl border border-brand-200 focus:border-brand-500 text-xs outline-none bg-white text-slate-800"
            >
          </div>

          <!-- Poster Image Source (URL or File Upload) -->
          <div class="p-3.5 rounded-2xl bg-brand-50/50 border border-brand-100 space-y-2.5">
            <div class="flex items-center justify-between">
              <label class="text-[11px] font-bold text-slate-800 uppercase tracking-wider">
                Poster Image <span class="text-rose-500">*</span>
              </label>
              <div class="flex bg-white rounded-lg p-0.5 border border-brand-200 text-[11px]">
                <button type="button" id="tabSourceUrlBtn" class="px-2.5 py-1 rounded-md bg-brand-600 text-white font-bold transition">Image URL</button>
                <button type="button" id="tabSourceFileBtn" class="px-2.5 py-1 rounded-md text-slate-600 hover:text-slate-900 font-semibold transition">Upload File</button>
              </div>
            </div>

            <!-- URL input box -->
            <div id="posterUrlBox">
              <input 
                type="url" 
                id="inputPosterUrl" 
                placeholder="https://example.com/poster.jpg" 
                class="w-full px-3.5 py-2.5 rounded-xl border border-brand-200 focus:border-brand-500 text-xs outline-none bg-white text-slate-800"
              >
            </div>

            <!-- Device upload box (Saved in offline storage) -->
            <div id="posterFileBox" class="hidden">
              <label for="inputPosterFile" class="border-2 border-dashed border-brand-300 bg-white hover:border-brand-500 rounded-xl p-4 flex flex-col items-center justify-center cursor-pointer text-center">
                <i class="fa-solid fa-camera text-brand-600 text-2xl mb-1"></i>
                <span class="text-xs font-bold text-slate-800">Tap to select photo from gallery or camera</span>
                <span class="text-[10px] text-slate-400">Stores offline as image data</span>
                <input type="file" id="inputPosterFile" accept="image/*" class="hidden">
              </label>
            </div>

            <!-- Live Preview Indicator -->
            <div id="formPreviewWrapper" class="hidden items-center gap-3 p-2 bg-white rounded-xl border border-brand-200">
              <img id="formPreviewImg" src="" alt="Poster Preview" class="w-10 h-14 object-cover rounded-md border border-brand-100">
              <div class="flex-1 min-w-0">
                <span class="text-xs font-bold text-slate-800 block truncate">Poster Loaded</span>
                <span id="formPreviewCaption" class="text-[10px] text-brand-700">Ready for gallery</span>
              </div>
              <button type="button" id="removePreviewBtn" class="text-slate-400 hover:text-rose-500 p-2 text-xs">
                <i class="fa-solid fa-trash-can"></i>
              </button>
            </div>
          </div>

          <!-- Movie URL by User (Required Feature) -->
          <div>
            <div class="flex items-center justify-between mb-1">
              <label class="text-[11px] font-bold text-slate-700 uppercase tracking-wider">
                Movie / Stream URL <span class="text-brand-600 font-semibold">(Added by User)</span>
              </label>
              <span class="text-[10px] text-slate-400">Direct link to watch or info</span>
            </div>
            <div class="relative">
              <i class="fa-solid fa-link absolute left-3 top-1/2 -translate-y-1/2 text-slate-400 text-xs"></i>
              <input 
                type="url" 
                id="inputItemWatchUrl" 
                placeholder="https://... your streaming or IMDb link" 
                class="w-full pl-8 pr-3.5 py-2.5 rounded-xl border border-brand-200 focus:border-brand-500 text-xs outline-none bg-white text-slate-800"
              >
            </div>
          </div>

          <!-- Movie Time / Runtime & How Many Episodes -->
          <div class="grid grid-cols-2 gap-3 p-3 rounded-2xl bg-brand-50/40 border border-brand-100">
            <div>
              <label class="block text-[11px] font-bold text-slate-700 uppercase tracking-wider mb-1">
                Movie Time / Runtime <span class="text-rose-500">*</span>
              </label>
              <input 
                type="text" 
                id="inputItemMovieTime" 
                required
                placeholder="e.g. 2h 28m or 148 mins" 
                class="w-full px-3 py-2 rounded-xl border border-brand-200 focus:border-brand-500 text-xs outline-none bg-white text-slate-800"
              >
            </div>

            <div id="episodesInputWrap">
              <label id="inputEpisodesLabel" class="block text-[11px] font-bold text-slate-700 uppercase tracking-wider mb-1">
                How Many Episodes?
              </label>
              <input 
                type="text" 
                id="inputItemEpisodes" 
                placeholder="e.g. 24 Episodes, S02 (8 eps)" 
                class="w-full px-3 py-2 rounded-xl border border-brand-200 focus:border-brand-500 text-xs outline-none bg-white text-slate-800"
              >
            </div>
          </div>

          <!-- Watchlist Specific Field: Status -->
          <div id="watchlistStatusWrap" class="grid grid-cols-2 gap-3">
            <div>
              <label class="block text-[11px] font-bold text-slate-700 uppercase tracking-wider mb-1">Watch Status</label>
              <select id="inputItemWatchStatus" class="w-full px-3 py-2 rounded-xl border border-brand-200 focus:border-brand-500 text-xs outline-none bg-white text-slate-800">
                <option value="Watching">Watching</option>
                <option value="Plan to Watch" selected>Plan to Watch</option>
                <option value="Completed">Completed</option>
              </select>
            </div>

            <div>
              <label class="block text-[11px] font-bold text-slate-700 uppercase tracking-wider mb-1">Genre</label>
              <select id="inputItemGenre" class="w-full px-3 py-2 rounded-xl border border-brand-200 focus:border-brand-500 text-xs outline-none bg-white text-slate-800">
                <option value="Action">Action</option>
                <option value="Sci-Fi">Sci-Fi</option>
                <option value="Anime / Shonen">Anime / Shonen</option>
                <option value="Drama">Drama</option>
                <option value="Dark Fantasy">Dark Fantasy</option>
                <option value="Thriller">Thriller</option>
                <option value="Animation">Animation</option>
                <option value="Adventure">Adventure</option>
                <option value="Comedy">Comedy</option>
                <option value="Crime">Crime</option>
                <option value="Horror">Horror</option>
              </select>
            </div>
          </div>

          <!-- Rating & Creator/Studio -->
          <div class="grid grid-cols-2 gap-3">
            <div>
              <label class="block text-[11px] font-bold text-slate-700 uppercase tracking-wider mb-1">Rating (1.0 - 10.0)</label>
              <input 
                type="number" 
                id="inputItemRating" 
                step="0.1" 
                min="1" 
                max="10" 
                placeholder="e.g. 8.8" 
                class="w-full px-3 py-2 rounded-xl border border-brand-200 focus:border-brand-500 text-xs outline-none bg-white"
              >
            </div>

            <div>
              <label id="inputCreatorLabel" class="block text-[11px] font-bold text-slate-700 uppercase tracking-wider mb-1">Director / Studio</label>
              <input 
                type="text" 
                id="inputItemDirector" 
                placeholder="e.g. Christopher Nolan, MAPPA" 
                class="w-full px-3 py-2 rounded-xl border border-brand-200 focus:border-brand-500 text-xs outline-none bg-white"
              >
            </div>
          </div>

          <!-- Detailed Notes (Optional) -->
          <div>
            <label class="block text-[11px] font-bold text-slate-700 uppercase tracking-wider mb-1">Extra Notes / Review</label>
            <textarea 
              id="inputItemNotes" 
              rows="2" 
              placeholder="Personal thoughts, where to watch..." 
              class="w-full px-3.5 py-2 rounded-xl border border-brand-200 focus:border-brand-500 text-xs outline-none bg-white resize-none"
            ></textarea>
          </div>

          <div class="pt-2 flex items-center justify-end gap-2.5">
            <button type="button" onclick="returnToPreviousPage()" class="px-4 py-2.5 rounded-xl border border-slate-200 text-xs font-semibold text-slate-600">
              Cancel
            </button>
            <button type="submit" class="px-5 py-2.5 rounded-xl bg-brand-600 hover:bg-brand-700 text-white text-xs font-bold shadow-xs active:scale-95 transition flex items-center gap-1.5">
              <i class="fa-solid fa-floppy-disk"></i>
              <span id="submitBtnText">Save Poster</span>
            </button>
          </div>
        </form>

      </div>
    </section>

    <!-- ================= PAGE 5: CUSTOMIZATION PAGE ================= -->
    <section id="customizationPageView" class="hidden space-y-4 max-w-3xl mx-auto">
      
      <!-- Customization Header -->
      <div class="bg-white rounded-3xl border border-brand-100 p-4 sm:p-5 shadow-xs flex items-center justify-between">
        <div class="flex items-center gap-3">
          <div class="w-10 h-10 rounded-xl bg-brand-600 text-white flex items-center justify-center text-lg shadow-sm">
            <i class="fa-solid fa-sliders"></i>
          </div>
          <div>
            <h2 class="font-display font-black text-base sm:text-lg text-slate-900">Vault Customization</h2>
            <p class="text-[11px] text-slate-500">Tune card framing, URL action buttons, corners &amp; backup offline data</p>
          </div>
        </div>
        <button onclick="restoreDefaultSettings()" class="text-xs px-3 py-1.5 rounded-xl border border-brand-200 text-brand-800 bg-brand-50 hover:bg-brand-100 font-bold transition">
          Reset
        </button>
      </div>

      <!-- Aspect Ratio Option -->
      <div class="bg-white rounded-3xl border border-brand-100 p-4 sm:p-5 shadow-xs space-y-3">
        <h3 class="font-display font-bold text-xs uppercase tracking-wider text-slate-800 flex items-center gap-2">
          <i class="fa-solid fa-crop-simple text-brand-600"></i> Poster Aspect Ratio
        </h3>
        <div class="grid grid-cols-2 sm:grid-cols-4 gap-2">
          <button type="button" onclick="setAspectRatio('aspect-poster-classic')" id="ratioClassicBtn" class="p-2.5 rounded-xl border-2 border-brand-500 bg-brand-50 text-brand-900 text-xs font-bold text-center">
            Classic 2:3
          </button>
          <button type="button" onclick="setAspectRatio('aspect-poster-wide')" id="ratioWideBtn" class="p-2.5 rounded-xl border border-slate-200 text-slate-700 text-xs font-medium text-center">
            Standard 3:4
          </button>
          <button type="button" onclick="setAspectRatio('aspect-poster-square')" id="ratioSquareBtn" class="p-2.5 rounded-xl border border-slate-200 text-slate-700 text-xs font-medium text-center">
            Square 1:1
          </button>
          <button type="button" onclick="setAspectRatio('aspect-poster-cinema')" id="ratioCinemaBtn" class="p-2.5 rounded-xl border border-slate-200 text-slate-700 text-xs font-medium text-center">
            Cinema 16:9
          </button>
        </div>
      </div>

      <!-- Controls & Toggles -->
      <div class="bg-white rounded-3xl border border-brand-100 p-4 sm:p-5 shadow-xs space-y-3">
        <h3 class="font-display font-bold text-xs uppercase tracking-wider text-slate-800 flex items-center gap-2">
          <i class="fa-solid fa-toggle-on text-brand-600"></i> Display Controls
        </h3>

        <!-- Toggle: Movie URL Button on Card -->
        <div class="flex items-center justify-between p-3 rounded-2xl bg-brand-50/50 border border-brand-100">
          <div>
            <div class="text-xs font-bold text-slate-800">Show Direct "Watch URL" Button</div>
            <p class="text-[10px] text-slate-500">Quick action button on card to open the user-provided movie link</p>
          </div>
          <label class="relative inline-flex items-center cursor-pointer">
            <input type="checkbox" id="customShowMovieUrlBtn" class="sr-only peer" checked onchange="updateSettingsToggle('showMovieUrlBtn', this.checked)">
            <div class="w-10 h-5 bg-slate-200 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:rounded-full after:h-4 after:w-4 after:transition-all peer-checked:bg-brand-600"></div>
          </label>
        </div>

        <!-- Toggle: Badges & One-line Summary -->
        <div class="flex items-center justify-between p-3 rounded-2xl bg-brand-50/50 border border-brand-100">
          <div>
            <div class="text-xs font-bold text-slate-800">Show Floating Badges &amp; Movie Time</div>
            <p class="text-[10px] text-slate-500">Displays runtime and genre tags directly on poster</p>
          </div>
          <label class="relative inline-flex items-center cursor-pointer">
            <input type="checkbox" id="customShowBadges" class="sr-only peer" checked onchange="updateSettingsToggle('showBadges', this.checked)">
            <div class="w-10 h-5 bg-slate-200 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:rounded-full after:h-4 after:w-4 after:transition-all peer-checked:bg-brand-600"></div>
          </label>
        </div>

        <!-- Uploaded Date Exclusion Guarantee -->
        <div class="flex items-center justify-between p-3 rounded-2xl bg-slate-50 border border-slate-200">
          <div>
            <div class="text-xs font-bold text-slate-700">Uploaded Dates</div>
            <p class="text-[10px] text-slate-500">Strictly excluded everywhere across all pages &amp; cards</p>
          </div>
          <span class="text-[10px] font-bold uppercase px-2.5 py-0.5 rounded-md bg-brand-100 text-brand-800">Never Displayed</span>
        </div>
      </div>

      <!-- Card Rounded Corners -->
      <div class="bg-white rounded-3xl border border-brand-100 p-4 sm:p-5 shadow-xs space-y-2.5">
        <h3 class="font-display font-bold text-xs uppercase tracking-wider text-slate-800 flex items-center gap-2">
          <i class="fa-solid fa-shapes text-brand-600"></i> Card Border Radius
        </h3>
        <div class="grid grid-cols-3 gap-2">
          <button type="button" onclick="setCardRadius('rounded-xl')" id="radiusXlBtn" class="py-2 px-2 rounded-xl border border-slate-200 text-slate-700 text-xs">Small (12px)</button>
          <button type="button" onclick="setCardRadius('rounded-2xl')" id="radius2XlBtn" class="py-2 px-2 rounded-2xl border-2 border-brand-500 bg-brand-50 text-brand-900 text-xs font-bold">Medium (16px)</button>
          <button type="button" onclick="setCardRadius('rounded-3xl')" id="radius3XlBtn" class="py-2 px-2 rounded-3xl border border-slate-200 text-slate-700 text-xs">Large (24px)</button>
        </div>
      </div>

      <!-- App Background Photo (Mobile Wallpaper & Dynamic Area Backgrounds) -->
      <div class="bg-white rounded-3xl border border-brand-100 p-4 sm:p-5 shadow-xs space-y-3.5">
        <div class="flex items-center justify-between">
          <h3 class="font-display font-bold text-xs uppercase tracking-wider text-slate-800 flex items-center gap-2">
            <i class="fa-solid fa-image text-brand-600"></i> Dynamic Mobile Backgrounds (Per Area)
          </h3>
          <button onclick="clearCustomBgPhoto()" class="text-[11px] font-bold text-rose-600 hover:text-rose-700">
            Reset to Themed Defaults
          </button>
        </div>

        <p class="text-[11px] text-slate-500 leading-snug">
          CineVault automatically switches backgrounds as you browse: <strong>White &amp; Green Daisies</strong> for Movies, <strong>Anime Characters Montage</strong> for Anime, and <strong>Web Series Logos Wall</strong> for Series. You can also customize your preferred mode below.
        </p>

        <!-- Area Theme Mode Selector -->
        <div class="grid grid-cols-2 sm:grid-cols-4 gap-2">
          <button type="button" onclick="setBgThemeMode('dynamic')" id="bgModeDynamicBtn" class="p-2.5 rounded-xl border-2 border-brand-600 bg-brand-50 text-brand-900 text-left transition flex items-center gap-2">
            <div class="w-7 h-7 rounded-lg bg-brand-600 flex items-center justify-center text-xs text-white">
              <i class="fa-solid fa-shuffle"></i>
            </div>
            <div>
              <span class="text-xs font-bold block">Smart Area</span>
              <span class="text-[9px] text-brand-800">Auto-Switches</span>
            </div>
          </button>

          <button type="button" onclick="setPresetBackground('floral')" id="presetFloralBtn" class="p-2.5 rounded-xl border border-brand-200 bg-white hover:bg-brand-50 text-left transition flex items-center gap-2">
            <div class="w-7 h-7 rounded-lg bg-emerald-200 border border-emerald-300 flex items-center justify-center text-xs text-emerald-800">
              <i class="fa-solid fa-spa"></i>
            </div>
            <div>
              <span class="text-xs font-bold text-slate-800 block">Daisies Flower</span>
              <span class="text-[9px] text-slate-400">Green &amp; White</span>
            </div>
          </button>

          <button type="button" onclick="setPresetBackground('anime')" id="presetAnimeBtn" class="p-2.5 rounded-xl border border-brand-200 bg-white hover:bg-brand-50 text-left transition flex items-center gap-2">
            <div class="w-7 h-7 rounded-lg bg-teal-100 border border-teal-300 flex items-center justify-center text-xs text-teal-800">
              <i class="fa-solid fa-user-ninja"></i>
            </div>
            <div>
              <span class="text-xs font-bold text-slate-800 block">Anime Cast</span>
              <span class="text-[9px] text-slate-400">Characters Art</span>
            </div>
          </button>

          <button type="button" onclick="setPresetBackground('series')" id="presetSeriesBtn" class="p-2.5 rounded-xl border border-brand-200 bg-white hover:bg-brand-50 text-left transition flex items-center gap-2">
            <div class="w-7 h-7 rounded-lg bg-slate-800 text-white flex items-center justify-center text-xs">
              <i class="fa-solid fa-clapperboard"></i>
            </div>
            <div>
              <span class="text-xs font-bold text-slate-800 block">Series Logos</span>
              <span class="text-[9px] text-slate-400">OTT Wall</span>
            </div>
          </button>
        </div>

        <!-- Custom Upload or URL -->
        <div class="p-3 rounded-2xl bg-brand-50/50 border border-brand-100 space-y-2">
          <div class="flex items-center justify-between">
            <span class="text-[11px] font-bold text-slate-700 uppercase">Upload Custom Wallpaper (Saved Offline)</span>
            <label for="bgPhotoFileInput" class="px-3 py-1 rounded-lg bg-brand-600 hover:bg-brand-700 text-white text-[11px] font-bold cursor-pointer transition">
              <i class="fa-solid fa-camera mr-1"></i> Choose File
            </label>
            <input type="file" id="bgPhotoFileInput" accept="image/*" class="hidden">
          </div>
          
          <div class="flex gap-2">
            <input type="url" id="bgPhotoUrlInput" placeholder="Or paste background photo link (https://...)" class="flex-1 px-3 py-1.5 rounded-xl border border-brand-200 text-xs bg-white outline-none">
            <button onclick="applyBgPhotoFromInput()" class="px-3 py-1.5 rounded-xl bg-brand-700 hover:bg-brand-800 text-white text-xs font-bold transition">
              Apply
            </button>
          </div>
        </div>

        <!-- Opacity & Tint Strength Controls -->
        <div class="grid grid-cols-2 gap-3 pt-1">
          <div>
            <div class="flex justify-between text-[11px] font-semibold text-slate-600 mb-1">
              <span>Photo Visibility</span>
              <span id="bgOpacityValText">35%</span>
            </div>
            <input type="range" id="bgOpacitySlider" min="5" max="90" value="35" class="w-full accent-brand-600" oninput="updateBgOpacity(this.value)">
          </div>

          <div>
            <div class="flex justify-between text-[11px] font-semibold text-slate-600 mb-1">
              <span>Background Blur</span>
              <span id="bgBlurValText">2px</span>
            </div>
            <input type="range" id="bgBlurSlider" min="0" max="10" value="2" class="w-full accent-brand-600" oninput="updateBgBlur(this.value)">
          </div>
        </div>
      </div>

      <!-- Offline Storage & JSON Backup / Restore -->
      <div class="bg-white rounded-3xl border border-brand-100 p-4 sm:p-5 shadow-xs space-y-3">
        <h3 class="font-display font-bold text-xs uppercase tracking-wider text-slate-800 flex items-center gap-2">
          <i class="fa-solid fa-hard-drive text-brand-600"></i> Offline Storage &amp; Backup
        </h3>
        <p class="text-[11px] text-slate-500">All posters, custom URLs, runtime, and episode counts are saved locally on your phone or device so they work offline.</p>
        <div class="flex gap-2">
          <button onclick="exportCollection()" class="flex-1 py-2.5 rounded-xl bg-brand-600 hover:bg-brand-700 text-white text-xs font-bold shadow-xs transition flex items-center justify-center gap-1.5">
            <i class="fa-solid fa-file-arrow-down"></i>
            <span>Export Backup</span>
          </button>
          <button onclick="triggerImportDialog()" class="flex-1 py-2.5 rounded-xl border border-brand-200 text-brand-800 bg-brand-50 hover:bg-brand-100 text-xs font-bold transition flex items-center justify-center gap-1.5">
            <i class="fa-solid fa-file-arrow-up"></i>
            <span>Import Backup</span>
          </button>
          <input type="file" id="importJsonFileInput" accept=".json" class="hidden">
        </div>
      </div>

    </section>

  </main>

  <!-- ================= LIGHTBOX / DETAILS MODAL ================= -->
  <div id="lightboxOverlay" class="fixed inset-0 z-50 flex items-end sm:items-center justify-center p-0 sm:p-4 bg-slate-950/80 backdrop-blur-sm hidden opacity-0 transition-opacity duration-200">
    <div class="bg-white rounded-t-3xl sm:rounded-3xl shadow-2xl overflow-hidden w-full max-w-xl max-h-[92vh] flex flex-col transform translate-y-8 sm:translate-y-0 sm:scale-95 transition-transform duration-200 border border-brand-100">
      
      <!-- Top Image View -->
      <div class="relative bg-slate-950 flex items-center justify-center min-h-[250px] max-h-[400px]">
        <img id="lightboxPosterImg" src="" alt="Poster" class="w-full h-full object-contain max-h-[400px]">
        <button onclick="closeLightboxModal()" class="absolute top-3 right-3 w-8 h-8 rounded-full bg-slate-900/70 text-white flex items-center justify-center text-xs">
          <i class="fa-solid fa-xmark"></i>
        </button>
      </div>

      <!-- Info Details (Date-Free) -->
      <div class="p-4 sm:p-6 overflow-y-auto space-y-3">
        <div class="flex items-center justify-between">
          <div class="flex items-center gap-1.5">
            <span id="lightboxCategoryBadge" class="px-2.5 py-0.5 rounded-full bg-brand-600 text-white text-[10px] font-bold uppercase tracking-wider">
              Movie
            </span>
            <span id="lightboxGenreBadge" class="px-2.5 py-0.5 rounded-full bg-brand-100 text-brand-800 text-[10px] font-bold uppercase tracking-wider">
              Sci-Fi
            </span>
            <span id="lightboxStatusBadge" class="hidden px-2.5 py-0.5 rounded-full bg-emerald-100 text-emerald-800 text-[10px] font-bold">
              Watching
            </span>
          </div>

          <span id="lightboxRating" class="text-amber-500 font-bold text-xs flex items-center gap-1">
            <i class="fa-solid fa-star text-[10px]"></i> 8.5
          </span>
        </div>

        <h3 id="lightboxTitle" class="font-display text-lg sm:text-xl font-black text-slate-900 leading-snug">
          Item Title
        </h3>

        <!-- One-line Summary Highlight -->
        <div class="p-3 rounded-2xl bg-brand-50 border border-brand-200 text-brand-950">
          <span class="text-[9px] uppercase font-bold text-brand-700 tracking-wider block mb-0.5">One-Line Summary</span>
          <p id="lightboxOneline" class="text-xs font-semibold leading-relaxed">
            Summary line here
          </p>
        </div>

        <!-- Movie Time & Episodes Row -->
        <div class="grid grid-cols-2 gap-2 text-xs">
          <div class="p-2.5 rounded-xl bg-slate-50 border border-slate-200">
            <span class="text-[9px] uppercase font-bold text-slate-400 block">Movie Time / Runtime</span>
            <span id="lightboxMovieTime" class="font-bold text-slate-800">2h 49m</span>
          </div>
          <div id="lightboxEpisodesBox" class="p-2.5 rounded-xl bg-slate-50 border border-slate-200">
            <span class="text-[9px] uppercase font-bold text-slate-400 block">Episodes Count</span>
            <span id="lightboxEpisodes" class="font-bold text-slate-800">24 eps</span>
          </div>
        </div>

        <!-- Director / Studio info -->
        <div class="flex items-center justify-between text-xs text-slate-500 border-b border-brand-50 pb-2">
          <span id="lightboxDirector">Director Name</span>
        </div>

        <!-- User Added URL -->
        <div id="lightboxUrlSection">
          <span class="text-[10px] uppercase font-bold text-slate-400 block mb-1">Movie / Stream URL (Added by User)</span>
          <a id="lightboxLinkAnchor" href="#" target="_blank" rel="noopener noreferrer" class="flex items-center justify-between p-2.5 rounded-xl bg-brand-50 border border-brand-200 text-brand-900 text-xs font-semibold hover:bg-brand-100 transition">
            <span id="lightboxLinkText" class="truncate pr-2">https://...</span>
            <span class="text-[11px] font-bold text-brand-700 shrink-0">Open Link <i class="fa-solid fa-arrow-up-right-from-square text-[9px] ml-0.5"></i></span>
          </a>
        </div>

        <p id="lightboxNotes" class="text-xs text-slate-600 leading-relaxed max-h-24 overflow-y-auto bg-slate-50 p-2.5 rounded-xl">
          Notes
        </p>

        <!-- Actions -->
        <div class="pt-2 flex items-center gap-2">
          <button id="lightboxEditBtn" class="flex-1 py-2.5 rounded-xl border border-slate-200 text-xs font-bold text-slate-700">
            <i class="fa-solid fa-pen mr-1"></i> Edit
          </button>
          <button id="lightboxDeleteBtn" class="py-2.5 px-4 rounded-xl bg-rose-50 text-rose-600 text-xs font-bold border border-rose-200">
            <i class="fa-solid fa-trash"></i>
          </button>
        </div>
      </div>

    </div>
  </div>

  <!-- Mobile Fixed Bottom Navigation Bar -->
  <nav class="fixed bottom-0 left-0 right-0 z-40 bg-white/95 backdrop-blur-md border-t border-brand-100 py-1.5 px-3 md:hidden flex items-center justify-around shadow-lg">
    <button onclick="switchPage('gallery')" id="mobileNavGallery" class="bottom-nav-item active flex flex-col items-center gap-0.5 text-slate-500">
      <div class="nav-icon-wrap w-9 h-7 rounded-xl flex items-center justify-center transition">
        <i class="fa-solid fa-film text-sm"></i>
      </div>
      <span class="text-[10px] font-bold">Movies</span>
    </button>

    <button onclick="switchPage('anime')" id="mobileNavAnime" class="bottom-nav-item flex flex-col items-center gap-0.5 text-slate-500">
      <div class="nav-icon-wrap w-9 h-7 rounded-xl flex items-center justify-center transition">
        <i class="fa-solid fa-dragon text-sm"></i>
      </div>
      <span class="text-[10px] font-bold">Anime</span>
    </button>

    <button onclick="switchPage('series')" id="mobileNavSeries" class="bottom-nav-item flex flex-col items-center gap-0.5 text-slate-500">
      <div class="nav-icon-wrap w-9 h-7 rounded-xl flex items-center justify-center transition">
        <i class="fa-solid fa-tv text-sm"></i>
      </div>
      <span class="text-[10px] font-bold">Series</span>
    </button>

    <button onclick="openAddModalFor('movie')" id="mobileNavAdd" class="bottom-nav-item flex flex-col items-center gap-0.5 text-slate-500">
      <div class="nav-icon-wrap w-9 h-7 rounded-xl flex items-center justify-center transition">
        <i class="fa-solid fa-circle-plus text-sm"></i>
      </div>
      <span class="text-[10px] font-bold">+ Add</span>
    </button>

    <button onclick="switchPage('custom')" id="mobileNavCustom" class="bottom-nav-item flex flex-col items-center gap-0.5 text-slate-500">
      <div class="nav-icon-wrap w-9 h-7 rounded-xl flex items-center justify-center transition">
        <i class="fa-solid fa-sliders text-sm"></i>
      </div>
      <span class="text-[10px] font-bold">Settings</span>
    </button>
  </nav>

  <!-- Notification Toast -->
  <div id="toastNotification" class="fixed top-4 left-1/2 -translate-x-1/2 z-50 flex items-center gap-2 bg-slate-900 text-white px-4 py-2.5 rounded-2xl shadow-xl border border-slate-800 transition-all duration-300 transform -translate-y-16 opacity-0 pointer-events-none text-xs font-medium">
    <i class="fa-solid fa-circle-check text-brand-400"></i>
    <span id="toastNotificationText">Notification</span>
  </div>

  <script>
    // Initial dataset across Movies, Anime & Web Series (All with oneline, movie time, episode count, strictly no uploaded dates)
    const INITIAL_COLLECTION = [
      // Movies
      {
        id: 'mv-1',
        type: 'movie',
        title: 'Interstellar',
        genre: 'Sci-Fi',
        director: 'Christopher Nolan',
        rating: 8.7,
        movieTime: '2h 49m',
        episodes: 'Film',
        oneline: 'A team of explorers travel through a wormhole in space in an attempt to ensure humanity\'s survival.',
        posterUrl: 'https://images.unsplash.com/photo-1506703719100-a0f3a48c0f86?w=600&auto=format&fit=crop&q=80',
        movieUrl: 'https://www.imdb.com/title/tt0816692/',
        notes: 'Groundbreaking score by Hans Zimmer and emotional father-daughter bond across time.'
      },
      {
        id: 'mv-2',
        type: 'movie',
        title: 'Dune: Part Two',
        genre: 'Adventure',
        director: 'Denis Villeneuve',
        rating: 8.6,
        movieTime: '2h 46m',
        episodes: 'Film',
        oneline: 'Paul Atreides unites with the Fremen people while seeking revenge against the conspirators.',
        posterUrl: 'https://images.unsplash.com/photo-1534447677768-be436bb09401?w=600&auto=format&fit=crop&q=80',
        movieUrl: 'https://www.imdb.com/title/tt15239678/',
        notes: 'Visually astonishing sci-fi epic with intense sand-worm battle sequences.'
      },
      {
        id: 'mv-3',
        type: 'movie',
        title: 'Inception',
        genre: 'Action',
        director: 'Christopher Nolan',
        rating: 8.8,
        movieTime: '2h 28m',
        episodes: 'Film',
        oneline: 'A thief who steals corporate secrets through dream-sharing technology is given the inverse task of planting an idea.',
        posterUrl: 'https://images.unsplash.com/photo-1518709268805-4e9042af9f23?w=600&auto=format&fit=crop&q=80',
        movieUrl: 'https://www.imdb.com/title/tt1375666/',
        notes: 'Mind-bending dream-within-a-dream architecture.'
      },
      // Anime Watchlist
      {
        id: 'an-1',
        type: 'anime',
        title: 'Attack on Titan',
        genre: 'Dark Fantasy',
        director: 'Wit Studio / MAPPA',
        rating: 9.1,
        watchStatus: 'Completed',
        movieTime: '24m / ep',
        episodes: '89 Episodes',
        oneline: 'Humanity lives surrounded by enormous concentric walls to protect themselves from gigantic humanoids called Titans.',
        posterUrl: 'https://images.unsplash.com/photo-1579783900882-c0d3dad7b119?w=600&auto=format&fit=crop&q=80',
        movieUrl: 'https://www.crunchyroll.com/series/GR751KNZY/attack-on-titan',
        notes: 'A masterclass in foreshadowing, political intrigue, and unforgettable action.'
      },
      {
        id: 'an-2',
        type: 'anime',
        title: 'Spirited Away',
        genre: 'Animation',
        director: 'Hayao Miyazaki',
        rating: 8.6,
        watchStatus: 'Completed',
        movieTime: '2h 05m',
        episodes: 'Anime Film',
        oneline: 'A sullen 10-year-old girl wanders into a magical world ruled by gods, witches, and spirits.',
        posterUrl: 'https://images.unsplash.com/photo-1578632767115-351597cf2477?w=600&auto=format&fit=crop&q=80',
        movieUrl: 'https://www.imdb.com/title/tt0245429/',
        notes: 'Academy Award winning Studio Ghibli masterpiece.'
      },
      {
        id: 'an-3',
        type: 'anime',
        title: 'Jujutsu Kaisen',
        genre: 'Anime / Shonen',
        director: 'MAPPA',
        rating: 8.6,
        watchStatus: 'Watching',
        movieTime: '23m / ep',
        episodes: '47 Episodes',
        oneline: 'A boy swallows a cursed finger to save his friends and is drawn into an esoteric world of sorcery.',
        posterUrl: 'https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?w=600&auto=format&fit=crop&q=80',
        movieUrl: 'https://www.crunchyroll.com/series/GRDV0019R/jujutsu-kaisen',
        notes: 'Incredible choreography and Shibuya Incident arc.'
      },
      // Web Series Watchlist
      {
        id: 'ws-1',
        type: 'series',
        title: 'Stranger Things',
        genre: 'Sci-Fi',
        director: 'The Duffer Brothers',
        rating: 8.7,
        watchStatus: 'Watching',
        movieTime: '55m / ep',
        episodes: '34 Episodes (4 Seasons)',
        oneline: 'When a young boy vanishes, a small town uncovers a supernatural mystery involving secret experiments and the Upside Down.',
        posterUrl: 'https://images.unsplash.com/photo-1509198397868-475647b2a1e5?w=600&auto=format&fit=crop&q=80',
        movieUrl: 'https://www.netflix.com/title/80057281',
        notes: '80s nostalgia with incredible synth score and monster horror.'
      },
      {
        id: 'ws-2',
        type: 'series',
        title: 'Breaking Bad',
        genre: 'Crime',
        director: 'Vince Gilligan',
        rating: 9.5,
        watchStatus: 'Completed',
        movieTime: '47m / ep',
        episodes: '62 Episodes (5 Seasons)',
        oneline: 'A high school chemistry teacher diagnosed with terminal lung cancer turns to manufacturing methamphetamine.',
        posterUrl: 'https://images.unsplash.com/photo-1478760329108-5c3ed9d495a0?w=600&auto=format&fit=crop&q=80',
        movieUrl: 'https://www.netflix.com/title/70143836',
        notes: 'The finest television drama character arc ever written.'
      }
    ];

    let items = [...INITIAL_COLLECTION];
    let currentPage = 'gallery';
    let previousPage = 'gallery';

    let customSettings = {
      aspectRatio: 'aspect-poster-classic',
      showMovieUrlBtn: true,
      showBadges: true,
      cardRadius: 'rounded-2xl',
      bgThemeMode: 'dynamic', // 'dynamic' auto-switches per area (Flower for Movies, Characters for Anime, Logos for Series)
      bgPreset: 'floral',
      bgPhotoData: '',
      bgOpacity: 35,
      bgBlur: 2
    };

    // Area-specific wallpapers (White & green flowers, Anime characters montage, Web series logos collage)
    const AREA_BACKGROUNDS = {
      gallery: 'https://images.unsplash.com/photo-1508615039623-a25605d2b022?w=1200&auto=format&fit=crop&q=80', // White & green daisies / floral
      anime: 'https://images.unsplash.com/photo-1607604276583-eef5d076aa5f?w=1200&auto=format&fit=crop&q=80', // Anime characters & manga art collage
      series: 'https://images.unsplash.com/photo-1489599849927-2ee91cede3ba?w=1200&auto=format&fit=crop&q=80', // Web series & cinematic logos wall
      custom: 'https://images.unsplash.com/photo-1508615039623-a25605d2b022?w=1200&auto=format&fit=crop&q=80',
      add: 'https://images.unsplash.com/photo-1508615039623-a25605d2b022?w=1200&auto=format&fit=crop&q=80'
    };

    const BG_PRESETS = {
      floral: 'https://images.unsplash.com/photo-1508615039623-a25605d2b022?w=1200&auto=format&fit=crop&q=80', // White & Green Daisies
      anime: 'https://images.unsplash.com/photo-1607604276583-eef5d076aa5f?w=1200&auto=format&fit=crop&q=80', // Anime characters montage
      series: 'https://images.unsplash.com/photo-1489599849927-2ee91cede3ba?w=1200&auto=format&fit=crop&q=80', // Series logos & cinematic wall
      none: ''
    };

    let activeFilterGenre = 'ALL';
    let activeSearchQuery = '';
    let activeSortKey = 'title_asc';
    let activeAnimeStatus = 'ALL';
    let activeSeriesStatus = 'ALL';
    let currentModalItemId = null;

    function loadSavedData() {
      try {
        const storedItems = localStorage.getItem('cinevault_offline_items');
        if (storedItems) {
          items = JSON.parse(storedItems);
        }
        const storedSettings = localStorage.getItem('cinevault_offline_settings');
        if (storedSettings) {
          customSettings = { ...customSettings, ...JSON.parse(storedSettings) };
        }
      } catch (err) {
        console.warn('Using local memory storage');
      }
      applyBackgroundStyles();
    }

    function persistData() {
      try {
        localStorage.setItem('cinevault_offline_items', JSON.stringify(items));
        localStorage.setItem('cinevault_offline_settings', JSON.stringify(customSettings));
      } catch (e) {
        console.warn('Storage quota limit reached');
      }
    }

    function applyBackgroundStyles() {
      const bgLayer = document.getElementById('appBackgroundLayer');
      const tintLayer = document.getElementById('appBackdropTint');
      const patternOverlay = document.getElementById('areaPatternOverlay');
      if (!bgLayer || !tintLayer) return;

      let photoSource = '';

      // Determine photo source based on user mode
      if (customSettings.bgPhotoData) {
        photoSource = customSettings.bgPhotoData;
      } else if (customSettings.bgThemeMode === 'dynamic') {
        photoSource = AREA_BACKGROUNDS[currentPage] || AREA_BACKGROUNDS.gallery;
      } else if (customSettings.bgPreset && BG_PRESETS[customSettings.bgPreset]) {
        photoSource = BG_PRESETS[customSettings.bgPreset];
      }

      if (photoSource) {
        bgLayer.style.backgroundImage = `url("${photoSource}")`;
        bgLayer.style.opacity = (customSettings.bgOpacity / 100).toString();
        bgLayer.style.filter = `blur(${customSettings.bgBlur}px)`;
        
        // Dynamic tint based on page theme
        if (currentPage === 'anime') {
          tintLayer.className = 'fixed inset-0 z-[-1] pointer-events-none bg-gradient-to-b from-teal-50/85 via-white/80 to-emerald-100/80 backdrop-blur-[2px]';
        } else if (currentPage === 'series') {
          tintLayer.className = 'fixed inset-0 z-[-1] pointer-events-none bg-gradient-to-b from-emerald-50/85 via-white/80 to-green-100/80 backdrop-blur-[2px]';
        } else {
          tintLayer.className = 'fixed inset-0 z-[-1] pointer-events-none bg-gradient-to-b from-brand-50/90 via-white/80 to-brand-100/80 backdrop-blur-[2px]';
        }
      } else {
        bgLayer.style.backgroundImage = 'none';
        bgLayer.style.opacity = '0';
        tintLayer.className = 'fixed inset-0 z-[-1] pointer-events-none bg-gradient-to-b from-brand-50/90 via-white/80 to-brand-100/80 backdrop-blur-[2px]';
      }
    }

    function setBgThemeMode(mode) {
      customSettings.bgThemeMode = mode;
      customSettings.bgPhotoData = '';
      persistData();
      applyBackgroundStyles();
      syncCustomizationUi();
      showToast('Smart area background switching active!');
    }

    function setPresetBackground(presetKey) {
      customSettings.bgThemeMode = 'fixed';
      customSettings.bgPreset = presetKey;
      customSettings.bgPhotoData = '';
      persistData();
      applyBackgroundStyles();
      syncCustomizationUi();
      showToast(`Wallpaper preset set to ${presetKey}`);
    }

    function clearCustomBgPhoto() {
      customSettings.bgThemeMode = 'dynamic';
      customSettings.bgPreset = 'floral';
      customSettings.bgPhotoData = '';
      persistData();
      applyBackgroundStyles();
      syncCustomizationUi();
      showToast('Backgrounds reset to default Flower, Anime & Series themes!');
    }

    function updateBgOpacity(val) {
      customSettings.bgOpacity = parseInt(val, 10);
      document.getElementById('bgOpacityValText').textContent = `${val}%`;
      persistData();
      applyBackgroundStyles();
    }

    function updateBgBlur(val) {
      customSettings.bgBlur = parseInt(val, 10);
      document.getElementById('bgBlurValText').textContent = `${val}px`;
      persistData();
      applyBackgroundStyles();
    }

    function applyBgPhotoFromInput() {
      const urlInput = document.getElementById('bgPhotoUrlInput');
      const val = urlInput.value.trim();
      if (!val) {
        showToast('Please paste a valid photo link');
        return;
      }
      customSettings.bgPhotoData = val;
      customSettings.bgThemeMode = 'custom';
      persistData();
      applyBackgroundStyles();
      showToast('Custom wallpaper photo applied!');
    }

    document.addEventListener('DOMContentLoaded', () => {
      const bgFileInput = document.getElementById('bgPhotoFileInput');
      if (bgFileInput) {
        bgFileInput.addEventListener('change', (e) => {
          const file = e.target.files[0];
          if (!file) return;

          const reader = new FileReader();
          reader.onload = (evt) => {
            customSettings.bgPhotoData = evt.target.result;
            customSettings.bgThemeMode = 'custom';
            persistData();
            applyBackgroundStyles();
            showToast('Offline photo saved as wallpaper!');
          };
          reader.readAsDataURL(file);
        });
      }
    });

    function updateOnlineOfflineIndicator() {
      const isOnline = navigator.onLine;
      const statusText = document.getElementById('offlineStatusText');
      if (statusText) {
        statusText.textContent = isOnline ? 'Offline Ready (Online)' : 'Offline Mode Active';
      }
    }

    window.addEventListener('online', updateOnlineOfflineIndicator);
    window.addEventListener('offline', updateOnlineOfflineIndicator);

    function showToast(text) {
      const toast = document.getElementById('toastNotification');
      const toastText = document.getElementById('toastNotificationText');
      toastText.textContent = text;
      toast.classList.remove('-translate-y-16', 'opacity-0');
      toast.classList.add('translate-y-0', 'opacity-100');

      setTimeout(() => {
        toast.classList.remove('translate-y-0', 'opacity-100');
        toast.classList.add('-translate-y-16', 'opacity-0');
      }, 2300);
    }

    function switchPage(pageName) {
      if (pageName !== 'add') {
        previousPage = pageName;
      }
      currentPage = pageName;

      const gView = document.getElementById('galleryPageView');
      const aView = document.getElementById('animePageView');
      const sView = document.getElementById('seriesPageView');
      const addView = document.getElementById('addMoviePageView');
      const cView = document.getElementById('customizationPageView');

      [gView, aView, sView, addView, cView].forEach(v => v.classList.add('hidden'));

      // Bottom mobile nav
      ['mobileNavGallery', 'mobileNavAnime', 'mobileNavSeries', 'mobileNavAdd', 'mobileNavCustom'].forEach(id => {
        const el = document.getElementById(id);
        if (el) el.classList.remove('active');
      });

      // Desktop tab buttons
      ['desktopTabGallery', 'desktopTabAnime', 'desktopTabSeries', 'desktopTabAdd', 'desktopTabCustom'].forEach(id => {
        const el = document.getElementById(id);
        if (el) el.className = 'px-3 py-1.5 rounded-xl text-slate-600 hover:text-slate-900 transition';
      });

      // Mobile Top Category Pills
      const pMovies = document.getElementById('pillBtnMovies');
      const pAnime = document.getElementById('pillBtnAnime');
      const pSeries = document.getElementById('pillBtnSeries');
      [pMovies, pAnime, pSeries].forEach(b => {
        b.className = 'flex-1 py-2 px-3 rounded-xl font-bold text-xs text-slate-600 hover:text-slate-900 flex items-center justify-center gap-1.5 shrink-0 transition';
      });

      if (pageName === 'gallery') {
        gView.classList.remove('hidden');
        document.getElementById('mobileNavGallery')?.classList.add('active');
        document.getElementById('desktopTabGallery').className = 'px-3 py-1.5 rounded-xl bg-brand-600 text-white shadow-xs transition';
        pMovies.className = 'flex-1 py-2 px-3 rounded-xl font-bold text-xs bg-brand-600 text-white shadow-xs flex items-center justify-center gap-1.5 shrink-0 transition';
        renderGalleryGrid();
      } else if (pageName === 'anime') {
        aView.classList.remove('hidden');
        document.getElementById('mobileNavAnime')?.classList.add('active');
        document.getElementById('desktopTabAnime').className = 'px-3 py-1.5 rounded-xl bg-brand-600 text-white shadow-xs transition';
        pAnime.className = 'flex-1 py-2 px-3 rounded-xl font-bold text-xs bg-brand-600 text-white shadow-xs flex items-center justify-center gap-1.5 shrink-0 transition';
        renderAnimeGrid();
      } else if (pageName === 'series') {
        sView.classList.remove('hidden');
        document.getElementById('mobileNavSeries')?.classList.add('active');
        document.getElementById('desktopTabSeries').className = 'px-3 py-1.5 rounded-xl bg-brand-600 text-white shadow-xs transition';
        pSeries.className = 'flex-1 py-2 px-3 rounded-xl font-bold text-xs bg-brand-600 text-white shadow-xs flex items-center justify-center gap-1.5 shrink-0 transition';
        renderSeriesGrid();
      } else if (pageName === 'add') {
        addView.classList.remove('hidden');
        document.getElementById('mobileNavAdd')?.classList.add('active');
        document.getElementById('desktopTabAdd').className = 'px-3 py-1.5 rounded-xl bg-brand-600 text-white shadow-xs transition';
        window.scrollTo({ top: 0, behavior: 'smooth' });
      } else if (pageName === 'custom') {
        cView.classList.remove('hidden');
        document.getElementById('mobileNavCustom')?.classList.add('active');
        document.getElementById('desktopTabCustom').className = 'px-3 py-1.5 rounded-xl bg-brand-600 text-white shadow-xs transition';
        syncCustomizationUi();
        window.scrollTo({ top: 0, behavior: 'smooth' });
      }

      // Re-apply background corresponding to current area
      applyBackgroundStyles();
      updateAllCounts();
    }

    function returnToPreviousPage() {
      switchPage(previousPage || 'gallery');
    }

    function openAddModalFor(type) {
      document.getElementById('itemEntryForm').reset();
      document.getElementById('editItemId').value = '';
      document.getElementById('formPreviewWrapper').classList.add('hidden');
      document.getElementById('formPreviewWrapper').classList.remove('flex');

      const radio = document.querySelector(`input[name="itemCategoryType"][value="${type}"]`);
      if (radio) {
        radio.checked = true;
        handleCategoryTypeChange(type);
      }

      document.getElementById('formPageTitle').textContent = `Save ${type.charAt(0).toUpperCase() + type.slice(1)} Poster`;
      document.getElementById('submitBtnText').textContent = `Save ${type.charAt(0).toUpperCase() + type.slice(1)}`;
      switchPage('add');
    }

    function updateAllCounts() {
      const movieCount = items.filter(i => (i.type || 'movie') === 'movie').length;
      const animeCount = items.filter(i => i.type === 'anime').length;
      const seriesCount = items.filter(i => i.type === 'series').length;
      const totalCount = items.length;

      // Header Total Available Posters Count
      document.getElementById('headerAvailableCount').textContent = totalCount;
      
      // Top Pills counts
      document.getElementById('pillCountMovies').textContent = movieCount;
      document.getElementById('pillCountAnime').textContent = animeCount;
      document.getElementById('pillCountSeries').textContent = seriesCount;

      // Movies Banner Available & URL Counts
      const movieUrls = items.filter(i => (i.type || 'movie') === 'movie' && i.movieUrl && i.movieUrl.trim() !== '').length;
      document.getElementById('heroAvailableCount').textContent = movieCount;
      document.getElementById('heroUrlsCount').textContent = movieUrls;

      // Anime & Series Count Badges
      const aCountEl = document.getElementById('animeAvailableCount');
      if (aCountEl) aCountEl.textContent = animeCount;

      const sCountEl = document.getElementById('seriesAvailableCount');
      if (sCountEl) sCountEl.textContent = seriesCount;
    }

    function updateGenreFilterOptions() {
      const select = document.getElementById('genreSelect');
      const val = select.value;
      const movieItems = items.filter(i => (i.type || 'movie') === 'movie');
      const genres = Array.from(new Set(movieItems.map(m => m.genre).filter(Boolean))).sort();

      let html = '<option value="ALL">All Genres</option>';
      genres.forEach(g => {
        html += `<option value="${g}">${g}</option>`;
      });
      select.innerHTML = html;
      select.value = genres.includes(val) ? val : 'ALL';
    }

    function getFilteredMovies() {
      let list = items.filter(i => (i.type || 'movie') === 'movie');

      if (activeFilterGenre !== 'ALL') {
        list = list.filter(m => m.genre === activeFilterGenre);
      }

      if (activeSearchQuery.trim() !== '') {
        const q = activeSearchQuery.toLowerCase().trim();
        list = list.filter(m => 
          (m.title && m.title.toLowerCase().includes(q)) ||
          (m.director && m.director.toLowerCase().includes(q)) ||
          (m.genre && m.genre.toLowerCase().includes(q)) ||
          (m.oneline && m.oneline.toLowerCase().includes(q)) ||
          (m.notes && m.notes.toLowerCase().includes(q))
        );
      }

      list.sort((a, b) => {
        if (activeSortKey === 'title_asc') return (a.title || '').localeCompare(b.title || '');
        if (activeSortKey === 'title_desc') return (b.title || '').localeCompare(a.title || '');
        if (activeSortKey === 'rating_desc') return (b.rating || 0) - (a.rating || 0);
        return 0;
      });

      return list;
    }

    function renderGalleryGrid() {
      const container = document.getElementById('moviePosterGrid');
      const emptyNotice = document.getElementById('emptyGalleryNotice');
      const filtered = getFilteredMovies();

      const totalMovies = items.filter(i => (i.type || 'movie') === 'movie').length;
      document.getElementById('galleryCurrentCount').textContent = filtered.length;

      const statusEl = document.getElementById('filteredStatusText');
      if (activeFilterGenre !== 'ALL' || activeSearchQuery.trim() !== '') {
        statusEl.textContent = `Showing ${filtered.length} of ${totalMovies} available`;
      } else {
        statusEl.textContent = `All ${totalMovies} movie posters available`;
      }

      if (filtered.length === 0) {
        container.innerHTML = '';
        emptyNotice.classList.remove('hidden');
        return;
      }
      emptyNotice.classList.add('hidden');
      container.innerHTML = filtered.map(movie => createPosterCardHtml(movie, 'movie')).join('');
    }

    function filterWatchStatus(category, status) {
      if (category === 'anime') {
        activeAnimeStatus = status;
        const btnIds = {
          'ALL': 'animeStatusAll',
          'Watching': 'animeStatusWatching',
          'Plan to Watch': 'animeStatusPlan',
          'Completed': 'animeStatusCompleted'
        };
        Object.keys(btnIds).forEach(k => {
          const el = document.getElementById(btnIds[k]);
          if (el) {
            el.className = k === status 
              ? 'px-3 py-1.5 rounded-xl text-xs font-bold bg-brand-600 text-white shrink-0'
              : 'px-3 py-1.5 rounded-xl text-xs font-bold bg-white text-slate-700 border border-brand-100 shrink-0';
          }
        });
        renderAnimeGrid();
      } else if (category === 'series') {
        activeSeriesStatus = status;
        const btnIds = {
          'ALL': 'seriesStatusAll',
          'Watching': 'seriesStatusWatching',
          'Plan to Watch': 'seriesStatusPlan',
          'Completed': 'seriesStatusCompleted'
        };
        Object.keys(btnIds).forEach(k => {
          const el = document.getElementById(btnIds[k]);
          if (el) {
            el.className = k === status 
              ? 'px-3 py-1.5 rounded-xl text-xs font-bold bg-brand-600 text-white shrink-0'
              : 'px-3 py-1.5 rounded-xl text-xs font-bold bg-white text-slate-700 border border-brand-100 shrink-0';
          }
        });
        renderSeriesGrid();
      }
    }

    function renderAnimeGrid() {
      const container = document.getElementById('animePosterGrid');
      const emptyNotice = document.getElementById('emptyAnimeNotice');
      let animeList = items.filter(i => i.type === 'anime');

      if (activeAnimeStatus !== 'ALL') {
        animeList = animeList.filter(a => a.watchStatus === activeAnimeStatus);
      }

      if (animeList.length === 0) {
        container.innerHTML = '';
        emptyNotice.classList.remove('hidden');
        return;
      }
      emptyNotice.classList.add('hidden');
      container.innerHTML = animeList.map(item => createPosterCardHtml(item, 'anime')).join('');
    }

    function renderSeriesGrid() {
      const container = document.getElementById('seriesPosterGrid');
      const emptyNotice = document.getElementById('emptySeriesNotice');
      let seriesList = items.filter(i => i.type === 'series');

      if (activeSeriesStatus !== 'ALL') {
        seriesList = seriesList.filter(s => s.watchStatus === activeSeriesStatus);
      }

      if (seriesList.length === 0) {
        container.innerHTML = '';
        emptyNotice.classList.remove('hidden');
        return;
      }
      emptyNotice.classList.add('hidden');
      container.innerHTML = seriesList.map(item => createPosterCardHtml(item, 'series')).join('');
    }

    // Card Builder: Contains Name, Genre, One-line summary, Movie Time, Episodes count & User URL (Never displays uploaded dates)
    function createPosterCardHtml(item, category) {
      const fallback = `https://placehold.co/400x600/15803d/ffffff?text=${encodeURIComponent(item.title)}`;
      const hasUrl = item.movieUrl && item.movieUrl.trim() !== '';

      let statusColor = 'bg-brand-900/90 text-white';
      if (item.watchStatus === 'Watching') statusColor = 'bg-emerald-600 text-white';
      if (item.watchStatus === 'Completed') statusColor = 'bg-brand-800 text-white';
      if (item.watchStatus === 'Plan to Watch') statusColor = 'bg-slate-800 text-slate-200';

      return `
        <div class="bg-white ${customSettings.cardRadius} border border-brand-100 shadow-xs overflow-hidden flex flex-col justify-between hover:shadow-md transition">
          
          <!-- Poster Image (Tap to open full details lightbox) -->
          <div class="relative w-full ${customSettings.aspectRatio} bg-slate-900 cursor-pointer overflow-hidden group" onclick="openLightbox('${item.id}')">
            <img 
              src="${item.posterUrl}" 
              alt="${item.title}" 
              class="w-full h-full object-cover group-hover:scale-105 transition duration-300"
              onerror="this.src='${fallback}'"
              loading="lazy"
            >

            ${customSettings.showBadges ? `
              <div class="absolute top-2 left-2 flex flex-col gap-1 pointer-events-none">
                ${item.rating ? `
                  <span class="inline-flex items-center gap-1 px-1.5 py-0.5 rounded-md bg-slate-950/80 text-amber-400 font-black text-[9px] backdrop-blur-xs">
                    <i class="fa-solid fa-star text-[7px]"></i> ${item.rating}
                  </span>
                ` : ''}
                <span class="inline-flex px-1.5 py-0.5 rounded-md bg-brand-950/80 text-brand-200 font-bold text-[8px] backdrop-blur-xs">
                  ${item.genre || 'Film'}
                </span>
              </div>
            ` : ''}

            <!-- Runtime & Episode badges on bottom of image -->
            <div class="absolute bottom-2 right-2 flex items-center gap-1 pointer-events-none">
              ${item.movieTime ? `
                <span class="px-1.5 py-0.5 rounded-md text-[8px] font-bold bg-slate-950/80 text-white backdrop-blur-xs">
                  <i class="fa-regular fa-clock text-[7px] mr-0.5"></i>${item.movieTime}
                </span>
              ` : ''}
            </div>

            ${(category !== 'movie' && item.watchStatus) ? `
              <div class="absolute bottom-2 left-2 pointer-events-none">
                <span class="px-1.5 py-0.5 rounded-md text-[8px] font-bold ${statusColor}">
                  ${item.watchStatus}
                </span>
              </div>
            ` : ''}
          </div>

          <!-- Card Content -->
          <div class="p-2.5 sm:p-3 flex-1 flex flex-col justify-between">
            <div>
              <div class="flex items-center justify-between gap-1">
                <h4 onclick="openLightbox('${item.id}')" class="font-display font-bold text-xs sm:text-sm text-slate-900 line-clamp-1 hover:text-brand-600 cursor-pointer" title="${item.title}">
                  ${item.title}
                </h4>
              </div>

              <!-- One-line summary -->
              <p class="text-[11px] text-slate-600 line-clamp-2 mt-1 leading-snug font-normal">
                ${item.oneline || 'No one-line summary added.'}
              </p>

              <!-- Episodes count & Creator details -->
              <div class="flex items-center justify-between text-[10px] text-slate-400 mt-2">
                <span class="truncate pr-1">${item.director || 'Studio'}</span>
                ${item.episodes ? `<span class="font-semibold text-brand-700 bg-brand-50 px-1 rounded">${item.episodes}</span>` : ''}
              </div>
            </div>

            <!-- Action Button: User Link or Details (Strictly without date) -->
            <div class="mt-2.5 pt-2 border-t border-brand-50">
              ${(customSettings.showMovieUrlBtn && hasUrl) ? `
                <a 
                  href="${item.movieUrl}" 
                  target="_blank" 
                  rel="noopener noreferrer" 
                  onclick="event.stopPropagation()"
                  class="w-full py-1.5 px-2 rounded-lg bg-brand-50 hover:bg-brand-100 text-brand-800 border border-brand-200 text-[10px] font-bold flex items-center justify-center gap-1.5 transition"
                >
                  <i class="fa-solid fa-arrow-up-right-from-square text-[9px] text-brand-600"></i>
                  <span>${category === 'anime' ? 'Stream Anime' : (category === 'series' ? 'Watch Series' : 'Open Movie URL')}</span>
                </a>
              ` : `
                <button onclick="openLightbox('${item.id}')" class="w-full text-[10px] font-bold text-brand-700 hover:text-brand-900 flex items-center justify-between">
                  <span>View Details</span>
                  <i class="fa-solid fa-chevron-right text-[8px]"></i>
                </button>
              `}
            </div>
          </div>
        </div>
      `;
    }

    function handleCategoryTypeChange(type) {
      const inputTitleLabel = document.getElementById('inputTitleLabel');
      const creatorLabel = document.getElementById('inputCreatorLabel');
      const epLabel = document.getElementById('inputEpisodesLabel');
      const epInput = document.getElementById('inputItemEpisodes');

      if (type === 'anime') {
        inputTitleLabel.innerHTML = 'Anime Name / Title <span class="text-rose-500">*</span>';
        creatorLabel.textContent = 'Animation Studio / Author';
        epLabel.textContent = 'How Many Episodes?';
        epInput.placeholder = 'e.g. 24 Episodes, Movie (1 ep)';
      } else if (type === 'series') {
        inputTitleLabel.innerHTML = 'Web Series Name <span class="text-rose-500">*</span>';
        creatorLabel.textContent = 'Showrunner / Network';
        epLabel.textContent = 'How Many Episodes / Seasons?';
        epInput.placeholder = 'e.g. 8 Episodes (S01), 3 Seasons';
      } else {
        inputTitleLabel.innerHTML = 'Movie Name / Title <span class="text-rose-500">*</span>';
        creatorLabel.textContent = 'Director / Studio';
        epLabel.textContent = 'Format / Part';
        epInput.placeholder = 'e.g. Feature Film, Part 1 & 2';
      }
    }

    const inputPosterUrl = document.getElementById('inputPosterUrl');
    const inputPosterFile = document.getElementById('inputPosterFile');

    document.getElementById('tabSourceUrlBtn').addEventListener('click', () => {
      document.getElementById('posterUrlBox').classList.remove('hidden');
      document.getElementById('posterFileBox').classList.add('hidden');
      document.getElementById('tabSourceUrlBtn').className = 'px-2.5 py-1 rounded-md bg-brand-600 text-white font-bold transition';
      document.getElementById('tabSourceFileBtn').className = 'px-2.5 py-1 rounded-md text-slate-600 hover:text-slate-900 font-semibold transition';
    });

    document.getElementById('tabSourceFileBtn').addEventListener('click', () => {
      document.getElementById('posterUrlBox').classList.add('hidden');
      document.getElementById('posterFileBox').classList.remove('hidden');
      document.getElementById('tabSourceFileBtn').className = 'px-2.5 py-1 rounded-md bg-brand-600 text-white font-bold transition';
      document.getElementById('tabSourceUrlBtn').className = 'px-2.5 py-1 rounded-md text-slate-600 hover:text-slate-900 font-semibold transition';
    });

    inputPosterFile.addEventListener('change', (e) => {
      const file = e.target.files[0];
      if (!file) return;

      const reader = new FileReader();
      reader.onload = function(evt) {
        inputPosterUrl.value = evt.target.result;
        showFormPosterPreview(evt.target.result, `${(file.size / 1024).toFixed(1)} KB offline file loaded`);
      };
      reader.readAsDataURL(file);
    });

    inputPosterUrl.addEventListener('input', (e) => {
      if (e.target.value.trim()) {
        showFormPosterPreview(e.target.value.trim(), 'Web Poster URL');
      }
    });

    function showFormPosterPreview(src, label) {
      const wrapper = document.getElementById('formPreviewWrapper');
      const img = document.getElementById('formPreviewImg');
      const caption = document.getElementById('formPreviewCaption');
      img.src = src;
      caption.textContent = label;
      wrapper.classList.remove('hidden');
      wrapper.classList.add('flex');
    }

    document.getElementById('removePreviewBtn').addEventListener('click', () => {
      inputPosterUrl.value = '';
      inputPosterFile.value = '';
      document.getElementById('formPreviewWrapper').classList.add('hidden');
      document.getElementById('formPreviewWrapper').classList.remove('flex');
    });

    document.getElementById('itemEntryForm').addEventListener('submit', (e) => {
      e.preventDefault();

      const editId = document.getElementById('editItemId').value;
      const type = document.querySelector('input[name="itemCategoryType"]:checked')?.value || 'movie';
      const title = document.getElementById('inputItemTitle').value.trim();
      const oneline = document.getElementById('inputItemOneline').value.trim();
      const posterUrl = inputPosterUrl.value.trim();
      const movieUrl = document.getElementById('inputItemWatchUrl').value.trim();
      const movieTime = document.getElementById('inputItemMovieTime').value.trim();
      const episodes = document.getElementById('inputItemEpisodes').value.trim() || (type === 'movie' ? 'Film' : 'N/A');
      const genre = document.getElementById('inputItemGenre').value;
      const rating = parseFloat(document.getElementById('inputItemRating').value) || null;
      const director = document.getElementById('inputItemDirector').value.trim();
      const notes = document.getElementById('inputItemNotes').value.trim();
      const watchStatus = document.getElementById('inputItemWatchStatus').value;

      if (!title || !posterUrl || !oneline) {
        showToast('Please provide a title, one-line summary, and poster');
        return;
      }

      if (editId) {
        const idx = items.findIndex(m => m.id === editId);
        if (idx !== -1) {
          items[idx] = { 
            ...items[idx], 
            type,
            title, 
            oneline,
            posterUrl, 
            movieUrl, 
            movieTime,
            episodes,
            genre, 
            rating, 
            director, 
            notes,
            watchStatus
          };
          showToast(`Updated "${title}"`);
        }
      } else {
        const newItem = {
          id: (type === 'anime' ? 'an-' : (type === 'series' ? 'ws-' : 'mv-')) + Date.now(),
          type,
          title,
          oneline,
          posterUrl,
          movieUrl,
          movieTime,
          episodes,
          genre,
          rating,
          director,
          notes,
          watchStatus
        };
        items.unshift(newItem);
        showToast(`Saved "${title}" poster`);
      }

      persistData();
      updateGenreFilterOptions();
      updateAllCounts();

      // Return to active tab
      if (type === 'anime') {
        switchPage('anime');
      } else if (type === 'series') {
        switchPage('series');
      } else {
        switchPage('gallery');
      }
    });

    function openLightbox(id) {
      const item = items.find(m => m.id === id);
      if (!item) return;

      currentModalItemId = item.id;
      document.getElementById('lightboxPosterImg').src = item.posterUrl;
      document.getElementById('lightboxTitle').textContent = item.title;
      document.getElementById('lightboxOneline').textContent = item.oneline || 'No one-line summary provided.';
      document.getElementById('lightboxMovieTime').textContent = item.movieTime || 'N/A';
      document.getElementById('lightboxEpisodes').textContent = item.episodes || 'N/A';
      
      const catBadge = document.getElementById('lightboxCategoryBadge');
      catBadge.textContent = item.type === 'anime' ? 'Anime' : (item.type === 'series' ? 'Web Series' : 'Movie');
      
      document.getElementById('lightboxGenreBadge').textContent = item.genre || 'Film';
      
      const statusBadge = document.getElementById('lightboxStatusBadge');
      if (item.watchStatus && item.type !== 'movie') {
        statusBadge.textContent = item.watchStatus;
        statusBadge.classList.remove('hidden');
      } else {
        statusBadge.classList.add('hidden');
      }

      document.getElementById('lightboxRating').innerHTML = item.rating ? `<i class="fa-solid fa-star text-[10px]"></i> ${item.rating}` : 'Not Rated';
      document.getElementById('lightboxDirector').textContent = item.director ? `Creator: ${item.director}` : 'Creator: Unspecified';
      document.getElementById('lightboxNotes').textContent = item.notes || 'No extra notes provided.';

      const urlSection = document.getElementById('lightboxUrlSection');
      const urlAnchor = document.getElementById('lightboxLinkAnchor');
      const urlText = document.getElementById('lightboxLinkText');

      if (item.movieUrl && item.movieUrl.trim() !== '') {
        urlSection.classList.remove('hidden');
        urlAnchor.href = item.movieUrl;
        urlText.textContent = item.movieUrl;
      } else {
        urlSection.classList.add('hidden');
      }

      const modal = document.getElementById('lightboxOverlay');
      modal.classList.remove('hidden');
      setTimeout(() => {
        modal.classList.remove('opacity-0');
        modal.firstElementChild.classList.remove('translate-y-8', 'sm:scale-95');
      }, 10);
    }

    function closeLightboxModal() {
      const modal = document.getElementById('lightboxOverlay');
      modal.classList.add('opacity-0');
      modal.firstElementChild.classList.add('translate-y-8', 'sm:scale-95');
      setTimeout(() => {
        modal.classList.add('hidden');
      }, 200);
    }

    document.getElementById('lightboxEditBtn').addEventListener('click', () => {
      if (!currentModalItemId) return;
      const item = items.find(m => m.id === currentModalItemId);
      if (!item) return;

      closeLightboxModal();

      document.getElementById('formPageTitle').textContent = `Edit ${item.type || 'Movie'}`;
      document.getElementById('editItemId').value = item.id;
      
      const catRadio = document.querySelector(`input[name="itemCategoryType"][value="${item.type || 'movie'}"]`);
      if (catRadio) {
        catRadio.checked = true;
        handleCategoryTypeChange(item.type || 'movie');
      }

      document.getElementById('inputItemTitle').value = item.title || '';
      document.getElementById('inputItemOneline').value = item.oneline || '';
      document.getElementById('inputPosterUrl').value = item.posterUrl || '';
      document.getElementById('inputItemWatchUrl').value = item.movieUrl || '';
      document.getElementById('inputItemMovieTime').value = item.movieTime || '';
      document.getElementById('inputItemEpisodes').value = item.episodes || '';
      document.getElementById('inputItemGenre').value = item.genre || 'Sci-Fi';
      document.getElementById('inputItemRating').value = item.rating || '';
      document.getElementById('inputItemDirector').value = item.director || '';
      document.getElementById('inputItemNotes').value = item.notes || '';
      document.getElementById('inputItemWatchStatus').value = item.watchStatus || 'Plan to Watch';

      if (item.posterUrl) {
        showFormPosterPreview(item.posterUrl, 'Current Poster');
      }

      switchPage('add');
    });

    document.getElementById('lightboxDeleteBtn').addEventListener('click', () => {
      if (!currentModalItemId) return;
      const idx = items.findIndex(m => m.id === currentModalItemId);
      if (idx !== -1) {
        const title = items[idx].title;
        items.splice(idx, 1);
        persistData();
        updateGenreFilterOptions();
        updateAllCounts();
        renderGalleryGrid();
        renderAnimeGrid();
        renderSeriesGrid();
        closeLightboxModal();
        showToast(`Removed "${title}"`);
      }
    });

    function syncCustomizationUi() {
      document.getElementById('customShowMovieUrlBtn').checked = customSettings.showMovieUrlBtn;
      document.getElementById('customShowBadges').checked = customSettings.showBadges;

      ['aspect-poster-classic', 'aspect-poster-wide', 'aspect-poster-square', 'aspect-poster-cinema'].forEach(r => {
        let bId = r === 'aspect-poster-classic' ? 'ratioClassicBtn' : 
                  r === 'aspect-poster-wide' ? 'ratioWideBtn' : 
                  r === 'aspect-poster-square' ? 'ratioSquareBtn' : 'ratioCinemaBtn';
        const btn = document.getElementById(bId);
        if (btn) {
          if (customSettings.aspectRatio === r) {
            btn.className = 'p-2.5 rounded-xl border-2 border-brand-500 bg-brand-50 text-brand-900 text-xs font-bold text-center';
          } else {
            btn.className = 'p-2.5 rounded-xl border border-slate-200 text-slate-700 text-xs font-medium text-center';
          }
        }
      });

      ['rounded-xl', 'rounded-2xl', 'rounded-3xl'].forEach(rad => {
        let bId = rad === 'rounded-xl' ? 'radiusXlBtn' : (rad === 'rounded-2xl' ? 'radius2XlBtn' : 'radius3XlBtn');
        const b = document.getElementById(bId);
        if (b) {
          if (customSettings.cardRadius === rad) {
            b.className = `py-2 px-2 ${rad} border-2 border-brand-500 bg-brand-50 text-brand-900 text-xs font-bold`;
          } else {
            b.className = `py-2 px-2 ${rad} border border-slate-200 text-slate-700 text-xs`;
          }
        }
      });

      // Dynamic mode vs preset highlights
      const dynamicBtn = document.getElementById('bgModeDynamicBtn');
      if (dynamicBtn) {
        if (customSettings.bgThemeMode === 'dynamic') {
          dynamicBtn.className = 'p-2.5 rounded-xl border-2 border-brand-600 bg-brand-50 text-brand-900 text-left transition flex items-center gap-2 ring-2 ring-brand-200';
        } else {
          dynamicBtn.className = 'p-2.5 rounded-xl border border-brand-200 bg-white hover:bg-brand-50 text-left transition flex items-center gap-2';
        }
      }

      const pMap = {
        floral: 'presetFloralBtn',
        anime: 'presetAnimeBtn',
        series: 'presetSeriesBtn'
      };
      Object.keys(pMap).forEach(key => {
        const btn = document.getElementById(pMap[key]);
        if (btn) {
          if (customSettings.bgThemeMode === 'fixed' && customSettings.bgPreset === key && !customSettings.bgPhotoData) {
            btn.classList.add('border-brand-600', 'bg-brand-50', 'ring-2', 'ring-brand-200');
          } else {
            btn.classList.remove('border-brand-600', 'bg-brand-50', 'ring-2', 'ring-brand-200');
          }
        }
      });

      // Slider sync
      const opSlider = document.getElementById('bgOpacitySlider');
      if (opSlider) {
        opSlider.value = customSettings.bgOpacity || 35;
        document.getElementById('bgOpacityValText').textContent = `${opSlider.value}%`;
      }
      const blSlider = document.getElementById('bgBlurSlider');
      if (blSlider) {
        blSlider.value = customSettings.bgBlur || 2;
        document.getElementById('bgBlurValText').textContent = `${blSlider.value}px`;
      }
    }

    function setAspectRatio(ratioClass) {
      customSettings.aspectRatio = ratioClass;
      persistData();
      syncCustomizationUi();
      showToast('Aspect ratio updated');
    }

    function setCardRadius(radClass) {
      customSettings.cardRadius = radClass;
      persistData();
      syncCustomizationUi();
      showToast('Border radius updated');
    }

    function updateSettingsToggle(key, val) {
      customSettings[key] = val;
      persistData();
      syncCustomizationUi();
      showToast('Preferences saved');
    }

    function restoreDefaultSettings() {
      customSettings = {
        aspectRatio: 'aspect-poster-classic',
        showMovieUrlBtn: true,
        showBadges: true,
        cardRadius: 'rounded-2xl',
        bgThemeMode: 'dynamic',
        bgPreset: 'floral',
        bgPhotoData: '',
        bgOpacity: 35,
        bgBlur: 2
      };
      persistData();
      applyBackgroundStyles();
      syncCustomizationUi();
      showToast('Settings restored');
    }

    // Search events
    const searchInput = document.getElementById('searchInput');
    const clearSearchBtn = document.getElementById('clearSearchBtn');

    searchInput.addEventListener('input', (e) => {
      activeSearchQuery = e.target.value;
      if (activeSearchQuery.length > 0) {
        clearSearchBtn.classList.remove('hidden');
      } else {
        clearSearchBtn.classList.add('hidden');
      }
      renderGalleryGrid();
    });

    clearSearchBtn.addEventListener('click', () => {
      searchInput.value = '';
      activeSearchQuery = '';
      clearSearchBtn.classList.add('hidden');
      renderGalleryGrid();
    });

    document.getElementById('genreSelect').addEventListener('change', (e) => {
      activeFilterGenre = e.target.value;
      renderGalleryGrid();
    });

    document.getElementById('sortSelect').addEventListener('change', (e) => {
      activeSortKey = e.target.value;
      renderGalleryGrid();
    });

    function exportCollection() {
      const dataStr = 'data:text/json;charset=utf-8,' + encodeURIComponent(JSON.stringify(items, null, 2));
      const a = document.createElement('a');
      a.setAttribute('href', dataStr);
      a.setAttribute('download', 'cinevault_offline_library_backup.json');
      document.body.appendChild(a);
      a.click();
      a.remove();
      showToast('Backup file downloaded');
    }

    function triggerImportDialog() {
      document.getElementById('importJsonFileInput').click();
    }

    document.getElementById('importJsonFileInput').addEventListener('change', (e) => {
      const file = e.target.files[0];
      if (!file) return;

      const reader = new FileReader();
      reader.onload = function(evt) {
        try {
          const parsed = JSON.parse(evt.target.result);
          if (Array.isArray(parsed)) {
            items = parsed;
            persistData();
            updateGenreFilterOptions();
            updateAllCounts();
            renderGalleryGrid();
            renderAnimeGrid();
            renderSeriesGrid();
            showToast(`Imported ${parsed.length} items successfully!`);
          }
        } catch (err) {
          showToast('Invalid JSON backup file');
        }
      };
      reader.readAsText(file);
    });

    window.onload = function() {
      loadSavedData();
      updateOnlineOfflineIndicator();
      updateGenreFilterOptions();
      updateAllCounts();
      renderGalleryGrid();
      syncCustomizationUi();
    };
  </script>
</body>
</html>
```

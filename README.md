# utm-photo-contest1
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>UTM Photo Contest</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .fade {
      transition: opacity 0.5s ease-in-out;
    }
    .hidden-section {
      opacity: 0;
      pointer-events: none;
      position: absolute;
      width: 100%;
    }
    .visible-section {
      opacity: 1;
      pointer-events: auto;
      position: relative;
    }
  </style>
</head>
<body class="bg-white text-gray-800 relative min-h-screen">

  <!-- Navbar -->
  <nav class="flex items-center justify-between p-4 bg-red-900 text-white">
    <div class="flex items-center space-x-2">
      <span class="font-bold text-xl">UTM Photo Contest</span>
    </div>
    <div class="flex items-center space-x-4">
      <button onclick="showSection('gallery')">Gallery</button>
      <button onclick="showSection('updates')">Campus Updates</button>
      <button onclick="switchLanguage('en')">EN</button>
      <button onclick="switchLanguage('ms')">BM</button>
      <button onclick="switchLanguage('zh')">中文</button>
    </div>
  </nav>

  <!-- Hero Section -->
  <section class="bg-gray-100 text-center py-12">
    <h1 id="title" class="text-4xl font-bold mb-4">Capture the Spirit of UTM!</h1>
    <p id="description" class="text-lg">Join our photo contest and share your best shots of UTM life and beauty!</p>
    <button onclick="uploadPhoto()" class="mt-6 px-6 py-2 bg-red-700 text-white rounded hover:bg-red-800">Upload Photo</button>
    <input type="file" id="photoInput" class="hidden" accept="image/*" onchange="previewPhoto(event)">
  </section>

  <!-- Gallery Section -->
  <section id="gallery" class="p-8 columns-1 md:columns-3 gap-6 fade visible-section">
    <!-- Initial sample photos -->
  </section>

  <!-- Campus Updates Section -->
  <section id="updates" class="p-8 bg-gray-100 fade hidden-section">
    <h2 id="updatesTitle" class="text-2xl font-bold mb-6 text-center">Campus Updates</h2>
    <div id="updatesContainer" class="space-y-6">
      <div class="bg-white p-6 rounded shadow">
        <h3 class="font-semibold text-lg">Photo Contest Extended!</h3>
        <p class="text-sm text-gray-500">2025-05-15</p>
        <p class="mt-2">Due to popular demand, the UTM photo contest deadline is extended by 2 weeks!</p>
      </div>
      <div class="bg-white p-6 rounded shadow">
        <h3 class="font-semibold text-lg">Campus Flora Photography Tips</h3>
        <p class="text-sm text-gray-500">2025-05-10</p>
        <p class="mt-2">Learn how to capture the beautiful flora around UTM campus with these simple tips.</p>
      </div>
    </div>
    <button onclick="addUpdate()" class="mt-6 px-6 py-2 bg-red-700 text-white rounded hover:bg-red-800 block mx-auto">Add Campus Update</button>
  </section>

  <!-- Footer -->
  <footer class="bg-gray-200 text-center p-4 text-sm mt-10">
    © 2025 Universiti Teknologi Malaysia. All rights reserved.
  </footer>

  <script>
    const photos = [];

    function uploadPhoto() {
      document.getElementById('photoInput').click();
    }

    function previewPhoto(event) {
      const file = event.target.files[0];
      if (!file) return;

      const reader = new FileReader();
      reader.onload = function(e) {
        const newPhoto = {
          src: e.target.result,
          likes: 0
        };
        photos.push(newPhoto);
        renderGallery();
      };
      reader.readAsDataURL(file);
    }

    function likePhoto(index) {
      photos[index].likes++;
      renderGallery();
    }

    function renderGallery() {
      const gallery = document.getElementById('gallery');
      gallery.innerHTML = '';

      photos.sort((a, b) => b.likes - a.likes);

      photos.forEach((photo, index) => {
        const photoDiv = document.createElement('div');
        photoDiv.className = "relative mb-6 break-inside-avoid";
        photoDiv.innerHTML = `
          <img src="${photo.src}" alt="Uploaded Photo" class="rounded shadow w-full">
          <button onclick="likePhoto(${index})" class="absolute bottom-2 right-2 bg-white p-2 rounded-full shadow flex items-center">
            ❤️ <span class="ml-1">${photo.likes}</span>
          </button>
        `;
        gallery.appendChild(photoDiv);
      });
    }

    function switchLanguage(lang) {
      const titles = {
        en: "Capture the Spirit of UTM!",
        ms: "Tangkap Semangat UTM!",
        zh: "捕捉UTM的精神！"
      };

      const descriptions = {
        en: "Join our photo contest and share your best shots of UTM life and beauty!",
        ms: "Sertai pertandingan foto kami dan kongsikan gambar terbaik kehidupan dan keindahan UTM!",
        zh: "加入我们的摄影比赛，分享你最精彩的UTM生活与美景！"
      };

      const updatesTitles = {
        en: "Campus Updates",
        ms: "Kemaskini Kampus",
        zh: "校园动态"
      };

      document.getElementById('title').textContent = titles[lang];
      document.getElementById('description').textContent = descriptions[lang];
      document.getElementById('updatesTitle').textContent = updatesTitles[lang];
    }

    function showSection(section) {
      const gallery = document.getElementById('gallery');
      const updates = document.getElementById('updates');

      if (section === 'gallery') {
        gallery.classList.remove('hidden-section');
        gallery.classList.add('visible-section');
        updates.classList.remove('visible-section');
        updates.classList.add('hidden-section');
      } else {
        updates.classList.remove('hidden-section');
        updates.classList.add('visible-section');
        gallery.classList.remove('visible-section');
        gallery.classList.add('hidden-section');
      }
    }

    function addUpdate() {
      const container = document.getElementById('updatesContainer');
      const update = document.createElement('div');
      update.className = "bg-white p-6 rounded shadow";
      update.innerHTML = `
        <h3 class="font-semibold text-lg">New Update!</h3>
        <p class="text-sm text-gray-500">${new Date().toISOString().split('T')[0]}</p>
        <p class="mt-2">Stay tuned for more exciting campus news and events!</p>
      `;
      container.prepend(update);
    }
  </script>

</body>
</html>

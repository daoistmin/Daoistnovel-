<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Painel Light Novels</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-900 text-white p-4">

  <h1 class="text-3xl font-bold mb-6">Painel de Light Novels</h1>

  <!-- Painel adicionar Light Novel -->
  <div class="mb-8">
    <h2 class="text-xl font-semibold mb-2">Adicionar Light Novel</h2>
    <input id="title" type="text" placeholder="Título" class="border p-2 mb-2 block text-black w-full">
    <input id="description" type="text" placeholder="Descrição" class="border p-2 mb-2 block text-black w-full">
    <input id="cover" type="text" placeholder="URL da Capa" class="border p-2 mb-2 block text-black w-full">
    <button onclick="addNovel()" class="bg-green-600 px-4 py-2 rounded">Adicionar Novel</button>
  </div>

  <!-- Lista de Light Novels -->
  <div id="novelList" class="grid grid-cols-1 md:grid-cols-2 gap-4"></div>

  <!-- Modal de capítulos -->
  <div id="chapterModal" class="hidden fixed inset-0 bg-black bg-opacity-80 flex items-center justify-center">
    <div class="bg-gray-800 p-6 rounded w-96">
      <h2 id="modalTitle" class="text-xl font-bold mb-4"></h2>
      <input id="chapterTitle" type="text" placeholder="Título do Capítulo" class="border p-2 mb-2 block text-black w-full">
      <textarea id="chapterContent" placeholder="Conteúdo" class="border p-2 mb-2 block text-black w-full h-32"></textarea>
      <button onclick="addChapter()" class="bg-blue-600 px-4 py-2 rounded mr-2">Adicionar Capítulo</button>
      <button onclick="closeChapterModal()" class="bg-red-600 px-4 py-2 rounded">Fechar</button>

      <div id="chapterList" class="mt-4"></div>
    </div>
  </div>

  <script>
    let currentNovelIndex = null;

    function addNovel() {
      const title = document.getElementById('title').value;
      const description = document.getElementById('description').value;
      const cover = document.getElementById('cover').value;

      if (!title || !description || !cover) {
        alert('Preencha todos os campos!');
        return;
      }

      const novels = JSON.parse(localStorage.getItem('novels') || '[]');
      novels.push({ title, description, cover, chapters: [] });
      localStorage.setItem('novels', JSON.stringify(novels));

      document.getElementById('title').value = '';
      document.getElementById('description').value = '';
      document.getElementById('cover').value = '';

      loadNovels();
    }

    function loadNovels() {
      const list = document.getElementById('novelList');
      list.innerHTML = '';
      const novels = JSON.parse(localStorage.getItem('novels') || '[]');

      novels.forEach((novel, index) => {
        const div = document.createElement('div');
        div.className = 'border border-gray-700 p-4 rounded';

        div.innerHTML = `
          <h3 class="text-lg font-bold">${novel.title}</h3>
          <img src="${novel.cover}" alt="Capa" class="w-full h-48 object-cover my-2 rounded">
          <p>${novel.description}</p>
          <button onclick="openChapterModal(${index})" class="bg-blue-600 px-3 py-1 mt-2 rounded mr-2">Capítulos</button>
          <button onclick="deleteNovel(${index})" class="bg-red-600 px-3 py-1 mt-2 rounded">Excluir Novel</button>
        `;
        list.appendChild(div);
      });
    }

    function deleteNovel(index) {
      const novels = JSON.parse(localStorage.getItem('novels') || '[]');
      novels.splice(index, 1);
      localStorage.setItem('novels', JSON.stringify(novels));
      loadNovels();
    }

    function openChapterModal(index) {
      currentNovelIndex = index;
      document.getElementById('chapterModal').classList.remove('hidden');
      const novels = JSON.parse(localStorage.getItem('novels') || '[]');
      document.getElementById('modalTitle').innerText = `Capítulos de: ${novels[index].title}`;
      loadChapters();
    }

    function closeChapterModal() {
      document.getElementById('chapterModal').classList.add('hidden');
      document.getElementById('chapterTitle').value = '';
      document.getElementById('chapterContent').value = '';
    }

    function addChapter() {
      const title = document.getElementById('chapterTitle').value;
      const content = document.getElementById('chapterContent').value;

      if (!title || !content) {
        alert('Preencha o título e o conteúdo!');
        return;
      }

      const novels = JSON.parse(localStorage.getItem('novels') || '[]');
      novels[currentNovelIndex].chapters.push({ title, content });
      localStorage.setItem('novels', JSON.stringify(novels));

      document.getElementById('chapterTitle').value = '';
      document.getElementById('chapterContent').value = '';
      loadChapters();
    }

    function loadChapters() {
      const list = document.getElementById('chapterList');
      list.innerHTML = '';
      const novels = JSON.parse(localStorage.getItem('novels') || '[]');
      const chapters = novels[currentNovelIndex].chapters;

      chapters.forEach((chap, index) => {
        const div = document.createElement('div');
        div.className = 'border border-gray-600 p-2 mt-2 rounded';
        div.innerHTML = `
          <h4 class="font-semibold">${chap.title}</h4>
          <p class="text-sm">${chap.content}</p>
          <button onclick="deleteChapter(${index})" class="bg-red-600 px-2 py-1 mt-1 rounded text-sm">Excluir Capítulo</button>
        `;
        list.appendChild(div);
      });
    }

    function deleteChapter(index) {
      const novels = JSON.parse(localStorage.getItem('novels') || '[]');
      novels[currentNovelIndex].chapters.splice(index, 1);
      localStorage.setItem('novels', JSON.stringify(novels));
      loadChapters();
    }

    loadNovels();
  </script>
</body>
</html>
  
  

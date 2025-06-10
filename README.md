<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>DaoistNovel</title>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: #0a0f24;
      color: #fff;
      <div id="novelsList">
  <div class="novel-item">
    <span>Minha Light Novel</span>
    
    }

    header {
      background: #1a1f3c;
      padding: 1rem;
      text-align: center;
    }

    header img {
      max-width: 100%;
      height: auto;
      border-radius: 12px;
    }

    nav {
      display: flex;
      justify-content: center;
      gap: 1rem;
      padding: 0.5rem;
      background: #121630;
    }

    nav button {
      background: #2a2f50;
      color: #fff;
      border: none;
      padding: 0.5rem 1rem;
      border-radius: 8px;
      cursor: pointer;
    }

    main {
      padding: 1rem;
    }

    .section {
      display: none;
    }

    .visible {
      display: block;
    }

    input, textarea {
      width: 100%;
      padding: 0.5rem;
      margin: 0.5rem 0;
      border-radius: 8px;
      border: 1px solid #333;
      background: #1c1f3a;
      color: #fff;
    }

    .card {
      background: #1c1f3a;
      padding: 1rem;
      margin: 0.5rem 0;
      border-radius: 8px;
    }

    .light-novel-cover {
      max-width: 100%;
      border-radius: 8px;
    }

    @media (max-width: 600px) {
      nav {
        flex-direction: column;
        align-items: center;
      }

      nav button {
        width: 90%;
      }
    }
  </style>
</head>
<body>

<header>
  <img src="daoistnovel_capa.jpg" alt="Capa DaoistNovel">
</header>

<nav>
  <button onclick="showSection('addNovel')">Adicionar Light Novel</button>
  <button onclick="showSection('addChapter')">Adicionar Capítulo</button>
  <button onclick="showSection('comments')">Comentários</button>
  <button onclick="deleteItem(this)">Excluir</button>
</nav>

<main>
  <div id="addNovel" class="section visible">
    <h2>Nova Light Novel</h2>
    <input type="text" id="novelTitle" placeholder="Título">
    <input type="file" id="novelCover">
    <textarea id="novelDescription" placeholder="Descrição"></textarea>
    <button onclick="saveNovel()">Salvar</button>
    <div id="novelList"></div>
    
}
  </div>

  <div id="addChapter" class="section">
    <h2>Adicionar Capítulo</h2>
    <input type="text" id="chapterTitle" placeholder="Título do Capítulo">
    <textarea id="chapterContent" placeholder="Conteúdo"></textarea>
    <button onclick="saveChapter()">Salvar Capítulo</button>
    <div id="chapterList"></div>
  </div>

  <div id="comments" class="section">
    <h2>Comentários e Memes</h2>
    <textarea id="commentText" placeholder="Seu comentário ou meme"></textarea>
    <button onclick="addComment()">Enviar</button>
    <div id="commentList"></div>
  </div>
</main>

<script>
  function showSection(id) {
    document.querySelectorAll('.section').forEach(s => s.classList.remove('visible'));
    document.getElementById(id).classList.add('visible');
    function deleteItem(button) {
  // Pega o elemento pai do botão (a div ou elemento da novel/capítulo)
  const item = button.parentElement;

  // Remove ele da tela
  item.remove();

  // Aqui você poderia adicionar um alerta ou console log se quiser
  console.log("Item excluído!");
    }
  }

  function saveNovel() {
    const title = document.getElementById('novelTitle').value;
    const description = document.getElementById('novelDescription').value;
    const cover = document.getElementById('novelCover').files[0];
    const reader = new FileReader();

    reader.onload = () => {
      const novel = { title, description, cover: reader.result };
      const novels = JSON.parse(localStorage.getItem('novels') || '[]');
      novels.push(novel);
      localStorage.setItem('novels', JSON.stringify(novels));
      loadNovels();
    };

    if (cover) reader.readAsDataURL(cover);
    else alert('Adicione uma capa.');
  }

  function loadNovels() {
    const list = document.getElementById('novelList');
    list.innerHTML = '';
    const novels = JSON.parse(localStorage.getItem('novels') || '[]');

    novels.forEach(novel => {
      const div = document.createElement('div');
      div.className = 'card';
      div.innerHTML = `<h3>${novel.title}</h3>
        <img src="${novel.cover}" class="light-novel-cover">
        <p>${novel.description}</p>`;
      list.appendChild(div);
    });
  }

  function saveChapter() {
    const title = document.getElementById('chapterTitle').value;
    const content = document.getElementById('chapterContent').value;
    const chapter = { title, content };
    const chapters = JSON.parse(localStorage.getItem('chapters') || '[]');
    chapters.push(chapter);
    localStorage.setItem('chapters', JSON.stringify(chapters));
    loadChapters();
  }

  function loadChapters() {
    const list = document.getElementById('chapterList');
    list.innerHTML = '';
    const chapters = JSON.parse(localStorage.getItem('chapters') || '[]');

    chapters.forEach(chap => {
      const div = document.createElement('div');
      div.className = 'card';
      div.innerHTML = `<h4>${chap.title}</h4>
        <p>${chap.content}</p>`;
      list.appendChild(div);
    });
  }

  function addComment() {
    const text = document.getElementById('commentText').value;
    const comments = JSON.parse(localStorage.getItem('comments') || '[]');
    comments.push(text);
    localStorage.setItem('comments', JSON.stringify(comments));
    loadComments();
  }

  function loadComments() {
    const list = document.getElementById('commentList');
    list.innerHTML = '';
    const comments = JSON.parse(localStorage.getItem('comments') || '[]');

    comments.forEach(c => {
      const div = document.createElement('div');
      div.className = 'card';
      div.innerText = c;
      list.appendChild(div);
    });
  }

  loadNovels();
  loadChapters();
  loadComments();
</script>

</body>
</html>
 Admin painel<!DOCTYPE html><html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Painel DaoistNovel</title>
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
</head>
<body class="bg-gray-900 text-white">
  <div class="max-w-3xl mx-auto p-6">
    <h1 class="text-3xl font-bold mb-6">Painel Administrativo DaoistNovel 📜✨</h1><!-- Login básico -->
<div id="loginArea" class="mb-6">
  <input type="password" id="senha" placeholder="Senha de acesso" class="p-2 rounded text-black">
  <button onclick="validarLogin()" class="ml-2 px-4 py-2 bg-yellow-500 rounded">Entrar</button>
  <p id="loginErro" class="text-red-400 mt-2 hidden">Senha incorreta!</p>
</div>

<!-- Painel só aparece depois do login -->
<div id="painelArea" class="hidden">
  <!-- Adicionar Light Novel -->
  <h2 class="text-2xl font-bold mt-4">Adicionar Light Novel 📖</h2>
  <input type="text" id="tituloNovel" placeholder="Título" class="mt-2 p-2 rounded text-black block">
  <input type="text" id="capaNovel" placeholder="Nome do arquivo da capa (ex: capa.jpg)" class="mt-2 p-2 rounded text-black block">
  <textarea id="descricaoNovel" placeholder="Descrição" class="mt-2 p-2 rounded text-black w-full"></textarea>
  <button onclick="adicionarNovel()" class="mt-2 px-4 py-2 bg-green-500 rounded">Salvar Novel</button>

  <!-- Adicionar Capítulo -->
  <h2 class="text-2xl font-bold mt-8">Adicionar Capítulo 📜</h2>
  <input type="number" id="idNovelCap" placeholder="ID da Novel" class="mt-2 p-2 rounded text-black block">
  <input type="text" id="tituloCapitulo" placeholder="Título do Capítulo" class="mt-2 p-2 rounded text-black block">
  <textarea id="conteudoCapitulo" placeholder="Conteúdo do capítulo" class="mt-2 p-2 rounded text-black w-full"></textarea>
  <button onclick="adicionarCapitulo()" class="mt-2 px-4 py-2 bg-green-500 rounded">Salvar Capítulo</button>
</div>

  </div>  <script>
    // Senha hardcoded (troque para sua preferida)
    const senhaCorreta = 'daoist2025';

    function validarLogin() {
      const senha = document.getElementById('senha').value;
      if (senha === senhaCorreta) {
        document.getElementById('loginArea').classList.add('hidden');
        document.getElementById('painelArea').classList.remove('hidden');
      } else {
        document.getElementById('loginErro').classList.remove('hidden');
      }
    }

    // Adiciona Light Novel (apenas no console por enquanto)
    function adicionarNovel() {
      const titulo = document.getElementById('tituloNovel').value;
      const capa = document.getElementById('capaNovel').value;
      const descricao = document.getElementById('descricaoNovel').value;
      console.log({ titulo, capa, descricao });
      alert('Light Novel adicionada no console! 🚀 (versão local, precisa de backend pra salvar)');
    }

    // Adiciona Capítulo (apenas no console por enquanto)
    function adicionarCapitulo() {
      const id = document.getElementById('idNovelCap').value;
      const titulo = document.getElementById('tituloCapitulo').value;
      const conteudo = document.getElementById('conteudoCapitulo').value;
      console.log({ id, titulo, conteudo });
      alert('Capítulo adicionado no console! 🚀 (versão local, precisa de backend pra salvar)');
    }
  </script></body>
</html>

# projeto-FalaIAPE-
<!DOCTYPE html>
<html lang="pt-BR">
<head>

  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Mini Twitter</title>

  <style>

    *{
      margin:0;
      padding:0;
      box-sizing:border-box;
      font-family:Arial,sans-serif;
    }

    body{
      background:#e6ecf0;
      display:flex;
      justify-content:center;
      padding:30px;
    }

    .app{
      width:100%;
      max-width:650px;
    }

    .header{
      background:#1d9bf0;
      color:white;
      padding:20px;
      border-radius:16px 16px 0 0;
      font-size:24px;
      font-weight:bold;
      text-align:center;
    }

    .composer{
      background:white;
      padding:20px;
      border-bottom:1px solid #ddd;
    }

    input,
    textarea{
      width:100%;
      padding:14px;
      border:1px solid #ccc;
      border-radius:10px;
      margin-bottom:12px;
      resize:none;
      font-size:15px;
    }

    button{
      background:#1d9bf0;
      color:white;
      border:none;
      padding:12px 20px;
      border-radius:999px;
      cursor:pointer;
      font-weight:bold;
      transition:0.2s;
    }

    button:hover{
      opacity:0.9;
    }

    .feed{
      background:white;
      border-radius:0 0 16px 16px;
      overflow:hidden;
    }

    .post{
      padding:18px;
      border-bottom:1px solid #eee;
    }

    .post:nth-child(odd){
      background:#f7f9fa;
    }

    .post:nth-child(even){
      background:white;
    }

    .user{
      font-weight:bold;
      margin-bottom:8px;
      color:#111;
    }

    .text{
      color:#333;
      line-height:1.5;
      margin-bottom:10px;
    }

    .date{
      font-size:12px;
      color:#777;
    }

    .empty{
      text-align:center;
      padding:30px;
      color:#777;
    }

  </style>

</head>
<body>

  <div class="app">

    <div class="header">
      Mini Twitter
    </div>

    <div class="composer">

      <input
        type="text"
        id="usuario"
        placeholder="Seu nome"
      >

      <textarea
        id="postagem"
        rows="4"
        placeholder="O que está acontecendo?"
      ></textarea>

      <button onclick="publicar()">
        Publicar
      </button>

    </div>

    <div class="feed" id="feed"></div>

  </div>

  <script>

    const API_URL =
      "COLE_AQUI_SUA_URL_DO_APPS_SCRIPT";

    async function carregarPosts() {

      try {

        const response =
          await fetch(API_URL);

        const posts =
          await response.json();

        const feed =
          document.getElementById("feed");

        feed.innerHTML = "";

        if(posts.length === 0){

          feed.innerHTML = `
            <div class="empty">
              Nenhuma postagem ainda
            </div>
          `;

          return;
        }

        posts.forEach(post => {

          const card =
            document.createElement("div");

          card.className = "post";

          const data =
            new Date(post.data);

          card.innerHTML = `

            <div class="user">
              @${post.usuario}
            </div>

            <div class="text">
              ${post.postagem}
            </div>

            <div class="date">
              ${data.toLocaleString("pt-BR")}
            </div>

          `;

          feed.appendChild(card);

        });

      } catch(erro){

        console.error(erro);

        alert("Erro ao carregar posts.");

      }

    }

    async function publicar(){

      const usuario =
        document
          .getElementById("usuario")
          .value
          .trim();

      const postagem =
        document
          .getElementById("postagem")
          .value
          .trim();

      if(!usuario || !postagem){

        alert("Preencha todos os campos.");

        return;
      }

      try{

        await fetch(API_URL, {

          method:"POST",

          body: JSON.stringify({
            usuario,
            postagem
          })

        });

        document
          .getElementById("postagem")
          .value = "";

        carregarPosts();

      } catch(erro){

        console.error(erro);

        alert("Erro ao publicar.");

      }

    }

    carregarPosts();

  </script>

</body>
</html>

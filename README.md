<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Manga Reader</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">
  <style>
    body { background-color: #f8f9fa; }
    .container { max-width: 800px; margin: auto; padding: 20px; }
    .manga-cover { max-width: 100%; border-radius: 8px; margin-bottom: 15px; }
  </style>
</head>
<body>
  <div class="container">
    <h1 class="text-center mb-4">📚 Manga Reader</h1>

    <div class="mb-3">
      <input type="text" id="username" class="form-control mb-2" placeholder="Username">
      <input type="password" id="password" class="form-control mb-2" placeholder="Password">
      <button onclick="login()" class="btn btn-primary w-100">Login</button>
    </div>

    <hr>

    <h3 class="mb-3">Available Manga</h3>
    <div id="mangaList"></div>
  </div>

  <script>
    const apiBase = 'https://your-api-url.onrender.com'; // Replace with your deployed API URL

    async function login() {
      const username = document.getElementById('username').value;
      const password = document.getElementById('password').value;

      try {
        const res = await fetch(`${apiBase}/login`, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ username, password })
        });

        const data = await res.json();
        if (res.ok) {
          alert('Login successful');
          fetchManga();
        } else {
          alert(data.message || 'Login failed');
        }
      } catch (error) {
        alert('Failed to connect to the server. Make sure your backend is deployed.');
        console.error('Fetch error:', error);
      }
    }

    async function fetchManga() {
      try {
        const res = await fetch(`${apiBase}/manga`);
        if (!res.ok) throw new Error('Failed to fetch manga');
        const data = await res.json();
        const list = document.getElementById('mangaList');
        list.innerHTML = '';
        data.forEach(manga => {
          const div = document.createElement('div');
          div.className = 'card mb-3';
          div.innerHTML = `
            <div class="card-body">
              <h5 class="card-title">${manga.title || manga.id}</h5>
              <p class="card-text">${manga.description || 'No description available'}</p>
            </div>
          `;
          list.appendChild(div);
        });
      } catch (error) {
        alert('Could not load manga list.');
        console.error('Fetch manga error:', error);
      }
    }
  </script>
</body>
</html>

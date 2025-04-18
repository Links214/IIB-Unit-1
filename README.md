# IIB-Unit-1
QR-Referencias
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Lista de Enlaces</title>
    <style>
        body {
            font-family: sans-serif;
            margin: 20px;
            background-color: #f4f4f4; /* Fondo por defecto */
        }
        #header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        #links-container {
            margin-top: 20px;
        }
        .link-box {
            background-color: #fff;
            padding: 10px;
            margin-bottom: 10px;
            border-radius: 5px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }
        .edit-controls {
            display: none; /* Oculto por defecto */
            margin-top: 10px;
            padding: 10px;
            background-color: #eee;
            border-radius: 5px;
        }
        .edit-controls input[type="text"],
        .edit-controls input[type="password"] {
            padding: 8px;
            margin-right: 10px;
            border: 1px solid #ccc;
            border-radius: 3px;
        }
        .edit-controls button {
            padding: 8px 12px;
            background-color: #5cb85c;
            color: white;
            border: none;
            border-radius: 3px;
            cursor: pointer;
        }
        .edit-controls button:hover {
            background-color: #4cae4c;
        }
        #change-background {
            margin-top: 10px;
        }
        #change-background input[type="color"] {
            margin-right: 10px;
        }
    </style>
</head>
<body>
    <div id="header">
        <h1>Mi Lista de Enlaces Favoritos</h1>
        <button id="edit-button">Editar</button>
    </div>

    <div id="edit-controls" style="display: none;">
        <input type="password" id="password" placeholder="Contraseña">
        <button id="login-button">Entrar</button>
        <div id="add-link-section" style="display: none; margin-top: 10px;">
            <input type="text" id="new-link-name" placeholder="Nombre del enlace">
            <input type="text" id="new-link-url" placeholder="URL del enlace">
            <button id="add-link-button">Agregar Enlace</button>
        </div>
        <div id="change-background" style="display: none; margin-top: 10px;">
            <label for="background-color">Cambiar Fondo:</label>
            <input type="color" id="background-color">
        </div>
    </div>

    <div id="links-container">
        </div>

    <script>
        const editButton = document.getElementById('edit-button');
        const editControls = document.getElementById('edit-controls');
        const passwordInput = document.getElementById('password');
        const loginButton = document.getElementById('login-button');
        const addLinkSection = document.getElementById('add-link-section');
        const newLinkNameInput = document.getElementById('new-link-name');
        const newLinkUrlInput = document.getElementById('new-link-url');
        const addLinkButton = document.getElementById('add-link-button');
        const linksContainer = document.getElementById('links-container');
        const backgroundColorInput = document.getElementById('background-color');
        const changeBackgroundSection = document.getElementById('change-background');

        const correctPassword = 'catocaye';
        let isLoggedIn = false;

        // Cargar enlaces guardados (si los hay)
        let links = localStorage.getItem('myLinks') ? JSON.parse(localStorage.getItem('myLinks')) : [];
        renderLinks();

        // Cargar el color de fondo guardado (si lo hay)
        const savedBackgroundColor = localStorage.getItem('backgroundColor');
        if (savedBackgroundColor) {
            document.body.style.backgroundColor = savedBackgroundColor;
            backgroundColorInput.value = savedBackgroundColor;
        }

        editButton.addEventListener('click', () => {
            editControls.style.display = 'block';
        });

        loginButton.addEventListener('click', () => {
            if (passwordInput.value === correctPassword) {
                isLoggedIn = true;
                addLinkSection.style.display = 'block';
                changeBackgroundSection.style.display = 'block';
                editButton.style.display = 'none';
                passwordInput.style.display = 'none';
                loginButton.style.display = 'none';
            } else {
                alert('Contraseña incorrecta.');
            }
            passwordInput.value = ''; // Limpiar el campo de contraseña
        });

        addLinkButton.addEventListener('click', () => {
            if (newLinkNameInput.value && newLinkUrlInput.value) {
                links.push({ name: newLinkNameInput.value, url: newLinkUrlInput.value });
                saveLinks();
                renderLinks();
                newLinkNameInput.value = '';
                newLinkUrlInput.value = '';
            } else {
                alert('Por favor, ingresa el nombre y la URL del enlace.');
            }
        });

        backgroundColorInput.addEventListener('input', () => {
            document.body.style.backgroundColor = backgroundColorInput.value;
            localStorage.setItem('backgroundColor', backgroundColorInput.value);
        });

        function renderLinks() {
            linksContainer.innerHTML = ''; // Limpiar el contenedor
            links.forEach((link, index) => {
                const linkBox = document.createElement('div');
                linkBox.classList.add('link-box');
                linkBox.innerHTML = `<a href="${link.url}" target="_blank">${link.name}</a>`;
                linksContainer.appendChild(linkBox);
            });
        }

        function saveLinks() {
            localStorage.setItem('myLinks', JSON.stringify(links));
        }
    </script>
</body>
</html>

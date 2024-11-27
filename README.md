<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerenciador de Registros</title>
    <!-- Link do Bootstrap -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body {
            background-color: black;
            color: white;
        }
        .btn-add {
            background-color: green;
            color: white;
        }
        .btn-add:hover {
            background-color: darkgreen;
        }
    </style>
</head>
<body>
    <div class="container mt-5">
        <h1 class="text-center mb-4">Gerenciador de Registros</h1>

        <div class="row mb-3">
            <div class="col-md-4">
                <input type="text" id="inputNome" class="form-control" placeholder="Digite o nome">
            </div>
            <div class="col-md-4">
                <input type="email" id="inputEmail" class="form-control" placeholder="Digite o e-mail">
            </div>
            <div class="col-md-4">
                <input type="tel" id="inputTelefone" class="form-control" placeholder="Digite o telefone (ex: 11987654321)">
            </div>
        </div>
        <div class="row">
            <div class="col-md-3 ms-auto">
                <button class="btn btn-add w-100" onclick="adicionarRegistro()">Adicionar Registro</button>
            </div>
        </div>

        <table class="table table-dark table-striped mt-4">
            <thead>
                <tr>
                    <th>#</th>
                    <th>Nome</th>
                    <th>E-mail</th>
                    <th>Telefone</th>
                    <th>Ação</th>
                </tr>
            </thead>
            <tbody id="tabelaRegistros">
                <!-- Os registros serão inseridos dinamicamente aqui -->
            </tbody>
        </table>
    </div>

    <!-- Script JS -->
    <script>
        // Função para carregar registros salvos no LocalStorage
        const carregarRegistros = () => {
            const registros = JSON.parse(localStorage.getItem("registros")) || [];
            const tabela = document.getElementById("tabelaRegistros");
            tabela.innerHTML = "";
            registros.forEach((item, index) => {
                tabela.innerHTML += `
                    <tr>
                        <td>${index + 1}</td>
                        <td>${item.nome}</td>
                        <td>${item.email}</td>
                        <td>${item.telefone}</td>
                        <td>
                            <button class="btn btn-danger btn-sm" onclick="removerRegistro(${index})">Remover</button>
                        </td>
                    </tr>
                `;
            });
        };

        // Função para validar o formato de e-mail
        const validarEmail = (email) => {
            const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            return emailRegex.test(email);
        };

        // Função para validar o formato de telefone
        const validarTelefone = (telefone) => {
            const telefoneRegex = /^\d{10,11}$/; // Aceita 10 ou 11 dígitos
            return telefoneRegex.test(telefone);
        };

        // Função para adicionar novo registro
        const adicionarRegistro = () => {
            const inputNome = document.getElementById("inputNome");
            const inputEmail = document.getElementById("inputEmail");
            const inputTelefone = document.getElementById("inputTelefone");
            const nome = inputNome.value.trim();
            const email = inputEmail.value.trim();
            const telefone = inputTelefone.value.trim();

            if (!nome || !email || !telefone) {
                alert("Por favor, preencha todos os campos.");
                return;
            }

            if (!validarEmail(email)) {
                alert("Por favor, insira um e-mail válido.");
                return;
            }

            if (!validarTelefone(telefone)) {
                alert("Por favor, insira um telefone válido (10 ou 11 dígitos, sem espaços ou símbolos).");
                return;
            }

            const registros = JSON.parse(localStorage.getItem("registros")) || [];

            // Verifica se o e-mail já existe
            if (registros.some((registro) => registro.email === email)) {
                alert("Este e-mail já está cadastrado.");
                return;
            }

            registros.push({ nome, email, telefone });
            localStorage.setItem("registros", JSON.stringify(registros));
            inputNome.value = "";
            inputEmail.value = "";
            inputTelefone.value = "";
            carregarRegistros();
        };

        // Função para remover registro pelo índice
        const removerRegistro = (index) => {
            const registros = JSON.parse(localStorage.getItem("registros")) || [];
            registros.splice(index, 1);
            localStorage.setItem("registros", JSON.stringify(registros));
            carregarRegistros();
        };

        // Inicializa a tabela ao carregar a página
        window.onload = carregarRegistros;
    </script>
    <!-- Script do Bootstrap -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>

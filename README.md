# gerador-de-curriculoss
http://localhost/gerador_curriculo_simples/index.php
<!DOCTYPE html>
<html lang="pt-br">a
<head>
  <meta charset="UTF-8">
  <title>Gerador de Currículo Simples</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f5f5f5;
      color: #333;
      padding: 20px;
    }
    h1, h3 {
      color: #004080;
      text-align: center;
    }
    form {
      background: #fff;
      padding: 20px;
      border-radius: 8px;
      max-width: 700px;
      margin: 20px auto;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    input, textarea, button {
      display: block;
      width: 100%;
      margin-bottom: 10px;
      padding: 8px;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
    button {
      background-color: #004080;
      color: #fff;
      border: none;
      cursor: pointer;
      border-radius: 5px;
      padding: 10px;
    }
    button:hover {
      background-color: #0066cc;
    }
    .container {
      max-width: 800px;
      background: #fff;
      padding: 20px;
      border-radius: 10px;
      margin: auto;
    }
    .voltar {
      background-color: gray;
      color: #fff;
      text-decoration: none;
      padding: 8px 12px;
      border-radius: 5px;
      display: inline-block;
      margin-top: 10px;
    }
  </style>
</head>
<body>

<?php
// Se o formulário foi enviado, gera o currículo
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
  $nome = $_POST['nome'];
  $email = $_POST['email'];
  $telefone = $_POST['telefone'];
  $endereco = $_POST['endereco'];
  $data_nascimento = $_POST['data_nascimento'];
  $idade = $_POST['idade'];
  $empresa = $_POST['empresa'];
  $cargo = $_POST['cargo'];
  $periodo = $_POST['periodo'];
  $descricao = $_POST['descricao'];
  $curso = $_POST['curso'];
  $instituicao = $_POST['instituicao'];
  $ano = $_POST['ano'];
  ?>

  <div class="container">
    <h1><?= htmlspecialchars($nome) ?></h1>
    <p><strong>E-mail:</strong> <?= htmlspecialchars($email) ?></p>
    <p><strong>Telefone:</strong> <?= htmlspecialchars($telefone) ?></p>
    <p><strong>Endereço:</strong> <?= htmlspecialchars($endereco) ?></p>
    <p><strong>Data de Nascimento:</strong> <?= htmlspecialchars($data_nascimento) ?></p>
    <p><strong>Idade:</strong> <?= htmlspecialchars($idade) ?> anos</p>

    <hr>
    <h3>Experiências Profissionais</h3>
    <?php
    if (!empty($empresa)) {
      foreach ($empresa as $i => $emp) {
        if (!empty($emp)) {
          echo "<p><strong>{$emp}</strong> - {$cargo[$i]} ({$periodo[$i]})<br>{$descricao[$i]}</p>";
        }
      }
    }
    ?>

    <hr>
    <h3>Formação Acadêmica</h3>
    <?php
    if (!empty($curso)) {
      foreach ($curso as $i => $c) {
        if (!empty($c)) {
          echo "<p><strong>{$c}</strong> - {$instituicao[$i]} ({$ano[$i]})</p>";
        }
      }
    }
    ?>

    <hr>
    <button onclick="window.print()">Baixar / Imprimir Currículo</button>
    <a href="index.php" class="voltar">Voltar</a>
  </div>

  <?php
} else {
  // Exibe o formulário
?>
  <h1>Gerador de Currículo</h1>
  <form method="POST">

    <h3>Dados Pessoais</h3>
    <label>Nome:</label>
    <input type="text" name="nome" required>

    <label>Data de Nascimento:</label>
    <input type="date" id="data_nascimento" name="data_nascimento" required>

    <label>Idade:</label>
    <input type="number" id="idade" name="idade" readonly>

    <label>E-mail:</label>
    <input type="email" name="email" required>

    <label>Telefone:</label>
    <input type="text" name="telefone">

    <label>Endereço:</label>
    <input type="text" name="endereco">

    <h3>Experiências Profissionais</h3>
    <div id="experiencias">
      <div class="exp-item">
        <input type="text" name="empresa[]" placeholder="Nome da empresa">
        <input type="text" name="cargo[]" placeholder="Cargo">
        <input type="text" name="periodo[]" placeholder="Período (Ex: 2020-2022)">
        <textarea name="descricao[]" placeholder="Descrição das atividades"></textarea>
      </div>
    </div>
    <button type="button" id="addExp">+ Adicionar Experiência</button>

    <h3>Formação Acadêmica</h3>
    <div id="formacoes">
      <div class="form-item">
        <input type="text" name="curso[]" placeholder="Curso">
        <input type="text" name="instituicao[]" placeholder="Instituição">
        <input type="text" name="ano[]" placeholder="Ano de Conclusão">
      </div>
    </div>
    <button type="button" id="addForm">+ Adicionar Formação</button>

    <button type="submit">Gerar Currículo</button>
  </form>

  <script>
    // Calcular idade automaticamente
    document.getElementById('data_nascimento').addEventListener('change', function() {
      const hoje = new Date();
      const nascimento = new Date(this.value);
      let idade = hoje.getFullYear() - nascimento.getFullYear();
      const m = hoje.getMonth() - nascimento.getMonth();
      if (m < 0 || (m === 0 && hoje.getDate() < nascimento.getDate())) idade--;
      document.getElementById('idade').value = idade;
    });

    // Adicionar novas experiências
    document.getElementById('addExp').addEventListener('click', function() {
      const div = document.createElement('div');
      div.classList.add('exp-item');
      div.innerHTML = `
        <input type="text" name="empresa[]" placeholder="Nome da empresa">
        <input type="text" name="cargo[]" placeholder="Cargo">
        <input type="text" name="periodo[]" placeholder="Período (Ex: 2020-2022)">
        <textarea name="descricao[]" placeholder="Descrição das atividades"></textarea>
      `;
      document.getElementById('experiencias').appendChild(div);
    });

    // Adicionar novas formações
    document.getElementById('addForm').addEventListener('click', function() {
      const div = document.createElement('div');
      div.classList.add('form-item');
      div.innerHTML = `
        <input type="text" name="curso[]" placeholder="Curso">
        <input type="text" name="instituicao[]" placeholder="Instituição">
        <input type="text" name="ano[]" placeholder="Ano de Conclusão">
      `;
      document.getElementById('formacoes').appendChild(div);
    });
  </script>

<?php
}
?>
</body>
</html>

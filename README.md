<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <title>Área de Avaliação Física</title>
</head>
<body>
  <h1>Bem-vindo à Avaliação Física</h1>
  <button onclick="window.location.href='personal.html'">Sou Personal Trainer</button>
  <button onclick="window.location.href='aluno.html'">Sou Aluno</button>
</body>
</html>
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Área do Personal</title>
</head>
<body>
  <h1>Área do Personal Trainer</h1>

  <h2>Cadastrar Aluno</h2>
  <input type="text" id="nomeAluno" placeholder="Nome do Aluno">
  <button onclick="cadastrarAluno()">Cadastrar</button>

  <h2>Registrar Avaliação</h2>
  <select id="alunoSelect"></select><br><br>
  <input type="number" id="peso" placeholder="Peso (kg)">
  <input type="number" id="altura" placeholder="Altura (cm)">
  <input type="text" id="observacao" placeholder="Observações"><br><br>
  <button onclick="registrarAvaliacao()">Salvar Avaliação</button>

  <script src="script.js"></script>
</body>
</html>
function getAlunos() {
  return JSON.parse(localStorage.getItem("alunos") || "[]");
}

function salvarAlunos(alunos) {
  localStorage.setItem("alunos", JSON.stringify(alunos));
}

function cadastrarAluno() {
  const nome = document.getElementById("nomeAluno").value.trim();
  if (!nome) return alert("Digite o nome do aluno.");
  const alunos = getAlunos();
  if (alunos.find(a => a.nome === nome)) return alert("Aluno já cadastrado.");
  alunos.push({ nome, avaliacoes: [] });
  salvarAlunos(alunos);
  alert("Aluno cadastrado!");
  atualizarSelectAlunos();
}

function atualizarSelectAlunos() {
  const select = document.getElementById("alunoSelect");
  if (!select) return;
  select.innerHTML = "";
  getAlunos().forEach(aluno => {
    const opt = document.createElement("option");
    opt.value = aluno.nome;
    opt.textContent = aluno.nome;
    select.appendChild(opt);
  });
}

function registrarAvaliacao() {
  const nome = document.getElementById("alunoSelect").value;
  const peso = parseFloat(document.getElementById("peso").value);
  const altura = parseFloat(document.getElementById("altura").value);
  const observacao = document.getElementById("observacao").value;
  const alunos = getAlunos();
  const aluno = alunos.find(a => a.nome === nome);
  if (!aluno) return alert("Aluno não encontrado.");
  aluno.avaliacoes.push({ data: new Date().toLocaleDateString(), peso, altura, observacao });
  salvarAlunos(alunos);
  alert("Avaliação salva!");
}

function verAvaliacoes() {
  const nomeBusca = document.getElementById("alunoBusca").value.trim();
  const alunos = getAlunos();
  const aluno = alunos.find(a => a.nome.toLowerCase() === nomeBusca.toLowerCase());
  const div = document.getElementById("resultado");
  if (!aluno) {
    div.innerHTML = "<p>Aluno não encontrado.</p>";
    return;
  }

  let html = `<h2>Avaliações de ${aluno.nome}</h2><ul>`;
  aluno.avaliacoes.forEach(av => {
    html += `<li><strong>${av.data}</strong>: ${av.peso}kg, ${av.altura}cm – ${av.observacao}</li>`;
  });
  html += "</ul>";
  div.innerHTML = html;
}

window.onload = atualizarSelectAlunos;

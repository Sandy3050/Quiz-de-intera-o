# Quiz-de-intera-o
<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<title>Quiz Interativo</title>
<style>
  body { font-family: Arial, sans-serif; padding: 20px; background: #f0f0f0; }
  #quiz-container { background: #fff; padding: 20px; border-radius: 10px; max-width: 600px; margin: auto; }
  button { padding: 10px 20px; margin-top: 10px; cursor: pointer; }
  .hidden { display: none; }
</style>
</head>
<body>

<div id="quiz-container">
  <h2>Escolha o tema:</h2>
  <select id="tema">
    <option value="geografia">Geografia</option>
    <option value="matematica">Matemática</option>
  </select>
  <button onclick="iniciarQuiz()">Iniciar Quiz</button>

  <div id="fase-container" class="hidden">
    <h2 id="fase-titulo"></h2>
    <p id="pergunta"></p>
    <input type="text" id="resposta" placeholder="Digite sua resposta">
    <button onclick="verificarResposta()">Responder</button>
    <p id="chances"></p>
  </div>

  <div id="resultado" class="hidden"></div>
</div>

<script>
// Banco de perguntas
const perguntas = {
  geografia: [
    {pergunta: "Qual a capital da França?", resposta: "Paris"},
    {pergunta: "Qual o maior país do mundo?", resposta: "Rússia"},
    {pergunta: "Qual país tem a maior população?", resposta: "China"},
    {pergunta: "Qual é o menor país do mundo?", resposta: "Vaticano"}
  ],
  matematica: [
    {pergunta: "2 + 2 = ?", resposta: "4"},
    {pergunta: "5 x 6 = ?", resposta: "30"},
    {pergunta: "12 ÷ 3 = ?", resposta: "4"},
    {pergunta: "10 - 7 = ?", resposta: "3"}
  ]
};

let temaEscolhido;
let fases = [];
let faseAtual = 0;
let perguntaAtual = 0;
let chances = 3;

function iniciarQuiz() {
  temaEscolhido = document.getElementById("tema").value;
  let banco = perguntas[temaEscolhido];
  let tamanhoFase = Math.ceil(banco.length / 4);
  fases = [];
  for (let i = 0; i < 4; i++) {
    fases.push(banco.slice(i*tamanhoFase, (i+1)*tamanhoFase));
  }
  faseAtual = 0;
  perguntaAtual = 0;
  chances = 3;
  document.getElementById("resultado").classList.add("hidden");
  document.getElementById("fase-container").classList.remove("hidden");
  mostrarPergunta();
}

function mostrarPergunta() {
  const perguntaObj = fases[faseAtual][perguntaAtual];
  document.getElementById("fase-titulo").innerText = `Fase ${faseAtual + 1}`;
  document.getElementById("pergunta").innerText = perguntaObj.pergunta;
  document.getElementById("resposta").value = "";
  document.getElementById("chances").innerText = `Chances restantes: ${chances}`;
}

function verificarResposta() {
  const resposta = document.getElementById("resposta").value.trim().toLowerCase();
  const correta = fases[faseAtual][perguntaAtual].resposta.toLowerCase();

  if(resposta === correta){
    perguntaAtual++;
    if(perguntaAtual >= fases[faseAtual].length){
      faseAtual++;
      if(faseAtual >= fases.length){
        document.getElementById("fase-container").classList.add("hidden");
        document.getElementById("resultado").classList.remove("hidden");
        document.getElementById("resultado").innerHTML = "🏆 Parabéns! Você concluiu todas as fases e ganhou o jogo!";
        return;
      }
      perguntaAtual = 0;
      chances = 3;
      alert("🎉 Parabéns! Você passou para a próxima fase!");
    }
    mostrarPergunta();
  } else {
    chances--;
    if(chances <= 0){
      document.getElementById("fase-container").classList.add("hidden");
      document.getElementById("resultado").classList.remove("hidden");
      document.getElementById("resultado").innerHTML = "Você perdeu todas as chances desta fase! Tente novamente.";
    } else {
      alert(`❌ Errou! Tentativas restantes: ${chances}`);
      mostrarPergunta();
    }
  }
}
</script>

</body>
</html>

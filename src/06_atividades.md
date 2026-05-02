# Pilar 6 - Atividades Interativas

## Proposito do catalogo

Este documento descreve o repertorio de atividades interativas que usamos em cursos e projetos educacionais. O objetivo e funcionar como um catalogo de implementacao: cada atividade tem descricao suficiente para ser reproduzida por uma IA ou desenvolvedor sem precisar consultar o codigo-fonte original.

As atividades estao organizadas por nivel de complexidade — de elementos simples de revelacao ate simuladores com multiplas fases. Cada contexto pede combinacoes diferentes; o cerne de cada atividade e transferivel entre temas.

---

## Nivel 1 — Revelacao e apresentacao de informacao

Atividades que nao exigem resposta ou avaliacao do aluno. O objetivo e apresentar conteudo de forma progressiva, evitando sobrecarga cognitiva.

---

### Accordion (perguntas e respostas expandiveis)

**Quando usar:** Para organizar conteudo denso em blocos. O aluno acessa o que precisa sem ter que rolar tudo de uma vez. Ideal para FAQs, glossarios, detalhes tecnicos, revisoes de modulo.

**Mecanica:** O usuario ve uma lista de itens com titulo visiveis. Ao clicar em um titulo, o conteudo abaixo expande. Clicar novamente recolhe.

**HTML:**
```html
<details class="accordion-item">
  <summary class="accordion-header">Qual e o prazo para solicitacao?</summary>
  <div class="accordion-body">
    <p>O prazo e de 5 dias uteis a partir da data de abertura do chamado.</p>
  </div>
</details>
```

**JS:** O elemento `<details>` e nativo do HTML — funciona sem JavaScript. Para adicionar animacao de abertura/fechamento suave, intercepte o click e anime a altura do `.accordion-body` com `grid-template-rows: 0fr` → `1fr` (ver catalogo de animacoes).

**Variacoes:**
- Accordion exclusivo: fechar o anterior ao abrir outro (JS controla `open` nos outros elements)
- Com icone de seta que gira ao abrir (ver animacao de chevron)
- Com contador de itens ou badge de "novo"

---

### Reveal button (revelar conteudo oculto)

**Quando usar:** Revelar resposta, dica, reflexao ou aprofundamento que so deve aparecer apos o aluno ter refletido. Evita que o aluno pule direto para o gabarito.

**Mecanica:** Botao com texto tipo "Ver resposta" ou "Revelar". Ao clicar, o conteudo abaixo aparece e o botao fica desabilitado (ou some).

**HTML:**
```html
<div class="reveal-container">
  <button class="reveal-btn btn-secondary">Ver resposta</button>
  <div class="reveal-content" hidden>
    <p>A resposta correta e: <strong>alternativa C</strong>, pois...</p>
  </div>
</div>
```

**JS:**
```js
document.querySelectorAll('.reveal-btn').forEach(btn => {
  btn.addEventListener('click', () => {
    const content = btn.nextElementSibling;
    content.hidden = false;
    content.classList.add('visible'); // para animar com CSS
    btn.disabled = true;
    btn.textContent = '✓ Revelado';
  });
});
```

**Variacoes:**
- Revelar em etapas (multiplos botoes sequenciais dentro do mesmo bloco)
- Com contador regressivo antes de liberar o botao ("Aguarde 10s para ver a resposta")

---

### Flip card (frente e verso)

**Quando usar:** Apresentar dois lados de uma informacao: pergunta/resposta, conceito/exemplo, antes/depois. Mais engajante que texto plano. Muito usado em "Voce sabia?" e cartoes de vocabulario.

**Mecanica:** O aluno ve a frente do cartao. Ao clicar, ele gira 180 graus (animacao 3D CSS) e revela o verso. Clicar de novo volta para a frente.

**HTML:**
```html
<div class="flip-card" tabindex="0" role="button" aria-label="Clique para revelar">
  <div class="flip-card-inner">
    <div class="flip-card-front">
      <p class="card-label">Voce sabia?</p>
      <p>Trabalhar com monitor muito alto causa tensao no pescoco.</p>
    </div>
    <div class="flip-card-back">
      <p>O monitor deve estar ao nivel dos olhos, a 50-70cm de distancia. A linha de visao natural deve coincidir com o terco superior da tela.</p>
    </div>
  </div>
</div>
```

**CSS essencial:**
```css
.flip-card { perspective: 1000px; }
.flip-card-inner {
  transform-style: preserve-3d;
  transition: transform 0.5s ease;
}
.flip-card.flipped .flip-card-inner { transform: rotateY(180deg); }
.flip-card-front, .flip-card-back {
  backface-visibility: hidden;
  position: absolute; inset: 0;
}
.flip-card-back { transform: rotateY(180deg); }
```

**JS:**
```js
document.querySelectorAll('.flip-card').forEach(card => {
  card.addEventListener('click', () => card.classList.toggle('flipped'));
  card.addEventListener('keydown', e => {
    if (e.key === 'Enter' || e.key === ' ') card.classList.toggle('flipped');
  });
});
```

---

### Card expansivel (saiba mais)

**Quando usar:** Conteudo com camadas de profundidade. A primeira camada e o resumo essencial. A segunda, o aprofundamento para quem quer mais. Evita sobrecarregar quem nao precisa do detalhe.

**Mecanica:** Card com cabecalho visivel. Ao clicar, o corpo expande revelando mais conteudo. Icone de seta ou "+" indica a acao.

**HTML:**
```html
<div class="interactive-card">
  <div class="card-header">
    <h3>Pausas regulares</h3>
    <span class="card-toggle-icon" aria-hidden="true">+</span>
  </div>
  <div class="card-body" hidden>
    <p>Pausas de 5 a 10 minutos a cada hora reduzem em 40% o risco de LER/DORT. O ideal e alternar entre tarefas diferentes durante a pausa.</p>
  </div>
</div>
```

**JS:**
```js
document.querySelectorAll('.interactive-card').forEach(card => {
  card.querySelector('.card-header').addEventListener('click', () => {
    const body = card.querySelector('.card-body');
    const isOpen = !body.hidden;
    body.hidden = isOpen;
    card.classList.toggle('expanded', !isOpen);
    card.querySelector('.card-toggle-icon').textContent = isOpen ? '+' : '−';
  });
});
```

---

### Glossario contextual (tooltip de termo)

**Quando usar:** Quando o conteudo usa jargao tecnico ou siglas. Em vez de interromper o fluxo com uma nota de rodape, o aluno clica no termo e ve a definicao no proprio lugar.

**Mecanica:** Termos com underline pontilhado. Ao passar o mouse (ou clicar em mobile), um popup com a definicao aparece proximo ao termo. Fecha ao clicar fora.

**HTML:**
```html
<p>Para acionar o <span class="glossary-term" data-term="sac">SAC</span>, siga o procedimento padrao.</p>

<!-- Popup gerado por JS -->
<div class="glossary-popup" hidden role="tooltip">
  <p class="glossary-title"></p>
  <p class="glossary-definition"></p>
</div>
```

**JS (logica central):**
```js
const glossary = {
  sac: { title: 'SAC', definition: 'Servico de Atendimento ao Consumidor. Canal oficial para registrar reclamacoes e solicitacoes.' },
  sla: { title: 'SLA', definition: 'Service Level Agreement. Acordo de nivel de servico que define prazo maximo para resolucao.' },
};

document.querySelectorAll('.glossary-term').forEach(term => {
  term.addEventListener('click', (e) => {
    const key = term.dataset.term;
    const entry = glossary[key];
    showGlossaryPopup(entry, term);
    e.stopPropagation();
  });
});

function showGlossaryPopup(entry, anchor) {
  const popup = document.querySelector('.glossary-popup');
  popup.querySelector('.glossary-title').textContent = entry.title;
  popup.querySelector('.glossary-definition').textContent = entry.definition;
  // posicionar proximo ao anchor
  const rect = anchor.getBoundingClientRect();
  popup.style.top = `${rect.bottom + window.scrollY + 8}px`;
  popup.style.left = `${rect.left + window.scrollX}px`;
  popup.hidden = false;
}

document.addEventListener('click', () => {
  document.querySelector('.glossary-popup').hidden = true;
});
```

---

## Nivel 2 — Verificacao e memorizacao

Atividades com resposta certa e errada. O objetivo e verificar compreensao e reforcar memoriz acao. Feedback imediato e obrigatorio.

---

### Questionario de multipla escolha

**Quando usar:** Verificar compreensao de conceitos, regras, procedimentos. O formato mais universal de avaliacao formativa. Funciona bem como fixacao ao final de modulo.

**Mecanica:** Uma pergunta por vez (ou todas em sequencia). O aluno clica em uma das opcoes. Feedback imediato: cor verde (correto) ou vermelho (incorreto) + explicacao. Ao final, tela de resultado com pontuacao.

**Estrutura de dados:**
```js
const questions = [
  {
    id: 1,
    text: 'Qual e a distancia recomendada entre os olhos e o monitor?',
    options: [
      { text: '20 a 30 cm', correct: false },
      { text: '50 a 70 cm', correct: true },
      { text: '80 a 100 cm', correct: false },
      { text: 'Nao ha distancia padrao', correct: false },
    ],
    explanation: 'A NR-17 recomenda 50 a 70cm para reduzir fadiga visual e tensao cervical.'
  },
];
```

**HTML:**
```html
<div class="quiz-container">
  <div class="quiz-progress">
    <span id="question-count">Pergunta 1 de 5</span>
    <div class="progress-bar"><div class="progress-fill" style="width: 20%"></div></div>
  </div>

  <div class="quiz-question">
    <p id="question-text"></p>
  </div>

  <div class="quiz-options" id="options-container">
    <!-- opcoes geradas por JS -->
  </div>

  <div class="quiz-feedback" id="feedback" hidden>
    <p id="feedback-text"></p>
    <p id="feedback-explanation"></p>
    <button id="btn-next">Proxima pergunta</button>
  </div>
</div>
```

**JS (logica central):**
```js
let currentIndex = 0;
let score = 0;

function loadQuestion() {
  const q = questions[currentIndex];
  document.getElementById('question-text').textContent = q.text;
  const container = document.getElementById('options-container');
  container.innerHTML = '';
  q.options.forEach((opt, i) => {
    const btn = document.createElement('button');
    btn.className = 'quiz-option';
    btn.textContent = opt.text;
    btn.addEventListener('click', () => selectOption(opt, btn));
    container.appendChild(btn);
  });
}

function selectOption(opt, btn) {
  document.querySelectorAll('.quiz-option').forEach(b => b.disabled = true);
  const isCorrect = opt.correct;
  if (isCorrect) score++;
  btn.classList.add(isCorrect ? 'correct' : 'incorrect');
  if (!isCorrect) {
    document.querySelector('.quiz-option[data-correct="true"]')?.classList.add('correct');
  }
  // mostrar feedback
  const feedback = document.getElementById('feedback');
  document.getElementById('feedback-text').textContent = isCorrect ? 'Correto!' : 'Incorreto.';
  document.getElementById('feedback-explanation').textContent = questions[currentIndex].explanation;
  feedback.hidden = false;
}

document.getElementById('btn-next').addEventListener('click', () => {
  currentIndex++;
  if (currentIndex < questions.length) loadQuestion();
  else showResults();
});
```

**Variacoes:**
- Modo cronometro: timer por questao, 5 segundos extras por acerto
- Embaralhar ordem das opcoes a cada carregamento
- Permitir navegacao entre questoes (anterior/proxima)
- Tela de resultados com revisao de cada resposta e fonte

---

### Verdadeiro ou Falso

**Quando usar:** Versao simplificada do multipla escolha. Excelente para desmistificar mitos, checar premissas e introduzir topicos controversos. Funciona bem em "Antes de comecar" e "Mitos e Fatos".

**Mecanica:** Uma afirmacao e apresentada. O aluno escolhe VERDADEIRO ou FALSO. Feedback com explicacao. Pode ter botao "Mostrar fontes".

**HTML:**
```html
<div class="vf-card">
  <p class="vf-statement">"Caminhar durante reunioes nao e permitido pelo codigo de conduta."</p>
  <div class="vf-options">
    <button class="vf-btn" data-answer="false">Verdadeiro</button>
    <button class="vf-btn" data-answer="true">Falso</button>
  </div>
  <div class="vf-feedback" hidden>
    <span class="vf-status"></span>
    <p class="vf-explanation"></p>
    <button class="vf-sources-btn">Mostrar fontes</button>
    <div class="vf-sources" hidden>
      <a href="#" target="_blank">Codigo de Conduta Interno - Secao 4.2</a>
    </div>
  </div>
</div>
```

**Logica:** Identica ao multipla escolha, mas com apenas dois botoes. O botao com `data-answer="true"` indica que a afirmacao e falsa (ou seja, a resposta correta para "Falso" e `data-answer="true"` quando `isFake = true`). Prefira `data-correct="true/false"` direto nos botoes para evitar inversao logica confusa.

---

### Flashcards (baralho de estudo)

**Quando usar:** Memorizacao de pares conceito/definicao, termo/exemplo, pergunta/resposta. Ideal para vocabulario, normas, siglas, processos. Funciona como revisao autonoma ao final de modulo.

**Mecanica:**
1. O aluno ve uma grade de modulos (ou decks tematicos) e escolhe um.
2. O primeiro cartao aparece com a frente visivel.
3. Clicar gira o cartao (flip 3D) e revela o verso.
4. Botoes "Anterior" e "Proximo" navegam pelo baralho.
5. Botao "Embaralhar" reordena os cartoes.
6. Contador e barra de progresso mostram posicao.

**Estrutura de dados (JSON externo ou inline):**
```json
{
  "decks": [
    {
      "id": "m1",
      "title": "Modulo 1 — Ergonomia",
      "cards": [
        { "front": "NR-17", "back": "Norma Regulamentadora sobre Ergonomia. Define parametros para adaptar o trabalho ao ser humano." },
        { "front": "LER", "back": "Lesao por Esforco Repetitivo. Afeta musculos, tendoes e nervos por movimentos repetitivos." }
      ]
    }
  ]
}
```

**HTML:**
```html
<!-- Seletor de decks -->
<div class="flashcard-deck-grid" id="deck-grid">
  <button class="deck-card" data-deck="m1">Modulo 1<br><small>12 cartoes</small></button>
</div>

<!-- Area de jogo (oculta ate selecionar deck) -->
<div class="flashcard-play" id="play-area" hidden>
  <div class="flip-card" id="flashcard">
    <div class="flip-card-inner">
      <div class="flip-card-front"><p id="card-front"></p></div>
      <div class="flip-card-back"><p id="card-back"></p></div>
    </div>
  </div>
  <p class="card-counter"><span id="current">1</span> de <span id="total"></span></p>
  <div class="progress-bar"><div id="progress-fill" class="progress-fill"></div></div>
  <div class="flashcard-controls">
    <button id="btn-prev" disabled>Anterior</button>
    <button id="btn-shuffle">Embaralhar</button>
    <button id="btn-next">Proximo</button>
  </div>
  <button id="btn-back-decks">Voltar aos modulos</button>
</div>
```

**JS (logica central):**
```js
let currentDeck = [];
let currentIndex = 0;

function loadDeck(deckId) {
  currentDeck = decks.find(d => d.id === deckId).cards;
  currentIndex = 0;
  document.getElementById('deck-grid').hidden = true;
  document.getElementById('play-area').hidden = false;
  renderCard();
}

function renderCard() {
  const card = currentDeck[currentIndex];
  document.getElementById('card-front').textContent = card.front;
  document.getElementById('card-back').textContent = card.back;
  document.getElementById('flashcard').classList.remove('flipped');
  document.getElementById('current').textContent = currentIndex + 1;
  document.getElementById('total').textContent = currentDeck.length;
  document.getElementById('progress-fill').style.width = `${((currentIndex + 1) / currentDeck.length) * 100}%`;
  document.getElementById('btn-prev').disabled = currentIndex === 0;
  document.getElementById('btn-next').disabled = currentIndex === currentDeck.length - 1;
}

document.getElementById('btn-shuffle').addEventListener('click', () => {
  currentDeck = [...currentDeck].sort(() => Math.random() - 0.5);
  currentIndex = 0;
  renderCard();
});
```

---

## Nivel 3 — Classificacao e organizacao

Atividades onde o aluno manipula elementos para categoriza-los, ordena-los ou associa-los. O ato de mover objetos reforça a aprendizagem de forma mais fisica.

---

### Drag-and-drop: associacao (item → categoria)

**Quando usar:** Associar conceitos a categorias, equipamentos a funcoes, etapas a processos. Mais engajante que multipla escolha quando a tarefa e de classificacao.

**Mecanica:** Coluna de itens arrastavel a esquerda. Zonas de destino a direita (uma por categoria). O aluno arrasta cada item para a zona correspondente. Feedback ao soltar: item fica na zona (correto) ou retorna a origem (incorreto). Contador de acertos visivel.

**HTML:**
```html
<div class="dnd-layout">
  <div class="dnd-source" id="items-column">
    <div class="dnd-item" draggable="true" data-item="monitor">Configurar altura do monitor</div>
    <div class="dnd-item" draggable="true" data-item="pausa">Fazer pausa a cada hora</div>
    <div class="dnd-item" draggable="true" data-item="cadeira">Ajustar encosto lombar</div>
  </div>
  <div class="dnd-targets">
    <div class="dnd-zone" data-accepts="monitor">
      <p class="zone-label">Monitor</p>
    </div>
    <div class="dnd-zone" data-accepts="pausa">
      <p class="zone-label">Habito Diario</p>
    </div>
    <div class="dnd-zone" data-accepts="cadeira">
      <p class="zone-label">Cadeira</p>
    </div>
  </div>
</div>
<p class="dnd-score">Acertos: <span id="score">0</span>/3</p>
```

**JS:**
```js
let score = 0;
let draggedItem = null;

document.querySelectorAll('.dnd-item').forEach(item => {
  item.addEventListener('dragstart', e => {
    draggedItem = item;
    e.dataTransfer.setData('text/plain', item.dataset.item);
  });
});

document.querySelectorAll('.dnd-zone').forEach(zone => {
  zone.addEventListener('dragover', e => e.preventDefault());
  zone.addEventListener('drop', e => {
    e.preventDefault();
    const itemId = e.dataTransfer.getData('text/plain');
    const isCorrect = zone.dataset.accepts === itemId;
    if (isCorrect) {
      zone.appendChild(draggedItem);
      draggedItem.draggable = false;
      draggedItem.classList.add('dropped');
      score++;
      document.getElementById('score').textContent = score;
    } else {
      draggedItem.classList.add('shake'); // animacao de erro
      setTimeout(() => draggedItem.classList.remove('shake'), 500);
    }
    draggedItem = null;
  });
});
```

**Variacoes:**
- Classificacao em colunas VERDADEIRO/FALSO (dois baldes)
- Associacao de pares (dois grupos de itens que precisam ser conectados)
- Sem feedback imediato: aluno monta tudo e clica "Verificar"

---

### Drag-and-drop: ordenacao (sequencia cronologica ou logica)

**Quando usar:** Ordenar etapas de um processo, eventos em ordem cronologica, prioridades. Reforça compreensao de fluxo e dependencia entre elementos.

**Mecanica:** Lista de itens embaralhada. O aluno arrasta para reordenar. Ao clicar "Verificar", o sistema compara com a ordem correta e destaca acertos/erros.

**HTML:**
```html
<ol class="sortable-list" id="sortable">
  <li class="sortable-item" draggable="true" data-order="3">Publicar o conteudo</li>
  <li class="sortable-item" draggable="true" data-order="1">Definir objetivos pedagogicos</li>
  <li class="sortable-item" draggable="true" data-order="2">Produzir o material</li>
</ol>
<button id="btn-check">Verificar ordem</button>
```

**JS (logica de reordenacao):**
```js
let dragging = null;

document.querySelectorAll('.sortable-item').forEach(item => {
  item.addEventListener('dragstart', () => dragging = item);
  item.addEventListener('dragover', e => {
    e.preventDefault();
    const target = e.currentTarget;
    const list = target.parentNode;
    const rect = target.getBoundingClientRect();
    const mid = rect.top + rect.height / 2;
    if (e.clientY < mid) list.insertBefore(dragging, target);
    else list.insertBefore(dragging, target.nextSibling);
  });
});

document.getElementById('btn-check').addEventListener('click', () => {
  const items = [...document.querySelectorAll('.sortable-item')];
  items.forEach((item, index) => {
    const isCorrect = parseInt(item.dataset.order) === index + 1;
    item.classList.toggle('correct', isCorrect);
    item.classList.toggle('incorrect', !isCorrect);
  });
});
```

---

## Nivel 4 — Analise e diagnostico

Atividades onde o aluno examina um cenario, documento ou conjunto de dados e identifica elementos especificos. O objetivo e desenvolver olhar critico e capacidade de analise.

---

### Identificacao de elementos em documento (leitura critica)

**Quando usar:** Treinar leitura analitica em contexto real. O aluno ve um documento (noticia, relatorio, contrato, imagem) e precisa identificar elementos especificos: fatos vs opiniao, erros, informacoes criticas, sinais de alerta.

**Mecanica:** O documento e exibido. Abaixo (ou ao lado), uma lista de elementos marcaveis. O aluno seleciona os que identifica no documento. Sistema verifica quais estao corretos. Pode ter botao "Analisar" ao final ou feedback item a item.

**Exemplo do OlharCritico:** Leitura de noticia e classificacao de cada trecho como "fato verificavel", "opiniao", "dado sem fonte" ou "afirmacao enganosa".

**HTML:**
```html
<div class="document-analysis">
  <div class="document-panel">
    <div class="document-content">
      <!-- conteudo real: texto, imagem ou iframe -->
      <article id="doc-text">
        <h2>Vacinas causam autismo, diz estudo</h2>
        <p>"Segundo pesquisadores anonimos, a vacina MMR estaria associada..."</p>
      </article>
    </div>
  </div>
  <div class="analysis-panel">
    <p class="analysis-instruction">Marque todos os sinais que indicam que esta noticia pode ser falsa:</p>
    <ul class="checklist" id="checklist">
      <li>
        <label><input type="checkbox" data-correct="true"> Fonte anonima ("pesquisadores anonimos")</label>
      </li>
      <li>
        <label><input type="checkbox" data-correct="false"> Titulo em letras maiusculas</label>
      </li>
      <li>
        <label><input type="checkbox" data-correct="true"> Ausencia de link para o estudo citado</label>
      </li>
    </ul>
    <button id="btn-analyze">Analisar</button>
    <div id="analysis-result" hidden></div>
  </div>
</div>
```

**JS:**
```js
document.getElementById('btn-analyze').addEventListener('click', () => {
  const items = document.querySelectorAll('.checklist input[type="checkbox"]');
  let correct = 0, total = 0;
  items.forEach(cb => {
    const shouldBeChecked = cb.dataset.correct === 'true';
    const isChecked = cb.checked;
    const isRight = shouldBeChecked === isChecked;
    if (isRight) correct++;
    total++;
    cb.parentElement.classList.toggle('item-correct', isRight);
    cb.parentElement.classList.toggle('item-incorrect', !isRight);
  });
  const result = document.getElementById('analysis-result');
  result.hidden = false;
  result.innerHTML = `<p>Voce acertou ${correct} de ${total} indicadores.</p>`;
});
```

**Variacoes:**
- Highlight interativo: o aluno clica em trechos do texto para marcar (como sublinhar)
- Identificar elementos em imagem: hotspots clicaveis sobre a imagem
- Comparar dois documentos lado a lado

---

### Diagnostico por formulario (avaliacao de cenario)

**Quando usar:** Simular uma avaliacao tecnica. O aluno examina um cenario e preenche uma ficha de diagnostico com julgamentos. Ao final, o sistema gera um laudo personalizado baseado nas respostas.

**Mecanica:** Conjunto de itens de avaliacao, cada um com opcoes de estado (ex: Bom / Precisa atencao / Ruim). O aluno avalia cada item. Ao clicar "Analisar", um modal mostra o laudo: icone de situacao geral, lista de problemas identificados, sugestoes de acao.

**Exemplo do Zaffari NR17:** Avaliacao de posto de trabalho — cadeira, monitor, teclado e headset cada um avaliado como Bom/Atencao/Ruim, gerando laudo ergonomico personalizado.

**HTML:**
```html
<form class="diagnosis-form" id="diagnosis-form">
  <div class="diagnosis-item">
    <p class="item-label">Cadeira</p>
    <div class="item-options">
      <label><input type="radio" name="cadeira" value="good"> Boa</label>
      <label><input type="radio" name="cadeira" value="warning"> Precisa atencao</label>
      <label><input type="radio" name="cadeira" value="bad"> Ruim</label>
    </div>
  </div>
  <!-- repetir para cada item -->
</form>
<button id="btn-diagnose">Gerar diagnostico</button>

<div class="diagnosis-modal" id="diagnosis-modal" hidden role="dialog">
  <div class="modal-content">
    <span class="modal-icon" id="diag-icon"></span>
    <h2 id="diag-title"></h2>
    <p id="diag-summary"></p>
    <ul id="diag-issues"></ul>
    <div id="diag-actions"></div>
    <button id="btn-close-modal">Fechar</button>
  </div>
</div>
```

**JS:**
```js
const items = {
  cadeira: {
    label: 'Cadeira',
    issues: {
      warning: 'A cadeira precisa de ajuste no encosto ou altura.',
      bad: 'A cadeira nao atende aos requisitos ergonomicos da NR-17.'
    }
  },
  // outros itens...
};

document.getElementById('btn-diagnose').addEventListener('click', () => {
  let bad = 0, warning = 0, issues = [];

  Object.keys(items).forEach(key => {
    const val = document.querySelector(`input[name="${key}"]:checked`)?.value;
    if (!val) return; // nao avaliado
    if (val === 'bad') { bad++; issues.push(items[key].issues.bad); }
    if (val === 'warning') { warning++; issues.push(items[key].issues.warning); }
  });

  const icon = bad > 1 ? '⚠️' : warning > 0 ? '💛' : '🌟';
  const title = bad > 1 ? 'Atencao necessaria' : warning > 0 ? 'Quase la' : 'Excelente!';

  document.getElementById('diag-icon').textContent = icon;
  document.getElementById('diag-title').textContent = title;
  document.getElementById('diag-issues').innerHTML = issues.map(i => `<li>${i}</li>`).join('');
  document.getElementById('diagnosis-modal').hidden = false;
});
```

---

### Dashboard de dados interativo

**Quando usar:** Explorar conjuntos de dados de forma ativa. O aluno filtra, seleciona e ve como os dados mudam. Desenvolve literacia de dados e pensamento analitico.

**Mecanica:** Botoes de filtro por categoria. Checkboxes para selecionar topicos dentro da categoria. Grafico (Chart.js ou similar) atualiza em tempo real conforme selecoes.

**Estrutura de dados:**
```js
const data = {
  saude: {
    label: 'Saude',
    topics: {
      vacinas: { label: 'Vacinas', mentions: [120, 340, 80] },
      cancer:  { label: 'Cancer',  mentions: [60,  200, 40] },
    }
  },
  // outras categorias...
};
```

**HTML:**
```html
<div class="dashboard">
  <div class="area-selector">
    <button class="area-btn active" data-area="saude">Saude</button>
    <button class="area-btn" data-area="politica">Politica</button>
  </div>
  <div class="topic-selector" id="topic-selector">
    <!-- checkboxes gerados por JS conforme area selecionada -->
  </div>
  <div class="chart-panel">
    <canvas id="main-chart"></canvas>
  </div>
</div>
```

**JS (logica central):**
```js
let chart = null;
let activeArea = 'saude';

function renderTopics(area) {
  const container = document.getElementById('topic-selector');
  container.innerHTML = '';
  Object.entries(data[area].topics).forEach(([key, topic]) => {
    const label = document.createElement('label');
    label.innerHTML = `<input type="checkbox" data-topic="${key}" checked> ${topic.label}`;
    label.querySelector('input').addEventListener('change', updateChart);
    container.appendChild(label);
  });
  updateChart();
}

function updateChart() {
  const selectedTopics = [...document.querySelectorAll('#topic-selector input:checked')]
    .map(cb => cb.dataset.topic);
  const datasets = selectedTopics.map(key => ({
    label: data[activeArea].topics[key].label,
    data: data[activeArea].topics[key].mentions,
  }));
  if (chart) chart.destroy();
  chart = new Chart(document.getElementById('main-chart'), {
    type: 'bar',
    data: { labels: ['2022', '2023', '2024'], datasets },
  });
}

document.querySelectorAll('.area-btn').forEach(btn => {
  btn.addEventListener('click', () => {
    activeArea = btn.dataset.area;
    document.querySelectorAll('.area-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    renderTopics(activeArea);
  });
});
```

---

## Nivel 5 — Simulacao e aplicacao

Atividades que colocam o aluno em um contexto realista e exigem tomada de decisao. O objetivo e transferencia: aplicar o que aprendeu em situacao proxima da real.

---

### Simulador de cenarios com personagem

**Quando usar:** Mostrar situacoes reais do cotidiano profissional. O aluno ve um ambiente, ouve um personagem descrever um problema e precisa identificar o que esta errado ou qual acao tomar. Ideal para temas de compliance, segurança, atendimento e ergonomia.

**Mecanica:**
1. Tela de introducao com instrucoes e botao "Iniciar".
2. Para cada cenario: imagem do ambiente + personagem em overlay + balao de dialogo descrevendo a situacao.
3. Barra de progresso visual mostrando posicao na sequencia.
4. Pergunta de analise com 4 opcoes.
5. Modal de feedback (correto/incorreto + explicacao).
6. Botao "Proximo cenario".
7. Tela final com pontuacao e botao "Tentar novamente".

**Estrutura de dados:**
```js
const scenarios = [
  {
    id: 1,
    image: 'assets/img/cenario-callcenter.jpg',
    character: { name: 'Ana', avatar: 'assets/img/ana.png' },
    dialogue: 'Trabalho 6 horas seguidas sem pausas. Ultimamente sinto muita tensao no pescoco.',
    question: 'O que esta causando o problema de Ana?',
    options: [
      { text: 'A cadeira esta muito alta', correct: false },
      { text: 'Ausencia de pausas regulares', correct: true },
      { text: 'O monitor e muito luminoso', correct: false },
      { text: 'Ela precisa de oculos', correct: false },
    ],
    feedback: {
      correct: 'Correto! Pausas de 5-10 min a cada hora sao obrigatorias pela NR-17.',
      incorrect: 'Analise o dialogo novamente: Ana menciona trabalhar sem pausa por 6 horas.'
    }
  },
];
```

**HTML (estrutura das telas):**
```html
<!-- Tela de introducao -->
<section id="screen-intro" class="screen active">
  <h2>Diagnostico Ergonomico</h2>
  <p>Voce vai analisar 5 situacoes de trabalho e identificar o problema ergonomico.</p>
  <button id="btn-start">Iniciar</button>
</section>

<!-- Tela de cenario -->
<section id="screen-scenario" class="screen" hidden>
  <div class="progress-steps" id="progress-steps">
    <!-- dots gerados por JS -->
  </div>
  <div class="scenario-image-wrapper">
    <img id="scenario-img" src="" alt="">
    <div class="character-overlay">
      <img id="char-avatar" src="" alt="">
      <div class="dialogue-bubble" id="dialogue"></div>
    </div>
  </div>
  <div class="scenario-question">
    <p id="question-text"></p>
    <div class="scenario-options" id="options"></div>
  </div>
</section>

<!-- Modal de feedback -->
<div class="feedback-overlay" id="feedback-overlay" hidden>
  <div class="feedback-modal">
    <span class="feedback-icon" id="feedback-icon"></span>
    <p id="feedback-text"></p>
    <button id="btn-next-scenario">Proximo cenario</button>
  </div>
</div>

<!-- Tela de resultado -->
<section id="screen-result" class="screen" hidden>
  <h2 id="result-title"></h2>
  <p id="result-score"></p>
  <button id="btn-retry">Tentar novamente</button>
</section>
```

**JS (logica central):**
```js
let currentIndex = 0;
let score = 0;

function loadScenario(index) {
  const s = scenarios[index];
  document.getElementById('scenario-img').src = s.image;
  document.getElementById('char-avatar').src = s.character.avatar;
  document.getElementById('dialogue').textContent = s.dialogue;
  document.getElementById('question-text').textContent = s.question;
  // renderizar opcoes
  const optContainer = document.getElementById('options');
  optContainer.innerHTML = '';
  s.options.forEach(opt => {
    const btn = document.createElement('button');
    btn.className = 'scenario-option';
    btn.textContent = opt.text;
    btn.addEventListener('click', () => evaluateAnswer(opt, s.feedback));
    optContainer.appendChild(btn);
  });
  updateProgress(index);
}

function evaluateAnswer(opt, feedback) {
  document.querySelectorAll('.scenario-option').forEach(b => b.disabled = true);
  if (opt.correct) score++;
  document.getElementById('feedback-icon').textContent = opt.correct ? '✅' : '❌';
  document.getElementById('feedback-text').textContent = opt.correct ? feedback.correct : feedback.incorrect;
  document.getElementById('feedback-overlay').hidden = false;
}

document.getElementById('btn-next-scenario').addEventListener('click', () => {
  document.getElementById('feedback-overlay').hidden = true;
  currentIndex++;
  if (currentIndex < scenarios.length) loadScenario(currentIndex);
  else showResults();
});

function showResults() {
  document.getElementById('screen-scenario').hidden = true;
  const screen = document.getElementById('screen-result');
  screen.hidden = false;
  document.getElementById('result-score').textContent = `Voce acertou ${score} de ${scenarios.length} cenarios.`;
}
```

---

### Guia de pratica com timer (exercicios, alongamentos, rotinas)

**Quando usar:** Guiar o aluno por uma sequencia de acoes praticas com tempo definido. Funciona para alongamentos, exercicios, tecnicas de foco, simulacoes de procedimento com tempo limite.

**Mecanica:**
1. Selecao de modo (ex: Rapido / Normal / Completo) com diferente numero de exercicios e duracao total.
2. Tela "Prepare-se" com contagem regressiva de 3 segundos.
3. Para cada exercicio: nome, instrucoes, imagem ou icone demonstrativo, timer visual regressivo.
4. Ao terminar o tempo: avanca automaticamente (ou botao "Proximo").
5. Tela final com resumo dos exercicios completados.
6. Opcao de ativar/desativar sons.

**Estrutura de dados:**
```js
const exercises = {
  rapido: [
    { id: 1, name: 'Rotacao do pescoco', icon: '🔄', instructions: 'Gire a cabeca lentamente para a direita. Segure 5 segundos. Repita para a esquerda.', duration: 30 },
    { id: 2, name: 'Alongamento de ombros', icon: '💪', instructions: 'Eleve os ombros ao maximo. Segure 3 segundos. Solte. Repita 5 vezes.', duration: 40 },
  ],
  normal: [ /* mesmos + 3 extras */ ],
};
```

**HTML:**
```html
<!-- Selecao de modo -->
<section id="screen-mode" class="screen active">
  <div class="mode-cards">
    <button class="mode-card" data-mode="rapido">⚡ Rapido<br><small>2 min · 3 exercicios</small></button>
    <button class="mode-card selected" data-mode="normal">🎯 Normal<br><small>3 min · 5 exercicios</small></button>
    <button class="mode-card" data-mode="completo">🧘 Completo<br><small>5 min · 8 exercicios</small></button>
  </div>
  <label class="sound-toggle"><input type="checkbox" id="sound-toggle" checked> 🔊 Sons</label>
  <button id="btn-start-session">Comecar</button>
</section>

<!-- Exercicio -->
<section id="screen-exercise" class="screen" hidden>
  <p class="exercise-counter">Exercicio <span id="ex-current">1</span> de <span id="ex-total"></span></p>
  <span class="exercise-icon" id="ex-icon"></span>
  <h3 id="ex-name"></h3>
  <p id="ex-instructions"></p>
  <div class="timer-ring">
    <svg viewBox="0 0 100 100">
      <circle class="timer-bg" cx="50" cy="50" r="45"/>
      <circle class="timer-progress" id="timer-circle" cx="50" cy="50" r="45"/>
    </svg>
    <span class="timer-number" id="timer-display"></span>
  </div>
  <button id="btn-next-exercise">Proximo</button>
</section>
```

**JS (logica de timer):**
```js
let timerInterval = null;
let timeLeft = 0;

function startExercise(exercise) {
  timeLeft = exercise.duration;
  document.getElementById('ex-icon').textContent = exercise.icon;
  document.getElementById('ex-name').textContent = exercise.name;
  document.getElementById('ex-instructions').textContent = exercise.instructions;
  updateTimerDisplay();
  timerInterval = setInterval(() => {
    timeLeft--;
    updateTimerDisplay();
    if (timeLeft <= 0) {
      clearInterval(timerInterval);
      autoAdvance();
    }
  }, 1000);
}

function updateTimerDisplay() {
  document.getElementById('timer-display').textContent = timeLeft;
  // atualizar stroke-dashoffset do SVG para o anel animado
  const circle = document.getElementById('timer-circle');
  const circumference = 2 * Math.PI * 45;
  const offset = circumference * (1 - timeLeft / currentExercise.duration);
  circle.style.strokeDashoffset = offset;
}
```

---

## Elementos de engajamento e sistema

Componentes que atravessam multiplas atividades e dao coerencia ao percurso do aluno.

---

### Sistema de gamificacao com badges

**Quando usar:** Cursos com multiplos modulos onde o progresso precisa ser tangivel. Badges desbloqueam acesso a atividades extras, criando motivacao por completude.

**Mecanica:**
- Cada modulo tem um conjunto de badges (ex: resumo, video, quiz, flashcard, drag-drop).
- Badge e concedido ao concluir a atividade correspondente.
- Progresso salvo em `localStorage`.
- Tela de extras mostra cards bloqueados/desbloqueados conforme badges conquistadas.
- Alguns extras exigem completar modulo especifico, outros exigem N modulos completos.

**Estrutura no localStorage:**
```js
// exemplo de estado salvo
{
  "badges": {
    "m1": { "resumo": true, "video": true, "quiz": false, "flashcard": true },
    "m2": { "resumo": true, "video": false, "quiz": false, "flashcard": false }
  }
}
```

**JS (logica central):**
```js
const GamificationManager = {
  addBadge(moduleId, badgeType) {
    const data = JSON.parse(localStorage.getItem('badges') || '{}');
    if (!data[moduleId]) data[moduleId] = {};
    data[moduleId][badgeType] = true;
    localStorage.setItem('badges', JSON.stringify(data));
    this.updateUI();
  },

  hasBadge(moduleId, badgeType) {
    const data = JSON.parse(localStorage.getItem('badges') || '{}');
    return data[moduleId]?.[badgeType] === true;
  },

  getCompletedModulesCount() {
    const data = JSON.parse(localStorage.getItem('badges') || '{}');
    return Object.keys(data).filter(mod => {
      const badges = data[mod];
      return Object.values(badges).filter(Boolean).length >= 3; // 3+ badges = modulo completo
    }).length;
  },

  updateUI() {
    document.querySelectorAll('.extra-card').forEach(card => {
      const required = card.dataset.requiredModule;
      const minModules = parseInt(card.dataset.minModules || '0');
      let unlocked = false;
      if (required) unlocked = this.hasBadge(required, 'quiz');
      else if (minModules) unlocked = this.getCompletedModulesCount() >= minModules;
      card.classList.toggle('locked', !unlocked);
      card.classList.toggle('unlocked', unlocked);
    });
  }
};
```

**HTML (card de extra):**
```html
<div class="extra-card locked" data-required-module="m1">
  <span class="badge-icon">🃏</span>
  <h3>Flashcards</h3>
  <p>Complete o Modulo 1 para desbloquear.</p>
</div>
```

---

### Barra de progresso do modulo

**Quando usar:** Em qualquer curso com sequencia de telas/paginas. Mostra ao aluno onde ele esta e quanto falta. Reduz ansiedade e aumenta sensacao de controle.

**Mecanica:** Barra horizontal ou pontos numerados. Avanca conforme o aluno navega. Pode ser linear (so avancar) ou livre (pode voltar).

**HTML (pontos numerados):**
```html
<nav class="progress-steps" aria-label="Progresso">
  <span class="step completed" aria-label="Passo 1 concluido">1</span>
  <span class="step completed" aria-label="Passo 2 concluido">2</span>
  <span class="step active" aria-current="step" aria-label="Passo 3 atual">3</span>
  <span class="step" aria-label="Passo 4">4</span>
  <span class="step" aria-label="Passo 5">5</span>
</nav>
```

**JS:**
```js
function updateProgress(currentStep, totalSteps) {
  document.querySelectorAll('.step').forEach((step, i) => {
    step.classList.toggle('completed', i < currentStep);
    step.classList.toggle('active', i === currentStep);
  });
}
```

---

### Sistema de feedback visual e sonoro

**Quando usar:** Em toda atividade com avaliacao. Feedback imediato e obrigatorio. A combinacao de cor + texto + som cria resposta multissensorial que reforca a aprendizagem.

**Regras:**
- Correto: verde + mensagem positiva + som de acerto (tom agudo curto)
- Incorreto: vermelho + mensagem explicativa + animacao shake + som de erro (tom grave curto)
- Aviso: amarelo/laranja + mensagem de orientacao
- Concluido: confetti ou particulas + som de conquista

**JS (gerenciador de feedback):**
```js
const FeedbackManager = {
  show(element, type, message) {
    const map = { correct: '✅', incorrect: '❌', warning: '⚠️', complete: '🎉' };
    const toast = document.createElement('div');
    toast.className = `feedback-toast feedback-${type}`;
    toast.textContent = message;
    document.body.appendChild(toast);
    setTimeout(() => toast.remove(), 3000);

    if (type === 'incorrect' && element) {
      element.classList.add('shake');
      setTimeout(() => element.classList.remove('shake'), 500);
    }
  },

  playSound(type) {
    if (!this.soundEnabled) return;
    const ctx = new AudioContext();
    const osc = ctx.createOscillator();
    const gain = ctx.createGain();
    osc.connect(gain);
    gain.connect(ctx.destination);
    const freq = type === 'correct' ? 880 : type === 'incorrect' ? 220 : 440;
    osc.frequency.value = freq;
    gain.gain.setValueAtTime(0.3, ctx.currentTime);
    gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.4);
    osc.start();
    osc.stop(ctx.currentTime + 0.4);
  }
};
```

---

## Notas gerais de implementacao

- **Salvar progresso no localStorage** e indispensavel em cursos com multiplas sessoes. Use uma chave por curso (`courseId_progress`) para evitar conflito.
- **Feedback sempre antes de avancar.** Nunca pular direto para a proxima questao — o aluno precisa processar o resultado.
- **Dados de atividade em arquivo JSON externo** (ou objeto JS separado) facilita manutencao e permite que o conteudo seja editado sem tocar na logica.
- **Acessibilidade:** todo elemento interativo precisa de `tabindex`, `role` e `aria-label` adequados. Atividades de drag-and-drop devem ter alternativa de teclado.
- **prefers-reduced-motion:** animacoes de feedback (shake, bounce, confetti) devem ser desativadas quando o usuario optou por reducao de movimento.
- **Reset e retry:** sempre oferecer botao para reiniciar a atividade, limpando estado e pontuacao.

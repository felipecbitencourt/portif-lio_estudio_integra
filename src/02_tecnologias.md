# Pilar 2 - Tecnologias

## Proposito do pilar

O pilar de tecnologias sustenta o desenvolvimento tecnico dos materiais educacionais interativos com foco em compatibilidade, desempenho e manutencao evolutiva. A proposta e garantir que os cursos funcionem de forma confiavel em diferentes LMS, navegadores, sistemas operacionais e dispositivos.

## Como aplicamos na pratica

### Stack base: HTML, CSS e JavaScript vanilla

Construimos interfaces educacionais sobre a tríade web padrao. A decisao de nao depender de frameworks (React, Vue, Angular) e deliberada: reduz atrito de manutencao, elimina dependencias que ficam desatualizadas e garante compatibilidade com qualquer ambiente, incluindo LMS corporativos com restricoes de rede.

HTML semantico, CSS com variaveis nativas e JavaScript modular permitem entregar todo o nivel de interatividade que cursos educacionais precisam sem overhead de bundle ou configuracao de build.

**Quando usamos React:** Em projetos com complexidade de estado significativa — multiplos componentes compartilhando dados, reuso intensivo de interface ou integracao com APIs externas. A decisao e orientada por necessidade real, nao por convencao.

### Arquitetura JavaScript modular (sem bundler)

Organizamos o codigo JavaScript em modulos por responsabilidade, cada um encapsulado em um objeto com metodos proprios. Nao usamos ferramentas de build — os arquivos sao entregues diretamente ao navegador.

**Padrao de modulo tipico:**
```js
// navigation.js
const NavigationManager = {
  currentPage: null,
  init() { /* inicializa listeners */ },
  goTo(pageId) { /* troca de tela */ },
  updateProgress(step, total) { /* atualiza barra */ },
};
```

**Divisao de responsabilidades comum nos projetos:**
- `main.js` — inicializacao e orquestracao geral
- `navigation.js` — fluxo de telas, prev/next, progresso
- `interactive.js` — modais, flip cards, accordions, reveal buttons
- `gamification.js` — badges, desbloqueio de conteudo, localStorage
- `accessibility.js` — dark mode, tamanho de fonte, modo dislexia, TTS
- `i18n.js` — traducoes, troca de idioma, fallback
- `audio.js` — sons de feedback, controle de volume, mute
- `feedback.js` — toasts, animacoes de acerto/erro

### CSS com design tokens (variaveis nativas)

Usamos variaveis CSS (`--var`) como sistema de tokens de design. Isso permite que todo o visual do curso seja ajustado em um unico arquivo de configuracao — cores, espacamentos, tipografia, raios, sombras e duracoes de animacao.

```css
:root {
  --color-primary: #2563eb;
  --color-surface: #ffffff;
  --font-body: 'Inter', sans-serif;
  --radius-card: 12px;
  --shadow-card: 0 2px 8px rgba(0,0,0,0.08);
  --transition-fast: 0.15s ease;
  --transition-med: 0.3s ease;
  --ease-out-soft: cubic-bezier(0.2, 0.8, 0.2, 1);
}
```

Dark mode e implementado sobrescrevendo os tokens em `[data-theme="dark"]`, sem duplicar regras de estilo.

### Conteudo orientado a dados (JSON externo)

Separamos conteudo de logica. Dados de quiz, flashcards, cenarios, exercicios e traducoes vivem em arquivos JSON carregados via `fetch()`. O JavaScript renderiza a interface a partir desses dados.

**Beneficio:** trocar ou expandir o conteudo do curso nao exige mexer no codigo. Um professor ou editor de conteudo pode atualizar o JSON sem entender JavaScript.

**Estrutura tipica de dados:**
```json
// quiz-data.json
{
  "questions": [
    {
      "id": 1,
      "text": "Qual e a distancia recomendada para o monitor?",
      "options": [
        { "text": "20 a 30 cm", "correct": false },
        { "text": "50 a 70 cm", "correct": true }
      ],
      "explanation": "A NR-17 define 50 a 70cm para reduzir fadiga visual."
    }
  ]
}
```

### Internacionalizacao (i18n) customizada

Quando o curso precisa de multiplos idiomas, usamos um sistema proprio baseado em atributos HTML (`data-i18n`) e arquivos JSON por idioma. Nao usamos bibliotecas externas de i18n.

**Fluxo:**
1. Cada texto traduzivel recebe `data-i18n="chave.subchave"` no HTML.
2. O sistema `I18n` carrega o JSON do idioma ativo via `fetch()`.
3. A funcao `apply()` percorre todos os elementos com `data-i18n` e substitui o texto.
4. Idioma e salvo em `localStorage` e persiste entre sessoes.
5. Fallback automatico para portugues quando a chave nao existe no idioma solicitado.

**Suporte tipico:** portugues, espanhol, frances, ingles, creole haitiano.

```js
// uso no HTML
<button data-i18n="nav.next" data-i18n-aria-label="nav.next_label">Proximo</button>

// i18n.js (simplificado)
const I18n = {
  async load(lang) {
    const res = await fetch(`locales/${lang}.json`);
    this.strings = await res.json();
    this.apply();
  },
  apply() {
    document.querySelectorAll('[data-i18n]').forEach(el => {
      const key = el.dataset.i18n;
      el.textContent = this.get(key);
    });
  },
};
```

### Persistencia com localStorage

Usamos `localStorage` para salvar estado que precisa sobreviver ao fechamento do navegador. Cada tipo de dado tem uma chave de namespace para evitar conflito entre cursos.

**O que tipicamente salvamos:**
- Progresso de modulos e paginas visitadas
- Badges e conquistas de gamificacao
- Preferencias de acessibilidade (dark mode, tamanho de fonte, modo dislexia)
- Idioma selecionado
- Preferencia de som (ativado/desativado)

**Padrao de leitura/escrita segura:**
```js
function saveProgress(courseId, data) {
  localStorage.setItem(`${courseId}_progress`, JSON.stringify(data));
}
function loadProgress(courseId) {
  return JSON.parse(localStorage.getItem(`${courseId}_progress`) || 'null');
}
```

### APIs nativas do navegador

Aproveitamos APIs modernas do browser sem dependencias externas:

- **IntersectionObserver** — animar elementos ao entrar no viewport (scroll reveal), lazy loading de imagens com `data-src`
- **Drag and Drop API** — atividades de arrastar e soltar (associacao, ordenacao, classificacao)
- **Web Audio API** — sons de feedback gerados sinteticamente (osciladores + gain nodes), sem arquivos mp3. Tom agudo para acerto, tom grave para erro
- **Speech Synthesis API** — leitura em voz alta de conteudo (TTS), com controle de voz, velocidade e volume
- **Fetch API** — carregamento de JSONs de conteudo, traducoes e dados de atividades
- **localStorage / sessionStorage** — persistencia de estado e preferencias

**Exemplo de som sintetico (Web Audio API):**
```js
function playFeedbackTone(type) {
  const ctx = new AudioContext();
  const osc = ctx.createOscillator();
  const gain = ctx.createGain();
  osc.connect(gain);
  gain.connect(ctx.destination);
  osc.frequency.value = type === 'correct' ? 880 : 220;
  gain.gain.setValueAtTime(0.3, ctx.currentTime);
  gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.4);
  osc.start();
  osc.stop(ctx.currentTime + 0.4);
}
```

### Desenvolvimento SCORM 1.2

Empacotamos cursos em SCORM 1.2 para publicacao em LMS corporativos. A integracao usa a API padrao SCORM exposta pelo LMS (`window.API`) para rastrear progresso, pontuacao e status de conclusao.

**Variaveis tipicamente usadas:**
- `cmi.core.lesson_status` — `incomplete`, `completed`, `passed`, `failed`
- `cmi.core.score.raw` — pontuacao numerica (0–100)
- `cmi.suspend_data` — JSON serializado com estado detalhado (pagina atual, respostas, badges)
- `cmi.core.session_time` — tempo de sessao no formato `HH:MM:SS`

**Padrao de integracao:**
```js
const SCORM = {
  api: null,
  init() {
    this.api = window.API || window.parent?.API;
    if (this.api) this.api.LMSInitialize('');
  },
  set(key, value) {
    this.api?.LMSSetValue(key, value);
    this.api?.LMSCommit('');
  },
  complete(score) {
    this.set('cmi.core.score.raw', score);
    this.set('cmi.core.lesson_status', score >= 70 ? 'passed' : 'failed');
  },
  finish() {
    this.api?.LMSFinish('');
  },
};
```

**Estrutura de empacotamento:** pasta `dist_scorm/` com `imsmanifest.xml`, todos os assets, HTMLs e scripts prontos para upload no LMS.

### PWA e funcionamento offline

Em cursos que precisam funcionar sem conexao (ambiente corporativo com rede restrita, dispositivos moveis), implementamos um Service Worker com estrategias de cache diferenciadas:

- **Cache First** — CSS, JS, fontes e imagens (raramente mudam)
- **Network First** — HTMLs de pagina (conteudo pode ser atualizado)
- **Stale While Revalidate** — JSONs de dados (serve do cache e atualiza em background)

O precache cobre todos os assets criticos no momento da instalacao, garantindo que o curso funcione completamente offline apos a primeira visita.

### Acessibilidade tecnica

Implementamos acessibilidade em duas camadas:

**Camada estrutural (sempre presente):**
- HTML semantico (`<header>`, `<main>`, `<nav>`, `<article>`)
- Hierarquia de headings correta (`<h1>` → `<h2>` → `<h3>`)
- ARIA labels, roles e estados (`aria-label`, `aria-expanded`, `aria-current`)
- Skip-to-content link no topo da pagina
- Foco visivel em todos os elementos interativos (`:focus-visible`)
- Navegacao completa por teclado

**Camada de preferencias do usuario (controladas via menu de acessibilidade):**
- Dark mode / modo escuro
- Tamanho de fonte ajustavel (A- / padrao / A+)
- Modo dislexia (fonte OpenDyslexic ou similar)
- Modo alto contraste
- Modo escala de cinza
- Leitura em voz alta (Speech Synthesis API)
- VLibras — interpretacao em Libras via widget oficial do governo federal

Preferencias sao salvas em `localStorage` e reaplicadas a cada visita.

### Visualizacao de dados

Quando o curso inclui dados quantitativos, usamos **Chart.js** (importado via CDN) para gerar graficos interativos em canvas. A biblioteca permite:
- Graficos de barra, linha, pizza e rosca
- Tooltips personalizados
- Filtros interativos que destroem e recriam o grafico com novos dados
- Escalas lineares e logaritmicas

Dados para os graficos sao definidos em objetos JavaScript (ou carregados via JSON) separados da logica de renderizacao.

### Python em etapas de apoio

Utilizamos Python em rotinas de apoio ao desenvolvimento: processamento de dados para popular JSONs de conteudo, automacao de tarefas repetitivas, validacoes de estrutura de arquivos e geracao de artefatos tecnicos quando necessario.

### Compatibilidade entre sistemas e dispositivos

Projetamos para funcionamento consistente em:

- Windows, macOS, Android e iOS
- Navegadores modernos: Chrome, Edge, Firefox, Safari
- Desktop, tablet e smartphone

A estrategia inclui layouts responsivos com CSS Grid e Flexbox, navegacao por touch em dispositivos moveis, testes de comportamento em diferentes resolucoes e fallback para variacoes de ambiente corporativo (LMS com restricoes de iframe, politicas de CSP, versoes antigas de navegador corporativo).


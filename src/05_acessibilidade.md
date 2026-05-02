# Pilar 5 - Acessibilidade: Guia de Implementação para Projetos Web

Este documento serve como a base de conhecimento e orientação técnica para a implementação de acessibilidade em nossos projetos. O objetivo é que qualquer IA ou desenvolvedor possa entender **como** aplicar os recursos, utilizando o projeto "Fato ou Fake" como referência primária. 

A acessibilidade é um critério inegociável. Todas as soluções devem garantir autonomia, segurança e adaptação a diferentes necessidades.

---

## 1. Estruturação Semântica e ARIA

O HTML deve ser a primeira camada de acessibilidade. Uma estrutura sólida diminui a dependência de scripts complexos para leitores de tela.

*   **Tags Semânticas (HTML5):** Use estritamente `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>` e `<footer>`. Evite usar `<div>` para envelopar blocos de conteúdo com significado estrutural.
*   **Atributos ARIA (Accessible Rich Internet Applications):**
    *   **Imagens e ícones decorativos:** Sempre use `aria-hidden="true"` (ou `alt=""` para `<img>`) em elementos SVG e ícones (`<i class="ph..."></i>`) que não transmitem informação adicional, para que o leitor de tela os ignore.
    *   **Controles customizados:** Quando um elemento não-padrão atuar como botão (ex: uma `div` interativa), adicione `role="button"` e certifique-se de tratar os eventos de teclado (Enter/Space).
    *   **Avisos dinâmicos:** Use `aria-live="polite"` em containers (`<main>` ou áreas de feedback) cujo conteúdo muda dinamicamente sem recarregar a página.
    *   **Rótulos ocultos:** Use `aria-label` para botões que possuem apenas ícones (ex: botão de configurações, fechar modal). `data-i18n-aria` pode ser usado para internacionalizar esse rótulo.

---

## 2. Navegação por Teclado e Foco

O projeto deve ser 100% operável por teclado, não dependendo exclusivamente de interações com o mouse.

*   **Gestão de Foco Visível:** Nunca utilize `outline: none;` sem prover uma alternativa visual clara (`:focus-visible`). Modais e elementos selecionados precisam indicar claramente onde o usuário está.
*   **Acesso Direto (Skip Links):** É boa prática incluir formas de saltar direto para o `<main>`.
*   **Teclas de Atalho Customizadas:** O projeto implementa um `keyboard.js` para escutar setas (ArrowRight, ArrowLeft) para controle de paginação do curso/apresentação, disparando os mesmos métodos dos botões `#btn-next` e `#btn-prev`. Modais devem fechar ao pressionar a tecla `Escape`.

---

## 3. Ajustes de Texto e Visuais da Tela (CSS e JS)

O painel de configurações agrupa os ajustes em categorias. Eles são controlados via classes no `<body>` ou propriedades no `<html>`.

*   **Texto:**
    *   **Tamanho da Fonte:** Controlado por botões (`A-`, `100%`, `A+`). Altera dinamicamente o `font-size` da raiz (`<html>`): `document.documentElement.style.fontSize = (fontSize / 100 * 16) + "px";`. O CSS deve usar `rem`.
    *   **Fonte para Dislexia:** Um toggle adiciona a classe `.dyslexia-mode` ao body, aplicando a fonte "OpenDyslexic" e ajustando espaçamentos.
    *   **Leitura Biônica (Foco/TDAH):** Um toggle adiciona uma classe `.bionic-reading` ao body. Esta implementação requer uma função JS que percorre os nós de texto da página e aplica uma tag `<b>` ou `<strong>` na metade inicial de cada palavra para guiar os olhos e manter o foco.
    *   **Ajuste Fino de Espaçamento:** Permite que o usuário adicione mais "respiro" aumentando o espaçamento entre letras e a altura das linhas. Implementado via sliders independentes que manipulam variáveis CSS no `:root` (`--user-line-height`, `--user-letter-spacing`), alterando o layout de forma não destrutiva.
    *   **Idioma / Language (PT, EN, ES):** Seletor de idiomas (detalhado na seção de i18n abaixo).

*   **Ajustes Visuais da Tela:**
    *   **Destaque Padrão (Modo Escuro / Contraste de Interface):** Um toggle adiciona `.high-contrast` ao body. Utiliza variáveis CSS para alterar cores de fundo e texto (`--bg-color`, `--text-color`).
    *   **Escala de Cinza:** Adiciona `.img-grayscale` ao body, aplicando `filter: grayscale(100%)` globalmente ou em imagens.
    *   **Alto Contraste (Imagens):** Adiciona `.img-high-contrast` ao body, aplicando `filter: contrast(150%) brightness(80%)` para melhorar a visibilidade de mídias.
    *   **Filtros para Daltonismo (Colorblind Modes):** Aplica matrizes de filtros SVG no container principal para compensar a discromatopsia (Protanopia, Deuteranopia, Tritanopia), garantindo que botões e gráficos de alerta fiquem distinguíveis.
    *   **Destaque de Links (Highlight Links):** Um toggle que aplica um fundo amarelo vibrante e um sublinhado grosso em absolutamente todos os links (`<a>`) e botões (`<button>`) interativos. Feito via `.highlight-links` no `<body>`.
    *   **Cursor Aumentado:** Um toggle que aumenta drasticamente o tamanho do cursor padrão, ajudando idosos ou pessoas com perda de visão periférica. Implementado no CSS via `body.large-cursor { cursor: url('cursor-grande.svg'), auto; }`.

---

## 4. Áudio e Leitura (Web Speech API e Áudio Nativo)

A guia de áudio e leitura controla tanto efeitos do sistema quanto a síntese de voz (TTS) construída com `window.speechSynthesis`.

*   **Efeitos Sonoros:** Toggle que ativa/desativa feedback auditivo (ex: cliques em botões, transições). O estado é salvo (`sfx_enabled`) e gerenciado por um módulo de áudio (`AudioManager`).
*   **Leitura Automática:** Um toggle que, se ativado, dispara a leitura da página (`TTS`) assim que a mesma é carregada.
*   **Controles de Síntese de Voz (TTS):**
    *   **Volume da Leitura:** Slider que ajusta a propriedade `utterance.volume`.
    *   **Velocidade da Voz:** Botões de incremento rápido (`0.8x`, `1.0x`, `1.5x`, `2.0x`) que alteram a propriedade `utterance.rate`.
    *   **Voz do Narrador:** Um `<select>` preenchido por `speechSynthesis.getVoices()`, priorizando as vozes do idioma atual (ex: "Google português do Brasil (pt-BR) ⭐"). 
    *   **Botão "Testar":** Dispara uma frase de teste curta ("Olá! Esta é uma prévia da voz.") com as configurações atuais para o usuário conferir.
*   **Higienização e Segmentação de Texto:** Para que a leitura seja fluida, o texto original do HTML passa por filtros (remoção de emojis e blocos ocultos). Blocos interativos (como *flip-cards*) são anunciados (ex: "Há 3 cards. Clique para revelar.") pausando a leitura automática até a interação.

---

## 5. Integração com VLibras e Internacionalização (i18n)

A acessibilidade também diz respeito a superar barreiras linguísticas.

*   **VLibras:**
    *   Insira a estrutura base do Widget (uma div `<div vw class="enabled">` contendo o `vw-plugin-wrapper`).
    *   A ativação via interface (`#btn-vlibras`) carrega o script `https://vlibras.gov.br/app/vlibras-plugin.js` e inicia a instância `new window.VLibras.Widget()`.

*   **Arquitetura do Sistema de Traduções (i18n Vanilla JS):**
    O projeto não utiliza o Google Tradutor automático (para evitar quebra de layout e perda de controle semântico), mas sim um módulo nativo (`i18n.js`) baseado em dicionários JSON.
    *   **Armazenamento e Inicialização:** O idioma escolhido fica salvo em `localStorage.getItem('app_lang')` (padrão `pt`). Ao inicializar, o JS carrega o pacote idiomático correspondente via fetch (ex: `./assets/locales/en/global.json`).
    *   **Dicionário Fallback:** Mantenha um dicionário menor de UI hardcoded no JS (ex: botões "Anterior", "Próximo") para carregamento imediato ou caso de falha no JSON.
    *   **Mapeamento de Atributos no HTML:**
        O HTML deve ser preparado com atributos `data-` para receber a tradução:
        *   `data-i18n="chave"`: O JS substituirá o `innerHTML` deste elemento pelo valor da tradução. Isso preserva sub-tags internas (como `<strong>`).
        *   `data-i18n-global="chave"`: Usado para chaves mapeadas no dicionário fallback nativo.
        *   `data-i18n-title="chave"`: Substitui o atributo `title` do elemento.
        *   `data-i18n-aria="chave"`: Substitui o atributo `aria-label`, essencial para botões iconográficos.
    *   **Eventos de Tradução (`CustomEvent`):** Ao concluir a substituição no DOM, o sistema deve emitir `document.dispatchEvent(new CustomEvent('i18n-applied'))` para que modais ou scripts independentes possam se re-renderizar caso necessário.
    *   **Mudança Dinâmica:** Ao trocar de idioma (ex: botões `.lang-btn[data-lang="es"]`), atualize o `localStorage` e utilize `window.location.reload()` ou re-inicialize o módulo (atualizando iframes filhos, se houver). Atualize sempre a tag `<html lang="...">`.

---

## 6. Persistência de Preferências (Gerenciamento de Estado)

**Regra de Ouro:** Todas as configurações de acessibilidade ativadas pelo usuário devem ser persistidas.

*   Sempre utilize `localStorage` para salvar o estado de cada opção assim que modificada.
*   No arquivo principal de acessibilidade (`accessibility.js`), na função `init()`, recupere todas as chaves do `localStorage` (ex: `font-size`, `high-contrast`, `tts-voice`, `dyslexia-mode`, `auto-read`).
*   Aplique as configurações salvas imediatamente durante o carregamento inicial da página para evitar um "flash" de interface sem acessibilidade.

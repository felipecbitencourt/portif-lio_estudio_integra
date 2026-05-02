# Pilar 3 - Design

## Proposito do pilar

O design, no nosso processo, organiza a experiencia de aprendizagem, em um sistema de decisoes que orienta leitura, navegacao, compreensao e retencao de conteudo Trabalhamos design para reduzir friccao, ampliar clareza e sustentar consistencia entre diferentes materiais.

## Como aplicamos na pratica

### Design como sistema

Adotamos padroes de interface e comunicacao visual para manter unidade entre telas, modulos e projetos. Isso aumenta previsibilidade para o usuario.

### Hierarquia de informacao

Organizamos o conteudo com niveis claros de prioridade visual, facilitando:

- identificacao de mensagens principais;
- navegacao por blocos;
- leitura rapida com compreensao progressiva.

### Legibilidade e ritmo de leitura

Trabalhamos tipografia, espacamento, contraste e densidade informacional para tornar o conteudo confortavel em diferentes contextos de uso.

### UX educacional

As escolhas de interacao consideram objetivo pedagogico, contexto de uso e perfil do publico, em que cada elemento interativo deve apoiar aprendizagem.

### Responsividade e adaptacao

Planejamos layouts e componentes para diferentes telas e cenarios de acesso, preservando consistencia de experiencia.

### Conceitos estruturantes que orientam o design

As decisoes de design sao sustentadas por principios consolidados de UX e comunicacao visual:

- **Hierarquia Visual:** organizacao de tamanho, peso, contraste e espacamento para orientar atencao e priorizacao da informacao.
- **Leis da Gestalt:** proximidade, similaridade e continuidade para facilitar leitura de blocos e relacao entre elementos.
- **Carga Cognitiva:** reducao de ruido visual e segmentacao do conteudo para evitar sobrecarga e melhorar compreensao.
- **Progressive Disclosure:** apresentacao gradual de informacoes e interacoes para manter foco no passo atual da jornada.
- **Affordance e Feedback:** controles devem parecer clicaveis e responder de forma clara a cada acao do usuario.
- **Consistencia e Padroes de UX:** repeticao de comportamentos e convencoes de interface para reduzir curva de aprendizado.
- **Acessibilidade Visual:** contraste, legibilidade tipografica e estados visuais claros para ampliar usabilidade.

## Animacoes e micro-interacoes

Animacoes sao parte do sistema de design, nao decoracao. Cada movimento tem proposito: confirmar uma acao, guiar atencao, reduzir friccao cognitiva ou transmitir qualidade. A regra e simples: animar so quando adiciona clareza ou sensacao de resposta. Nada de animar por animar.

### Principios que orientam o uso de animacao

- **Resposta imediata:** o usuario deve sentir que a interface escuta. Feedbacks de hover e click devem ocorrer em menos de 200ms.
- **Proporcionalidade:** transicoes maiores para acoes maiores. Um hover em botao e sutil. Uma entrada de modal e mais pronunciada.
- **Continuidade:** animacoes de entrada e saida devem parecer parte do mesmo movimento. Nada de aparecer do nada.
- **Nao bloquear:** animacoes nao devem atrasar o usuario. Elementos interativos respondem imediatamente; animacoes de ornamento sao em paralelo.

### Variaveis de referencia


| Variavel            | Valor                              | Uso                                        |
| ------------------- | ---------------------------------- | ------------------------------------------ |
| `--transition-fast` | `0.15s ease`                       | Hover, focus, active em elementos pequenos |
| `--transition-med`  | `0.3s ease`                        | Transicoes de cards, menus, modais         |
| `--transition-slow` | `0.5s ease`                        | Zoom de imagem, mudancas de layout         |
| `--ease-out-soft`   | `cubic-bezier(0.2, 0.8, 0.2, 1)`   | Entradas de elementos (parece natural)     |
| `--ease-smooth`     | `cubic-bezier(0.25, 0.8, 0.25, 1)` | Elevacoes e hovers de cards                |


---

### Catalogo de animacoes

#### Micro-interacoes de hover


| Elemento                       | Animacao                                                                                                                                                      | Proposito                                                                                                                       |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Botao principal                | `scale(1.02)` + `box-shadow` expandida + mudanca de cor de fundo para tom levemente mais escuro. Duration: 160ms, ease-out-soft                               | Feedback tatil, sensacao premium. Indica que o elemento e clicavel sem ser gritante                                             |
| Botao com icone                | Icone desloca `translateX(3px)` ao hover. Duration: 0.3s, ease                                                                                                | Reforco direcional — indica que o botao leva a algum lugar                                                                      |
| Botao shimmer                  | Pseudo-elemento `::after` com gradiente diagonal percorre o botao da esquerda para direita (`translateX(-100%)` → `translateX(100%)`). Duration: 0.6s, linear | Efeito de brilho sutil que reforca a sensacao de material                                                                       |
| Card                           | `translateY(-4px)` + `box-shadow` aumenta. Duration: 0.3s, ease-smooth                                                                                        | Elevacao simula camada fisica; indica interatividade                                                                            |
| Card com borda lateral         | Pseudo-elemento `::before` (linha colorida na borda esquerda) entra com `scaleY(0)` → `scaleY(1)` + fade de `opacity`. Duration: 220ms, ease                  | Destaque visual que nao quebra o layout                                                                                         |
| Card com overlay de cor        | Pseudo-elemento `::after` expande com `scaleX(0)` → `scaleX(1)` da esquerda para direita. Duration: 480ms, ease. Transform-origin: left center                | Revelacao cinematica da cor de destaque. Textos filhos mudam de cor em delay de 40ms para parecer que sao cobertos pelo overlay |
| Link de navegacao              | `color` muda + `background` aparece suavemente. Duration: 160ms, ease                                                                                         | Resposta clara sem movimento brusco                                                                                             |
| Logo                           | `scale(1.02)`. Duration: 150ms, ease                                                                                                                          | Leve pulso de vida sem chamar atencao                                                                                           |
| Icone de configuracoes (gear)  | `rotate(12deg)` + `scale(1.02)`. Duration: 150ms, ease                                                                                                        | Metafora de acao — o icone "gira" como se estivesse sendo acionado                                                              |
| Imagem dentro de card          | `scale(1.05)`. Duration: 0.5s, ease. `overflow: hidden` no container                                                                                          | Zoom suave que nao quebra o layout — transmite profundidade                                                                     |
| Imagem em tons de cinza        | `filter: grayscale(30%)` → `grayscale(0)` + `scale(1.05)`. Duration: 0.3s, ease                                                                               | Colorir ao hover da sensacao de "ativar" o conteudo                                                                             |
| Icone de rede social no footer | `translateY(-3px)`. Duration: 0.3s, ease                                                                                                                      | Saltinho sutil, reforca clicabilidade                                                                                           |
| Botao scroll-to-top            | `translateY(-3px)`. Duration: 0.3s, ease                                                                                                                      | Mesma logica — sobe ao hover, reforca direcionalidade                                                                           |
| Chip de filtro / tag           | `translateY(-1px)` + `box-shadow` suave. Duration: 150ms, ease                                                                                                | Elevacao minima para chips pequenos                                                                                             |
| Seta de CTA                    | `translateX(6px)` + inversao de cores (fundo e icone trocam). Duration: 220ms, ease                                                                           | Movimento direcional mais dramatico para CTAs principals                                                                        |


#### Estados de foco e ativo


| Elemento                                     | Animacao                                                                                                                     | Proposito                                                             |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| Qualquer elemento interativo (focus-visible) | `outline` ou `box-shadow` colorida aparece com fade. Duration: 0.15s                                                         | Acessibilidade: foco visivel sem ser agressivo para usuarios de mouse |
| Botao no click (active)                      | `scale(0.98)` + `translateY(1px)` + sombra reduzida. Duration: 80ms, ease                                                    | Sensacao de pressao fisica — o botao "afunda" ao ser clicado          |
| Chip ativo                                   | `background` muda para gradiente com cor primaria + `border-color` + `box-shadow` interna. Sem duration (estado persistente) | Diferencia claramente o item selecionado dos demais                   |


#### Entradas de elementos (scroll-triggered e carregamento)


| Elemento                                    | Animacao                                                                                                          | Proposito                                                                        |
| ------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Conteudo ao entrar no viewport (reveal)     | `opacity: 0` → `1` + `translateY(30px)` → `translateY(0)`. Duration: 0.6s, ease. Ativado por IntersectionObserver | Aparicao organica. O conteudo "sobe" como se emergisse da pagina                 |
| Cards em lista ou grid (entrada escalonada) | Mesma animacao acima, porem com delay escalonado entre os itens: 80ms, 150ms, 220ms, etc.                         | Sequencia de entrada que guia o olhar e da ritmo a pagina                        |
| Fade puro                                   | `opacity: 0` → `1`. Duration: 0.6s, ease                                                                          | Para elementos que nao devem ter movimento vertical — textos sobrepostos, badges |
| Slide da esquerda                           | `opacity: 0` + `translateX(-30px)` → estado normal. Duration: 0.6s, ease                                          | Elementos que chegam de fora do viewport lateral                                 |
| Slide da direita                            | `opacity: 0` + `translateX(30px)` → estado normal. Duration: 0.6s, ease                                           | Idem, para elementos que chegam pelo outro lado                                  |


#### Entradas de overlays, modais e menus


| Elemento                                | Animacao                                                                                                                                     | Proposito                                                                  |
| --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| Dropdown / menu flutuante               | `opacity: 0` + `translateY(-8px)` + `scale(0.98)` → estado normal. Duration: 0.22s, ease-out-soft                                            | Aparece de cima para baixo, como se caindo — gesto natural                 |
| Modal centralizado                      | `opacity: 0` + `translate(-50%, -48%)` + `scale(0.97)` → `opacity: 1` + `translate(-50%, -50%)` + `scale(1)`. Duration: 0.38s, ease-out-soft | Surgimento leve com leve crescimento. Parece material fisico se expandindo |
| Backdrop de modal                       | `opacity: 0` → `1`. Duration: 0.3s, ease-out-soft                                                                                            | Escurece o fundo antes do modal aparecer — direciona atencao               |
| Painel colapsavel (filtros, accordions) | `grid-template-rows: 1fr` → `0fr`. Duration: 0.4s, ease-out-soft                                                                             | Tecnica de grid para collapse sem JavaScript calculando altura             |
| Chevron de accordion                    | `rotate(0)` → `rotate(180deg)`. Duration: 0.35s, ease-out-soft                                                                               | Icone acompanha o estado — aberto ou fechado fica claro                    |


#### Animacoes continuas e de ambiente


| Elemento                                | Animacao                                                                                                                                 | Proposito                                                                                              |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Fundo hero com gradiente                | `background-position: 0% 40%` → `100% 60%`. Duration: 20s, ease-in-out, infinite alternate                                               | Gradiente respira — cria vida sem distrair. Background-size deve ser 200% para o movimento ser visivel |
| Fundo de secao com gradiente            | `background-position: 0% 50%` → `100% 50%`. Duration: 16s, ease-in-out, infinite alternate                                               | Variacao mais contida para secoes internas                                                             |
| Marquee / texto correndo                | `translateX(0)` → `translateX(-50%)`. Duration: 38s, linear, infinite. O track tem o conteudo duplicado para loop perfeito               | Comunicacao em fluxo continuo. Linear e obrigatorio para manter velocidade uniforme                    |
| Mascote ou personagem flutuante         | `translateY(0) rotate(0deg)` → `translateY(-14px) rotate(1.5deg)` → retorno. Duration: 5s, ease-in-out, infinite                         | Leveza e presenca. A rotacao minima imita respiracao                                                   |
| Brilho ao redor de mascote (glow pulse) | `scale(1) opacity(0.65)` → `scale(1.12) opacity(1)` → retorno. Duration: 4s, ease-in-out, infinite                                       | Indica vida ou destaque sem movimento de posicao                                                       |
| Bolhas flutuantes decorativas           | `translateY(0)` → `translateY(-10px)` → retorno. Duration: 3.5s, ease-in-out, infinite. Delay variavel por elemento para nao sincronizar | Profundidade e movimento sutil no fundo                                                                |
| Indicador de scroll para baixo          | `translateY(0)` → `translateY(-15px)` → `translateY(-7px)` → `translateY(0)`. Duration: 2s, ease-in-out, infinite                        | Bounce ensinado: mostra direcao do gesto esperado                                                      |


#### Animacoes de feedback e estado


| Elemento                                   | Animacao                                                                                                                                                         | Proposito                                                                                                                            |
| ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Elemento com erro (resposta incorreta)     | `shake`: translacoes rapidas horizontais alternadas (`translateX(-8px)` → `+8px`, decaindo). Duration: 0.5s, ease-in-out                                         | Gesto universal de negacao. Preciso e imediato                                                                                       |
| Elemento que precisa de atencao (tutorial) | `wiggle`: rotacoes alternadas leves (`rotate(-2deg)` → `rotate(2deg)`). Duration: 0.4s, ease-in-out, 2-3 repeticoes                                              | Chama atencao sem bloquear o usuario                                                                                                 |
| Indicador de carregamento / progresso      | Animacao de escala ou translacao com `ease-out`. Duration: 1.5s                                                                                                  | Comunica espera de forma engajante                                                                                                   |
| Pulsacao de alerta                         | `scale(1)` → `scale(1.2)` + `opacity: 0.8` → `opacity: 0`. Duration: 1.5s, ease-in-out, infinite                                                                 | Ring pulsante ao redor de elemento critico — atencao persistente                                                                     |
| Pulsacao generica de icone/badge           | `scale(1)` → `scale(1.05)` → retorno. Duration: 2s, ease-in-out, infinite                                                                                        | Indica item ativo, novo ou aguardando acao                                                                                           |
| Dica flutuante (tooltip hint)              | `opacity: 0` + `translateX(-10px)` → estavel → `opacity: 0` + `translateX(-10px)`. Duration total: 5s (10% entrada, 70% visivel, 20% saida). Easing: ease-in-out | Dica que aparece e desaparece sem botao de fechar. Seta interna pulsa com `translateX(0)` → `translateX(-5px)` para reforcar direcao |


#### Animacoes de navegacao e header


| Elemento                            | Animacao                                                                                                               | Proposito                                                              |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Header ao scrollar para baixo       | `translateY(-100%)`. Duration: 0.3s, ease                                                                              | Esconde header para ganhar espaco de leitura. Threshold: 5px de scroll |
| Header ao scrollar para cima        | `translateY(0)`. Duration: 0.3s, ease                                                                                  | Retorna imediatamente ao retomar scroll reverso                        |
| Header ganha sombra ao sair do topo | `box-shadow` aumenta gradativamente. Duration: 0.3s, ease                                                              | Distingue header "flutuante" do conteudo abaixo                        |
| Botao scroll-to-top (aparecer)      | `opacity: 0; visibility: hidden` → `opacity: 1; visibility: visible`. Duration: 0.3s, ease. Threshold: 300px de scroll | Aparece apenas quando relevante                                        |


---

### Notas de implementacao

- `**will-change: transform`** deve ser usado com moderacao — apenas em elementos com animacoes continuas ou de alta frequencia.
- O padrao de **reveal com IntersectionObserver** e preferivel a bibliotecas externas para projetos leves. Classe `.reveal` em estado inicial, classe `.visible` adicionada por JS ao entrar no viewport.
- Para efeitos de **collapse/expand** de altura variavel, a tecnica de `grid-template-rows: 1fr` → `0fr` evita calculo dinamico de altura em JavaScript.
- Animacoes com `::before` e `::after` exigem `position: relative` no elemento pai e `overflow: hidden` quando o pseudo-elemento nao deve vazar para fora do container.

## Ferramentas e recursos utilizados

Na frente de design, trabalhamos com:

- bibliotecas de componentes visuais e padroes de interface;
- guias de estilo para manter consistencia editorial e visual;
- prototipos para validar estrutura e navegacao;
- criterios de UX para ambientes educacionais interativos;
- revisoes de qualidade visual antes da publicacao.


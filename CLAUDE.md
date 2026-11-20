# Pulsar

Jogo de ritmo à moda do Guitar Hero em que **quem joga toca a melodia**. Começou em 2026-11-20 a partir do protótipo `ritmo2.html` do repositório `um-passo` (o 2048 Astros). Publicado em https://guipfranco.github.io/pulsar/.

O foco é total na música: todo som de quem joga é uma nota dela. Não há efeito sonoro de jogo.

## Como joga

- A trilha é a Pulsar, tocada na hora pelo navegador (Web Audio, sem arquivo de áudio). A melodia é o tema lento "Órbita lenta" (`PUL.TEMAS[1]`), em roda de 8 compassos, e **não toca sozinha**: cada toque na tela, ou qualquer tecla, toca a próxima nota, com a altura, a duração e o timbre da Pulsar. A nota perdida fica faltando na música.
- Traços vêm das pontas até uma barra fixa no meio; a hora da nota é quando encostam nela. Cada traço tem um rastro do tamanho da nota.
- Janela de acerto: ±0,2 s (`JANELA`). Vale a nota mais perto do toque.
- 3 acertos por multiplicador (`MIN=[0,3,6,9]`). Erro (toque fora da nota) ou nota perdida volta ao ×1.
- A banda entra presa ao multiplicador e para no ×4: ×1 acordes, ×2 + bateria, ×3 + baixo, ×4 + arpejo (`camadas()`, que liga e desliga `opc.mudo` ao vivo).
- Calibração de atraso: 10 toques junto com os traços; o meio-termo do desvio (sem os 2 primeiros) vira o `atraso`, gravado em `localStorage.pulsar_atraso`.

Testado e descartado no um-passo: arpejo em semicolcheias (frenético demais), baixo (difícil de ouvir), um botão por nota. A melodia lenta é a que funcionou. O Guilherme acha a dificuldade atual difícil e quer um modo fácil.

## Onde mora cada coisa

- `index.html`: o jogo inteiro (HTML, CSS e o módulo com as regras, a entrada, a calibração e o desenho da barra). `window.__r` expõe o estado para testes.
- `musica/eletronica.js`: o motor que toca as trilhas na hora (`cria`, `sorteio`, `escolhe`). Todo ganho de nota nasce fechado, senão o chimbal estala.
- `musica/pulsar.js`: a trilha (temas, `montaParte`, `PADRAO`, `iniciarMusica`/`pararMusica`). O jogo fixa o arranjo com `opc.compasso` e acompanha os passos com `opc.aoPasso`.

Os dois arquivos de música vieram do `um-passo`; se mudarem lá, copie de novo à mão (não há ligação entre os repositórios).

## Rodar e publicar

- Precisa de servidor (módulos ES não abrem por `file://`): `python -m http.server 8000` na pasta e abrir http://localhost:8000/.
- Depois de testar com som num navegador controlado por ferramenta, feche a aba: a música continua tocando.
- Publicar é commit e push em `main`; o GitHub Pages atualiza em 1 a 2 minutos.

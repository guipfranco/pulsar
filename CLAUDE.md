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

- `robo.html` (rascunho, 2026-11-20): trilha original no estilo do house francês (batida reta a 123 bpm, riff recortado, filtro que abre com o combo, "bombeio" a cada bumbo) e voz robótica feita com filtros nas formantes das vogais (`FORM`), cantando uma frase de 4 sílabas que se repete. Três frases (nenhuma agradou), dois timbres (o vocoder é o preferido do Guilherme e é o padrão; talkbox), combo ×1 a ×4 e um modo "você canta" sem pontos. Arquivo único, não usa o motor de `musica/`. Abre um `.mid` do Guilherme ("Sua música", desde 2026-11-20): `leMidi` lê formatos 0 e 1; `chutaMelodia` escolhe a faixa com mais cara de melodia; `montaMidi` encaixa o trecho escolhido na grade de semicolcheias (uma nota por vez, a mais aguda), ajusta a oitava, dá vogal pela altura e estima um acorde por compasso (a nota grave que mais soa é a raiz; a terça decide menor ou maior) para o riff e o baixo. O andamento vem do arquivo. Tudo fica só no navegador (`localStorage.pulsar_midi`). Compasso sempre de 4 tempos. Cada canal vira uma faixa (arquivos de formato 0 põem a banda numa faixa só). Banda "Do arquivo" (padrão): as outras faixas tocam com timbres nossos escolhidos pela família do General MIDI (`tipoDe`: pad, órgão, piano, metais, sintetizador, baixo, bateria; `TOCA`), na hora exata de cada nota, e entram com o combo (`ENTRA`: ×1 pads, órgão e piano; ×2 bateria; ×3 baixo; ×4 metais e sintetizadores); dá para calar cada instrumento. "A nossa" toca a batida e o riff gerados nos acordes estimados. O trecho começa onde a melodia já está cheia (`melhorInicio`), não na introdução. `?midi=midis/arquivo.mid` abre um arquivo direto no servidor local (a pasta `midis/` e todo `.mid` ficam fora do git). Nunca suba MIDI de música protegida para o repositório: ele é público. Inspirado no estilo do Daft Punk; nada de melodia, letra ou sample deles.

- `acordes.html` (esboços, 2026-11-20): modo "conte as notas", testado com o Hammer Bros do Super Mario Bros 3 (baixo + 2 vozes, como o NES). Não importa qual tecla, só quantas juntas: cada momento chega como 1, 2 ou 3 quadradinhos numa estrada em perspectiva. Cada tecla toca uma voz (a de cima, depois o baixo, depois a do meio), então apertar menos soa incompleto mas musical. Três abordagens: Pilha (fileira de N), Pistas (uma pista por voz: baixo, meio, cima), Segurar (segura N teclas e a música toca no ritmo; o desafio é mudar quando a textura muda). Densidade (todas / uma por tempo / duas por compasso; as que não são suas tocam sozinhas) e velocidade (60, 80, 100%). Som de NES de mentira (pulso 25% em cima, triângulo no baixo). Abre `.mid` pelo botão ou `?midi=`. Desde a segunda versão: uma faixa por canal; a faixa da melodia (`chutaMelodia`, escolhível) é a sua e todo o resto toca sozinho em som de videogame (`prepara`: baixo em triângulo, acompanhamento em quadrada abafada, bateria de ruído). "Você toca": só a melodia (padrão: a nota de cima, uma por vez), melodia e harmonia, ou tudo (nunca mais de 3 juntas). Músicas com mais de 24 compassos abrem num trecho de 16 que começa onde a melodia está cheia (`melhorInicio`); a música toda também dá. O Guilherme achou o Hammer Bros com todas as vozes difícil demais e descartou teclas fixas alternando lados (ASD / JKL).

- `linhas.html` (esboços, 2026-11-21): "três linhas". A melodia vira obstáculos com uma passagem só (grave, média, aguda) e quem joga vai para ela na hora com A S D ou ← ↓ → (celular: terço da tela); o toque na passagem certa toca a nota, mesmo sem mudar de linha. Linha errada: bate (nota some, combo zera); sem tocar: perde. A linha de cada altura vem de dividir as alturas do trecho em três partes com o mesmo número de notas (a mesma altura cai sempre na mesma linha). Duas abordagens: avião no túnel (perspectiva, paredes com uma janela acesa, rastro na nota longa) e bichinho nas plataformas (de lado, grave embaixo, pula num arco). Motor, trecho, densidade, velocidade e som de videogame copiados do `acordes.html`. No servidor local aparecem atalhos para `midis/hammer-bros.mid` e `midis/one-more-time.mid`.

Os dois arquivos de música vieram do `um-passo`; se mudarem lá, copie de novo à mão (não há ligação entre os repositórios).

## Rodar e publicar

- Precisa de servidor (módulos ES não abrem por `file://`): `python -m http.server 8000` na pasta e abrir http://localhost:8000/.
- Depois de testar com som num navegador controlado por ferramenta, feche a aba: a música continua tocando.
- Publicar é commit e push em `main`; o GitHub Pages atualiza em 1 a 2 minutos.

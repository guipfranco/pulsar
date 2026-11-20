# Pulsar

Um jogo de ritmo em que você toca a melodia. Os traços vêm das pontas até a barra do meio; toque na tela (ou aperte qualquer tecla) quando eles encostarem, e cada acerto é uma nota da música. Errou, a nota fica faltando.

A cada 3 acertos seguidos o multiplicador sobe e a banda ganha uma parte: acordes no ×1, bateria no ×2, baixo no ×3 e arpejo no ×4. Um erro volta tudo ao ×1.

Toda a música é gerada na hora pelo navegador (Web Audio), sem arquivo de áudio.

**Jogar:** https://guipfranco.github.io/pulsar/

## Rodar local

```
python -m http.server 8000
```

e abra http://localhost:8000/. Use fones ou caixa de som e, na primeira vez, toque em **Calibrar** para acertar o atraso do seu aparelho.

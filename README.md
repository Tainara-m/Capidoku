# Capidoku

Um puzzle de lógica fofinho, com capivaras, para jogar no navegador. É uma homenagem independente ao **Meowdoku** (Oakever Games): em vez de gatos, você posiciona capivaras numa grade colorida usando só dedução.

Tudo está em **um único arquivo HTML**, em JavaScript puro, sem dependências, sem build e sem arquivos de áudio.

> **Aviso:** este é um projeto de fã, sem qualquer vínculo com a Oakever Games. Não usa código, arte ou sons do jogo original.

<!-- Adicione aqui um screenshot ou GIF: ![Capidoku](docs/screenshot.png) -->

## Como jogar

Coloque **exatamente uma capivara em cada linha, em cada coluna e em cada região colorida**. Duas capivaras nunca podem se tocar, nem na diagonal.

Não há números nem chute: cada tabuleiro gerado tem **uma única solução**, que sempre pode ser encontrada por dedução.

### Controles

| Ação | O que faz |
|---|---|
| Toque duplo (ou duplo clique) | Coloca uma capivara |
| Toque simples | Marca ou desmarca ✕ |
| Toque simples numa capivara | Remove a capivara |
| Arrastar o dedo ou o mouse | Marca vários ✕ de uma vez (ou apaga, se começar num ✕) |
| ↩ Desfazer | Volta a última jogada |
| 💡 Dica | Coloca uma capivara correta |
| ✨ Guia | Apaga e marca com ✕ as casas bloqueadas pelas capivaras já postas |
| ↺ Reiniciar | Recomeça o mesmo tabuleiro |
| 🎵 / 🔊 / slider | Liga a música, muta tudo e ajusta o volume |

### Regras do jogo

- Você tem **3 vidas** ❤️. Colocar uma capivara no lugar errado custa uma vida e a casa vira ✕. Sem vidas, você tenta o mesmo tabuleiro de novo.
- Os níveis são infinitos e o tabuleiro cresce de **5×5 até 9×9**, aumentando a cada 3 níveis.
- Você começa com **1 dica no nível 1** e ganha mais uma a cada nível (nível N = N dicas).
- Seu progresso, o volume e as preferências de som ficam salvos no navegador (`localStorage`).

## Rodando localmente

Não precisa instalar nada:

```bash
git clone https://github.com/SEU-USUARIO/capidoku.git
cd capidoku
# abra o index.html no navegador
```

A fonte *Fredoka* vem do Google Fonts. Sem internet, o jogo funciona normalmente com uma fonte do sistema no lugar.

### Publicando no GitHub Pages

1. Em **Settings → Pages**, escolha a branch `main` e a pasta `/ (root)`.
2. Aguarde o deploy e acesse `https://SEU-USUARIO.github.io/capidoku/`.

## Como foi feito

Este projeto foi construído **conversando com o Claude** (IA da Anthropic) no claude.ai, em várias rodadas. Vale ser transparente sobre como isso funcionou:

- **Quem fez o quê.** O código foi escrito pelo Claude a partir de pedidos em linguagem natural. A direção de design, as imagens, as decisões de jogabilidade e os testes jogando foram da pessoa autora.
- **A primeira versão foi um palpite.** A ideia inicial das regras veio de descrições superficiais do Meowdoku. Só depois foi feita uma pesquisa na web sobre as regras e a mecânica, e o jogo foi ajustado: toque duplo para colocar, vidas, níveis progressivos e sem cronômetro.
- **O original não foi jogado nem analisado diretamente.** Tudo sobre o Meowdoku vem de textos públicos (lojas de apps, reviews, páginas de regras). Detalhes como o número de dicas, a curva de dificuldade e o limite de 9×9 são escolhas deste projeto, e não cópias do original.
- **Não há testes automatizados.** O Claude não rodou o jogo num navegador durante a criação. Os testes foram manuais, jogando e reportando o que estava estranho.
- **Evolução por iterações.** A ordem foi: versão básica com regras do tipo "Queens" → visual mais limpo e arrastar para marcar ✕ → mecânica estilo Meowdoku → guia visual e dicas por nível → efeitos sonoros → trilha 8-bit.

## Como funciona por dentro

Tudo está em `index.html`, com CSS e JS no mesmo arquivo.

- **Geração de tabuleiros.** Sorteia uma posição válida (uma capivara por linha e coluna, sem vizinhas), planta cada região a partir de uma capivara e a faz crescer aleatoriamente. Um solver por backtracking conta as soluções (parando em 2) e só aceita tabuleiros com **solução única**.
- **Validação.** Ao colocar uma capivara, o jogo compara com a solução guardada. Por isso a unicidade importa: sem ela, uma jogada válida poderia ser tratada como erro.
- **Layout.** A tela usa `100dvh` e `container queries` para o tabuleiro ser sempre quadrado e ocupar o espaço disponível, sem rolagem.
- **Som e música.** Os efeitos e a trilha 8-bit são **sintetizados em tempo real** com a Web Audio API (ondas quadrada, triangular, seno e ruído). A melodia é original e foi escrita para o projeto.
- **Persistência.** O progresso e as preferências de som usam `localStorage`, com `try/catch` para quando o armazenamento não estiver disponível.
- **Imagens.** A capivara e o ícone estão embutidos no HTML como WebP em base64, o que deixa o arquivo único e autocontido.

## Limitações conhecidas

- O gerador tem um limite de tempo de 3 segundos. Se não achar um tabuleiro de solução única nesse prazo, ele usa o último gerado, e isso pode (raramente) permitir mais de uma solução válida, mas só uma contará como certa.
- O tamanho máximo é 9×9. O original chega a 10 capivaras, mas o gerador por tentativa e erro fica lento em tabuleiros maiores.
- As dicas crescem 1 por nível, então nos níveis altos o jogo fica fácil. É uma escolha de design que talvez valha rever.
- O desfazer não devolve as vidas perdidas nem as dicas gastas.
- Não há controle por teclado e a acessibilidade (leitores de tela, daltonismo) não foi trabalhada. As regiões se distinguem só pela cor.
- Só foi testado em poucos dispositivos e navegadores. Precisa de um navegador moderno, com suporte a *container queries* e Web Audio.
- A trilha é um loop curto (cerca de 15 segundos) e pode cansar em sessões longas.

## Ideias para o futuro

- Modo diário, com o mesmo tabuleiro para todo mundo.
- Opção de alto contraste ou padrões nas regiões, para quem tem daltonismo.
- Controle por teclado.
- Curva de dificuldade mais cuidadosa e tabuleiros 10×10.
- Mais faixas de música e conquistas.

## Créditos e licença

- **Ideia original:** Meowdoku, da Oakever Games (este projeto não tem relação com a empresa).
- **Código:** escrito com a ajuda do Claude (Anthropic) e dirigido por **Tainara Martins**.
- **Imagens (capivara e ícone):** Imagem gerada com Inteligência Artificial <!-- informe aqui a origem e a licença das imagens que você usou -->
- **Fonte:** [Fredoka](https://fonts.google.com/specimen/Fredoka), licença OFL, carregada do Google Fonts.
- **Licença do código:** MIT <!-- ex.: MIT. Adicione um arquivo LICENSE e deixe claro que as imagens têm licença à parte -->

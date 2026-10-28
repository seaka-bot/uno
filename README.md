<div align="center">

<img src="https://mnsa.com.br/assets/img/github/uno-capa.png" alt="Uno — Seaka-bot" width="100%">

**O Uno completo dentro do Discord: party de até 8 pessoas, +2 e +4 acumulando, escolha de cor e um painel de host.**

[Jogue a simulação no navegador](https://mnsa.com.br/projetos/uno/) · [Outros projetos do Seaka](https://github.com/seaka-bot)

</div>

> [!WARNING]
> **Código de acervo.** Foi escrito pra uma versão antiga do BDFD e não é mantido. Algumas funções podem ter mudado. Use como referência e adapte.

## Funcionando

<img src="https://mnsa.com.br/assets/img/uno-discord-2023.webp" alt="Lobby do Uno no Discord, com os botões Iniciar Jogo, Abrir Party, Fechar Party, Entrar na Party e Host" width="480">

*Lobby de uma partida, 19/09/2023.*

## Arquivos

| Arquivo | Gatilho no BDFD | O que faz |
|---|---|---|
| `prefix/uno.bdscript2` | `$alwaysExecute` | Comando `uno`. Confere o prefixo sozinho, abre a party e dá 7 cartas pro host. |
| `oninteraction/unoMenu.bdscript2` | `$onInteraction` | Lobby: entrar, abrir/fechar a party e o painel de host antes da partida. |
| `oninteraction/unoJogo.bdscript2` | `$onInteraction` | A partida: iniciar, comprar, jogar, +2/+4, bloqueio, coringa, vitória e o painel de host durante o jogo. |
| `oninteraction/unoCarta.bdscript2` | `$onInteraction` | Quando o jogador toca numa carta da mão: valida a jogada e separa a carta. |

Os quatro são **BDScript 2**.

## Variáveis

Crie todas no app do BDFD, na aba de variáveis, com o valor padrão da tabela.

| Variável | Padrão | Usada como | Pra que serve |
|---|---|---|---|
| `Prefixo` | `!` *(ou o seu)* | servidor | Prefixo dos comandos. O comando lê ele pra saber se a mensagem é `!uno`. |
| `UnoParty` | *(vazio)* | canal | Jogadores da partida, por ID, separados por `-`. O primeiro é o host. |
| `UnoCartas` | *(vazio)* | canal | A mão de todo mundo: jogadores separados por `/`, cartas separadas por `-`. |
| `UnoCarta` | *(vazio)* | canal | A carta que está na mesa, ex.: `r5`. |
| `UnoCartaJogar` | *(vazio)* | canal | A carta que o jogador selecionou e ainda não confirmou em **Jogar**. |
| `UnoCompra` | **`0`** | canal | Quantas cartas estão acumuladas de +2/+4. **Tem que começar em `0`**: o código compara com zero. |
| `UnoEmojis` | *(vazio)* | canal **e** bot | Duas funções, veja abaixo. |

### ⚠️ `UnoEmojis` tem dois usos

- **Como variável de canal**, é a trava da partida: `fim-da-trava-idDaMensagem`. Enquanto o horário não passar, ninguém abre outra party no mesmo canal (5 minutos a partir do `!uno`). Se estiver vazia, o próprio código grava `1-1`.
- **Como variável do bot** (`$getVar[UnoEmojis;$botID]`), guarda o **ID do servidor onde estão os emojis das cartas**.

Por isso **o padrão tem que ficar vazio**. Se você colocar o ID do servidor como padrão, todo canal novo vai achar que já tem uma partida rolando.

Pra gravar o ID do servidor, crie um comando temporário, rode uma vez e apague:

```
$setVar[UnoEmojis;ID_DO_SERVIDOR_DOS_EMOJIS;$botID]
Pronto.
```

## Emojis

### Cartas (dinâmicos)

O bot lê os emojis do servidor de emojis **pelo nome** e cria uma variável pra cada carta. Os nomes têm que ser exatamente estes:

| Tipo | Nomes |
|---|---|
| Números | `r1`…`r9` · `b1`…`b9` · `g1`…`g9` |
| +2 | `rb` `yb` `gb` `bb` |
| Bloqueio | `rx` `yx` `gx` `bx` |
| Coringa e +4 | `0c` `0b` |
| Cor escolhida no coringa | `r0` `g0` `b0` `y0` |

`r` vermelho · `b` azul · `g` verde · `y` amarelo · `0` coringa



O bot lê no máximo **50 emojis** desse servidor, então use um servidor só pras cartas.

### Botões (fixos)

Estes estão com o ID dos emojis do meu servidor e **não vão aparecer no seu**. Troque pelo `<:nome:id>` de um emoji seu ou apague o emoji do botão:

`aUnoIniciar` · `aUnoAbrir` · `aUnoFechar` · `aUnoSair` · `aUnoHost` · `emoji_15` (coroa do host) · `aBotCirculo` (jogador)

## Permissões

O bot precisa de **Inserir links** (Embed Links) no canal.

## Como jogar

1. `!uno` abre o lobby. Quem criou é o host.
2. Os outros entram em **Entrar na Party** (até 8).
3. O host aperta **Iniciar Jogo**.
4. Na sua vez, aperte **Jogar**: sua mão aparece em botões, 5 por linha. Escolha a carta e aperte **Jogar** de novo.
5. Sem carta que sirva, aperte **Comprar**.
6. +2 e +4 acumulam: se o próximo tiver carta de compra, pode devolver. Se não tiver, compra tudo.
7. Coringa e +4 abrem os botões de cor.
8. Quem zerar a mão ganha.

**Painel de host:** pular a vez de quem sumiu, remover jogador (menos o host), fechar a sala e reenviar a mensagem do jogo.

## Técnicas usadas

- Laço gerado com `$eval`: [explicação geral](https://github.com/seaka-bot/.github/blob/main/TECNICAS.md#laço-com-eval)
- Emojis dinâmicos: [explicação geral](https://github.com/seaka-bot/.github/blob/main/TECNICAS.md#emojis-dinâmicos)
- Estado da partida numa string: [explicação geral](https://github.com/seaka-bot/.github/blob/main/TECNICAS.md#string-como-banco-de-dados)

## Créditos

Código e cartas por **hirozn** · [mnsa.com.br](https://mnsa.com.br)

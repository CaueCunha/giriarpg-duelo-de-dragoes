# Módulo Foundry "Duelo de Dragões"

Leva cada revista da campanha para o Foundry com um clique: fichas com tokens, parceiros, tabelas, macros, diários (plano do mestre, cartas e handouts). **Cenas e mapas não vêm no módulo**: você sobe cada mapa à mão para preparar a cena do seu jeito. As automações da campanha ficam ligadas enquanto o módulo estiver ativo.

Feito para **Foundry v14 + sistema Tormenta20 1.6.x** (declara compatibilidade a partir da v13, mas só foi pensado para a v14). Nada foi rodado num Foundry real ainda; a lógica do importador foi testada fora do Foundry, com o Foundry simulado.

## Instalar (pela Forge ou qualquer Foundry)

Em **Configuração > Módulos > Instalar Módulo**, cole no campo **URL do Manifesto**:

`https://raw.githubusercontent.com/CaueCunha/giriarpg-duelo-de-dragoes/main/module.json`

Cada melhoria publicada sobe a versão; na lista de módulos aparece **Atualizar**. Depois de atualizar, reimporte só a revista que mudou.

## Instalar à mão (alternativa)

1. Descompacte `duelo-de-dragoes.zip` dentro de `Data/modules/` do Foundry (fica `Data/modules/duelo-de-dragoes/module.json`). No Windows o padrão é `%localappdata%\FoundryVTT\Data\modules`.
2. Reinicie o Foundry, abra o mundo e ative o módulo em **Gerenciar Módulos**.
3. Abra **Configurações > Configurar Opções > Duelo de Dragões > Importar revistas** (ou rode `ddd.abrir()` no console) e clique em **Importar** em **Ferramentas gerais** (uma vez por mundo) e depois na revista.

Tudo vai para pastas "Duelo de Dragões / Revista N" em Atores, Itens, Tabelas, Macros e Diários. As macros que servem para a campanha toda (DdD · Teste estendido, DdD · Frio e o atalho do importador) ficam em "Duelo de Dragões / Ferramentas gerais".

## Sincronização automática (desde 0.7.0)

Ao abrir o mundo, o mestre ativo cria as revistas novas e reimporta as que mudaram no módulo (fichas, itens, tabelas, macros e diários, nas pastas "Duelo de Dragões"). Cada revista tem uma impressão digital (`hash` no `indice.json`); só reimporta quando ela muda. O que o mestre editou é mantido, como no botão Reimportar. Revista removida pela janela não volta sozinha. Um sussurro no chat resume o que mudou. Desliga em Configurações > Duelo de Dragões > "Criar e atualizar as revistas no mundo automaticamente".

## Compêndios (desde 0.5.0)

Com o módulo ativo, a aba Compêndios mostra a pasta **Giriarpg - Duelo de Dragões** com cinco compêndios: Ameaças e aliados, Itens e parceiros, Macros, Tabelas e Diários do mestre. Cada um tem uma pasta por revista. Basta arrastar o que precisar para o mundo ou para a cena. Os compêndios são só do mestre (jogadores não veem). As automações vão junto, porque ficam nas flags dos atores e itens. Os compêndios são gerados em `construir.py` (`construir_compendios`) com a ferramenta oficial `@foundryvtt/foundryvtt-cli`, instalada em `~/.cache/ddd-ferramentas`.

## Reimportar e remover

- **Instalar uma versão nova do módulo não muda nada no mundo.** Só o botão Importar/Reimportar de uma revista cria ou troca documentos, e só os daquela revista. Importar a revista 3 nunca toca na 2.
- **Reimportar** atualiza fichas, itens, tabelas, macros e diários da revista. Os ids são fixos, então tokens já colocados continuam ligados às fichas.
- **Suas edições são preservadas:** o que você editou no Foundry depois da última importação (incluindo itens e efeitos dentro de uma ficha, páginas de diário e resultados de tabela) é listado antes de reimportar, e por padrão é mantido. Só é trocado se você escolher "Substituir pela versão do módulo". O que foi mantido uma vez continua protegido nas próximas reimportações.
- **Cenas que versões antigas do módulo criaram continuam no mundo**, intactas. Reimportar não as atualiza mais; Remover ainda apaga as que têm a flag do módulo.
- **Remover** apaga do mundo tudo o que o módulo criou para aquela revista (procura pela flag `duelo-de-dragoes.revista`).
- Para uma correção valer para sempre (e em mundos novos), peça aqui: ela é feita na fonte da revista e entra em toda versão nova do módulo.

## Automações (sempre ligadas, executadas pelo mestre ativo)

Cada ficha liga a sua por flag (`flags.duelo-de-dragoes.automacoes`):

| Chave | O que faz | Usado em |
|---|---|---|
| `ferocidade: true` | Ao perder PV, ganha +2 em ataque e dano até o fim do próximo turno | Carcaju (#2, #3) |
| `soproCompartilhado: "<grupo>"` | Só um membro do grupo usa o sopro por vez; recarrega quando um membro cai a 0 PV; avisa o mestre por sussurro | Cultistas de Kallyadranoch (#2) |
| `ataqueEmBando: true` | Ao atacar com arma, compara o ataque com a Defesa de cada alvo marcado e avisa o mestre: ×2 (excedeu por 10+), ×1 ou ×½ (errou) | Turba zumbi (#4) |
| `verVermelho: true` | O efeito "Ver Vermelho" (+2 em perícias) desliga quando a ficha é alvo de ataque ou habilidade e religa no fim do turno dela | Minotauro da Manada (#4) |
| `lutarEmFormacao: true` | O efeito "Lutar em Formação" (+2 ataque e Defesa) liga quando o token está adjacente a outro token vivo com a mesma automação e a mesma disposição, e desliga quando se afasta | Puristas (#5) |
| `evacuacao: 20` | Avisa o mestre quando a ficha começa o turno com PV entre 1 e o número dado | Corcel de Comando (#5) |
| `auraVenenosa: {dano, cd}` | No início do turno de cada inimigo adjacente, rola a Fortitude dele, tira o dano em PV (metade se passar) e aplica fraco | Kallyagrai do veneno (#7) |
| `fimToxico: {dano, cd, raio}` | Ao cair a 0 PV, explode: Fortitude de todos no raio sem imunidade a veneno, dano e fraco | Kallyagrai do veneno (#7) |
| `formaDraconica: "<ficha>"` | Ao cair a 0 PV, troca o token pelo da ficha indicada (PV e PM cheios), no mesmo lugar; não troca se a ficha tiver o efeito "Sem forma dracônica" ligado | Ária → Aranzirius (#8) |
| `avisoPV: {limite, texto}` | Avisa o mestre quando os PV caem para o limite ou menos | Aranzirius a 80 PV, Tabalinn interrompe (#8) |
| `auraArdente: {dano, tipo, pericia, cd}` | No início do turno de cada criatura adjacente: teste da perícia e dano do tipo (metade se passar) | kallyagrai do fogo (#10) |
| `curaAcelerada: N` | No início do próprio turno, cura N PV se estiver vivo e ferido | barão e duplo cultista (#10) |
| `naturalUm: {efeito, texto}` | Com o efeito indicado ligado, avisa o mestre quando a ficha rola 1 natural num d20 | Stridnix perde a forma dracônica (#11) |
| `aoSerAtingido: {nome, pericia, cd, condicao, soComPVTemp, corpoACorpo}` | Quem acerta a ficha (alvo marcado) testa a perícia; se falhar, recebe a condição | Escudo de Gelo do golem de Nor: Fortitude CD 14 ou enredado, só com os PV temporários e em corpo a corpo (#2) |

Novas revistas acrescentam chaves em `scripts/automacoes.mjs`.

## Como cada revista entra no módulo

Fonte de cada revista: `duelo-de-dragoes/revista-NN/`
- `foundry/gerar-json.py` gera `foundry/atores`, `foundry/itens` e `foundry/tabelas` (molde: Dragão Adulto do compêndio do sistema).
- `foundry/macros/*.js` (macros de script), `foundry/automacoes.json` (flags por ficha), `foundry/diarios/*.md` (um diário por arquivo, ex.: a carta de "E." na revista 3).
- As funções comuns de ficha ficam em `fonte/t20lib.py`; as macros gerais em `fonte/macros-gerais/`.
- `plano-revista-NN.md` vira o diário "Plano do mestre", uma página por seção.
- `assets/{tokens,handouts}` são cópias dos arquivos da pasta do Google Drive.

`python3 foundry-modulo/construir.py` junta tudo em `duelo-de-dragoes/data/revista-NN.json`, copia os assets e gera o zip. Para acrescentar uma revista: crie a pasta dela no mesmo formato e adicione uma linha em `REVISTAS` no `construir.py`.

## Arquivos do módulo

- `module.json`
- `scripts/main.mjs`: registra o menu e a API (`ddd.importarRevista("02")`, `ddd.removerRevista("02")`, `ddd.abrir()`).
- `scripts/importador.mjs` e `scripts/importador-app.mjs`: importação e janela.
- `scripts/automacoes.mjs`: ganchos das automações.
- `data/indice.json` e `data/revista-NN.json`: os pacotes.
- `data/geral.json`: o pacote Ferramentas gerais.
- `assets/revista-NN/`: tokens e handouts.

## Perigos complexos (desde 0.16.0)

Cada perigo complexo com ficha no livro vem como ficha **Perigo Complexo** do sistema (Objetivo, Efeito e Ações, com o texto do livro), na pasta de ameaças da revista: Obter Recursos e Avalanche Menor (#2), Céus Hostis e Deslizamento (#6), Tempestade Fluvial (#8), Jornada para Tyros (#10) e Jornada para Thenarallan (#11). Elas começam escondidas. Para mostrar aos jogadores, clique com o botão direito na ficha, escolha **Configurar permissões** e dê **Observador** a todos (ou arraste o token para a cena). As rolagens continuam nas macros de cada perigo, que ignoram o token do perigo se ele estiver selecionado.

## Testes automáticos (desde 0.15.0)

Antes de cada publicação, uma bateria de testes confere todas as fichas, itens, efeitos, macros, automações, a importação e os compêndios com as regras do sistema Tormenta20, num Foundry simulado. Se algum teste falha, a versão não é publicada. Os testes não substituem abrir o mundo: a aparência das janelas e a convivência com outros módulos só se veem no Foundry.

## O que testar primeiro

1. Importar a revista 2 num mundo de teste e abrir cada ficha (atributos, perícias, Defesa, ataques).
2. Pôr um carcaju numa cena, causar dano e ver se a Ferocidade aparece e some no fim do próximo turno.
3. Pôr 2 cultistas, usar o Sopro de Dragão de um, tentar o do outro (deve avisar) e derrubar um (deve recarregar).
4. Revista 3: rodar **R3 · Servos do dragão** com Cerise selecionada (gasta 2 PM e põe os kobolds), **R3 · Bugulentos** num herói e **R3 · Nevasca** ligando e desligando; arrastar o item **Montaria: Trobo** para um herói e ver se o deslocamento vira 9 m.
5. Revista 4: **R4 · Queda de braço** com um herói e Borr selecionados; **R4 · Escalada do cume** até cair uma vez; **R4 · Pertences de Zaldrin** (precisa do compêndio de tabelas do sistema); atacar um Minotauro da Manada marcado como alvo e ver o efeito Ver Vermelho desligar; atacar com a Turba Zumbi com um herói marcado e conferir o aviso de ×2/×1/×½; ferrão da serpe aplicando a peçonha.
6. Revista 5: **R5 · Desabamento** com os heróis selecionados; **R5 · Perseguição** algumas rodadas até captura ou fuga; mover tokens de puristas lado a lado e ver Lutar em Formação ligar e desligar; deixar o Corcel de Comando com 20 PV ou menos e passar o turno; ligar Montada em Víbora na ficha de Dame Lia.
7. Revista 6: **R6 · Céus Hostis** até 7 sucessos; **R6 · Deslizamento** (1º clique = Iniciativa, depois 5 rodadas) e conferir o efeito Soterrado; **R6 · Longa escalada** até todos chegarem; **R6 · Conexão com Dame Lia** subindo até 3; o Poder Usurpado de Marlinor com os quatro elementos; as formas da Espada Enigma.
8. Revista 7: iniciar combate com um kallyagrai adjacente a um herói e ver a Aura Venenosa no turno do herói; reduzir um kallyagrai a 0 PV e conferir o Fim Tóxico; **R7 · Poça de veneno** com a opção de queda; **R7 · Descanso no covil** duas vezes; ligar Sangue de dragão no Pentalépide.
9. Revista 8: pôr Ária numa cena e reduzir a 0 PV (o token deve virar Aranzirius); baixar Aranzirius para 80 PV (aviso de Tabalinn); **R8 · Deriva da carruagem** com tokens e tile selecionados; **R8 · Tempestade Fluvial** algumas rodadas; **R8 · Testes estendidos do Tritão** e **R8 · Dragoa Sorrateira**.
10. Revista 9: **R9 · Reforços de Moggo** com Moggo selecionado (tokens aparecem em volta); **R9 · Olho do Tigre** com o token de Zakkrad ferido na cena e "Zakkrad vence o terço" (PV cheios de novo); **R9 · Vantagens táticas** com tokens na cena.
11. Revista 10: kallyagrai e um herói adjacentes num combate (Aura Ardente no início do turno do herói); barão ferido (cura 10 no turno dele, aviso a 0 PV); **R10 · Jornada para Tyros**, **R10 · Dado de investigação**, **R10 · Urro na escadaria** e **R10 · Despedida de Lia**.
12. Revista 11: ligar "Forma dracônica" em Stridnix e rolar até sair 1 natural (aviso); atacar um herói marcado com a Revoada de Serpes (aviso de bando) e baixá-la para 500 PV (o ginete foge); **R11 · Jornada para Thenarallan** com os botões Ruínas e Serpes vencidas; **R11 · Stridnix**, **R11 · Suspeita dos rebeldes**, **R11 · Sou rebelde!**, **R11 · Entre os rebeldes** e **R11 · Desfecho em Thenarallan**.

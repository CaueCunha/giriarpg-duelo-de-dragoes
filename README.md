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

## O que testar primeiro

1. Importar a revista 2 num mundo de teste e abrir cada ficha (atributos, perícias, Defesa, ataques).
2. Pôr um carcaju numa cena, causar dano e ver se a Ferocidade aparece e some no fim do próximo turno.
3. Pôr 2 cultistas, usar o Sopro de Dragão de um, tentar o do outro (deve avisar) e derrubar um (deve recarregar).
4. Revista 3: rodar **R3 · Servos do dragão** com Cerise selecionada (gasta 2 PM e põe os kobolds), **R3 · Bugulentos** num herói e **R3 · Nevasca** ligando e desligando; arrastar o item **Montaria: Trobo** para um herói e ver se o deslocamento vira 9 m.
5. Revista 4: **R4 · Queda de braço** com um herói e Borr selecionados; **R4 · Escalada do cume** até cair uma vez; **R4 · Pertences de Zaldrin** (precisa do compêndio de tabelas do sistema); atacar um Minotauro da Manada marcado como alvo e ver o efeito Ver Vermelho desligar; atacar com a Turba Zumbi com um herói marcado e conferir o aviso de ×2/×1/×½; ferrão da serpe aplicando a peçonha.

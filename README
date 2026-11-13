# Musanches Export Preflight — Documentação

Versão 0.1.3 · Plugin para Autodesk Maya

## Visão geral

O Musanches Export Preflight confere a cena do Maya antes do export, corrige o que é seguro corrigir e exporta o FBX com as opções certas para **Unreal Engine 5, Unity ou Substance Painter**. Depois de exportar, ele lê o FBX gravado e confirma se eixo, unidade, UVs, tangentes e skin saíram como o perfil pedia.

A versão atual é a **0.1.3**, testada no Maya 2026.3 (PySide6). Funciona também no Maya 2022–2024 (PySide2), ainda sem teste.

O fluxo é sempre o mesmo:

1. Você escolhe o **perfil** (o destino) e o **escopo** (o que verificar).
2. **Verificar** gera uma lista de problemas, agrupados por categoria e com gravidade.
3. Itens com correção automática podem ser corrigidos com um clique; os demais você resolve no Maya.
4. **Exportar FBX** aplica as opções do perfil e confere o arquivo gravado.

Cada gravidade tem um significado:

| Gravidade | Significa | Exemplo |
| --- | --- | --- |
| Erro (✖) | O export vai sair errado ou a engine vai recusar | Mais de um root de joints para a Unreal |
| Aviso (⚠) | Provavelmente é um problema; confira | Histórico de construção nas malhas |
| Info (ℹ) | Informação ou boa prática, sem efeito direto | Malha com um único UV set (sem lightmap) |

## Instalação

A instalação é copiar dois itens para a pasta de módulos do Maya e reiniciar o Maya. Não há instalador nem dependência externa.

### Requisitos

- Autodesk Maya 2022 ou mais novo. O plugin usa PySide6 no Maya 2025+ e PySide2 no 2022–2024.
- O plugin FBX do Maya ativo (vem ligado por padrão).
- Nenhum pacote Python extra.

### Passo a passo

1. **Baixe e extraia o zip** `musanches-exporter-0.1.3.zip` (ou clone este repositório). Há a pasta `musanches-exporter`, com o arquivo `Musanches.mod` e a pasta `musanches`.
2. **Abra a pasta de módulos do Maya.** Se ela não existir, crie-a:
    - Windows: `Documentos\maya\modules\`
    - macOS: `~/Library/Preferences/Autodesk/maya/modules/`
    - Linux: `~/maya/modules/`
3. **Copie para dentro dela** o arquivo `Musanches.mod` e a pasta `musanches`. Os dois ficam lado a lado, no mesmo nível.
4. **Feche e abra o Maya.** O menu **Musanches** aparece na barra de menus principal.
5. **Abra a ferramenta** em Musanches → Export Preflight….

O resultado final na pasta de módulos deve ser este:

```
modules/
├── Musanches.mod
└── musanches/
    ├── python/musanches_exporter/
    └── scripts/userSetup.py
```

### Se o Documentos estiver no OneDrive

Em muitos computadores a pasta Documentos fica sincronizada com o OneDrive, então o caminho real é algo como `OneDrive\Documentos\maya\modules`. Para descobrir o caminho certo, rode no Script Editor (aba Python):

```python
import maya.cmds as cmds
print(cmds.internalVar(userAppDir=True))   # pasta do usuário do Maya; crie 'modules' dentro dela (ou na pasta pai, 'maya')
```

### Como conferir que deu certo

- O menu **Musanches** aparece ao lado de Help.
- Ou rode no Script Editor:

```python
import musanches_exporter
print(musanches_exporter.__version__)   # deve imprimir 0.1.3
musanches_exporter.show()               # abre a janela
```

### Instalar em outra pasta

Se preferir manter o plugin fora da pasta padrão (por exemplo, numa pasta compartilhada da turma), extraia-o onde quiser e aponte a variável de ambiente `MAYA_MODULE_PATH` para a pasta que contém o `Musanches.mod`. Use `;` entre vários caminhos no Windows e `:` no macOS e Linux.

### Atualizar e desinstalar

- **Atualizar:** feche o Maya, substitua a pasta `musanches` e o `Musanches.mod` pelos novos e abra o Maya de novo. Sem reiniciar, o Maya continua usando o código antigo da memória.
- **Desinstalar:** feche o Maya e apague o `Musanches.mod` e a pasta `musanches`.

## Primeiros passos

Em cinco minutos você verifica, corrige e exporta uma cena.

1. Abra a cena e vá em **Musanches → Export Preflight…**.
2. Em **Perfil**, escolha o destino: Unreal Engine 5, Unity ou Substance Painter.
3. Em **Verificar**, escolha **Cena inteira** para a primeira vez. Para checar só uma parte, selecione os objetos no Maya e use **Só o que está selecionado no Maya**.
4. Clique em **Verificar**. A barra de status no rodapé resume o resultado, por exemplo: `1 erro(s), 7 aviso(s), 2 info (3 com correção automática)`.
5. Leia a lista. Dê **duplo clique** num item para selecionar no Maya os objetos com o problema.
6. Clique em **Corrigir marcados ☑** para aplicar as correções automáticas, ou resolva cada item manualmente.
7. Clique em **Verificar** de novo para confirmar que a lista diminuiu.
8. Clique em **Exportar FBX…**, escolha o nome do arquivo e confira o resultado da verificação pós-export.

Todas as correções são desfeitas com **Ctrl+Z**. Mesmo assim, salve a cena antes de aplicar correções em massa.

## A janela, controle por controle

A janela do Export Preflight tem três faixas: os seletores no topo, a lista de problemas no meio e a barra de status embaixo. Ela pode flutuar ou ser acoplada a um painel do Maya.

### Seletores e botões

| Controle | O que faz | Detalhes |
| --- | --- | --- |
| Perfil | Define o destino e todas as regras da verificação e do export | Unreal Engine 5, Unity, Substance Painter e os perfis que você adicionar. Veja a seção Perfis |
| Verificar: Só o que está selecionado no Maya | Verifica apenas os objetos selecionados e todos os seus filhos | Aceita malhas, joints e grupos. Face ou vértice selecionado vale como o objeto inteiro. Materiais e outros nós que não são objetos de cena são ignorados |
| Verificar: Cena inteira | Verifica tudo o que existe na cena | Ignora a seleção. Se o escopo é "selecionado" e nada está selecionado, o plugin pergunta se deve verificar a cena inteira |
| Verificar (botão) | Roda todas as verificações do perfil e preenche a lista | Com o escopo "selecionado", lê a seleção do Maya no momento do clique |
| Corrigir marcados ☑ | Aplica a correção automática nos itens marcados na lista | Depois de corrigir, verifica de novo. Se alguma correção falhar, a barra de status informa quantas |
| Corrigir tudo (automáticos) | Corrige todos os itens com Correção: Automática, um de cada vez | Verifica de novo após cada correção, porque renomear ou congelar muda os caminhos dos objetos. Ignora as caixas marcadas e desmarcadas: aplica em todos |
| Selecionar no Maya | Seleciona na cena os objetos do item escolhido na lista | Equivale ao duplo clique no item |
| Exportar FBX… | Verifica, pergunta o nome do arquivo e exporta com as opções do perfil | Veja a seção Exportar FBX e relatório |
| Salvar relatório… | Salva o resultado da última verificação em HTML ou JSON | Rode Verificar antes |

### A lista de problemas

Os problemas aparecem agrupados por categoria (Cena, Geometria, UVs, Materiais, Rig, Blend shapes, Nomes). Cada linha tem três colunas:

| Coluna | Conteúdo |
| --- | --- |
| Verificação | O ícone de gravidade (✖ erro, ⚠ aviso, ℹ info), a mensagem e a cor correspondente. Passe o mouse para ver até 30 nomes de objetos afetados |
| Correção | **Automática**: o plugin sabe corrigir, e a linha vem com a caixa ☑ marcada. **Manual (no Maya)**: você precisa resolver no Maya |
| Dica | Por que aquilo importa e o que fazer |

Desmarque a caixa de um item automático para que **Corrigir marcados** não o aplique. Duplo clique numa linha seleciona no Maya os objetos do problema.

### Barra de status

A última linha resume o resultado: número de erros, avisos e infos, quantos têm correção automática, o perfil e o que foi verificado ("cena inteira" ou "3 objeto(s) selecionado(s) no Maya (e filhos)"). Depois de um export, mostra também o nome do arquivo.

## Perfis

Um perfil é um arquivo JSON que diz ao plugin o que o destino espera: unidade, eixo, opções do FBX, limite de UV sets, regras de rig e materiais aceitos. Trocar de perfil troca as regras da verificação e as opções do export.

### Os três perfis incluídos

| Regra | Unreal Engine 5 | Unity | Substance Painter |
| --- | --- | --- | --- |
| Unidade | cm | cm | cm |
| Eixo para cima | Z | Y | Y |
| UV sets (máximo) | 2 | 2 | 1 |
| UVs dentro de 0–1 | exigido | exigido | não exigido |
| UDIM | não | não | permitido |
| Aviso de lightmap | sim | sim | não |
| Root na origem | sim | não | não se aplica |
| Root sem rotação | sim | sim | não se aplica |
| Influências por vértice (máximo) | 8 | 4 | não se aplica |
| Exporta skin e blend shapes | sim | sim | não |
| Shaders aceitos | Lambert, Blinn, Phong, PhongE, StingrayPBS, ShaderFX | Lambert, Blinn, Phong, PhongE, StingrayPBS, ShaderFX | Lambert, Blinn, Phong, PhongE, StingrayPBS |
| Verificações de rig e blend shape | ativas | ativas | desligadas |

Nos três perfis o FBX sai com grupos de suavização, tangentes e triangulação ligados.

### Criar o seu perfil

1. Crie uma pasta para os seus perfis, por exemplo `C:\Musanches\perfis`.
2. Dentro dela, crie um arquivo `.json` (o nome do arquivo é a chave do perfil, por exemplo `meu_projeto.json`).
3. Aponte a variável de ambiente `MUSANCHES_PROFILES` para essa pasta. Para várias pastas, separe por `;` no Windows e `:` no macOS e Linux.
4. Reinicie o Maya. O perfil aparece na lista **Perfil**.

Campos que você omitir herdam os valores padrão. Um perfil com o mesmo nome de arquivo de um perfil incluído o substitui.

```json
{
  "name": "Meu projeto",
  "units": "cm", "up_axis": "z",
  "fbx": {"convert_unit": "cm", "up_axis": "z", "triangulate": true, "tangents": true},
  "uv": {"max_sets": 2, "allow_udim": false},
  "rig": {"max_influences": 8, "root_at_origin": true},
  "disabled_checks": ["rig.bind_pose"]
}
```

### Todos os campos do perfil

| Campo | Padrão | O que controla |
| --- | --- | --- |
| `name` | "Perfil sem nome" | Nome mostrado na lista Perfil e nos relatórios |
| `units` | `cm` | Unidade linear esperada na cena (`mm`, `cm`, `m`, `in`, `ft`, `yd`) |
| `up_axis` | `y` | Eixo para cima esperado na cena (`y` ou `z`) |
| `fbx.convert_unit` | `cm` | Unidade gravada no FBX (Convert Units To) |
| `fbx.up_axis` | `y` | Eixo para cima gravado no FBX |
| `fbx.version` | `FBX201800` | Versão do arquivo FBX |
| `fbx.smoothing_groups` | ligado | Grava grupos de suavização |
| `fbx.tangents` | ligado | Grava tangentes e binormais (só saem em malhas trianguladas) |
| `fbx.triangulate` | ligado | Triangula as malhas no export |
| `fbx.skins` | ligado | Exporta skin |
| `fbx.shapes` | ligado | Exporta blend shapes |
| `fbx.constraints` | desligado | Exporta constraints |
| `fbx.input_connections` | desligado | Exporta conexões de entrada |
| `fbx.embed_media` | desligado | Embute as texturas dentro do FBX |
| `fbx.bake_animation` | desligado | Faz bake da animação usando o intervalo do timeline |
| `fbx.cameras`, `fbx.lights` | desligado | Exporta câmeras e luzes |
| `fbx.ascii` | desligado | Grava o FBX em texto em vez de binário |
| `fbx.verify_after_export` | ligado | Confere o arquivo gravado depois do export |
| `naming.pattern` | `^[A-Za-z0-9_]+$` | Expressão regular dos nomes válidos |
| `geometry.zero_area_tolerance` | 0,000001 | Área abaixo da qual a face conta como degenerada |
| `geometry.transform_tolerance` | 0,0001 | Diferença aceita de zero (posição, rotação) e de 1 (escala) |
| `uv.max_sets` | 2 | Máximo de UV sets com dados |
| `uv.require_0_1` | ligado | Exige UVs dentro de 0–1 no primeiro set |
| `uv.allow_udim` | desligado | Aceita UVs fora de 0–1 (UDIM) |
| `uv.want_lightmap_set` | ligado | Informa malhas com um único UV set |
| `rig.single_root` | ligado | Exige um único joint root |
| `rig.root_at_origin` | ligado | Exige o root na origem |
| `rig.root_zero_rotation` | ligado | Exige o root sem rotação |
| `rig.max_influences` | 4 | Máximo de influências por vértice |
| `rig.warn_non_unit_scale` | ligado | Avisa joints com escala diferente de 1 |
| `materials.allowed` | Lambert, Blinn, Phong, PhongE, StingrayPBS, ShaderFX | Tipos de shader aceitos |
| `materials.check_textures` | ligado | Confere se os arquivos de textura existem |
| `blendshapes.warn_inbetweens` | ligado | Avisa blend shapes com inbetweens |
| `blendshapes.require_pre_deformation` | ligado | Exige blend shape avaliado antes do skin |
| `disabled_checks` | vazio | Lista de ids de verificações a desligar (veja a referência das verificações) |

## Referência das verificações

São 17 verificações mais as conferências pós-export. Cada uma tem um **id**, que você usa em `disabled_checks` para desligá-la num perfil. A coluna Correção diz se o botão de correção resolve sozinho.

### Cena

| Id | O que verifica | Gravidade | Correção |
| --- | --- | --- | --- |
| `scene.units` | A unidade linear da cena é a que o perfil espera. Unidade errada é a principal causa de objetos 100 vezes maiores ou menores na engine | Aviso | Automática: troca a unidade da cena para a do perfil. O tamanho físico dos objetos não muda; mudam os valores exibidos |
| `scene.up_axis` | O eixo para cima da cena (Y ou Z) em relação ao do perfil | Info | Manual. O exportador converte o eixo no FBX; confira o root do rig depois |

### Geometria

| Id | O que verifica | Gravidade | Correção |
| --- | --- | --- | --- |
| `geometry.history` | Malhas com histórico de construção | Aviso | Automática: apaga só o histórico não deformador. Skin e blend shapes são preservados |
| `geometry.transforms` | Objetos com posição, rotação ou escala diferentes de 0/0/1. Malhas com skin são ignoradas | Aviso | Automática: Freeze Transformations |
| `geometry.transforms` (escala negativa) | Objetos espelhados (número ímpar de eixos com escala negativa); as normais podem sair invertidas | Erro | Automática: congela e depois reverte as normais dos objetos espelhados |
| `geometry.faces` (área zero) | Faces com área quase zero, que geram normais e tangentes inválidas. O Substance costuma reclamar | Aviso | Manual: Mesh → Cleanup, ou una os vértices sobrepostos preservando os UVs |
| `geometry.faces` (n-gons) | Faces com mais de 4 lados | Info se o perfil triangula no export; Aviso se não | Manual: triangule antes do bake para que o Painter e a engine usem a mesma triangulação |

### UVs

| Id | O que verifica | Gravidade | Correção |
| --- | --- | --- | --- |
| `uv.sets` (sem UVs) | Malha sem nenhum UV | Erro | Manual |
| `uv.sets` (sets vazios) | UV sets vazios além do principal; o FBX os descarta e isso desloca os canais na engine | Aviso | Automática: apaga os sets vazios |
| `uv.sets` (excesso) | Mais UV sets com dados do que o perfil permite | Aviso | Manual |
| `uv.sets` (lightmap) | Malha com um único UV set, quando o perfil quer um set de lightmap | Info, uma vez para o conjunto de malhas | Manual: crie o UV1 ou gere o lightmap na importação da engine |
| `uv.sets` (fora de 0–1) | Malhas com UVs fora de 0–1 no primeiro set, quando o perfil exige e não permite UDIM. Tolerância de 0,01 | Aviso, uma vez para o conjunto | Manual. Se o UDIM é proposital, use o perfil do Substance |

### Materiais

| Id | O que verifica | Gravidade | Correção |
| --- | --- | --- | --- |
| `materials.shader_type` | Shaders de um tipo que o perfil não aceita (por exemplo, Arnold aiStandardSurface ou standardSurface). O FBX não carrega esses shaders | Erro | Automática: troca por um Lambert com o mesmo nome, preservando a cor base ou a textura de cor. Os demais mapas não são copiados |
| `materials.shader_type` (material padrão) | Malhas com o material padrão do Maya (como `openPBR_shader1`), que é somente leitura | Aviso | Automática: cria um Lambert novo chamado `<nome>_lambert`, copia a cor base e reatribui as malhas |
| `materials.textures` | Nós `file` que apontam para uma textura inexistente. Cobre a cena toda, não só o escopo | Aviso | Manual: corrija o caminho em File Path Editor |

O nome do material vira o nome do texture set no Substance Painter. Por isso a correção mantém o nome original.

### Rig

| Id | O que verifica | Gravidade | Correção |
| --- | --- | --- | --- |
| `rig.single_root` | Mais de uma hierarquia de joints. A Unreal só aceita um root por skeletal mesh | Erro | Manual: parenteie tudo a um único joint root |
| `rig.root_transform` | Root fora da origem (quando o perfil exige) ou com rotação aplicada | Aviso | Manual: passe a rotação para o Joint Orient; rotação no root costuma virar giro de 90° na engine |
| `rig.joint_scale` | Joints com escala diferente de 1 | Aviso | Manual. Escala em joints se comporta de outro jeito fora do Maya (Scale Compensate) |
| `rig.max_influences` | Vértices com mais influências de joint do que o perfil permite (padrão 4; Unreal 8) | Aviso | Manual: Skin → Prune Small Weights ou limite as influências no skin |
| `rig.bind_pose` | Malha com skin sem bind pose conectada | Aviso | Manual. A UE5 (Interchange) avisa de bind pose inválida |
| `rig.constraints` | Constraints na cena, que o FBX não leva para a engine | Info | Manual. Em jogos, armas e acessórios costumam virar sockets criados na engine |

### Blend shapes

| Id | O que verifica | Gravidade | Correção |
| --- | --- | --- | --- |
| `blendshapes.order` | Blend shape avaliado depois do skin (pós-deformação). Há relatos de blend shapes quebrando no FBX nesse caso | Aviso | Manual: recrie o blend shape com a ordem pré-deformação |
| `blendshapes.inbetweens` | Blend shapes com inbetweens. Relatos indicam que a Unreal ignora os inbetweens do Maya | Aviso | Manual |

### Nomes

| Id | O que verifica | Gravidade | Correção |
| --- | --- | --- | --- |
| `naming.names` | Nomes de malhas, joints e materiais fora do padrão do perfil (espaços, acentos, namespaces) | Aviso | Automática: troca cada caractere inválido por `_` e retira os namespaces |

### Conferência pós-export

Depois de exportar, o plugin grava uma cópia ASCII temporária do mesmo FBX, lê o que foi realmente gravado e compara com o perfil. Esses resultados entram na mesma lista.

| Id | O que confere | Gravidade |
| --- | --- | --- |
| `verify.up_axis` | O eixo para cima gravado no FBX é o do perfil | Erro |
| `verify.unit` | A unidade gravada (UnitScaleFactor) bate com o perfil | Aviso |
| `verify.uv` | Cada malha foi gravada com UVs; UV sets perdidos | Erro se não há UVs, Aviso se perdeu sets |
| `verify.tangents` | Tangentes gravadas, quando o perfil pede. Só saem em malhas trianguladas | Aviso |
| `verify.normals` | Todas as malhas têm normais gravadas | Aviso |
| `verify.skin` | O skin foi gravado, quando o perfil exporta skin | Aviso |

### Como desligar uma verificação

Adicione o id em `disabled_checks` no perfil. Exemplo: `"disabled_checks": ["rig.bind_pose", "scene.up_axis"]`. Os ids dos grupos com várias linhas (como `uv.sets`) são desligados por inteiro; não há id separado por subitem.

## Exportar FBX e relatório

### Exportar FBX

O botão **Exportar FBX…** faz tudo em sequência:

1. Roda a verificação com o perfil e o escopo escolhidos e mostra o resultado na lista.
2. Se houver **erros**, pergunta "Exportar mesmo assim?". Responda Não para voltar e corrigir.
3. Abre a janela para escolher o nome do arquivo `.fbx`.
4. Reinicia as opções do exportador FBX do Maya e aplica as do perfil: eixo, unidade, versão do arquivo, grupos de suavização, tangentes, triangulação, skin, blend shapes e as demais opções do perfil.
5. Exporta. Com o escopo "selecionado", exporta só os objetos verificados (e seus filhos); com "Cena inteira", exporta a cena toda.
6. Se `fbx.verify_after_export` está ligado, grava uma cópia ASCII temporária, lê o que foi realmente gravado, compara com o perfil e acrescenta o resultado à lista. A cópia é apagada em seguida.

A seleção que você tinha no Maya é restaurada ao final. As opções do exportador FBX do Maya ficam com os valores do perfil depois do export; se você exporta FBX por outros caminhos, o Maya mantém essas opções até você mudá-las.

O plugin **não corrige a cena sozinho** ao exportar: ele apenas avisa. Corrija antes, com os botões de correção.

### Salvar relatório

O botão **Salvar relatório…** grava o resultado da última verificação. Rode **Verificar** antes. O formato vem da extensão do arquivo.

| Formato | Conteúdo | Para que serve |
| --- | --- | --- |
| `.html` | Tabela com nível, categoria, mensagem, objetos afetados e dica, mais o perfil, o escopo e a data | Entregar a um colega, anexar a uma tarefa ou guardar como registro |
| `.json` | Os mesmos dados: perfil, escopo, data, contagem por gravidade e a lista de problemas com id da verificação, gravidade, mensagem, objetos, se tem correção automática e dica | Ler por script ou comparar relatórios de entregas diferentes |

## Linha de comando e uso por script

O plugin também roda sem janela, para validar muitas cenas de uma vez (por exemplo, entregas de alunos) ou dentro de um pipeline. **Este modo ainda não foi testado no Maya real**; teste com uma cena sua antes de depender dele.

### Pelo terminal (mayapy)

```
mayapy -m musanches_exporter.cli cena.ma --profile unreal5 --report relatorio.html
```

O `mayapy` fica na pasta `bin` da instalação do Maya. O comando só acha o plugin se o módulo estiver instalado ou se a pasta `musanches/python` estiver no `PYTHONPATH`.

| Opção | O que faz |
| --- | --- |
| `cena` (obrigatório) | Caminho da cena `.ma` ou `.mb` |
| `--profile CHAVE` | Perfil a usar: `unreal5` (padrão), `unity`, `substance` ou a chave de um perfil seu |
| `--report ARQUIVO` | Salva o relatório; `.html` ou `.json` |
| `--fix` | Aplica as correções automáticas, uma por vez, e verifica de novo. **Altera a cena na memória, mas não a salva** |
| `--export ARQUIVO.fbx` | Exporta a cena inteira para esse FBX. A exportação é ignorada se a verificação tem erros |

O terminal imprime uma linha de resumo e uma linha por problema. O código de saída é **0** quando não há erros e **1** quando há erros, o que permite usar o comando em scripts e em integração contínua.

No modo de linha de comando a verificação sempre cobre a cena inteira.

### Dentro do Maya (Script Editor)

A função `check` roda as verificações e devolve o relatório, sem abrir a janela:

```python
import musanches_exporter as mx

report = mx.check("unity", selection_only=True)   # perfil e escopo
print(report.counts())      # {'erro': 0, 'aviso': 2, 'info': 1}
print(report.has_errors)    # True se houver algum erro
report.save("C:/temp/relatorio.html")   # ou .json
print(report.to_json())
```

| Chamada | O que faz |
| --- | --- |
| `mx.show()` | Abre a janela |
| `mx.check(perfil, selection_only)` | Verifica e devolve um relatório. `perfil` é a chave (`"unreal5"`, `"unity"`, `"substance"`). Com `selection_only=True`, usa os objetos selecionados no Maya |
| `report.counts()` | Contagem de erros, avisos e infos |
| `report.has_errors` | Verdadeiro se houver pelo menos um erro |
| `report.issues` | Lista dos problemas, cada um com `check_id`, `category`, `severity`, `message`, `nodes`, `fixable` e `hint` |
| `report.save(caminho)` | Salva em HTML ou JSON, conforme a extensão |
| `report.to_json()`, `report.to_html()` | Devolvem o relatório como texto |

## Solução de problemas e limitações

### Problemas comuns

| Sintoma | Causa provável | O que fazer |
| --- | --- | --- |
| O menu Musanches não aparece | O `.mod` ou a pasta `musanches` estão no lugar errado, ou o Maya não foi reiniciado | Confira que os dois estão lado a lado em `modules/`. Abra pelo Script Editor com `import musanches_exporter; musanches_exporter.show()`. Se der erro de importação, o módulo não foi encontrado |
| "Nenhum objeto está selecionado no Maya" | O escopo é "Só o que está selecionado" e a seleção está vazia | Responda Sim para verificar a cena inteira, ou selecione objetos na viewport ou no Outliner |
| "A seleção atual não tem objetos de cena" | O que está selecionado é um material ou outro nó que não é objeto | Selecione malhas, joints ou grupos |
| "Nenhum item marcado para correção" | Nenhuma linha com Correção: Automática está marcada ☑ | Clique em Verificar e marque os itens automáticos. Itens Manual não têm correção pelo botão |
| A janela mostra textos antigos depois de atualizar | O Maya manteve o código antigo na memória | Feche e abra o Maya |
| "N correção(ões) falharam" na barra de status | Uma correção encontrou algo que não sabe tratar | Verifique de novo; o item continua na lista. Resolva-o manualmente e veja o Script Editor para a mensagem de erro |
| O objeto chega 100 vezes maior ou menor na engine | Unidade da cena diferente da do perfil | Veja `scene.units` na lista; a correção troca a unidade, e a conferência pós-export valida `UnitScaleFactor` |
| O personagem chega deitado ou girado | Eixo para cima ou rotação aplicada no root | Veja `scene.up_axis` e `rig.root_transform`; confira o root depois do export |
| A textura não aparece na engine | O FBX não leva shaders Arnold nem os mapas além da cor base | Use a correção de `materials.shader_type` e refaça os mapas no Substance ou na engine |
| A verificação de texturas lista nós de fora do escopo | `materials.textures` olha a cena toda | É uma limitação conhecida; ignore os nós que não são dos objetos verificados |

As correções são desfeitas com **Ctrl+Z**: cada correção é um passo de undo separado. Ao usar **Corrigir tudo** podem ser vários passos; desfaça até voltar ao estado desejado.

### O que foi testado

Testado no Maya 2026.3: instalação como módulo, menu, janela, os três perfis, o escopo "selecionado" e "cena inteira", as correções de unidade, histórico, transformações, UV sets vazios, material e nomes, e a exportação FBX com conferência.

Ainda não testado: a linha de comando (mayapy), texturas UDIM reais do Substance, o perfil Unreal num rig completo e o plugin no Maya 2022–2024 (PySide2).

### Limitações conhecidas

- A exportação usa a seleção ou a cena de trabalho diretamente. Não há uma cópia "proxy" para corrigir sem tocar na cena.
- A conversão de materiais leva só a cor base ou a textura de cor. Normal, rugosidade, metal e outros mapas não são copiados.
- Não implementados: orientação consistente dos joints, sobreposição de UV no lightmap, vértices sobrepostos e checagem de UDIM por tile.
- A verificação de texturas cobre a cena toda, não só o escopo.
- Algumas dicas vêm de relatos de fóruns e da documentação da Epic e da Autodesk e podem estar defasadas na UE5 com Interchange.
- O plugin não substitui o ajuste final na engine: confira escala, root e materiais depois de importar.

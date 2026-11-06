# NekoWorld RPG — versão 3.6.0

Atualizado conforme os PDFs oficiais (sistema e raças): 25 raças, 14 classes, 8 elementos com 42 magias, armas e regras.

Projeto web estático. Abra `index.html` no navegador ou publique a pasta no GitHub Pages.
O visual usa o Tailwind via CDN, então é preciso estar online na primeira abertura de cada página.

## Estrutura

```
index.html              página inicial
pages/                  uma página por assunto (lore, raças, classes, magias, ficha...)
css/style.css           ajustes globais (foco, impressão, acessibilidade)
js/main.js              menu e rodapé, iguais em todas as páginas
js/tailwind-config.js   cores do tema
js/data/racas.js        dados das raças (lore, atributos, habilidades)
js/data/classes.js      dados das classes (status base, recursos, características)
js/data/magias.js       dados dos elementos e magias
js/racas.js             lógica da página de raças
js/classes.js           lógica da página de classes
js/magias.js            lógica da página de magias (filtro por elemento e busca)
js/ficha-core.js        raças, fórmulas, cálculo da ficha e lista de personagens (usado por Personagens, Ficha e Combate)
js/personagens.js       página Meus Personagens: lista, criar, duplicar, excluir, foto, exportar/importar
js/ficha.js             página Ficha: calculadora, inventário, dados, backup
js/combate.js           página Combate: barras de HP/PE/Mana/Sanidade, Esquiva e perícias
```

## Como editar

- **Menu:** edite a lista `NAV` no começo de `js/main.js`. Vale para o site inteiro.
- **Cores:** `js/tailwind-config.js`.
- **Uma raça:** `js/data/racas.js`. Para ilustrar, preencha o campo `img` (ex.: `"../assets/images/elfos.png"`).
  Cada raça tem link próprio: `pages/racas.html#elfos`.
- **Uma magia ou elemento:** `js/data/magias.js` (cada elemento tem uma lista de magias).
- **Uma classe:** `js/data/classes.js`. A página de Classes e a ficha usam o mesmo arquivo.
- **Bônus de atributo das raças na ficha:** lista `RACES` em `js/ficha-core.js`.
- **Fórmulas da ficha:** objeto `FORMULAS` em `js/ficha-core.js`.
- **Páginas de lore:** `pages/lore-*.html` já têm título e introdução; só `lore-divindades.html` ainda está em construção (o PDF não traz material sobre divindades).
- **Nova página:** copie uma página existente, mantenha `<div id="site-header"></div>`, o `<script src="../js/main.js">`
  logo abaixo e `<div id="site-footer"></div>` no fim, e adicione-a na lista `NAV`.

## Meus Personagens

`pages/personagens.html` lista os personagens salvos neste navegador (até 15, ajustável por `MAX_CHARS` em `js/ficha-core.js`).
Cada cartão tem **Acessar Ficha**, **Combate** e um menu ⚙ (foto, duplicar, exportar JSON, excluir). **Importar JSON** cria um personagem novo a partir de um backup.
A ficha e o combate abrem por `ficha.html?p=<id>` / `combate.html?p=<id>`; o botão **Combate** do menu abre o último personagem usado.
Quem tinha uma ficha única da versão anterior a vê aparecer automaticamente como o primeiro personagem.

## Ficha automática

A ficha (`pages/ficha.html`) calcula atributos totais, HP, PE, Mana/Fé, Sanidade e Esquiva.
Também guarda o inventário, rola dados, faz backup em JSON e imprime.

- Fórmulas: HP = base + Vigor×5 · PE = base + Força×4 · Mana = base + Int×5 · Fé (Paladino e Clérigo) = base + Int×3 ·
  Sanidade = 20 + Carisma×5 · Esquiva = base + ⌊Agilidade÷2⌋.
- O campo **Bônus raciais livres** cobre raças com escolha de atributo (Vanara, Growkies, Selakians, Plantídeos...).

## Combate

A página **Combate** (`pages/combate.html`, atalho no menu) lê a ficha salva e mostra:

- **Controle de combate:** barras de HP, PE, Mana/Fé e Sanidade, Esquiva e recurso de classe livre. Altere com os botões −5/−1/+1/+5 ou digitando.
  As barras são só indicadores (sem campo por cima), então passar o dedo nelas não muda o valor.
- **Perícias:** tabela com 32 perícias + 3 especiais, Treino (+2 escolhidas, +5 de classe automático), Outros e rolagem de d20 conforme o atributo. Lista `SKILLS` e bônus de classe `CLASS_SKILLS` em `js/combate.js`.

Se a ficha mudar, os máximos se atualizam na página de Combate (quando o valor atual estava cheio, ele acompanha o novo máximo).

Tudo é salvo no navegador (localStorage). Use **Exportar JSON** na ficha para backup ou para levar o personagem a outro aparelho.

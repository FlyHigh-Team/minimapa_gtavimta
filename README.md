# 🗺️ Minimapa GPS para MTA:SA

Minimapa no estilo navegador GPS para **Multi Theft Auto: San Andreas**, inspirado nos GPS do GTA V e GTA VI. Ele mostra o nome da rua, um card com a próxima manobra e um mapa escuro que gira com a câmera. A rota rosa segue as ruas reais do jogo.

**Desenvolvedor:** Daniel Developer

https://imgur.com/a/fkyT7RA

---

## ✨ Recursos

- **Rota pelas ruas:** o caminho é calculado com A* sobre a malha de ruas do próprio GTA SA e respeita ruas de **mão única**.
- **Recalcula sozinho:** se você sair da rota (mais de 60 m), um novo caminho é calculado.
- **Card de navegação:** "Vire à direita", "Vire à esquerda", "Faça o retorno", "Siga em frente" e "Você chegou", com a distância até a próxima manobra.
- **Distância total** até o destino, mostrada no canto do mapa.
- **Mapa escuro e dessaturado** (via shader), que gira acompanhando a câmera.
- **Zoom dinâmico:** o mapa se afasta conforme a velocidade.
- **Destino em rosa:** a bolinha fica presa na borda quando o destino está fora da tela.
- **Bússola (N)**, blips do servidor e seta do jogador.
- **Nome da zona** onde o jogador está.
- **Cálculo sem travar o jogo:** a rota é processada aos poucos, em vários frames.
- **Painel de configurações** para cada jogador:
  - **Tamanho:** de 60% a 160%
  - **Formato:** redondo, quadrado ou retangular
  - **Opacidade:** de 20% a 100%
  - As preferências ficam salvas no computador do jogador.

---

## 📦 Instalação

1. Baixe ou clone este repositório.
2. Coloque a pasta `minimapa_gps` dentro de:
   ```
   MTA San Andreas 1.6/server/mods/deathmatch/resources/
   ```
3. No console do servidor, rode:
   ```
   refresh
   start minimapa_gps
   ```
4. Para iniciar junto com o servidor, adicione no `mtaserver.conf`:
   ```xml
   <resource src="minimapa_gps" startup="1" protected="0" />
   ```

**Requisito:** MTA:SA 1.5.6 ou superior.

---

## 🎮 Como usar

| Ação | Como fazer |
|---|---|
| Marcar destino | Abra o mapa (**F11**) e clique com o **botão direito** |
| Abrir configurações | **F6** ou `/configmapa` |
| Definir destino por coordenadas | `/gps x y [z]` |
| Remover destino | `/limpargps` |
| Ligar/desligar o minimapa | `/minimapa` |

---

## 🧩 Exports (client-side)

Outros resources podem controlar o GPS:

```lua
-- Define um destino (a rota é calculada pelas ruas)
exports.minimapa_gps:setDestination(x, y, z)

-- Remove o destino atual
exports.minimapa_gps:clearDestination()

-- Usa uma rota pronta (lista de pontos)
exports.minimapa_gps:setRoute({ {x1, y1}, {x2, y2}, {x3, y3} })

-- Retorna o destino atual (x, y, z) ou false
local x, y, z = exports.minimapa_gps:getDestination()
```

---

## ⚙️ Personalização

As opções fixas ficam na tabela `CFG`, no início do `client.lua`:

| Opção | Descrição |
|---|---|
| `units` | `"metric"` (m/km) ou `"imperial"` (pés/mi) |
| `routeColor`, `routeGlow`, `routeCore` | Cores da linha da rota |
| `accentColor`, `destColor` | Cor de destaque do card e do destino |
| `zoomNear`, `zoomFar`, `zoomSpeed` | Comportamento do zoom |
| `rerouteDistance` | Distância (m) fora da rota para recalcular |
| `arriveDistance` | Distância (m) para considerar que chegou |
| `settingsKey` | Tecla do painel de configurações (padrão `F6`) |

No `gps.lua`, `HIGHWAY_FACTOR` controla o quanto as rodovias são preferidas (menor que 1 = prefere rodovias).

---

## 📁 Estrutura

```
minimapa_gps/
├── meta.xml          # Definição do resource
├── client.lua        # Minimapa, card, painel de configurações
├── gps.lua           # Pathfinding (A*) pelas ruas
├── files/
│   ├── map.png       # Imagem do mapa de San Andreas
│   ├── nodes.dat     # Malha de ruas (gerada dos NODES*.DAT do GTA SA)
│   ├── map.fx        # Shader do mapa escuro
│   ├── mask.fx       # Shader de cantos arredondados / círculo
│   └── blips/        # Ícones dos blips
├── docs/
│   └── preview.png   # Imagem usada no README
└── tools/
    ├── build.js      # Conversor NODES*.DAT -> nodes.dat
    └── lib.js
```

---

## 🛠️ Regenerar a malha de ruas (opcional)

Só é necessário se você usar um mod que altere as ruas do jogo. Precisa do [Node.js](https://nodejs.org/). Rode dentro da pasta do resource:

```bash
node tools/build.js "C:/caminho/do/GTA San Andreas/data/Paths" files/nodes.dat
```

---

## ⚠️ Limitações

- A malha de ruas é a mesma que os carros do GTA usam. Alguns becos e estacionamentos não fazem parte dela, e nesses lugares a rota termina na rua mais próxima.
- A imagem do mapa tem 2048×2048 pixels e pode ficar um pouco borrada no zoom mais próximo.

---

## 📜 Créditos

- **Desenvolvimento:** Daniel Developer
- **Imagem do mapa e ícones de blips:** vêm do cliente do Multi Theft Auto.
- **Dados das ruas:** extraídos dos arquivos de paths do *Grand Theft Auto: San Andreas* (© Rockstar Games).

# tournament_boosters_calculador
Uma ferramenta para calcular a distribuição de boosters (premiação) em torneios de card games, garantindo uma divisão que se adapta a diferentes categorias e quantidades de jogadores.

## Tipos de Calculadoras

O sistema conta com 4 tipos de calculadoras, cada uma desenvolvida para um formato específico de torneio. Abaixo estão a explicação de cada uma, seus inputs e as regras de cálculo.

---

### 1. Calculadora para Torneios Gerais (Distribuição para o Top)
**O que é:** Ideal para torneios mais competitivos. Esse modelo junta toda a premiação gerada pelas inscrições e foca na distribuição entre os jogadores que alcançarem as melhores colocações (como o Top 4 ou Top 8).

**Inputs (Como usar):**
- `Inscritos Masters`, `Seniors` e `Juniors`: Quantidade de jogadores inscritos em cada categoria. A soma deles forma o total de jogadores, e a proporção de cada um define qual a fatia do pool de premiação destinada a cada grupo.
- `Valor da Inscrição (R$)`: O valor cobrado de cada jogador para participar do evento. Serve para calcular a arrecadação bruta do torneio.
- `Preço do Booster (R$)`: O custo ou valor de repasse unitário do booster. É utilizado para converter o saldo do fundo de premiação financeiro na quantidade física de boosters do Pool.
- `Staff e Juízes (%)`: A porcentagem da arrecadação total que será deduzida para pagamento da equipe (staff/juízes) antes de gerar o Pool de prêmios.

**Como é feito o cálculo:**
Primeiro, a calculadora faz a **Arrecadação Total** multiplicando o número de jogadores pelo valor da inscrição. Depois, ela separa a parte da equipe (**Staff/Juízes**) e utiliza o valor que sobrou no fundo para dividir pelo **Preço do Booster**. É assim que chegamos ao **Pool Total** de boosters. A partir daí, dividimos esse total entre as categorias (Master, Senior e Junior), seguindo a distribuição abaixo com base em quantos jogadores cada uma tem (já cuidando dos arredondamentos de forma automática):

* **1 Jogador:** 100% do pool.
* **2 a 3 jogadores (focado no Top 2):**
  - **1º Lugar:** ~70%
  - **2º Lugar:** ~30%
  - **3º Lugar:** *Se houver 3 jogadores e saldo, tira-se 1 booster do 1º colocado para dar ao 3º.*
* **4 Jogadores (Focado no Top 3):**
  - **1º Lugar:** 50%
  - **2º Lugar:** 30%
  - **3º Lugar:** 20%
* **5 a 7 jogadores (focado no Top 4):**
  - **1º Lugar:** ~43.7%
  - **2º Lugar:** ~23.9%
  - **3º e 4º Lugares:** ~11% cada
* **8 ou mais jogadores (Corte para Top 8):**
  - **1º Lugar:** ~43.7%
  - **2º Lugar:** ~23.9%
  - **3º e 4º Lugares:** ~11% cada
  - **5º ao 8º Lugares:** ~2.6% cada

* **Correção de Arredondamento:** Se os arredondamentos tentarem distribuir mais boosters do que o *Pool* permite, o algoritmo deduz essa diferença das premiações menores até que o montante iguale exatamente o limite financeiro disponível.*

---

### 2. Calculadora para Formato Pré-release (Participação + Top)
**O que é:** Para eventos como o pré-release. A ideia aqui é garantir que todo mundo volte para casa com pelo menos um prêmio só por ter participado, mas ainda assim guardando uma quantidade de boosters para recompensar os melhores colocados.

**Inputs (Como usar):**
- `Inscritos Masters`, `Seniors` e `Juniors`: Quantidade de jogadores em cada categoria. A proporção define como a premiação do Top será fatiada.
- `Total de Boxes` / `Total de Boosters (Pool)`: A quantidade máxima em estoque reservada para o evento. Os campos são sincronizados (1 Box = 36 boosters).
- `Boosters de participação (por pessoa)`: Quantos boosters cada jogador recebe garantidos apenas por jogar.
- `Juízes/Staff (Total Boosters)`: Quantidade de boosters retirada do montante total para pagamento da equipe (diferente de torneios gerais, aqui desconta-se em boosters físicos em vez de R$).
- `Promoção Traga um Amigo (Total Boosters)`: Quantidade de boosters separada previamente para sorteios ou campanhas.

**Como é feito o cálculo:**
1. **Destinos Fixos:** Antes de tudo, separamos os boosters que já estão prometidos. O sistema pega a quantidade de jogadores e multiplica pelos `Boosters de participação` e depois soma isso com os boosters que vão para os juízes e para as promoções.
2. **Pool dos Vencedores (Top Global):** Com os prêmios fixos separados, a gente tira esse valor do `Total de Boosters`. O que sobrar é considerado o fundo de premiação para os vencedores.
3. **Distribuição Proporcional (O Top Cut):** O pool dos vencedores é distribuído entre as categorias (Master, Senior e Junior), dependendo de quantos inscritos cada uma teve. O sistema cuida dos arredondamentos para não seja possível ganhar "meio booster" e corrige os valores para a conta funcione:
   * **1 Jogador:** 100% do pool da categoria.
   * **2 a 3 jogadores (focado no Top 2):**
     - **1º Lugar:** ~70%
     - **2º Lugar:** ~30%
     - **3º Lugar:** *Se houver 3 jogadores e saldo, tira-se 1 booster do 1º colocado para dar ao 3º.*
   * **4 Jogadores (Focado no Top 3):**
     - **1º Lugar:** 50%
     - **2º Lugar:** 30%
     - **3º Lugar:** 20%
   * **5 a 7 jogadores (focado no Top 4):**
     - **1º Lugar:** ~43.7%
     - **2º Lugar:** ~23.9%
     - **3º e 4º Lugares:** ~11% cada
   * **8 ou mais jogadores (Corte para Top 8):**
     - **1º Lugar:** ~43.7%
     - **2º Lugar:** ~23.9%
     - **3º e 4º Lugares:** ~11% cada
     - **5º ao 8º Lugares:** ~2.6% cada

   * **Correção de Arredondamento:** Se os arredondamentos para cima tentarem distribuir mais boosters do que o *Pool* da categoria permite, o algoritmo deduz a diferença das premiações menores até que o fechamento chegue exatamente no estoque reservado.
4. **Sobras de Estoque:** No final, a calculadora checa se a soma de tudo o que foi distribuído bate com o seu estoque inicial e mostra direitinho quantos boosters sobraram (ou te avisa se você tentar dar mais prêmios do que realmente tem disponíveis).

---

### 3. Calculadora de Distribuição por Pontuação (Proporcional Automática)
**O que é:** Ideal para organizadores que preferem premiar com base na performance real de cada jogador (pontuação final) em vez de focar apenas no Top Cut. Distribuição: quem faz mais pontos ao longo das rodadas leva uma fatia maior do pool de boosters.

**Inputs (Como usar):**
- `Inscritos Masters`, `Seniors` e `Juniors`: A quantidade total de jogadores define automaticamente o número de rodadas do torneio (Regras de Suíço).
- `Valor da Inscrição`, `Preço do Booster` e `Staff e Juízes (%)`: Seguem a mesma lógica da calculadora 1 para formar o fundo de premiação ("Pool").
- `Tabela de Pontuação`: A calculadora gera automaticamente as faixas de vitórias com base no número de rodadas. Você só precisa digitar na coluna **"Qtd de Jogadores"** quantas pessoas terminaram com aquela pontuação.
- `Adicionar Pontuação Extra`: Um botão prático para inserir pontuações "quebradas" por empates (ex.: 10 pontos = 3 vitórias e 1 empate).

**Como é feito o cálculo:**
Diferente das outras calculadoras que dividem por "Colocação", esta usa a pontuação como peso. A mágica acontece em três fases automáticas:

1. **O Valor do Ponto:** O sistema pega os resultados que você digitou na tabela, multiplica pelos pontos e soma tudo. Depois, ele divide o nosso **Pool Total** de boosters por essa "massa de pontos" para descobrir matematicamente quanto vale 1 único ponto no torneio.
2. **Distribuição Base:** Cada jogador recebe a sua pontuação multiplicada por esse "valor do ponto". O sistema corta as casas decimais (arredonda para baixo) para garantir que você nunca distribua mais boosters do que realmente arrecadou.
3. **Distribuição de Sobras (Método do Maior Resto):** Como arredondamos tudo para baixo, sobram boosters! A calculadora identifica quem foi mais prejudicado na matemática (ex: um jogador que deveria ganhar `2.8` boosters e ficou com 2, perdeu `0.8`). Ela devolve as sobras para essas pessoas com "maior resto" de forma decrescente, até fechar o estoque!
   * *Regra de Ouro:* Ela só entrega a sobra se você tiver boosters suficientes para premiar **todos** os jogadores empatados naquela pontuação.
4. **Estoque Final:** Se sobrar algum booster isolado (por exemplo, sobrou 1 booster, mas a próxima pontuação elegível tem 3 jogadores e isso daria briga), ele ficará registrado na caixa verde de **Sobras do Fundo (Estoque)** para você realizar um sorteio ("Lucky Drop") ou usar como prêmio de consolação!

---

### 4. Calculadora de Distribuição por Pontuação (Crédito na Loja)
**O que é:** Uma variação do formato de pontuação, mas focada inteiramente em distribuir prêmios em **Crédito na Loja** (dinheiro/saldo) em vez de boosters físicos. Ideal para eventos regulares onde os jogadores preferem acumular saldo para comprar cartas avulsas ou outros produtos da loja.

**Inputs (Como usar):**
- `Inscritos Masters`, `Seniors` e `Juniors`: A quantidade total de jogadores define o número de rodadas.
- `Valor da Inscrição (R$)` e `Staff e Juízes (%)`: Usados para calcular a arrecadação e descontar a equipe, formando o nosso **Pool de Crédito (R$)**. *(Diferente das outras calculadoras, não é preciso o "Preço do Booster" aqui!)*
- `Tabela de Pontuação` e `Adicionar Pontuação Extra`: Funcionam exatamente como na calculadora 3, onde você informa a quantidade de jogadores que atingiram cada pontuação (incluindo empates).

**Como é feito o cálculo:**
A grande diferença desta calculadora para a anterior é a precisão. Como estamos lidando com dinheiro, o sistema não fatiará o fundo em "números inteiros" (como faria com um booster de plástico), mas sim na exatidão de **centavos (R$ 0,01)**:

1. **O Valor do Ponto em Dinheiro:** O sistema soma todos os pontos registrados na tabela e divide o **Pool de Crédito** por essa quantidade. Isso revela exatamente quantos reais e centavos (R$) vale 1 ponto no torneio.
2. **Distribuição Base (Centavos):** Cada jogador recebe sua pontuação multiplicada por esse "valor do ponto monetário". O sistema faz um corte nas casas decimais, arredondando tudo rigorosamente para baixo na segunda casa decimal (centavos), evitando que a loja pague mais dinheiro do que o arrecadado.
3. **Distribuição de Trocos (Método do Maior Resto):** Como as dízimas periódicas podem fazer o total ficar alguns centavos abaixo do arrecadado, a calculadora usa o mesmo método da calculadora 3. Ela descobre qual grupo de jogadores perdeu a maior fração monetária no arredondamento inicial e entrega o "crédito restante" que sobraria até a conta fechar de forma perfeita.
4. **Caixa Final:** O sistema exibe o custo final em R$, que, devido aos cálculos de alta precisão em centavos, fechará perfeitamente ou deixará no máximo alguns centavos residuais na **Sobra do Fundo (Caixa)**, blindando o caixa da sua loja contra furos!

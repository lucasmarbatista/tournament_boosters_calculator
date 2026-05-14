# tournament_boosters_calculador
Uma ferramenta para calcular a distribuição de boosters (premiação) em torneios de card games, garantindo uma divisão justa que se adapta a diferentes categorias e quantidades de jogadores.

## Tipos de Calculadoras

O sistema conta com 2 tipos de calculadoras, cada uma desenvolvida para um formato específico de torneio. Abaixo está a explicação de cada uma, seus inputs e as regras de cálculo.

---

### 1. Calculadora para Torneios Gerais (Distribuição para o Top)
**O que é:** Ideal para torneios mais competitivos. Esse modelo junta toda a premiação gerada pelas inscrições e foca a distribuição nos jogadores que alcançarem as melhores colocações (como o Top 4 ou Top 8).

**Inputs (Como usar):**
- `Inscritos Masters`, `Seniors` e `Juniors`: Quantidade de jogadores inscritos em cada categoria. A soma deles forma o total de jogadores, e a proporção de cada um define qual a fatia do pool de premiação destinada a cada grupo.
- `Valor da Inscrição (R$)`: O valor cobrado de cada jogador para participar do evento. Serve para calcular a arrecadação bruta do torneio.
- `Preço do Booster (R$)`: O custo ou valor de repasse unitário do booster. É utilizado para converter o saldo do fundo de premiação financeiro na quantidade física de boosters do Pool.
- `Staff e Juízes (%)`: A porcentagem da arrecadação total que será deduzida para pagamento da equipe (staff/juízes) antes de gerar o Pool de prêmios.

**Como é feito o cálculo:**
Primeiro, a calculadora descobre a **Arrecadação Total** multiplicando o número de jogadores pelo valor da inscrição. Depois, ela já separa a parte da equipe (**Staff/Juízes**) e pega o valor que sobrou no fundo para dividir pelo **Preço do Booster**. É assim que chegamos ao **Pool Total** de boosters! A partir daí, dividimos esse total de forma justa entre as categorias (Master, Senior e Junior), seguindo a distribuição abaixo com base em quantos jogadores cada uma tem (já cuidando dos arredondamentos de forma automática):

* **1 Jogador:** 100% do Pool.
* **2 a 3 Jogadores (Focado no Top 2):**
  - **1º Lugar:** ~70%
  - **2º Lugar:** ~30%
  - **3º Lugar:** *Se houverem 3 jogadores e saldo, tira-se 1 booster do 1º colocado para dar ao 3º.*
* **4 Jogadores (Focado no Top 3):**
  - **1º Lugar:** 50%
  - **2º Lugar:** 30%
  - **3º Lugar:** 20%
* **5 a 7 Jogadores (Focado no Top 4):**
  - **1º Lugar:** ~43.7%
  - **2º Lugar:** ~23.9%
  - **3º e 4º Lugares:** ~11% cada
* **8 ou mais Jogadores (Corte para Top 8):**
  - **1º Lugar:** ~43.7%
  - **2º Lugar:** ~23.9%
  - **3º e 4º Lugares:** ~11% cada
  - **5º ao 8º Lugares:** ~2.6% cada

* **Correção de Arredondamento:** Se os arredondamentos tentarem distribuir mais boosters do que o *Pool* permite, o algoritmo deduz essa diferença das premiações menores até o montante igualar exatamente o limite financeiro disponível.*

---

### 2. Calculadora para Formato Pre-release (Participação + Top)
**O que é:** Perfeito para eventos mais casuais como o Pre-release. A ideia aqui é garantir que todo mundo volte para casa com pelo menos um prêmio só por ter participado, mas ainda assim guardando uma quantidade bacana de boosters para recompensar os melhores colocados.

**Inputs (Como usar):**
- `Inscritos Masters`, `Seniors` e `Juniors`: Quantidade de jogadores em cada categoria. A proporção define como a premiação do Top será fatiada.
- `Total de Boxes` / `Total de Boosters (Pool)`: A quantidade máxima em estoque reservada para o evento. Os campos são sincronizados (1 Box = 36 boosters).
- `Boosters de participação (por pessoa)`: Quantos boosters cada jogador recebe garantido apenas por jogar.
- `Juízes/Staff (Total Boosters)`: Quantidade de boosters retirada do montante total para pagamento da equipe (diferente de torneios gerais, aqui desconta-se em boosters físicos em vez de R$).
- `Promoção Traga um Amigo (Total Boosters)`: Quantidade de boosters separada previamente para sorteios ou campanhas.

**Como é feito o cálculo:**
1. **Destinos Fixos:** Antes de tudo, separamos os boosters que já estão prometidos. O sistema pega a quantidade de jogadores e multiplica pelos `Boosters de participação`, e depois soma isso com os boosters que vão para os juízes e para as promoções.
2. **Pool dos Vencedores (Top Global):** Com os prêmios fixos separados, a gente tira esse valor do `Total de Boosters`. O que sobrar é o nosso fundo de premiação para os mais competitivos!
3. **Distribuição Proporcional (O Top Cut):** Agora, pegamos esse pool dos vencedores e dividimos de forma justa entre as categorias (Master, Senior e Junior), dependendo de quantos inscritos cada uma teve. O sistema cuida dos arredondamentos para ninguém ganhar "meio booster" e corrige os valores para a conta bater perfeitamente:
   * **1 Jogador:** 100% do Pool da categoria.
   * **2 a 3 Jogadores (Focado no Top 2):**
     - **1º Lugar:** ~70%
     - **2º Lugar:** ~30%
     - **3º Lugar:** *Se houverem 3 jogadores e saldo, tira-se 1 booster do 1º colocado para dar ao 3º.*
   * **4 Jogadores (Focado no Top 3):**
     - **1º Lugar:** 50%
     - **2º Lugar:** 30%
     - **3º Lugar:** 20%
   * **5 a 7 Jogadores (Focado no Top 4):**
     - **1º Lugar:** ~43.7%
     - **2º Lugar:** ~23.9%
     - **3º e 4º Lugares:** ~11% cada
   * **8 ou mais Jogadores (Corte para Top 8):**
     - **1º Lugar:** ~43.7%
     - **2º Lugar:** ~23.9%
     - **3º e 4º Lugares:** ~11% cada
     - **5º ao 8º Lugares:** ~2.6% cada

   * **Correção de Arredondamento:** Se os arredondamentos para cima tentarem distribuir mais boosters do que o *Pool* da categoria permite, o algoritmo deduz a diferença das premiações menores até o fechamento bater exatamente no estoque reservado.
4. **Sobras de Estoque:** No fim das contas, a calculadora faz uma prova real. Ela checa se a soma de tudo que foi distribuído bate com o seu estoque inicial e te mostra direitinho quantos boosters sobraram (ou te avisa se você tentar dar mais prêmios do que realmente tem disponível).

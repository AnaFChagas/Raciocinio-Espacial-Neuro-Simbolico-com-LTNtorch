# 1. Introdução Teórica

## IA Neuro-Simbólica (NeSy) e Logic Tensor Networks (LTN)
A Inteligência Artificial Neuro-Simbólica (NeSy) é um campo que busca integrar a capacidade de aprendizado e percepção das Redes Neurais Profundas (Deep Learning) com a capacidade de raciocínio dedutivo e interpretabilidade da Lógica Simbólica$.

Neste trabalho, utilizamos o framework **Logic Tensor Networks (LTN)**. Diferente do Deep Learning padrão que apenas mapeia entradas para saídas, o LTN utiliza a **Real Logic** (Lógica Real), onde os valores verdade não são binários (0 ou 1), mas contínuos no intervalo [0, 1]. Isso permite:

* **Grounding:** Símbolos lógicos (predicados) são fundamentados em tensores e operações neurais.
* **Aprendizado via Satisfatibilidade:** O treinamento busca maximizar a satisfatibilidade (SAT) de uma Base de Conhecimento (KB) contendo axiomas lógicos e dados.

# 2. O Dataset CLEVR (Simplificado)

Para este projeto, simulamos um ambiente baseado no CLEVR$^4$.

### O que é o CLEVR original?
O CLEVR (Compositional Language and Elementary Visual Reasoning) é um dataset clássico de diagnóstico em IA, composto por imagens renderizadas em 3D de formas geométricas simples e perguntas textuais complexas. Ele serve para testar a capacidade de modelos visuais em entender lógica composicional (ex: "Qual a cor do objeto à esquerda da esfera?").

### A versão utilizada neste trabalho:
Ao invés de processar imagens pesadas (o que exigiria CNNs complexas), utilizamos uma versão vetorial simplificada focada puramente na lógica espacial$^5$. Cada objeto é representado por um vetor de características (*feature vector*) de tamanho 11 contendo$^6$:

1.  **Posição [0-1]:** Coordenadas x, y normalizadas (0.0 a 1.0).
2.  **Cores [2-4]:** One-hot encoding (Vermelho, Verde, Azul).
3.  **Formas [5-9]:** One-hot encoding (Círculo, Quadrado, Cilindro, Cone, Triângulo).
4.  **Tamanho [10]:** Escalar (0.0 para pequeno, 1.0 para grande).

# 3. Metodologia Experimental

Conforme solicitado, o experimento foi executado 5 vezes (Runs) utilizando 5 datasets gerados com sementes (*seeds*) aleatórias distintas para validar a robustez do modelo.

### Configuração
* **Axiomas:** Foram implementadas regras de taxonomia (forma única), raciocínio espacial horizontal (esquerda/direita), vertical (abaixo/acima) e restrições de proximidade.
* **Métricas:** Monitoramos a Satisfatibilidade da Base de Conhecimento (SAT KB), Acurácia, Precisão, Recall e F1-Score do predicado `LeftOf`.

# 4. Resultados Experimentais

Abaixo apresentamos a consolidação dos resultados das 5 execuções.

## 4.1 Tabela de Performance (5 Execuções)

| Run | Dataset (Seed) | Valor Const. | Final SAT KB | Acurácia (LeftOf) | Precisão | Recall | F1 Score |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | 1 | 45.0 | 0.9758 | 0.6250 | 0.8600 | 0.3250 | 0.4651 |
| 2 | 2 | 45.0 | 0.9766 | 0.6500 | 0.9000 | 0.3402 | 0.4927 |
| 3 | 3 | 45.0 | 0.9653 | 0.6583 | 0.9333 | 0.3561 | 0.5099 |
| 4 | 4 | 45.0 | 0.9881 | 0.6250 | 0.7467 | 0.2758 | 0.3966 |
| 5 | 5 | 45.0 | 0.9762 | 0.6250 | 0.9267 | 0.3364 | 0.4887 |
| **Média** | **-** | **-** | **0.9764** | **0.6367** | **0.8733** | **0.3267** | **0.4706** |

## 4.2 Satisfação de Fórmulas Específicas
No conjunto de teste final, as fórmulas compostas atingiram os seguintes níveis de satisfação (valores aproximados baseados na convergência da SAT KB):

* **Consultas de Existência** (Ex: Pequeno abaixo de Cilindro):approx. 0.98
* **Restrições** (Ex: Triângulos próximos):approx. 0.99

## 4.3 Análise Visual e Discussão
Observando os gráficos de treinamento gerados:

* **Convergência Lógica:** A satisfação média da base de conhecimento (SAT KB) convergiu para approx 0.976, indicando que o modelo aprendeu a respeitar as regras lógicas impostas (como transitividade e assimetria).
* **Desbalanceamento de Classes (Conservadorismo):** Houve uma discrepância notável entre a detecção de casos negativos e positivos para a relação `LeftOf`:
    * Casos Negativos (x *não* está à esquerda): Acurácia approx. 95
    * Casos Positivos (x está à esquerda): Acurácia approx. 32
* **Interpretação:** O modelo adotou uma postura "conservadora". Para evitar violar axiomas estritos (como a assimetria: se A é esq de B, B não pode ser esq de A), a rede tendeu a predizer "Falso" com mais frequência, resultando em alta Precisão mas baixo Recall.

# 5. Explicação do Raciocínio (Ponto Extra)

Nesta seção, explicamos a lógica por trás das consultas complexas exigidas na tarefa de "Raciocínio Composto".

## 5.1 Filtragem Composta
**A Pergunta:**
> "Existe algum objeto Pequeno que esteja Abaixo de um Cilindro E à Esquerda de um Quadrado?"

**Explicação do Raciocínio:**
Esta consulta testa a capacidade da rede de realizar a interseção de múltiplos conjuntos de propriedades.
1. O modelo identifica os objetos que satisfazem o predicado unário `IsSmall(x)`.
2. Simultaneamente, verifica duas relações binárias: `Below(x,y)` onde y é Cilindro, e `LeftOf(x,z)` onde z é Quadrado.
3. O quantificador existencial (exists) agrega essas verificações. A alta satisfação indica que o modelo conseguiu ancorar corretamente os atributos visuais (forma/tamanho) com as posições relativas.

## 5.2 Dedução de Posição Absoluta
**A Pergunta:**
> "Existe um Cone Verde que está Entre (InBetween) dois outros objetos quaisquer?".

**Explicação do Raciocínio:**
O conceito de "Entre" (*InBetween*) não é um dado de entrada, mas uma construção lógica derivada.
Isso demonstra raciocínio de ordem superior: o modelo precisa entender que para x estar entre y e z, ele deve satisfazer relações opostas simultaneamente em relação aos vizinhos.

## 5.3 Restrição de Proximidade (Constraint)
**A Regra:**
> "Se dois objetos são Triângulos e estão Próximos (CloseTo), então eles devem ter o mesmo Tamanho.".

**Explicação do Raciocínio:**
Diferente das anteriores, esta não é uma pergunta, mas uma regra impositiva que molda o aprendizado.
Esta fórmula cria um gradiente que penaliza a rede se ela detectar dois triângulos próximos com tamanhos distintos. O raciocínio aqui é correlacional: força a rede a aprender que a propriedade espacial (`CloseTo`) e a propriedade categórica (`IsTriangle`) têm uma implicação direta na propriedade dimensional (`SameSize`). Isso exemplifica como o conhecimento simbólico pode corrigir ou refinar a percepção neural.

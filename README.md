# Raciocinio-Espacial-Neuro-Simbolico-com-LTNtorch

## Fundamentos de Inteligência Artificial (ES01)

Integrantes:

- ANA FLÁVIA FREITAS DAS CHAGAS
- FRANCISCO BRILHANTE BRAGA
- DAVI EMANUEL DE LIMA REIS
- THARCIO JOSÉ DE OLIVEIRA ASSUNÇÃO
- RAFAEL FARIAS DE LIMA

 Email:
 
- anaf.chagas@icomp.ufam.edu.br
- franciscobraga99@gmail.com
- davi.emanuel@icomp.ufam.edu.br
- tharcio.assuncao@icomp.ufam.edu.br
- rafael.farias@icomp.ufam.edu.br

---

# O Projeto

### 🚀 Sobre o Projeto

O objetivo principal é classificar e raciocinar sobre formas geométricas e suas posições relativas (como "dentro de", "acima de", "à esquerda de") utilizando uma abordagem que combina:

 - Percepção (Neuro): Redes Neurais para extração de características visuais.

 - Raciocínio (Simbólico): Lógica Fuzzy para impor restrições e regras de conhecimento sobre o domínio espacial.

### 🛠️ Tecnologias Utilizadas

 - Python 3.x

 - PyTorch: Framework base para aprendizado profundo.

 - LTNtorch: Extensão para implementação de Logic Tensor Networks.

 - Matplotlib/OpenCV: Para visualização e processamento de imagens.
   
### 📂 Estrutura do Repositório

 - data/: Scripts para geração ou carregamento do dataset de formas geométricas.

 - models/: Definição da arquitetura da rede neural e dos predicados lógicos.

 - train.py: Script principal para o treinamento do modelo unificado.

 - notebooks/: Exemplos interativos e visualização de resultados.

### 📊 Resultados Esperados

O modelo deverá ser capaz de identificar não apenas o tipo de objeto, mas validar se as restrições lógicas impostas nos axiomas estão sendo respeitadas, apresentando uma acurácia superior a modelos puramente neurais em cenários com poucos dados ou que exigem consistência geométrica.

### 🧾 Resultados Obtidos

O modelo foi validado através de um protocolo experimental rigoroso, consistindo em 5 execuções (Runs) independentes com sementes aleatórias distintas para garantir a robustez estatística dos achados.

📈 Desempenho Quantitativo (Média das Execuções)

Após o treinamento, o modelo demonstrou uma alta capacidade de aprendizado lógico, atingindo uma estabilidade notável na satisfatibilidade da base de conhecimento.

![Desempenho](assets/Resultados-FIA.PNG)

🧠 Análise de Raciocínio e Convergência

 - Convergência Lógica: A satisfação média da base de conhecimento (SAT KB) de aproximadamente 97,6% indica que o modelo aprendeu com sucesso a respeitar as regras lógicas impostas, como a transitividade e a assimetria das relações espaciais.

 - Comportamento Conservador: Observou-se um desbalanceamento na detecção da relação LeftOf. O modelo apresentou alta acurácia em casos negativos (~95%) e menor em positivos (~32%). Isso sugere uma postura "conservadora" para evitar a violação de axiomas estritos: para não contradizer a lógica (ex: se A está à esquerda de B, B não pode estar à esquerda de A), a rede prefere predizer "Falso" com mais frequência, resultando em alta precisão.

 - Satisfação de Fórmulas Compostas:
    - Consultas de Existência: (Ex: "Pequeno abaixo de Cilindro") atingiram cerca de 0.98 de satisfação.

    - Restrições de Proximidade: (Ex: "Triângulos próximos devem ter o mesmo tamanho") alcançaram cerca de 0.99.

💡 Capacidades de Raciocínio Demonstradas

 - Filtragem Composta: O modelo provou ser capaz de realizar a interseção de múltiplos conjuntos de propriedades (ex: identificar um objeto que seja simultaneamente Pequeno, esteja Abaixo de um Cilindro e à Esquerda de um Quadrado).

 - Dedução de Conceitos Abstratos: O conceito de "Estar entre" (InBetween) foi derivado logicamente a partir de relações opostas, demonstrando raciocínio de ordem superior sem a necessidade de dados de entrada diretos para essa classe.

 - Refinamento via Axiomas: O uso de regras impositivas (como a restrição de tamanho para triângulos próximos) demonstrou como o conhecimento simbólico pode corrigir a percepção neural, forçando a rede a aprender correlações entre propriedades espaciais e categoriais.

# Venezia

Sistema para monitoramento de torra de cafés especiais e posterior correlação com a avaliação sensorial (cupping) segundo o protocolo da SCA (Specialty Coffee Association).

> **Sobre este repositório.** Este é um repositório de apresentação, sem código-fonte.
> O código e os dados da pesquisa estão em repositório privado, com acesso mediante solicitação: mjcg12@yahoo.com.br

## Objetivo

Construir uma base de dados de torras reais e suas avaliações sensoriais, para investigar com aprendizado de máquina como as variáveis do processo de torra se relacionam com os atributos da bebida.

## Como funciona

1. O software monitora e registra as variáveis da torra em um torrador comercial, via Modbus/TCP.
2. Após a torra, a amostra é avaliada por cupping (protocolo SCA) e as notas são vinculadas à torra correspondente.
3. O algoritmo DBSCAN identifica agrupamentos por densidade entre as torras, tolerando ruídos dos sensores.
4. A análise de correlação (Pearson) relaciona as variáveis do processo aos atributos sensoriais, e os resultados são apresentados ao usuário.
5. O processo é contínuo: cada nova torra registrada atualiza os resultados pela reexecução das análises.

Como a obtenção de amostras reais é cara e limitada, o estudo também utilizou uma rede adversarial generativa (GAN) para ampliar de forma controlada o conjunto de dados, mantendo os dados sintéticos separados dos reais.

## Resultados principais

Através da análise paramétrica estratificada, observou-se que a linhagem genética e o *terroir* (representados por cada lote real) estabelecem *baselines* diferentes para a pontuação SCA, mas respondem a padrões termodinâmicos semelhantes durante a torra:
- **Influência do Lote:** Há uma clara segregação nas pontuações de Doçura, Acidez e Corpo dependendo do lote analisado (Mundo Novo, Catuaí Amarelo e Blend Próprio), com as curvas de tendência operando em patamares e variâncias distintos.
- **Comportamento Sensorial vs. Processo:** O tempo total e a taxa de ascensão (RoR) afetam diretamente a estrutura da bebida, demonstrando fortes tendências estatísticas (como o decaimento em certas notas em fases prolongadas).
- **Validação Sintética (GAN):** A eficácia do *Data Augmentation* foi medida objetivamente comparando as distribuições reais e sintéticas. O modelo adversário gerou amostras com um erro médio absoluto (MAE) quase nulo em relação ao *baseline* original, comprovando alta precisão na simulação do domínio real. As diferenças médias alcançadas foram: **Doçura (Δ = 0,04 pontos SCA)**, **Acidez (Δ = 0,06 pontos SCA)**, **Corpo (Δ = 0,03 pontos SCA)** e **Ponto de Torra (Δ = 0,62 graus Agtron)**. 
  > **Nota Metodológica:** Devido ao tamanho da amostragem física inicial (55 torras na fase original do estudo), margens de erro tão estreitas podem apontar para um possível *overfitting* (memorização da rede) durante o treinamento. Contudo, isso não invalida a metodologia arquitetada: o sistema é alimentado continuamente. Conforme a base empírica evolui e cresce com novos dados reais, a GAN é submetida a ciclos iterativos de retreinamento, o que assegurará expansão da generalização do modelo algorítmico em fases futuras.

A base contínua de dados que fundamenta esta análise conta hoje com cerca de 100 torras reais rigorosamente monitoradas e 500 torras sintéticas geradas para o estudo validatório inicial.

## Publicação

Esta pesquisa foi desenvolvida como Trabalho de Conclusão do MBA em Engenharia de Software da USP/Esalq (janeiro de 2026), com nota 10 em todas as etapas e indicação ao prêmio de melhor TCC. Seu resumo executivo foi publicado na Revista Estratégias e Soluções (Pecege):
https://revistaes.com.br/resumo-executivo/machine-learning-na-torra-e-analise-sensorial-de-cafes-especiais

## Análise de Dados e Correlações Sensoriais

Como parte dos resultados consolidados da pesquisa, realizou-se o cruzamento paramétrico entre as variáveis físicas da torra e as respectivas avaliações sensoriais (segundo protocolo SCA). As distribuições foram divididas entre as **amostras reais** (lotes distintos de café) e as **amostras sintéticas**, geradas artificialmente pela Rede Adversarial Generativa (GAN) em processo de *Data Augmentation*. 

Os painéis a seguir demonstram as dispersões e correlações (curvas de tendência suavizadas por regressão *LOWESS*) das notas de **Doçura, Acidez e Corpo** em função de três métricas termodinâmicas e temporais do processo de torrefação:

### 1. Ponto da Torra (Agtron) vs Atributos Sensoriais
Observa-se nas torras empíricas que perfis mais claros (maior grau Agtron) tendem a preservar ácidos orgânicos (acentuando a acidez), enquanto graus de torra mais escuros e avançados (menor grau Agtron) favorecem o desenvolvimento do corpo da bebida. O lote de torras sintéticas gerado pela GAN acompanhou com fidelidade essa inclinação, o que é corroborado estatisticamente pela continuidade das regressões locais e pela manutenção do suporte (*range*) da distribuição física subjacente.
![Correlações com Ponto de Torra](images/PontoTorra_consolidado.png)

### 2. Tempo Total da Torra vs Atributos Sensoriais
A duração macro do processo indica impacto na degradação prolongada de açúcares (diminuição relativa de doçura para tempos longos extremos) e na solidificação estrutural do grão (corpo). O modelo adversário demonstrou altíssima robustez ao captar exatamente os mesmos patamares de resposta sensorial encontrados na faixa de torra dos lotes originais.
![Correlações com Tempo Total](images/T_Total_consolidado.png)

### 3. Rate of Rise (RoR) em Pirólise vs Atributos Sensoriais
O RoR (*Rate of Rise* - °C/min) na fase de pirólise (ou Fase de Desenvolvimento, pós-primeiro crack) possui forte impacto nas reações aromáticas finais. Taxas mais contidas de transferência de calor nessa etapa auxiliam no controle das reações pirolíticas exotérmicas, modelando os atributos sensoriais da bebida resultante. Visualmente, a distribuição dos dados sintéticos replica com sucesso as aglomerações e tendências mapeadas pelas amostras físicas reais.
![Correlações com RoR na Pirólise](images/RoR_Pirolise_consolidado.png)

## Arquitetura

_(Diagramas UML - Visão macro do sistema)_

### Casos de Uso
![Casos de Uso](images/01%20-%20Casos%20de%20Uso.png)

### Diagrama de Classes (Visão Geral)
![Classes Geral](images/02%20-%20Classes%20Geral.png)

### Diagrama de Classes (Visão Lógica)
![Classes Visão Lógica](images/03%20-%20Classes%20Vis%C3%A3o%20L%C3%B3gica.png)

### Classes de Fronteira
![Classes Fronteira](images/04%20-%20Classes%20Fronteira.png)

### Classes Controladoras
![Classes Controladores](images/05%20-%20Classes%20Controladores.png)

### Classes de Entidades
![Classes Entidades](images/06%20-%20Classes%20Entidades.png)

### Diagrama de Implantação
![Diagrama de Implantação](images/07%20-%20Diagrama%20de%20Implanta%C3%A7%C3%A3o.png)

## Capturas de Tela (Interface do Sistema)

Abaixo estão algumas imagens ilustrando as principais funcionalidades e interfaces do software desenvolvido.

### 1. Visualização de Torra
![Visualização de Torra](images/07%20-%20Screenshot01%20-%20Visualiza%C3%A7%C3%A3o%20de%20Torra.png)

### 2. Análise e Agrupamento (DBSCAN)
![Análise DBSCAN](images/08%20-%20Screenshot02%20-%20An%C3%A1lise%20DBSCAN.png)

### 3. Gráfico de Clusters (DBSCAN)
![Gráfico DBSCAN Clusters](images/09%20-%20Screenshot03%20-%20Gr%C3%A1fico%20DBSCAN%20Clusters.png)

### 4. Configurações do Torrador
![Configurações Torrador](images/10%20-%20Screenshot04%20-%20Configura%C3%A7%C3%B5es%20Torrador.png)

### 5. Treinamento da GAN (Rede Adversarial Generativa)
![Treinamento da GAN](images/11%20-%20Screenshot05%20-%20Treinamento%20da%20GAN.png)

### 6. Cadastro de Lote de Café
![Cadastro de Lote](images/12%20-%20Screenshot06%20-%20Cadastro%20de%20Lote.png)

### 7. Informações e Histórico da Torra
![Informações da torra](images/13%20-%20Screenshot07%20-%20Informa%C3%A7%C3%B5es%20da%20torra.png)

### 8. Avaliação Sensorial (Protocolo SCA)
![Avaliação SCA](images/14%20-%20Screenshot08%20-%20Avalia%C3%A7%C3%A3o%20SCA.png)

### 9. Perfil Sensorial (Gráfico Radar)
![Perfil Sensorial](images/15%20-%20Screenshot09%20-%20Perfil%20Sensorial.png)

## Tecnologias

C# (.NET 8), WPF, Modbus/TCP, DBSCAN, GAN

## Autor

Marco Aurélio Aloise Filho

## Licença

Copyright © 2026 Marco Aurélio Aloise Filho. Todos os direitos reservados.

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

Na pesquisa original, com 55 torras reais de três lotes distintos:

- Torras mais claras e com menor tempo de pirólise favoreceram acidez e doçura (correlação de −60% entre acidez e tempo de pirólise).
- Torras mais escuras e prolongadas favoreceram corpo e notas de chocolate.
- Nos dados sintéticos gerados pela GAN, as tendências das correlações se mantiveram.

Desde a conclusão do trabalho, a base foi ampliada e conta hoje com cerca de 100 torras reais.

## Publicação

Esta pesquisa foi desenvolvida como Trabalho de Conclusão do MBA em Engenharia de Software da USP/Esalq (janeiro de 2026), com nota 10 em todas as etapas e indicação ao prêmio de melhor TCC. Seu resumo executivo foi publicado na Revista Estratégias e Soluções (Pecege):
https://revistaes.com.br/resumo-executivo/machine-learning-na-torra-e-analise-sensorial-de-cafes-especiais

## Arquitetura

_(diagramas UML elaborados manualmente pelo autor)_

### Casos de Uso
![Casos de Uso](images/01%20-%20Casos%20de%20Uso.png)

### Diagrama de Classes
![Classes Geral](images/02%20-%20Classes%20Geral.png)

### Visão Lógica
![Classes Visão Lógica](images/03%20-%20Classes%20Vis%C3%A3o%20L%C3%B3gica.png)

### Classes de Fronteira
![Classes Fronteira](images/04%20-%20Classes%20Fronteira.png)

### Classes Controladoras
![Classes Controladores](images/05%20-%20Classes%20Controladores.png)

### Classes de Entidades
![Classes Entidades](images/06%20-%20Classes%20Entidades.png)

## Tecnologias

C# (.NET 8), WPF, Modbus/TCP, DBSCAN, GAN

## Autor

Marco Aurélio Aloise Filho

## Licença

Copyright © 2026 Marco Aurélio Aloise Filho. Todos os direitos reservados.

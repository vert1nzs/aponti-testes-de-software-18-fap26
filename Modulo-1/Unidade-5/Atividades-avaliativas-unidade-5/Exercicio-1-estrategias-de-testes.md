# Atividade Avaliativa — Planejamento de Estratégia de Testes

Este documento apresenta a especificação da Estratégia de Testes para o projeto atual, considerando um cenário de desenvolvimento ativo, equipe reduzida e prazo de entrega definido.

---

## 1. Objetivo da Estratégia
* **O que é mais importante garantir:** A estabilidade e a segurança dos fluxos críticos do sistema (como Login, consultas principais e o salvamento de dados), assegurando que o usuário real consiga utilizar a plataforma sem sofrer com travamentos.
* **Aspectos que merecem maior atenção:** As telas de formulários onde há entrada de dados e a camada de persistência (Banco de Dados), garantindo que nenhuma informação seja corrompida.

---

## 2. Tipos de Teste Prioritários
* **Tipos executados:** Testes Funcionais, Testes de Fumaça (*Smoke Test*) e Testes de Regressão.
* **Menor prioridade:** Testes de Performance complexos (Carga e Estresse) e Auditorias de Segurança avançadas.
* **Justificativa:** Em um cenário de escassez de tempo e pessoal, focar na prevenção de falhas críticas visíveis ao usuário gera maior valor ao negócio do que pulverizar o esforço do time em testes não funcionais complexos nesta fase.

---

## 3. Abordagens de Teste
* **Testes Manuais:** Validação de novas funcionalidades (testes exploratórios), usabilidade e comportamento visual das telas.
* **Testes Automatizados:** Focados estritamente na esteira de regressão dos fluxos mais críticos e repetitivos (ex: Login e fluxos principais de salvamento).
* **Justificativa:** Automatizar todo o sistema demanda um tempo que o time reduzido não possui. A automação focada no que é repetitivo libera a equipe para realizar testes manuais mais detalhados e rápidos nas novidades do software.

---

## 4. Riscos e Mitigação
* **Principais Riscos:** O surgimento de novos defeitos devido às correções constantes (efeito dominó) e o estouro do prazo final de entrega do projeto.
* **Como a estratégia ajuda:** A aplicação de *Smoke Tests* a cada nova entrega bloqueia versões muito instáveis logo no início, enquanto os testes de regressão garantem que as correções não destruam o que já estava funcionando.

---

## 5. Recursos e Cronograma
* **Pessoas envolvidas:** Equipe reduzida atual do projeto de QA.
* **Momento dos testes:** Os testes ocorrerão de forma **contínua**, acompanhando o desenvolvimento ativo desde o início (*Shift Left Testing*).
* **Foco temporal:** Totalmente contínuos. Concentrar os testes apenas em uma fase final traria uma sobrecarga de bugs acumulados impossível de corrigir antes do fim do prazo de entrega.

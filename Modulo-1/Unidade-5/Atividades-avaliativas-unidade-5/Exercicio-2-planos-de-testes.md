# Atividade Avaliativa — Plano de Testes

Este documento apresenta o Plano de Testes operacional para o projeto atual, aplicando a estratégia de gerenciamento de riscos para um time reduzido com prazo de entrega estabelecido.

---

## 1. Escopo de Testes
*   **O que será testado (Em Escopo):**
    *   Fluxo de Autenticação (Login, Logout e restrições de acesso).
    *   Funcionalidades principais da tela inicial (Dashboard e carregamento de componentes vitais).
    *   Formulários essenciais de entrada de dados e salvamento de novos registros.
*   **O que NÃO será testado (Fora de Escopo):**
    *   Testes de estresse de carga e performance extrema de infraestrutura.
    *   Testes de compatibilidade em navegadores muito antigos ou legados.
    *   Opções estéticas secundárias de customização de layout do usuário.

---

## 2. Tipos de Teste Aplicados
Para otimizar o tempo da equipe enxuta, aplicaremos três tipos de testes focados na saúde do software:
*   **Testes de Fumaça (*Smoke Tests*):** Testes curtíssimos manuais para validar se as telas básicas abrem sempre que uma nova versão for publicada.
*   **Testes Funcionais Manuais:** Execução passo a passo com dados válidos e inválidos nos formulários principais.
*   **Testes de Regressão Automatizados:** Scripts simples focados apenas nas rotinas repetitivas de login e gravação de dados para garantir que nada antigo quebre.

---

## 3. Critérios de Entrada e Saída
*   **Critérios de Entrada (Para iniciar os testes):**
    *   A versão do código estar publicada e disponível no ambiente de testes.
    *   O desenvolvedor entregar a funcionalidade com os testes unitários básicos aprovados.
    *   A build passar no teste de fumaça inicial de carregamento de tela.
*   **Critérios de Saída (Para finalizar os testes):**
    *   100% dos casos de teste críticos e de alto risco executados com sucesso.
    *   Nenhum bug de gravidade "Alta" ou "Bloqueante" aberto no sistema.
    *   O saldo final de falhas menores mapeado e aceito pelo time de desenvolvimento.

---

## 4. Ambiente de Testes
*   **Ambiente Utilizado:** Ambiente de Homologação/Staging (uma cópia idêntica ao sistema do cliente, mas isolada para não afetar dados reais).
*   **Ferramentas e Acesso:** Acesso via navegador web padrão (Google Chrome/Mozilla Firefox), utilizando credenciais e dados de teste 100% fictícios criados para a validação.

---

## 5. Recursos e Responsabilidades
Contando com o nosso time reduzido, as funções serão divididas de forma direta:
*   **Analista de QA (Equipe do Projeto):** Responsável por mapear os cenários, executar os testes manuais, disparar os scripts automatizados e reportar os bugs encontrados.
*   **Equipe de Desenvolvimento:** Responsável por corrigir as falhas críticas encontradas pelo QA com máxima prioridade antes do prazo de entrega.

---

## 6. Cronograma Básico
Para respeitar o prazo fixado, adotaremos um cronograma contínuo casado com o desenvolvimento:
*   **Semana 1:** Planejamento dos cenários de teste e preparação das massas de dados fictícios.
*   **Semana 2:** Execução dos testes funcionais manuais nas primeiras telas entregues.
*   **Semana 3:** Testes contínuos de regressão e validação das correções de bugs feitas pelos desenvolvedores.
*   **Dias Finais:** Rodada final de testes de fumaça e assinatura do termo de liberação do sistema (*Sign-off*).

---

## 7. Riscos e Contingências
*   **Risco 1: Os desenvolvedores atrasarem a entrega das funcionalidades para o QA.**
    *   *Contingência:* O time de QA focar estritamente nos fluxos mais pesados e críticos do sistema, reduzindo o tempo gasto com telas secundárias para garantir o prazo.
*   **Risco 2: Um testador do time reduzido faltar por motivos de força maior.**
    *   *Contingência:* Utilizar a esteira de testes automatizados de regressão para cobrir o básico do sistema enquanto o foco manual fica concentrado apenas nas novidades da entrega.

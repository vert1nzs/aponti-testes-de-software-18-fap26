# Atividade Avaliativa — Testes Manuais x Testes Automatizados

Este documento apresenta a análise de cenários do projeto e a tomada de decisão estratégica entre as abordagens de execução Manual e Automatizada, avaliando custo, repetição, estabilidade e objetivo.

---

### Cenário 1: Autenticação com Sucesso (Fluxo básico de Login)
*   **Classificação:** **AUTOMATIZADO**
*   **Justificativa:** É um fluxo altamente repetitivo, estável e de execução frequente. Automatizar essa rotina garante feedback rápido a cada novo deploy, alinhando-se perfeitamente com os critérios de execuções frequentes em fluxos estáveis.

### Cenário 2: Validação Visual e de Layout da nova tela de Dashboard
*   **Classificação:** **MANUAL**
*   **Justificativa:** O objetivo do teste é avaliar a usabilidade e a validação visual do layout. Essa abordagem baseia-se estritamente na observação humana e na flexibilidade a mudanças de design, características nativas do teste manual.

### Cenário 3: Fluxo Completo de Transferência Instantânea (Pix)
*   **Classificação:** **AUTOMATIZADO**
*   **Justificativa:** Trata-se de um teste de regressão crítico para o negócio em um fluxo já consolidado. Rodar esse cenário de forma automatizada protege a integridade da aplicação contra efeitos dominó gerados por novas correções.

### Cenário 4: Teste Exploratório em funcionalidade recém-desenvolvida
*   **Classificação:** **MANUAL**
*   **Justificativa:** Funcionalidades novas ou instáveis possuem alto custo de manutenção de script caso sejam automatizadas precocemente. O teste exploratório manual executado por uma pessoa permite flexibilidade para encontrar falhas de forma livre.

### Cenário 5: Testes de Fumaça (*Smoke Test*) de carregamento de páginas
*   **Classificação:** **AUTOMATIZADO**
*   **Justificativa:** Por ser uma validação executada obrigatoriamente a cada nova build para checar a estabilidade básica do sistema, sua automação reduz o esforço repetitivo da equipe enxuta, otimizando o tempo do projeto.

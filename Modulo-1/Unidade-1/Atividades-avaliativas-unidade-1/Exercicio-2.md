# Exercício 2 - Importância dos Testes e Custo de Defeitos

## Análise do Sistema PsicoCare

Este relatório apresenta a análise dos 8 defeitos identificados no sistema PsicoCare, avaliando suas fases ideais de detecção, os impactos gerados e a relação direta com a curva de custo de correção de defeitos.

---

### Defeito 1 – Ficha de Anamnese (Erro de Persistência de Dados)

*   **Fase do Ciclo para Detecção:** **Fase de Desenvolvimento (Testes Unitários / Integração).** O desenvolvedor deveria ter testado se a query de inserção (`INSERT`) no banco de dados estava funcionando antes de liberar a funcionalidade.
*   **Impacto para o Usuário:** Alta frustração da recepcionista e perda total de tempo, precisando refazer o trabalho de digitação da ficha médica.
*   **Impacto para o Negócio:** Risco severo de perda de informações clínicas confidenciais de saúde (anamnese), gerando desorganização no atendimento médico.
*   **Relação com o Custo de Correção:** Encontrar isso em produção exige parar a equipe médica, fazer engenharia reversa no banco e retrabalho de código, custando muito mais caro do que ter validado a persistência localmente no ambiente de desenvolvimento.

---

### Defeito 2 – Agenda de Consultas (Regra de Negócio)

*   **Fase do Ciclo para Detecção:** **Fase de Requisitos / Design de Software.** O analista de negócios e a equipe de desenvolvimento deveriam ter definido na especificação funcional que o sistema bloquearia cruzamento de horários idênticos para um mesmo profissional.
*   **Impacto para o Usuário:** Constrangimento extremo tanto para o psicólogo quanto para os dois pacientes que comparecerem no mesmo horário, gerando atrasos.
*   **Impacto para o Negócio:** Quebra de profissionalismo da clínica, perda de clientes devido à má experiência organizacional e ociosidade forçada de horários subsequentes para resolver o conflito.
*   **Relação com o Custo de Correção:** Erro de design/regra descoberto tardiamente exige alteração estrutural na lógica de agendamento do banco e da API, o que multiplica o custo de correção se comparado a ajustar a regra no papel antes de codificar.

---

### Defeito 3 – Cadastro do Psicólogo (Validação de Entrada)

*   **Fase do Ciclo para Detecção:** **Fase de Desenvolvimento (Frontend / Testes Unitários).** O preenchimento incorreto de campos com poucos dígitos deveria ser travado direto na tela do usuário via máscaras de validação de formulário.
*   **Impacto para o Usuário:** Permite que dados incorretos e profissionais inválidos entrem no sistema sem nenhum aviso visual de erro.
*   **Impacto para o Negócio:** Risco de problemas de conformidade legal e fiscal com os conselhos regionais de psicologia (CRP), permitindo cadastros falsos na plataforma da clínica.
*   **Relação com o Custo de Correção:** Custo de correção baixo a médio se corrigido no código da interface, mas se houver necessidade de rodar scripts de limpeza de dados (*data cleansing*) no banco de produção para limpar cadastros errados, o custo operacional sobe de preço.

---

### Defeito 4 – Controle Financeiro (Validação de Negócio)

*   **Fase do Ciclo para Detecção:** **Fase de Desenvolvimento (Testes de API e Integração).** As rotas de transações financeiras na API backend devem conter validações que impeçam valores menores ou iguais a zero no recebimento.
*   **Impacto para o Usuário:** Erros graves no saldo exibido para o gestor financeiro da clínica, mascarando os recebimentos reais.
*   **Impacto para o Negócio:** Prejuízos financeiros reais imediatos, desbalanço de fluxo de caixa e falhas graves de contabilidade.
*   **Relação com o Custo de Correção:** Custo de correção muito alto. Lidar com erros de dinheiro em produção envolve auditorias demoradas, conciliação manual de contas e retrabalho técnico prioritário devido ao risco financeiro.

---

### Defeito 5 – Controle de Estoque (Regra de Negócio)

*   **Fase do Ciclo para Defeito:** **Fase de Desenvolvimento (Testes Funcionais de Sistema).** Um fluxo de testes ponta a ponta (E2E) simulando baixas maiores que o estoque atual pegaria essa falha facilmente.
*   **Impacto para o Usuário:** O responsável pelo almoxarifado não consegue confiar no estoque virtual, gerando falta física de materiais para os psicólogos (ex: testes impressos).
*   **Impacto para o Negócio:** Gargalos operacionais na clínica, compras emergenciais mais caras por falta de previsibilidade e interrupção de consultas específicas.
*   **Relação com o Custo de Correção:** Segue a regra clássica de crescimento exponencial do gráfico de Boehm: corrigir uma lógica básica de cálculo em produção demanda novos deploys complexos e paralisação do sistema de inventário.

---

### Defeito 6 – Exclusão de Paciente (Integridade de Banco de Dados)

*   **Fase do Ciclo para Detecção:** **Fase de Arquitetura de Software / Modelagem de Banco.** O arquiteto ou desenvolvedor deveria ter configurado chaves estrangeiras (`Foreign Keys`) com restrições de exclusão impedindo a deleção de registros pais com filhos órfãos.
*   **Impacto para o Usuário:** O sistema exibe consultas fantasmas na agenda sem link ou identificação de qual paciente pertencia, quebrando o fluxo de trabalho.
*   **Impacto para o Negócio:** Inconsistência severa e corrupção do banco de dados da clínica, gerando perda do histórico clínico e estatístico.
*   **Relação com o Custo de Correção:** Custo de reparo elevadíssimo. Corrigir integridade de dados corrompidos na produção exige que o DBA pare as operações para rodar scripts de recuperação complexos e perigosos.

---

### Defeito 7 – Pesquisa de Pacientes (Usabilidade)

*   **Fase do Ciclo para Detecção:** **Fase de Desenvolvimento / Testes Funcionais.** Um teste simples de validação de busca validaria o comportamento de *Case Insensitivity* (ignorar maiúsculas/minúsculas).
*   **Impacto para o Usuário:** Dificuldade na busca rápida, dando a falsa impressão de que o paciente não está cadastrado se a inicial não for digitada exatamente igual.
*   **Impacto para o Negócio:** Lentidão crônica no atendimento da recepção e geração de duplicidade de cadastros por engano dos operadores.
*   **Relação com o Custo de Correção:** Custo muito baixo. A alteração de uma função de busca simples no banco (como aplicar um `LOWER()` ou alterar o collation da busca) é rápida e barata de implementar.

---

### Defeito 8 – Login (Segurança)

*   **Fase do Ciclo para Detecção:** **Fase de Planejamento de Segurança (Requisitos não-funcionais).** Políticas elementares de segurança cibernética (como tentativas máximas de login) devem ser definidas antes do código começar.
*   **Impacto para o Usuário:** Exposição total das contas dos profissionais e dados de saúde de seus pacientes a ataques externos.
*   **Impacto para o Negócio:** Risco imenso de vazamento massivo de dados sensíveis (violação direta da LGPD), passível de processos judiciais severos, multas milionárias e encerramento das atividades da clínica por danos de reputação.
*   **Relação com o Custo de Correção:** Custo crítico e imensurável. Corrigir isso preventivamente custa horas de código. Corrigir isso após sofrer uma invasão real por ataque de força bruta pode custar a falência do negócio.

---

## Conclusão Geral e o Gráfico de Custo de Bugs

Todos os cenários acima comprovam empiricamente a tese defendida pelo gráfico de **Barry Boehm**: o custo de corrigir um bug cresce exponencialmente a cada etapa avançada no ciclo de vida do software. 

Se os problemas do PsicoCare fossem capturados nas etapas de **Requisitos**, **Arquitetura** ou **Testes Unitários**, o custo seria equivalente apenas ao tempo do desenvolvedor ajustar algumas linhas de raciocínio. Ao serem detectados em fases tardias ou em produção, geram impactos em cascata, custos massivos com horas extras, estresse operacional e prejuízos diretos à imagem e ao caixa da empresa.

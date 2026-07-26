# Desafio - Testes Funcionais e Não-Funcionais (Clínica Psi)

Este documento apresenta a análise de qualidade da **Clínica Psi**, cobrindo o planejamento de testes funcionais (do unitário ao aceitação) e o mapeamento de requisitos não funcionais.

---

## Parte 1 — Testes Funcionais

### Exercício 1 — Identificação das Funcionalidades

| Funcionalidade | Usuário | Entrada principal | Resultado esperado | Possível erro |
| :--- | :--- | :--- | :--- | :--- |
| **Cadastrar Paciente** | Recepcionista | Nome, CPF, E-mail e Telefone | Paciente salvo com sucesso e disponível para busca | CPF já cadastrado no sistema |
| **Agendar Consulta** | Recepcionista | Paciente, Psicólogo, Data e Horário | Consulta registrada e exibida na agenda do profissional | Conflito: Horário indisponível para o psicólogo |
| **Cancelar Consulta** | Recepcionista / Psicólogo | ID da Consulta e Justificativa | Horário desmarcado e liberado na agenda | Tentar cancelar consulta retroativa (passada) |
| **Lançar Receita** | Administrador | Valor, Categoria e Data do Recebimento | Saldo do caixa atualizado positivamente | Valor negativo ou campo obrigatório em branco |
| **Consultar Prontuário** | Psicólogo | Registro do Paciente e Credenciais | Evoluções clínicas exibidas na tela | Usuário sem permissão (ex: Recepcionista) tentar abrir |

---

### Exercício 2 — Testes Unitários

| Função / Regra | Entrada | Resultado esperado | Por que é unitário? |
| :--- | :--- | :--- | :--- |
| **Calcular Saldo** | Receitas: R$ 2.000; Despesas: R$ 800 | R$ 1.200 | Avalia isoladamente uma pequena unidade de código (função matemática), sem depender de banco de dados ou interfaces. |
| **Validar CPF** | "123.456.789-00" (Inválido) | Retorna `False` (Falso) | Valida apenas o algoritmo lógico do cálculo dos dígitos verificadores do documento de forma isolada. |
| **Validar E-mail** | "everthon.com" (Sem @) | Retorna `False` (Falso) | Testa isoladamente a expressão regular (Regex) responsável por validar a sintaxe do texto inserido. |
| **Estoque Mínimo** | Atual: 2 unidades; Mínimo: 5 unidades | Alerta `True` (Estoque Baixo) | Verifica estritamente a condição lógica de comparação entre duas variáveis locais. |
| **Campos Obrigatórios** | Nome: ""; CPF: "111.222.333-44" | Retorna `Erro: Nome Obrigatório` | Testa se a função de validação interna do formulário rejeita strings vazias antes de enviar os dados. |

---

### Exercício 3 — Testes de Integração

| Componentes Integrados | Ação | Resultado esperado | Risco |
| :--- | :--- | :--- | :--- |
| **Cadastro de Paciente + Banco de Dados** | Salvar nova ficha de anamnese | Dados gravados de forma persistente e localizáveis | Sistema exibir mensagem de sucesso mas não gravar no banco |
| **Agendamento + Agenda do Psicólogo** | Registrar consulta às 14:00 | Horário passa a constar como ocupado na grade | Dois pacientes agendados no mesmo horário por falha de sincronia |
| **Check-in + Controle de Presença** | Marcar "Paciente Presente" | Taxa de assiduidade do paciente recalculada | Check-in não atualizar o histórico estatístico de presença |
| **Consulta Realizada + Lançamento Financeiro** | Finalizar atendimento clínico | Valor da sessão injetado automaticamente nas receitas | Consulta encerrada sem gerar o registro de entrada no caixa |
| **Login + Controle de Permissões** | Autenticar como Recepcionista | Menu de prontuários ocultado ou bloqueado | Falha no token permitir acesso a dados sigilosos de saúde |

---

### Exercício 4 — Testes de Sistema (Cenários Completos)

#### Cenário A — Atendimento Completo
*   **Pré-condições:** Usuário logado como administrador; Psicólogo cadastrado no sistema.
*   **Dados utilizados:** Nome: "José da Silva", CPF: "123.456.789-11", Horário: 15:00, Valor: R$ 150,00.
*   **Passos:** 1. Cadastrar o paciente José; 2. Localizar José na barra de pesquisa; 3. Agendar consulta para as 15:00; 4. Realizar o check-in na chegada; 5. Registrar evolução da sessão; 6. Lançar receita de R$ 150; 7. Conferir relatório financeiro.
*   **Resultado esperado:** Fluxo concluído do início ao fim, saldo financeiro acrescido e evolução salva no histórico do paciente.
*   **Resultado obtido:** Sistema operou com sucesso e todos os dados foram persistidos conforme esperado.
*   **Situação:** Aprovado
*   **Evidência:** `Logs de sucesso na execução e tabelas atualizadas no navegador.`
*   **Justificativa:** É teste de sistema pois avalia a aplicação completa e ponta a ponta (E2E), simulando a jornada real de uso através da interface gráfica.

#### Cenário B — Reagendamento e Liberação de Horários
*   **Pré-condições:** Consulta previamente agendada para Terça-feira às 14:00.
*   **Dados utilizados:** ID da consulta, Nova Data: Quinta-feira às 10:00.
*   **Passos:** 1. Localizar agendamento atual; 2. Clicar em Reagendar; 3. Selecionar Quinta-feira às 10:00; 4. Confirmar; 5. Abrir a grade de Terça-feira 14:00; 6. Abrir a grade de Quinta-feira 10:00.
*   **Resultado esperado:** O horário de Terça às 14:00 deve voltar a ficar vago (disponível) e o de Quinta às 10:00 deve ficar ocupado.
*   **Resultado obtido:** Horário anterior foi liberado com sucesso e o novo horário foi bloqueado para o psicólogo.
*   **Situação:** Aprovado
*   **Evidência:** `Grade da agenda atualizada visualmente na interface.`
*   **Justificativa:** Classificado como sistema por validar múltiplas regras de interface e comportamento de telas coordenadas em tempo real.

#### Cenário C — Controle de Estoque
*   **Pré-condições:** Produto "Testes Impressos" cadastrado com 10 unidades. Estoque mínimo definido como 5.
*   **Dados utilizados:** Saída: 7 unidades.
*   **Passos:** 1. Acessar controle de estoque; 2. Registrar saída de 7 unidades; 3. Verificar quantidade final; 4. Checar painel de alertas.
*   **Resultado esperado:** Quantidade final igual a 3 e o alerta visual de "Estoque abaixo do mínimo" ativo na tela.
*   **Resultado obtido:** Saldo atualizado para 3 unidades e o alerta crítico foi disparado corretamente.
*   **Situação:** Aprovado
*   **Evidência:** `Banner vermelho de alerta visível no painel de estoque.`
*   **Justificativa:** É um teste de sistema pois avalia o fluxo de controle de insumos integrado à camada de avisos de usabilidade para o gestor.

#### Cenário D — Controle de Acesso de Segurança
*   **Pré-condições:** Perfis de "Recepcionista" e "Psicólogo" configurados com suas devidas restrições.
*   **Dados utilizados:** Usuário: `recep_clinica`, Senha: `123`.
*   **Passos:** 1. Fazer login como Recepcionista; 2. Tentar forçar a entrada na página de Prontuários; 3. Deslogar; 4. Fazer login como Psicólogo; 5. Tentar acessar a mesma página.
*   **Resultado esperado:** O sistema deve exibir "Acesso Negado" para a recepcionista e permitir livre acesso para o psicólogo.
*   **Resultado obtido:** Bloqueio de segurança funcionou na conta de recepção e liberou o acesso na conta médica.
*   **Situação:** Aprovado
*   **Evidência:** `Mensagem de erro HTTP 403 (Forbidden) capturada no perfil sem permissão.`
*   **Justificativa:** Valida o comportamento global da aplicação quanto aos níveis de autorização do usuário final via interface.

---

### Exercício 5 — Testes de Aceitação

*   **Critério 1 (Agendamento Eficiente):**
    *   **Dado que** o paciente e o psicólogo estejam cadastrados,
    *   **quando** a recepcionista selecionar uma data e um horário disponível,
    *   **então** o sistema deverá registrar a consulta e exibi-la na agenda do psicólogo.
*   **Critério 2 (Bloqueio de Choque de Horários):**
    *   **Dado que** o psicólogo já possui uma consulta marcada na Segunda-feira às 14:00,
    *   **quando** outra recepcionista tentar agendar um novo paciente no mesmo horário e profissional,
    *   **então** o sistema deve bloquear a operação e exibir um alerta de horário indisponível.
*   **Critério 3 (Privacidade de Prontuários):**
    *   **Dado que** um usuário está logado com o perfil de Recepcionista,
    *   **quando** tentar abrir a tela de evolução clínica ou prontuário de um paciente,
    *   **então** o sistema deve negar o acesso e manter o sigilo das informações médicas.
*   **Critério 4 (Atualização Automática de Caixa):**
    *   **Dado que** o administrador registrou o recebimento de uma consulta de R$ 200,00,
    *   **quando** o fluxo for finalizado,
    *   **então** o saldo financeiro geral deve ser recalculado imediatamente somando o novo valor.
*   **Critério 5 (Persistência pós-relogin):**
    *   **Dado que** a recepcionista cadastrou 3 novos pacientes na sessão atual,
    *   **quando** ela deslogar do sistema, fechar o navegador e realizar o login novamente,
    *   **então** todos os dados inseridos anteriormente devem continuar preservados e consultáveis.

> **Justificativa do Nível:** O teste de aceitação valida se o comportamento macro do software atende perfeitamente aos requisitos de negócio contratuais, permitindo que a direção da clínica aprove a ferramenta para uso oficial.

---

### Exercício 6 — Classificação dos Testes

1.  **Verificar se receitas − despesas retorna o saldo correto:** **Unitário.** Testa isoladamente a integridade de uma operação matemática interna do código.
2.  **Verificar se uma receita salva aparece no relatório financeiro:** **Integração.** Avalia a comunicação e a passagem correta de dados entre o módulo de lançamentos e o módulo gerador de relatórios.
3.  **Executar todo o fluxo entre cadastro, atendimento e pagamento:** **Sistema.** Um cenário de ponta a ponta (E2E) que simula o dia a dia completo do usuário através da interface do sistema.
4.  **Confirmar com a direção da clínica se o relatório atende às necessidades:** **Aceitação.** Validação de negócio realizada diretamente com o cliente/usuário responsável para dar o aval de conformidade.
5.  **Verificar isoladamente a validação de CPF:** **Unitário.** Valida uma única regra lógica de validação de dados dentro de uma função isolada do código.

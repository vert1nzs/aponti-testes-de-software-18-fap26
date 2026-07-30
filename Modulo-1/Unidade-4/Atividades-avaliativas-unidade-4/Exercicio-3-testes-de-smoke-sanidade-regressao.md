# Atividade Avaliativa: Testes de Smoke, Sanidade e Regressão

**Cenário de Análise:** Uma nova versão de um sistema bancário foi implantada trazendo uma correção específica no fluxo de login e um ajuste técnico na exibição do saldo na tela inicial.

---

## 1. Testes de Smoke (Teste de Fumaça)

> **Justificativa do Teste Smoke:** Esses testes avaliam a integridade geral do sistema. Se qualquer um deles falhar, o aplicativo está inutilizável e a build deve ser rejeitada imediatamente para correções da equipe de desenvolvimento.

### SMK01 - Carregamento Inicial do Aplicativo/Página Bancária
*   **PRÉ-REQUISITOS:** Servidor da aplicação ativo.
*   **DADOS:** URL do sistema bancário.
*   **PASSOS:** 
    1. Inserir a URL do banco no navegador.
    2. Aguardar o carregamento da interface gráfica.
*   **RESULTADO ESPERADO:** A página inicial carrega completamente em menos de 3 segundos, exibindo os campos de login e o logotipo do banco sem erros visuais.

### SMK02 - Autenticação Básica com Sucesso
*   **PRÉ-REQUISITOS:** Usuário cadastrado e ativo.
*   **DADOS:** Agência: `1234` | Conta: `55667-8` | Senha: `SenhaValida01`
*   **PASSOS:** 
    1. Inserir agência, conta e senha corretas.
    2. Clicar no botão “Entrar”.
*   **RESULTADO ESPERADO:** O sistema processa o login e redireciona o usuário para o painel principal (`/dashboard`).

### SMK03 - Visualização da Tela Inicial Logada (Dashboard)
*   **PRÉ-REQUISITOS:** Usuário ter acabado de realizar o login com sucesso.
*   **DADOS:** Fluxo de redirecionamento de tela.
*   **PASSOS:** 
    1. Observar a tela do painel principal após a autenticação.
*   **RESULTADO ESPERADO:** A interface gráfica principal é exibida com todos os menus utilizáveis, provando que a área restrita está de pé.

### SMK04 - Exibição do Saldo Bancário
*   **PRÉ-REQUISITOS:** Usuário posicionado na tela inicial logada.
*   **DADOS:** Nenhuns (Apenas Leitura).
*   **PASSOS:** 
    1. Olhar para a área reservada à exibição de valores financeiros no topo do painel.
*   **RESULTADO ESPERADO:** O componente de saldo exibe as informações numéricas de forma legível, sem falhas de comunicação com o servidor.

### SMK05 - Encerramento de Sessão Seguro (Logout)
*   **PRÉ-REQUISITOS:** Usuário logado na conta.
*   **DADOS:** Clique do Mouse.
*   **PASSOS:** 
    1. Clicar no botão “Sair” localizado no menu de perfil.
*   **RESULTADO ESPERADO:** O sistema destrói a sessão e redireciona o usuário de volta para a tela de login vazia.

---

## 2. Testes de Sanidade

> **Justificativa do Teste de Sanidade:** Esses testes concentram esforços em validar se as correções feitas no login e no layout do saldo corrigem os bugs reportados anteriormente sem quebrar o comportamento dessas duas telas específicas.

### SAN01 - Login Rejeitado com Senha Incorreta
*   **PRÉ-REQUISITOS:** Usuário cadastrado.
*   **DADOS:** Agência: `1234` | Conta: `55667-8` | Senha: `SenhaErrada02`
*   **PASSOS:** 
    1. Preencher os dados de conta válidos.
    2. Inserir uma senha inválida.
    3. Clicar em "Entrar".
*   **RESULTADO ESPERADO:** O sistema bloqueia a entrada, permanece na tela de login e exibe a mensagem: "Agência, conta ou senha inválidos".

### SAN02 - Tratamento de Campos de Entrada de Login Vazios
*   **PRÉ-REQUISITOS:** Nenhuma.
*   **DADOS:** Campos de texto sem preenchimento.
*   **PASSOS:** 
    1. Acessar a tela de login.
    2. Deixar todos os campos em branco.
    3. Clicar em “Entrar”.
*   **RESULTADO ESPERADO:** O sistema impede o envio do formulário e sinaliza em vermelho as caixas obrigatórias.

### SAN03 - Formatação Monetária do Ajuste de Saldo Corrente
*   **PRÉ-REQUISITOS:** Usuário logado com saldo positivo em conta.
*   **DADOS:** Saldo real de `R$ 1.250,00`.
*   **PASSOS:** 
    1. Acessar o painel inicial.
    2. Validar a máscara de formatação do saldo.
*   **RESULTADO ESPERADO:** O saldo deve ser renderizado perfeitamente seguindo o padrão nacional (`R$ 1.250,00`), sem erros de espaçamento, fonte ou símbolos corrompidos.

### SAN04 - Comportamento Visual de Conta com Saldo Negativo
*   **PRÉ-REQUISITOS:** Usuário logado utilizando o limite do cheque especial.
*   **DADOS:** Saldo devedor de `R$ 500,00`.
*   **PASSOS:** 
    1. Acessar o painel inicial e olhar a exibição do saldo.
*   **RESULTADO ESPERADO:** O sistema exibe o sinal gráfico correto de negativo (`-R$ 500,00`) ou destaca o valor em vermelho, conforme o novo ajuste visual implementado.

### SAN05 - Ocultação Dinâmica de Saldo para a Privacidade
*   **PRÉ-REQUISITOS:** Usuário visualizando o seu saldo na tela inicial.
*   **DADOS:** Clique no botão de privacidade.
*   **PASSOS:** 
    1. Clicar no ícone de “olho/ocultar” posicionado ao lado do saldo.
*   **RESULTADO ESPERADO:** O valor numérico do saldo é substituído instantaneamente por caracteres ocultos (`R$ ******`), confirmando que a alteração foi concluída com sucesso.

---

## 3. Testes de Regressão

> **Justificativa do Grupo Regressão:** Esses cenários atestam que as partes consolidadas e imutáveis da plataforma bancária (módulos transacionais e operacionais) continuam operando perfeitamente após a introdução da nova build de login e saldo.

### REG01 - Realização de Transferência Instantânea (PIX) com Sucesso
*   **PRÉ-REQUISITOS:** Usuário logado na conta e com saldo disponível para a transação.
*   **DADOS:** Chave PIX do destinatário e o valor desejado.
*   **PASSOS:** 
    1. Acessar a área “Pix” através do menu lateral.
    2. Inserir a chave pix e o valor da transferência.
    3. Digitar a sua senha de transações e clicar em “Confirmar Transação”.
*   **RESULTADO ESPERADO:** A transferência é processada, o comprovante de envio é gerado na tela e o dinheiro é debitado corretamente do saldo da conta.

### REG02 - Consulta ao Extrato Detalhado dos Últimos 30 Dias
*   **PRÉ-REQUISITOS:** Usuário logado com transações recentes registradas.
*   **DADOS:** Clique no menu de extrato bancário.
*   **PASSOS:** 
    1. Acessar the menu "Extrato".
    2. Selecionar o filtro "Últimos 30 dias".
*   **RESULTADO ESPERADO:** O sistema carrega a listagem cronológica de todas as entradas e saídas financeiras do mês sem falhas.

### REG03 - Pagamento de Boleto Bancário via Código de Barras
*   **PRÉ-REQUISITOS:** Usuário com fundos disponíveis em conta.
*   **DADOS:** Linha digitável de um boleto válido e dentro do prazo de vencimento.
*   **PASSOS:** 
    1. Acessar a aba "Pagamentos".
    2. Colar o código numérico do boleto.
    3. Confirmar o agendamento/pagamento.
*   **RESULTADO ESPERADO:** O sistema identifica o emissor do boleto, processa a baixa do título e emite a autenticação bancária de sucesso.

### REG04 - Alteração de Dados Cadastrais no Perfil do Cliente
*   **PRÉ-REQUISITOS:** Usuário logado na conta corrente.
*   **DADOS:** Novo número de telefone para contato.
*   **PASSOS:** 
    1. Acessar "Configurações do Perfil".
    2. Alterar o campo de telefone de contato.
    3. Clicar em "Salvar alterações".
*   **RESULTADO ESPERADO:** O sistema salva as novas informações e exibe o alerta: "Dados cadastrais atualizados com sucesso".

### REG05 - Solicitação de Atendimento via Chat de Suporte Integrado
*   **PRÉ-REQUISITOS:** Usuário posicionado na área logada.
*   **DADOS:** Clique no botão de ajuda.
*   **PASSOS:** 
    1. Localizar e clicar no ícone flutuante de "Ajuda/Chat" no canto inferior da tela.
*   **RESULTADO ESPERADO:** A janela interna do chat de atendimento se abre de forma responsiva, permitindo o envio de mensagens textuais para a equipe de suporte.

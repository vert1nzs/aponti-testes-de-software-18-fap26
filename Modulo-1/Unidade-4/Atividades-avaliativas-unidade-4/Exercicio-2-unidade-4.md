# Atividade Avaliativa - Análise e Escrita de Testes: Sistema Bancário

Este documento apresenta o mapeamento de fluxos, a escrita de casos de teste de Sistema e de Aceitação, e a devida justificativa técnica para o cenário de um sistema bancário de consulta de saldo.

---

## Etapa 1 - Compreensão do Cenário

*   **Sistema:** Plataforma Bancária Digital.
*   **Objetivo:** Permitir que o usuário realize login, acesse o painel da sua conta e visualize o saldo atualizado.

### 1. Funcionalidades Envolvidas
*   **Módulo de Autenticação (Login):** Validação das credenciais do cliente (Agência, Conta e Senha).
*   **Painel da Conta (Dashboard):** Carregamento e integração das telas pós-login.
*   **Consulta de Saldo Corrente:** Consulta ao banco de dados e renderização do valor monetário na tela.

### 2. Fluxo Principal
*   Usuário acessa a página de login do banco.
*   Inserção de Agência, Conta e Senha eletrônica corretas.
*   Usuário clica no botão "Entrar".
*   Sistema valida os dados e carrega a tela inicial da conta.
*   saldo atualizado é exibido na tela no formato `R$ X.XXX,XX`.

### 3. Variações de Fluxo
*   **Variação A (Erro de credenciais):** Usuário digita senha incorreta e o sistema barra o acesso na tela inicial.
*   **Variação B (Instabilidade na API):** Usuário loga com sucesso, mas o servidor de saldo está fora do ar (sistema deve exibir mensagem de erro amigável em vez de quebrar a tela).

---

## Etapa 2 - Testes de Sistema
*Foco técnico: Validar o funcionamento e a integração entre as telas pós-login.*

### TS01: Login Eficiente e Redirecionamento de Tela
*   **Pré-condições:** Usuário com conta ativa no banco de dados.
*   **Dados:** Agência: `1234` | Conta: `55667-8` | Senha: `SenhaValida1`
*   **Passos:**
    1. Acessar a URL de login do sistema bancário.
    2. Preencher os campos "Agência", "Conta" e "Senha" com dados válidos.
    3. Clicar no botão "Entrar".
*   **Resultado Esperado:** O sistema autentica a requisição, fecha a tela de login e carrega a tela do Painel Principal (`/dashboard`).

### TS02: Exibição Automatizada do Componente de Saldo
*   **Pré-condições:** Usuário logado com sucesso e posicionado na tela `/dashboard`.
*   **Dados:** Fluxo automatizado de carregamento de tela.
*   **Passos:**
    1. Visualizar o topo da página do painel principal após o login.
*   **Resultado Esperado:** O componente visual de saldo carrega automaticamente em menos de 2 segundos, exibindo o valor financeiro sem quebras de layout.

### TS03: Bloqueio de Navegação Direta via URL (Fluxo Alternativo)
*   **Pré-condições:** Usuário deslogado do sistema.
*   **Dados:** URL restrita interna.
*   **Passos:**
    1. Digitar diretamente na barra do navegador o endereço interno do painel (`://banco.com`) sem estar logado.
*   **Resultado Esperado:** O sistema intercepta o acesso, impede o carregamento do painel e redireciona o navegador de volta para a tela de login.

### TS04: Tratamento de Erro ao Digitar Dados Incorretos (Fluxo Alternativo)
*   **Pré-condições:** Nenhuma.
*   **Dados:** Agência: `0000` | Conta: `00000-0` | Senha: `Invalida`
*   **Passos:**
    1. Acessar a página de login.
    2. Inserir os dados inválidos nos campos.
    3. Clicar no botão "Entrar".
*   **Resultado Esperado:** O sistema impede a autenticação, mantém o usuário na mesma página e exibe a mensagem técnica: "Usuário ou senha não encontrados".

---

## Etapa 3 - Testes de Aceitação
*Foco do negócio: Verificar a entrega de valor real e as expectativas do cliente final.*

### TA01: Conferência Prática do Saldo para Tomada de Decisão
*   **Pré-condições:** Cliente possui R$ 250,00 reais em conta corrente.
*   **Dados:** Credenciais de acesso do cliente.
*   **Passos:**
    1. Realizar o login no sistema do banco.
    2. Olhar o campo "Saldo Disponível" na tela inicial para planejar o pagamento de uma conta.
*   **Resultado Esperado:** O cliente consegue visualizar o valor exato de `R$ 250,00` de forma clara, legível e em formato de moeda nacional, confirmando que possui fundos disponíveis.

### TA02: Ocultação de Saldo por Motivos de Privacidade
*   **Pré-condições:** Usuário logado no painel principal visualizando seu saldo.
*   **Dados:** Clique no botão de privacidade.
*   **Passos:**
    1. Clicar no ícone de "olho/ocultar" ao lado do valor do saldo para esconder o dinheiro de pessoas próximas.
*   **Resultado Esperado:** O valor numérico do saldo é substituído imediatamente por caracteres simbólicos (ex: `R$ ******`), garantindo a privacidade visual do cliente em locais públicos.

### TA03: Visualização Limpa de Conta sem Fundos (Fluxo Alternativo)
*   **Pré-condições:** Cliente está com a conta zerada (R$ 0,00).
*   **Dados:** Credenciais de conta sem saldo.
*   **Passos:**
    1. Efetuar o login na plataforma bancária.
    2. Verificar a área de saldo do painel.
*   **Resultado Esperado:** O sistema exibe o valor de `R$ 0,00` de forma transparente, permitindo que o cliente saiba exatamente que não possui dinheiro sem gerar confusões ou telas em branco.

### TA04: Aviso de Indisponibilidade de Consulta Financeira (Fluxo Alternativo)
*   **Pré-condições:** Canal de comunicação do banco de dados passando por manutenção técnica.
*   **Dados:** Credenciais válidas de acesso.
*   **Passos:**
    1. Efetuar o login na plataforma durante a manutenção do servidor.
*   **Resultado Esperado:** O sistema realiza o login para que o usuário não fique trancado fora do banco, mas na área do saldo exibe um aviso claro: "Não conseguimos carregar seu saldo agora. Tente em instantes", garantindo a transparência do serviço.

---

## Etapa 4 - Justificativa e Classificação

### Por que os casos TS01, TS02, TS03 e TS04 são Testes de Sistema?
*   **Objetivo:** Validar a estabilidade do software, os fluxos técnicos de navegação e se os dados estão sendo transmitidos corretamente entre a tela de login e o painel interno.
*   **Ponto de Vista:** Adota a perspectiva técnica do testador de software (Analista de QA).
*   **Tipo de Validação:** Caixa-preta funcional, focada em testar se os componentes, botões, redirecionamentos e mensagens de erro do sistema funcionam integrados conforme as especificações do projeto.

### Por que os casos TA01, TA02, TA03 e TA04 são Testes de Aceitação?
*   **Objetivo:** Verificar se o sistema atende aos critérios de aceitação do negócio, cumpre os requisitos de valor para o cliente (como privacidade e clareza de dados) e está pronto para o uso real.
*   **Ponto de Vista:** Adota a perspectiva do usuário final (cliente do banco) e da diretoria do negócio.
*   **Tipo de Validação:** Validação de Critérios de Aceite e Experiência do Usuário (UX), garantindo que a informação do saldo seja útil, confiável e entregue valor real ao dia a dia do correntista.

---

## Etapa 5 - Revisão por Pares (Template)
*Espaço reservado para documentar a análise das atividades dos colegas de classe conforme os critérios de Clareza, Estrutura e Coerência.*

*   **Projeto Revisado:** [Nome do Aluno / Link do Repositório]
*   **Análise de Clareza:** [Escrever se os passos do colega foram fáceis de entender]
*   **Análise de Estrutura:** [Verificar se continha ID, Título, Passos e Resultados]
*   **Análise de Coerência:** [Indicar se o colega soube separar o que era técnico (Sistema) do que era valor de negócio (Aceitação)]

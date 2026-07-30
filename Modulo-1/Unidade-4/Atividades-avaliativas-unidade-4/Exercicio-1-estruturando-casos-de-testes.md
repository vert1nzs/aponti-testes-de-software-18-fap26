# ESTRUTURANDO CASOS DE TESTES

## CENÁRIOS DE TESTE COMPLETOS:

### PARTE 1 - CAMINHO FELIZ:

**CT01 - Autenticação com Sucesso:**
* **Pré condições:** Usuário cadastrado e ativo no sistema
* **Dados:** E-mail: `caminhofeliz@gmail.com` | Senha: `testefeliz01`
* **Passos:** 
  1. Acessar a página inicial do sistema
  2. Digitar o e-mail válido
  3. Digitar uma senha válida, de acordo com o cadastro
  4. Clicar no Botão Enter
* **Resultado Esperado:** É esperado que o login seja aprovado com sucesso.

**CT02 - Funcionalidade - “Lembrar de mim”:**
* **Pré - condições:** Caixa de marcação para lembrar o login daquele usuário.
* **Dados:** E-mail: `caminhofeliz@gmail.com` | Senha: `testefeliz01`
* **Passos:** 
  1. Acessar a página inicial do sistema
  2. Digitar o e-mail válido
  3. Digitar uma senha válida, de acordo com o cadastro
  4. Clicar no Botão Enter e Marcar o “Lembrar de mim”
* **Resultado Esperado:** O login seja salvo como um token de acesso vinculado ao seu navegador e fique salvo durante um longo período.

**CT03 - Exibição de Senha em Texto Limpo (Ícone do Olho):**
* **Pré condições:** Campo de senha configurado com máscara protetora ativa por padrão
* **Dados:** Senha: `testefeliz01`
* **Passos:** 
  1. Acessar a página inicial do sistema
  2. Digitar o e-mail válido
  3. Digitar a senha correta no campo ocultado por bolinhas
  4. Clicar no ícone de "olho" no canto direito do campo
* **Resultado Esperado:** A máscara de proteção é removida e o texto `testefeliz01` passa a ficar visível e legível na tela.

**CT04 - Remoção de Espaços em Branco Involuntários (Trim):**
* **Pré condições:** Usuário cadastrado com o e-mail exato de teste
* **Dados:** E-mail: `caminhofeliz@gmail.com` (com um espaço no início e um no fim) | Senha: `testefeliz01`
* **Passos:** 
  1. Acessar a página inicial do sistema
  2. Digitar o e-mail inserindo os espaços extras
  3. Digitar a senha correta
  4. Clicar no Botão Enter
* **Resultado Esperado:** O sistema ignora os espaços nas pontas do texto, processa o tratamento dos dados e efetua o login redirecionando para a área logada.

**CT05 - Link para Recuperação de Senha:**
* **Pré condições:** Nenhuma
* **Dados:** Clique no link do mouse
* **Passos:** 
  1. Acessar a página inicial do sistema
  2. Localizar a opção "Esqueci minha senha" abaixo do formulário
  3. Clicar no link de texto correspondente
* **Resultado Esperado:** O sistema abre a página de redefinição de credenciais (`recuperar-senha`) exibindo a caixa para inserção de e-mail.

---

### PARTE 2 - CENÁRIOS ALTERNATIVOS:

**CT06 - Login com E-mail Não Cadastrado:**
* **Pré condições:** Nenhuma
* **Dados:** E-mail: `inexistente@gmail.com` | Senha: `testefeliz01`
* **Passos:** 
  1. Acessar a página inicial do sistema
  2. Digitar o e-mail não registrado
  3. Digitar a senha de teste
  4. Clicar no Botão Enter
* **Resultado Esperado:** O sistema impede o login, permanece na mesma página e renderiza a mensagem em vermelho: "E-mail ou senha inválidos".

**CT07 - Login com Senha Incorreta:**
* **Pré condições:** Usuário previamente cadastrado e ativo
* **Dados:** E-mail: `caminhofeliz@gmail.com` | Senha: `senhaerrada123`
* **Passos:** 
  1. Acessar a página inicial do sistema
  2. Digitar o e-mail válido
  3. Digitar a senha incorreta
  4. Clicar no Botão Enter
* **Resultado Esperado:** O sistema impede o login, permanece na mesma página e renderiza a mensagem em vermelho: "E-mail ou senha inválidos".

**CT08 - Bloqueio de Conta por Tentativas Excessivas (Segurança):**
* **Pré condições:** Usuário cadastrado e ativo no banco de dados
* **Dados:** E-mail: `caminhofeliz@gmail.com` | Senha: `senhaerrada123`
* **Passos:** 
  1. Digitar o e-mail correto e a senha incorreta
  2. Clicar no Botão Enter
  3. Repetir esse processo por 5 vezes consecutivas
  4. Na 6ª tentativa, digitar a senha correta `testefeliz01` e tentar entrar
* **Resultado Esperado:** O sistema rejeita o acesso na 6ª tentativa e dispara o alerta crítico: "Conta temporariamente bloqueada por 15 minutos".

**CT09 - Validação de Campos Obrigatórios Vazios (Usabilidade):**
* **Pré condições:** Nenhuma
* **Dados:** Campos de texto sem preenchimento (em branco)
* **Passos:** 
  1. Acessar a página inicial do sistema
  2. Deixar os inputs de e-mail e senha intocados
  3. Clicar diretamente no Botão Enter
* **Resultado Esperado:** O formulário impede a submissão e ativa os rótulos de erro textuais: "O campo E-mail é obrigatório" e "O campo Senha é obrigatório".

**CT10 - Injeção de Código Malicioso nos Inputs (SQL Injection):**
* **Pré condições:** Nenhuma
* **Dados:** E-mail: `' OR '1'='1` | Senha: `' OR '1'='1`
* **Passos:** 
  1. Acessar a página inicial do sistema
  2. Copiar e colar a sintaxe de script SQL nos campos de e-mail e senha
  3. Clicar no Botão Enter
* **Resultado Esperado:** O sistema bloqueia a requisição de banco de dados por segurança, trata a entrada puramente como texto inválido e exibe o aviso de dados inválidos na tela.

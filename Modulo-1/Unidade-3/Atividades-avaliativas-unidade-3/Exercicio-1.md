# Exercício 1 - Níveis de Teste de Software (SGP)

Este documento apresenta a análise do **SGP (Sistema de Gerenciamento de Pedidos)** com a indicação e justificativa dos cenários aplicados a cada nível da pirâmide de testes (Unitário, Integração, Sistema e Aceitação).

---

## 1. Testes Unitários

### O que são?
Focados em testar a menor unidade de código isolada (funções, métodos ou classes), sem interagir com recursos externos como bancos de dados ou APIs.

### Cenários propostos para o SGP:
* **Validação do cálculo de preço:** Testar isoladamente a função matemática que multiplica a `quantidade` pelo `preço` de um produto.
* **Validação de regras de carrinho vazio:** Testar o método responsável por impedir o avanço caso a lista de itens do pedido possua tamanho igual a zero.
* **Formatador de dados de usuário:** Testar se as funções de validação de e-mail ou força de senha no cadastro de usuários retornam verdadeiro ou falso corretamente de acordo com os inputs.

### Justificativa:
O objetivo neste nível é garantir que os algoritmos internos e as regras lógicas básicas da aplicação funcionem perfeitamente em nível de código antes de qualquer comunicação complexa.

---

## 2. Testes de Integração

### O que são?
Focados em validar se a comunicação entre diferentes módulos do sistema ou sistemas externos (APIs, Banco de Dados, Serviços de terceiros) ocorre sem falhas.

### Cenários propostos para o SGP:
* **Integração com o Banco de Dados de Produtos:** Testar se a aplicação consegue realizar uma consulta (Query SQL/NoSQL) e retornar com sucesso a listagem correta de produtos gravados no banco.
* **Integração com o Serviço de Autenticação:** Testar a comunicação entre o SGP e o serviço responsável por validar o token de login do usuário.
* **Integração com o Serviço de Pedidos:** Testar se o módulo de finalização consegue enviar com sucesso a carga de dados do pedido gerado para a API externa de gerenciamento de ordens.

### Justificativa:
As regras de negócio do SGP listam explicitamente três integrações críticas. Os testes de integração garantem que as interfaces de comunicação e contratos de API entre esses componentes distintos estejam integrados e funcionando sem perda ou corrupção de dados.

---

## 3. Testes de Sistema

### O que são?
Testes de ponta a ponta (End-to-End / E2E) que avaliam o comportamento do sistema completo e integrado, simulando a jornada real executada por meio da interface gráfica (UI) ou APIs principais.

### Cenários propostos para o SGP:
* **Fluxo completo de compra por usuário não autenticado:** Simular um usuário navegando no sistema deslogado, adicionando produtos ao carrinho, tentando finalizar a compra e validando se o sistema bloqueia o fluxo exigindo a autenticação (Regra de Negócio).
* **Fluxo de alteração pós-confirmação:** Executar a jornada completa de fechar um pedido com sucesso e, em seguida, tentar realizar uma requisição de modificação, garantindo que o sistema retorne um erro informando que o pedido não pode ser alterado após confirmado (Regra de Negócio).
* **Fluxo de exclusão de itens e recálculo:** Adicionar múltiplos itens, remover uma unidade do carrinho e checar se a tela atualiza o valor total dinamicamente de forma correta.

### Justificativa:
O teste de sistema foca no ecossistema completo. Ele valida se as regras de negócio complexas especificadas no slide funcionam em harmonia quando acionadas a partir da experiência final do cliente no navegador web.

---

## 4. Testes de Aceitação

### O que são?
Geralmente conduzidos ou validados sob a perspectiva do cliente/usuário de negócio para assegurar que a aplicação atende perfeitamente aos requisitos contratuais, critérios de aceite e necessidades do negócio.

### Cenários propostos para o SGP:
* **Homologação da Jornada do Administrador:** Validar se os administradores da clínica/loja conseguem extrair relatórios funcionais e gerenciar o fluxo de pedidos de forma eficiente conforme a especificação do SGP.
* **Testes de Caixa-Preta Baseados em Critérios de Aceite:** O usuário final realizar cenários reais do dia a dia (cadastrar-se, comprar itens, receber confirmação) para carimbar que o produto entregue resolve seu problema de gerenciamento.

### Justificativa:
O objetivo principal não é encontrar falhas técnicas ou exceções de código, mas sim certificar se o SGP está aderente às necessidades de mercado descritas para clientes e administradores, garantindo luz verde para o lançamento em produção.

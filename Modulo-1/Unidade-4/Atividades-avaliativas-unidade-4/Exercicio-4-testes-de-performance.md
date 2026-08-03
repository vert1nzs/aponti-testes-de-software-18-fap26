# Atividade Avaliativa — Análise de Relatório de Teste de Performance

**Cenário de Análise Base:** Avaliação de um relatório conceitual de performance executado na API de um sistema bancário sob uma carga simulada de 5.000 usuários simultâneos realizando acessos e consultas de saldo durante um período de 15 minutos.

---

### 1. O sistema pode ser considerado aprovado?
* **Resposta:** **Não, o sistema deve ser considerado REPROVADO.**
* **Justificativa:** Para ser aprovado, o sistema precisa operar dentro dos limites aceitáveis de mercado (SLAs). Nesse cenário, o tempo médio de resposta ultrapassou o limite saudável e a taxa de erros disparou sob carga máxima, indicando que a aplicação não está estável o suficiente para suportar o volume de produção de forma confiável.

---

### 2. Quais métricas indicam problemas de performance?
Ao analisar o comportamento dos gráficos, as seguintes métricas sinalizaram falhas críticas de desempenho:
* **Tempo de Resposta Médio (*Average Response Time*):** Iniciou estável em 800ms, mas saltou de forma ascendente para 6,2 segundos conforme o volume de usuários aumentou (o limite aceitável de mercado é de até 2 segundos).
* **Taxa de Erros (*Error Rate*):** Atingiu a marca de 8,5% do total de requisições, com forte incidência de erros do tipo HTTP 504 (*Gateway Timeout*) e HTTP 500 (*Internal Server Error*).
* **Vazão (*Throughput*):** O gráfico de requisições processadas por segundo estagnou (criou um teto plano), provando que o sistema atingiu um gargalo e parou de escalar mesmo com mais usuários tentando acessar.
* **Uso de CPU do Servidor:** O monitoramento de infraestrutura apontou que o processamento do servidor de banco de dados permaneceu cravado em 100% durante os minutos de pico do teste.

---

### 3. Quais possíveis gargalos podem existir?
Com base no comportamento das métricas avaliadas, os principais gargalos técnicos do ecossistema são:
* **Gargalo no Banco de Dados:** Consultas (*queries*) pesadas e sem indexação para buscar o saldo dos clientes, além do esgotamento do pool de conexões disponíveis para processar requisições simultâneas.
* **Efeito Cascata na API (*Timeout*):** Como o banco de dados ficou sobrecarregado e demorou para responder, a API estourou o tempo limite de espera (*timeout*), gerando os erros HTTP 504 exibidos para os usuários na ponta.
* **Falta de Escalabilidade da Infraestrutura:** O servidor de aplicação está operando de forma estática, sem capacidade de criar novas instâncias para dividir o peso do tráfego.

---

### 4. Esse cenário se aproxima mais de Carga, Stress ou Capacidade?
* **Resposta:** O cenário se aproxima mais de um teste de **STRESS (ESTRESSE)**.
* **Justificativa:** O objetivo principal do teste foi empurrar o sistema além do seu limite operacional normal para observar o seu ponto de ruptura (onde ele começaria a falhar). O gráfico de estresse clássico foi desenhado perfeitamente aqui: o sistema funcionou bem até un determinado número de acessos, estabilizou a vazão e, logo em seguida, a taxa de erros disparou e o tempo de resposta degradou ao extremo.

---

### 5. O que você recomendaria ao time técnico?
Para mitigar os gargalos encontrados e preparar a plataforma bancária para o ambiente real, as recomendações de Engenharia de Performance são:
* **Implementação de Camada de Cache (ex: Redis):** Salvar o saldo consultado em memória temporária por alguns minutos. Isso evita que o sistema precise fazer uma consulta pesada direto no banco de dados toda vez que o usuário atualizar a tela inicial.
* **Otimização e *Tuning* de *Queries* SQL:** Revisar a indexação das tabelas de contas e clientes no banco de dados para acelerar o tempo de busca das informações de saldo.
* **Configuração de *Auto-scaling* (Escalabilidade Horizontal):** Configurar regras na nuvem para que o sistema crie novos servidores automaticamente sempre que o consumo de CPU da aplicação ultrapassar o limite de 75%.
* ***Circuit Breaker* e Tratamento de Filas:** Implementar padrões de resiliência no código para que, em caso de lentidão, o sistema responda com mensagens amigáveis de espera de forma assíncrona, em vez de travar o navegador do cliente com erros de *timeout*.

# Exercício 2 - Checklist de Testes Não Funcionais (Plataforma PACO)

Este documento apresenta o checklist de testes não funcionais elaborado para a **Plataforma PACO (Plataforma de Agendamento de Consultas)**, cobrindo os pilares de Performance, Segurança, Usabilidade e Compatibilidade com base nos requisitos e regras do sistema.

---

## 1. Performance (Desempenho e Carga)

- [x] **Verificação:** Simular o uso simultâneo de múltiplos usuários realizando agendamentos concorrentes durante horários de pico (ex: abertura de agendas de especialistas altamente demandados).
    * **Risco Associado:** O sistema ficar lento, exibir erros de timeout ou cair completamente, impedindo que os pacientes consigam marcar suas consultas e sobrecarregando os canais físicos de atendimento da clínica.
- [x] **Verificação:** Validar o tempo de resposta da funcionalidade de consulta de especialidades e profissionais disponíveis sob carga normal de acessos (o ideal é que responda em menos de 2 segundos).
    * **Risco Associado:** Tempo de carregamento alto gera alta taxa de rejeição da página, fazendo o usuário desistir do agendamento virtual devido à má experiência.

## 2. Segurança (Proteção de Dados e Acesso)

- [x] **Verificação:** Validar se os dados pessoais e históricos médicos dos pacientes são criptografados tanto em trânsito (uso de protocolo HTTPS) quanto em repouso no banco de dados, atendendo às boas práticas e leis vigentes (LGPD).
    * **Risco Associado:** Vazamento massivo de informações confidenciais de saúde, resultando em processos judiciais severos, multas financeiras pesadas e destruição da reputação e confiabilidade da clínica.
- [x] **Verificação:** Forçar o acesso direto via URL às rotas internas da área administrativa e de agendamentos de terceiros sem um token de autenticação ativo.
    * **Risco Associado:** Falha de controle de acesso (BOLA/IDOR), permitindo que usuários comuns ou cibercriminosos alterem horários de médicos, cancelem consultas alheias ou acessem dados administrativos restritos.

## 3. Usabilidade (Facilidade de Uso e Fluxo)

- [x] **Verificação:** Avaliar a clareza e o fluxo para o cancelamento de consultas, garantindo que o sistema alerte visualmente e impeça a ação caso falte menos de 24 horas para o horário agendado (Regra de Negócio).
    * **Risco Associado:** O paciente não entender a regra de 24 horas por falta de avisos claros no layout, gerando frustração, reclamações de suporte e confusão na política de cancelamentos.
- [x] **Verificação:** Validar a legibilidade dos elementos textuais, contraste de cores nos botões de ação ("Agendar", "Cancelar") e o tempo de entrega das notificações de confirmação de agendamento na tela.
    * **Risco Associado:** Pacientes idosos ou com limitações visuais não conseguirem concluir um agendamento sozinhos, resultando em abandono da plataforma e necessidade de suporte humano.

## 4. Compartibilidade (Dispositivos e Navegadores)

- [x] **Verificação:** Testar a responsividade e o comportamento da interface web da plataforma PACO ao ser acessada nativamente através de dispositivos Desktop, Tablets e Celulares (Android e iOS).
    * **Risco Associado:** Elementos, botões ou calendários ficarem cortados, quebrados ou inacessíveis em telas menores (smartphones), impedindo que a maior fatia do mercado atual (usuários mobile) consiga usar o sistema.
- [x] **Verificação:** Executar os fluxos principais de agendamento nos navegadores mais utilizados do mercado (Google Chrome, Safari, Mozilla Firefox e Microsoft Edge).
    * **Risco Associado:** Incompatibilidade de scripts ou folhas de estilo (CSS) que façam o sistema funcionar perfeitamente no Chrome, mas falhar miseravelmente em iPhones que utilizam o Safari, gerando perda silenciosa de agendamentos.

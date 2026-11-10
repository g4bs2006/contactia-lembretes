# Lembretes de consulta

App que lê as consultas marcadas no prontuário da clínica e agenda, em lote,
lembretes pelo WhatsApp para os pacientes. Ele abre dentro da plataforma de
mensagens que a clínica já usa, e acompanha cada mensagem até ser lida.

> Este repositório é uma vitrine: mostra as telas e o desenho do sistema. O código
> é privado. Todos os dados dos prints são fictícios, de uma clínica de demonstração.

![Consultas para agendar, com a prévia do lembrete](telas/03-lembretes-previa-do-2o-lembrete.png)

## O que ele resolve

A recepção ligava ou mandava mensagem paciente por paciente para confirmar a
consulta do dia seguinte. Com o app, ela vê a agenda do prontuário agrupada por
paciente e dia, marca quem vai receber e agenda tudo de uma vez. Se a consulta é
desmarcada ou remarcada no prontuário, o lembrete é cancelado sozinho.

## Telas

### Lembretes

A agenda do prontuário consultada na hora, com filtros por situação, período,
profissional e busca. Por padrão sai um lembrete por paciente por dia, com o
horário da primeira consulta.

| Para agendar | Selecionados |
|---|---|
| ![Lista para agendar](telas/01-lembretes.png) | ![Consultas selecionadas](telas/02-lembretes-selecionados.png) |

| Todas as situações | Com lembrete |
|---|---|
| ![Todas](telas/04-lembretes-todos.png) | ![Com lembrete](telas/05-lembretes-com-lembrete.png) |

| Remarcadas | Período |
|---|---|
| ![Remarcadas](telas/06-lembretes-remarcadas.png) | ![Seletor de período](telas/08-lembretes-periodo.png) |

| Profissionais | Busca por nome |
|---|---|
| ![Filtro de profissionais](telas/09-lembretes-profissionais.png) | ![Busca](telas/10-lembretes-busca-por-nome.png) |

O lote roda no servidor, em segundo plano. Se a aba fechar, o progresso aparece de
novo quando ela abre.

| Agendando | Resultado do lote |
|---|---|
| ![Lote em andamento](telas/11-lembretes-agendando.png) | ![Resultado do lote](telas/12-lembretes-resultado-do-lote.png) |

### Modelos

Cada modelo de mensagem tem as variáveis ligadas aos dados da consulta e a regra de
quando sai ("1 dia antes, às 18h"). A prévia usa a mesma função do envio, então o
que aparece é o que o paciente recebe.

![Modelos](telas/20-modelos.png)

| Quando sai | Horário do envio |
|---|---|
| ![Antecedência](telas/22-modelos-quando-sai.png) | ![Horário em grade](telas/23-modelos-horario-do-envio.png) |

| Campo da variável | Alteração não salva |
|---|---|
| ![Campo da variável](telas/24-modelos-campo-da-variavel.png) | ![Alteração não salva](telas/25-modelos-alteracao-nao-salva.png) |

| Modelo com vídeo | Modelo de outro número |
|---|---|
| ![Modelo com vídeo](telas/27-modelos-com-video.png) | ![Outro número](telas/28-modelos-de-outros-numeros.png) |

### Histórico

Cada envio, com a situação real vinda da plataforma (enviada, entregue, lida ou
com falha), filtros e cancelamento em lote.

![Histórico](telas/30-historico.png)

| Com falha | Cancelar em lote |
|---|---|
| ![Com falha](telas/33-historico-com-falha.png) | ![Cancelar em lote](telas/31-historico-cancelar-em-lote.png) |

A ficha do envio mostra a linha do tempo, a mensagem como saiu e, quando falha, o
motivo traduzido em "o que fazer".

| Lida | Falhou |
|---|---|
| ![Ficha de envio lido](telas/35-ficha-do-envio-lida.png) | ![Ficha de envio com falha](telas/36-ficha-do-envio-falhou.png) |

| Não saiu no horário | Cancelado |
|---|---|
| ![Não saiu](telas/38-ficha-do-envio-nao-saiu.png) | ![Cancelado](telas/39-ficha-do-envio-cancelado.png) |

### Configurações

Mais de um lembrete por consulta (até três, cada um num dia) e as agendas do
prontuário que ficam de fora, como a do laboratório.

| Configurações | Qual lembrete continua |
|---|---|
| ![Configurações](telas/40-configuracoes.png) | ![Escolha do lembrete](telas/43-configuracoes-qual-lembrete-continua.png) |

### Setup

A área da equipe que implanta o app: cadastro da clínica, credenciais do
prontuário (só de escrita), canal de WhatsApp, etiqueta, teste de conexão, link
assinado do painel e unidades.

| Entrada | Clínicas |
|---|---|
| ![Entrada do setup](telas/50-setup-entrar.png) | ![Lista de clínicas](telas/52-setup-clinicas.png) |

| Clínica | Canal de WhatsApp |
|---|---|
| ![Clínica](telas/53-setup-clinica.png) | ![Escolher canal](telas/55-setup-clinica-canal-de-whatsapp.png) |

| Teste de conexão e link | Unidades |
|---|---|
| ![Teste e link](telas/57-setup-clinica-teste-de-conexao-e-link.png) | ![Unidades](telas/58-setup-clinica-unidades.png) |

## Principais funcionalidades

- Agenda do prontuário lida na hora, agrupada por paciente e dia.
- Lembretes agendados em lote, em segundo plano no servidor.
- Até três lembretes por consulta, cada um num dia. Quando um já passou do prazo,
  agenda só os que cabem.
- Cancelamento automático quando a consulta é desmarcada ou remarcada.
- Situação real de cada mensagem, conferida a cada 15 minutos.
- Modelos com variáveis, mídia e regra de envio, com prévia fiel.
- Conferência de que o modelo sai pelo número certo da unidade.
- Várias unidades por clínica, cada uma com o seu número.
- API versionada com contrato OpenAPI para outros sistemas da equipe.

## Integrações

- **Prontuários:** Clinicorp, e-Clínica e Controle Odonto, atrás de uma porta comum.
- **Plataforma de mensagens:** WhatsApp, etiquetas, canais e equipes.
- **Cadastro comum:** pode ler a clínica do cadastro central dos produtos.

## Como foi construído

- **Fatia vertical:** um diretório por caso de uso, um núcleo puro sem framework
  nem relógio, e portas para o prontuário e para as mensagens.
- **Acesso sem login:** a clínica entra pela aba da plataforma ou por um link
  assinado (HMAC). O setup tem sessão própria.
- **Banco isolado:** esquema próprio no Postgres, com acesso só pelo servidor.
- **Contrato do banco conferido no deploy:** o app confere as colunas antes de subir.
- **Tarefas de horário:** conferência de status a cada 15 minutos e de remarcações
  a cada 30.

| Camada | Tecnologias |
|---|---|
| Telas | Next.js 16, React 19, Tailwind 4, Radix, react-day-picker |
| API | Hono, zod, OpenAPI 3.1 |
| Dados | PostgreSQL (Supabase) |
| Qualidade | TypeScript, Vitest, ESLint, CI no GitHub Actions |
| Entrega | Docker, nginx, VPS |

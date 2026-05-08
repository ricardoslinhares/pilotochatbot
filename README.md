# Chatbot de Atendimento — Prestadores de Serviço
**SENAR Goiás · Departamento de Gestão de Contratos (DAF)**

> Piloto de automação do atendimento inicial na prestação de contas de eventos.  
> 🔗 **[Acessar o documento publicado](https://ricardoslinhares.github.io/pilotochatbot/)**

---

## Sobre o projeto

O SENAR Goiás recebe ~500 eventos simultâneos em aberto, gerando demanda recorrente de atendimento manual. Prestadores de serviço buscam constantemente informações sobre status de eventos, prazos e pagamentos pelos canais WhatsApp, e-mail e Fluig.

Este repositório contém o **documento piloto de proposta de implementação** de um chatbot de atendimento inicial, cobrindo:

- Fluxo completo de decisão com todos os cenários
- 8 scripts de atendimento institucionais
- Cadeia de prazos regulatórios
- Melhorias complementares de notificação
- Template reutilizável para outras áreas da empresa

---

## Estrutura do repositório

```
pilotochatbot/
│
└── index.html        # Documento completo — publicado via GitHub Pages
```

---

## Regras de negócio

### Pré-requisitos para geração da prestação de contas

| # | Condição |
|---|---|
| 1 | Evento com status **Realizado** no Fluig |
| 2 | **F9/Relatório** entregue no Fluig pelo prestador |

### Cadeia de prazos

| Prazo | Regra |
|---|---|
| Prestador → entrega da F9 | 8 dias corridos após o último dia do evento |
| DAF → geração da prestação | 10 dias corridos após a entrega da F9 |
| Após pendência | Prazo de 10 dias **zera e reinicia** na reentrega da F9 |
| Devoluções | Sem limite — e-mail automático a cada devolução |

---

## Fluxo de decisão

```
Prestador informa o número do evento
│
├─► Status REALIZADO?         → Não: orienta atualização
├─► F9 entregue?              → Não: lembra prazo de 8 dias
├─► Dentro do prazo de 10d?   → Não: escala para humano
├─► Em análise → informa data limite
├─► Pendência identificada?   → Sim: notifica devolução, prazo reinicia
├─► Prestação gerada?         → Não + prazo vencido: ANOMALIA → escala
└─► Confirma encerramento
```

---

## Scripts de atendimento

| # | Cenário | Tipo |
|---|---|---|
| 01 | Abertura e Identificação | Automático |
| 02 | Evento não está com status Realizado | Automático |
| 03 | F9 / Relatório ainda não entregue | Automático |
| 04 | Em análise — dentro do prazo | Automático |
| 05 | Pendência identificada — aguardando correção | Automático |
| 06 | Prestação de contas gerada com sucesso | Automático |
| 07 | Anomalia — prazo vencido sem geração | Escala humano |
| 08 | Encerramento do atendimento | Automático |

---

## Melhorias complementares

Independentes do chatbot — podem ser implementadas antes da automação:

- **Confirmação de recebimento da F9** — notificação automática ao prestador (~30% menos consultas)
- **Notificação de encerramento** — disparo automático ao encerrar o evento (elimina a principal queixa)
- **Alerta de prazo próximo** — aviso 2 dias antes do vencimento da F9 (reduz devoluções)

---

## Como atualizar o documento

1. Edite o arquivo `index.html`
2. Faça o commit no branch `main`
3. O GitHub Pages atualiza automaticamente em até 2 minutos

---

## Decisões de arquitetura

### Fluxo linear em vez de Intents

O modelo adota um fluxo linear baseado em número único de evento, em vez de reconhecimento de intenções (intents) como o usado em plataformas como Dialogflow ou IBM Watson. Essa foi uma **decisão consciente**, não uma lacuna:

- O caso de uso é específico — o prestador sempre precisa de um número de evento para obter qualquer resposta útil
- A identificação por protocolo único elimina ambiguidade sem processamento de linguagem natural
- Simplifica a implementação e reduz o tempo de entrada em produção
- É igualmente eficaz para o volume e perfil de atendimento atual

> A evolução para intents pode ser considerada em fases posteriores, se houver necessidade de entender frases livres como *"quero saber do meu evento de março"*.

### Número do evento informado manualmente em vez de Entities

O mercado usa extração automática de entidades, onde o bot identifica o número mesmo em frases não estruturadas. No modelo atual, o número é informado diretamente — adequado para o perfil dos prestadores, que já utilizam o número do evento no Fluig como referência principal.

---

## Próximos passos

- [ ] Validar scripts com a equipe DAF
- [ ] Definir nome oficial do assistente
- [ ] Definir plataforma de chatbot (Zendesk, Intercom, WhatsApp Business API)
- [ ] Mapear integração com Fluig para consulta em tempo real
- [ ] Implementar melhorias complementares no Fluig
- [ ] Expandir template para outras áreas da empresa

---

**Elaborado por:** Ricardo Linhares  
**Departamento:** DAF — Gestão de Contratos  
**Versão:** 1.0 — Piloto 2026

# Guia: WhatsApp Cloud API — credenciais e primeiros passos

Documento local para consulta (extraído do estudo sobre migração para a API oficial da Meta).

**Escopo deste arquivo:** processo até obter **Phone Number ID**, **WhatsApp Business Account ID (WABA)** e **Bearer token permanente**.

---

## O que você vai obter no final

| Nome | O que é | Onde aparece |
|------|---------|--------------|
| **Phone Number ID** | ID do número na API | App → WhatsApp → **API Setup** |
| **WhatsApp Business Account ID (WABA ID)** | ID da conta WhatsApp Business (muitos chamam de “business id”) | Mesmo painel **API Setup** |
| **Bearer token permanente** | Token do **System User** (não expira em poucas horas) | [business.facebook.com/settings](https://business.facebook.com/settings/) → System Users |

> **Meta Business Portfolio ID** é outro identificador (do portfólio empresarial). Para enviar mensagens via API, o essencial é: **Phone Number ID** + **WABA ID** + **token**.

### Checklist final

```
WABA_ID              = ___________________
PHONE_NUMBER_ID      = ___________________
BEARER_TOKEN         = EAAJ...  (guardar em local seguro, ex.: .env)
```

### Teste rápido

```bash
curl "https://graph.facebook.com/v22.0/SEU_PHONE_NUMBER_ID" \
  -H "Authorization: Bearer SEU_BEARER_TOKEN"
```

Se retornar JSON com dados do número, as credenciais estão corretas.

---

## Pré-requisito: Página do Facebook

Criar a Página do Facebook é um passo inicial, mas **não substitui** o app + WABA + número na API.

---

## Fase 1 — Base (Business + desenvolvedor)

### Passo 1 — Meta Business Portfolio + Página do Facebook

1. Abra: [business.facebook.com](https://business.facebook.com/)
2. Crie ou selecione seu **Meta Business Portfolio**
3. Vá em **Configurações**: [business.facebook.com/settings](https://business.facebook.com/settings/)
4. Em **Contas → Páginas**, adicione a Página criada

---

### Passo 2 — Conta de desenvolvedor Meta

1. Registre-se: [developers.facebook.com/async/registration](https://developers.facebook.com/async/registration/)
2. Confirme e-mail e aceite os termos

**Documentação:** [Get Started – Prerequisites](https://developers.facebook.com/documentation/business-messaging/whatsapp/get-started)

---

## Fase 2 — App + WABA + Phone Number ID

### Passo 3 — Criar app com caso de uso WhatsApp

1. Abra: [developers.facebook.com/apps](https://developers.facebook.com/apps)
2. Clique em **Criar app**
3. Informe nome e e-mail
4. Caso de uso: **“Conectar com clientes pelo WhatsApp”**
5. Selecione o **Business Portfolio** do Passo 1
6. Clique em **Criar app**

**Documentação:** [Get Started – Step 1](https://developers.facebook.com/documentation/business-messaging/whatsapp/get-started)

---

### Passo 4 — Conectar WABA e obter os IDs

1. No app criado: menu **WhatsApp** → **API Setup**  
   URL típica: `https://developers.facebook.com/apps/SEU_APP_ID/whatsapp-business/wa-dev-console/`
2. Conecte ou crie uma **WhatsApp Business Account (WABA)**
3. Adicione um número:
   - **Número de teste** da Meta (mais rápido para aprender), ou
   - **Número real** (verificação por SMS/ligação) — [Registrar número](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/registration/)
4. Na tela **API Setup**, anote:

| Campo no painel | Variável sugerida |
|-----------------|-------------------|
| WhatsApp Business Account ID | `WABA_ID` |
| Phone number ID | `PHONE_NUMBER_ID` |
| App ID (opcional) | `APP_ID` (visível na URL ou no painel) |

**Documentação:** [Get Started – Step 2](https://developers.facebook.com/documentation/business-messaging/whatsapp/get-started)

**Visão geral:** [WhatsApp Business Platform Overview](https://developers.facebook.com/documentation/business-messaging/whatsapp/overview)

---

### Passo 5 — (Opcional) Confirmar no Gerenciador do WhatsApp

1. Abra: [Gerenciador do WhatsApp](https://business.facebook.com/latest/whatsapp_manager/)
2. Confira se WABA e número aparecem
3. O **Phone Number ID** usado na API é o do painel **API Setup** do app

**Documentação:** [Business phone numbers](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers)

---

## Fase 3 — Bearer token permanente (System User)

> O token temporário gerado em **API Setup** expira rápido. Para integrações (Chatwoot, n8n, backend), use o token do **System User**.

### Passo 6 — Criar System User

1. Abra: [business.facebook.com/settings](https://business.facebook.com/settings/)
2. Menu **Usuários → Usuários do sistema** (System Users)
3. **Adicionar** → nome (ex.: `whatsapp-api`) → função **Admin** (mais simples para começar)

**Documentação:** [Access Tokens – Generate system user tokens](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens)

---

### Passo 7 — Atribuir ativos ao System User

1. Clique no System User → **Atribuir ativos** (Assign assets)
2. **App** → permissão **Gerenciar app** (Manage app) → controle total
3. **Conta do WhatsApp (WABA)** → **Gerenciar contas do WhatsApp Business** → controle total
4. Confirme e aguarde 1–2 minutos (recarregue a página se não aparecer)

**Documentação:** [Get Started – Step 5](https://developers.facebook.com/documentation/business-messaging/whatsapp/get-started)

**Se usar System User Employee:** conceda acesso à WABA em Business Suite → Configurações → **Contas → Contas do WhatsApp** → aba **Acesso à conta**.  
Ver: [Business asset access](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens)

---

### Passo 8 — Gerar o token permanente

1. No System User → **Gerar token** (Generate token)
2. Selecione o **app** criado no Passo 3
3. Expiração: **Nunca** (Never), se disponível
4. Permissões obrigatórias:
   - `business_management`
   - `whatsapp_business_management`
   - `whatsapp_business_messaging`
5. **Gerar token** → copie e guarde em local seguro

Uso nas requisições:

```http
Authorization: Bearer SEU_TOKEN_AQUI
```

**Documentação:** [Access Tokens Guide](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens)

---

## Links úteis (resumo)

| Etapa | Link |
|-------|------|
| Business Suite | https://business.facebook.com/ |
| Configurações / System Users | https://business.facebook.com/settings/ |
| Registro desenvolvedor | https://developers.facebook.com/async/registration/ |
| Criar app | https://developers.facebook.com/apps |
| Get Started (guia oficial) | https://developers.facebook.com/documentation/business-messaging/whatsapp/get-started |
| Access Tokens | https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens |
| Registrar número | https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/registration/ |
| Gerenciador WhatsApp | https://business.facebook.com/latest/whatsapp_manager/ |
| Cloud API (docs legado) | https://developers.facebook.com/docs/whatsapp/cloud-api/get-started |
| Migrar número existente | https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/migrate-existing-whatsapp-number-to-a-business-account/ |

---

## Referência: hierarquia Meta (visão geral)

```
Página do Facebook
    └── Meta Business Portfolio
            └── App (Meta for Developers, caso WhatsApp)
                    └── WhatsApp Business Account (WABA)
                            └── Número → Phone Number ID
                    └── System User → Bearer token permanente
```

---

## Referência: migração completa (além deste guia)

Este arquivo para em **credenciais**. Depois disso, para produção, ainda entram:

- Webhook HTTPS (receber mensagens)
- Templates aprovados (mensagens fora da janela de 24h)
- App Review + forma de pagamento no Business Portfolio
- Reconfigurar Chatwoot/n8n (canal **WhatsApp Cloud API**, não Evolution)

### Prazo realista (1 semana)

| Meta | Viável em ~7 dias? |
|------|---------------------|
| Teste funcionando (número teste, enviar/receber, token permanente) | Sim, com foco |
| Produção completa (número real migrado, tudo aprovado, sem API não oficial) | Talvez; planejar 2–4 semanas |

### Cenários de número

| Cenário | Observação |
|---------|------------|
| **Número novo / teste** | Mais simples |
| **Número no celular (WhatsApp comum ou Business)** | Precisa migrar; não funciona nos dois ao mesmo tempo |
| **Evolution / Baileys / UZAPI** | Desligar antes de registrar na Meta; depois usar Cloud API no Chatwoot/n8n |

### Erros comuns

1. Confundir **Página do Facebook** com **API configurada**
2. Usar token temporário do API Setup em produção
3. Mesmo número em API não oficial e oficial ao mesmo tempo
4. Enviar mensagem livre fora da janela de 24h sem template aprovado
5. Esquecer webhook (só envia, não recebe de forma confiável)

---

## Perfis Chrome (referência local)

No perfil **Eduardo** (`Default`, eduardolima.contatoprofissional@gmail.com), favoritos relevantes encontrados:

- Pasta **API OFICILA**: [WhatsApp Business Platform – Overview](https://developers.facebook.com/documentation/business-messaging/whatsapp/overview)
- Pasta **Comunidades - n8n, chatwoot, nocode**: [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids)

Histórico (visitado, não favoritado) — docs Cloud API:

- https://developers.facebook.com/docs/whatsapp/cloud-api/get-started
- https://developers.facebook.com/docs/whatsapp/cloud-api/
- https://developers.facebook.com/docs/whatsapp/cloud-api/guides/send-message-templates

---

*Última atualização do documento: maio/2026*

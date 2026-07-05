# GAS — Backend de Analytics (Reconexão Materna)

Backend em Google Apps Script que coleta os eventos do funil (quiz + página de
vendas), recebe os postbacks de venda do checkout, (opcional) envia eventos para
a Meta Conversions API e serve os dados para o `admin.html`.

## Passo a passo para publicar

1. Acesse https://script.google.com → **Novo projeto**.
2. Cole o conteúdo de **`Codigo.gs`** no arquivo `Código.gs` (apague o exemplo).
3. Ative o manifesto: ⚙️ **Configurações do projeto** → marque
   **"Mostrar arquivo de manifesto appsscript.json"**. Abra `appsscript.json` e
   cole o conteúdo de **`appsscript.json`** deste repositório.
4. (Opcional) Se já usa Pixel/CAPI: preencha `PIXEL_ID` e cole o **token da
   Conversions API** em `CAPI_TOKEN` no topo do `Codigo.gs`. Sem isso, os eventos
   são salvos normalmente e só o envio server-side fica desativado.
5. No editor, rode a função **`autorizar()`** uma vez e aceite as permissões.
   Isso cria a planilha *"Reconexão Materna — Analytics DB"* no seu Drive.
6. **Implantar → Nova implantação → Tipo: App da Web**
   - *Executar como:* **Eu**
   - *Quem tem acesso:* **Qualquer pessoa**
   - Copie a **URL do app da Web** (termina em `/exec`).
7. Cole essa URL `/exec` em **dois lugares**:
   - `track.js` → constante `GAS`
   - `admin.html` → constante `GAS`
8. (Opcional) Rode **`testeRapido()`** para gravar 1 evento + 1 venda de teste e
   conferir no `admin.html`. Depois é só usar **Resetar dados** no painel.

## Postback de vendas (checkout → aba `vendas`)

No painel do seu checkout (Hotmart / Kiwify / Cakto / Payt / etc.), em
**Webhook / Postback / Integrações**, aponte para:

```
SUA_URL_DO_GAS/exec?src=venda
```

O GAS grava na aba `vendas`. Status considerado "aprovado":
`finaliz | aprovad | paid | pago | approved | confirmed`.

## Abas da planilha (criadas sozinhas)

| Aba | Colunas |
|-----|---------|
| `eventos`  | data, evento, session, step, nome, resposta, ms, referrer, ua, event_id, fbp, fbc, url |
| `vendas`   | data, status, metodo, valor, nome, email, telefone, order_id, raw, produto |
| `capi_log` | data, evento, event_id, s, status_code, response |

## Mapa de steps do funil

| step | significado |
|------|-------------|
| `view 0`            | abriu o quiz (visitante) |
| `answer 1`          | respondeu a 1ª pergunta (Lead) |
| `answer 2..16`      | demais perguntas do quiz |
| `view diagnostico`  | viu a tela de diagnóstico |
| `click cta_pv`      | clicou em "ver minha solução" (fim do quiz) |
| `view pv`           | abriu a página de vendas |
| `checkout_click`    | clicou em comprar |

## Observações

- O projeto é **standalone**: cria/abre a própria planilha via
  `PropertiesService` (guarda o `SHEET_ID`). Não precisa vincular a nenhuma planilha.
- Sempre que mudar os escopos do manifesto, rode `autorizar()` de novo e
  publique uma **Nova versão** da implantação.

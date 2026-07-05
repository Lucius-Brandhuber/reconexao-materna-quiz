# Reconexão Materna — Quiz / Autoteste

Quiz funnel (autoteste) que antecede a página de vendas do **Método Reconexão Materna — As 3 Camadas**.

15 perguntas divididas nas 4 etapas de resposta direta (Engajamento → Problema → Desejo → Solução), com diagnóstico personalizado por camada (respiração / core profundo / postura) e handoff para a página de vendas.

## Configuração
- **URL da Página de Vendas:** edite a constante `PV_URL` no topo do `<script>` em `index.html`.
- **Imagens da autora:** `assets/autora-capa.png` (capa) e `assets/autora-guia.png` (transição).
- **Imagens de referência** (tipos de barriga, antes/depois): hoje são placeholders on-brand. Substitua trocando os blocos `.ph` / `.carousel` por `<img>` reais.

## Rodar localmente
```
python3 -m http.server 4599
```
Acesse http://localhost:4599

Site estático — hospedado no GitHub Pages.

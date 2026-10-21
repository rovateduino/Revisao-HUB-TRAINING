# REVISORIA — pasta para publicar no GitHub

Conteúdo gerado por `node scripts/nr/pagina_revisor.cjs` a partir de `PACOTE_REVISAO/`.
Não edite estes arquivos à mão: a fonte é `content/nrs/nr-XX.json`.

**Gerada em 20/10/2026, 20:23:13 — 8 cursos, 581 linhas de matriz, 25 perguntas.**

## O que há nesta pasta

| Arquivo | Para quê | Vai para o link público? |
| --- | --- | --- |
| `index.html` | **É a página do técnico.** Só três abas: Matriz, Formulário e Sua identificação, mais o botão de exportar a devolução. | **Sim** — é este o link a enviar. |
| `dados/MATRIZ_AUDITORIA.csv` | Mesma matriz em planilha (UTF-8 com BOM, separador `;`) para quem preferir Excel. | Opcional. |
| `README.md` | Esta explicação. | Sim. |

A página com o texto integral das 8 apostilas (com gabarito), a carta, as referências e a pré-auditoria
fica em `PACOTE_REVISAO/PAGINA_COMPLETA.html` e **não** é copiada para cá de propósito: se esta pasta
virar um repositório público, nada do conteúdo pago aparece no link. O material que o revisor precisa
para conferir item por item vai com o pacote (`PACOTE_REVISAO/`, enviado por e-mail ou Drive).

## Como publicar

1. Repositório novo (pode ser público) e envie **somente o conteúdo desta pasta** para a raiz.
2. Settings → Pages → deploy from branch (main, raiz). O link fica
   `https://<usuario>.github.io/<repositorio>/`, que já abre a página do técnico.
3. Se preferir sem Pages, mande o link do arquivo: `.../REVISORIA/index.html` (GitHub Pages não
   renderiza HTML de dentro da timeline do repositório — só pela URL do Pages ou em "Open with Raw").

## O que o técnico faz

1. Abre a página, filtra por curso e preenche na matriz: **Evidência encontrada**,
   **Correção necessária**, **Status** (Aprovado / Reprovado / Corrigir), **Revisor** e **Data**.
2. Responde ao formulário P1–P{PERGUNTAS} (Sim / Não / Corrigir para) com a fundamentação.
3. Preenche **Sua identificação**: nome, registro (CREA/CRT), NRs revisadas, data, evidência da
   habilitação e se autoriza a publicação (total ou parcial, com alcance).
4. O botão **Exportar devolução final (JSON)** destrava quando os três passos acima estiverem
   completos. Os botões de CSV, `.md` e backup funcionam a qualquer momento, para trabalhar em partes.

Nada é enviado por rede: a página não tem `fetch`, `XMLHttpRequest` nem recurso externo — o
preenchimento fica no `localStorage` do navegador dele e sai pelos botões de download.

## O que eu (operação) faço com o arquivo devolvido

O arquivo baixado se chama `HUB_REVISOR_<nome>_DEVOLUCAO_FINAL.json`. Ele traz, por linha da matriz:
`curso`, `item`, `onde`, `afirmacao`, `evidencia`, `correcao`, `status`, `revisor`, `data`;
por pergunta: `numero`, `resposta`, `observacao`; e o bloco `revisor` com nome, registro, data e
autorização.

Regra de aplicação, sem exceção:

- Um curso só sai de `status_revisao: "pendente"` quando **todas** as linhas dele estiverem
  `Aprovado` **e** o bloco `revisor` tiver nome, registro e autorização `Sim` (ou `Parcial` citando
  aquele curso).
- Linhas `Corrigir`/`Reprovado` viram alteração em `content/nrs/nr-XX.json` — nunca edição direta
  no banco ou no pacote.
- `revisor` e `data_revisao` recebem exatamente o texto assinado por ele. O agente não aprova
  conteúdo por conta própria.

## Regenerar

```
node scripts/nr/pacote_revisao.cjs
node scripts/nr/pagina_revisor.cjs
```

Guarda automática: `node tests/unit/test_revisao_registro.cjs` falha se as páginas ficarem
dessincronizadas do CSV, se passarem a carregar algo da rede, ou se reaparecer uma expressão já
retirada do material.

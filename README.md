# data-published

Repositorio publico, so de dados. Aqui ficam snapshots de dados gerados por
projectos meus (privados) que corram automaticamente - sem expor o codigo
fonte desses projectos.

## Estrutura

Uma pasta por projecto de origem, com os ficheiros de dados desse projecto
tal como ele os produz - normalmente em dois formatos, lado a lado:

- JSON (`latest.json` snapshot, `history.jsonl` historico com uma linha
  por dia) - para consumo programatico.
- CSV (`latest.csv` snapshot, `history.csv` historico em formato longo) -
  para ligar directamente a graficos no Excel ou Google Sheets.

```
<nome-do-projecto>/
  latest.json
  history.jsonl
  latest.csv
  history.csv
```

## Projectos publicados aqui

### routine-data-etf

Dados de mercado (preco, retornos, volatilidade, Sharpe ratio, drawdown,
distancia ao ATH) para uma carteira de 11 ETFs/ETPs UCITS europeus,
actualizados nos dias uteis. Codigo fonte num repositorio privado separado.

- `routine-data-etf/latest.json` - snapshot mais recente
- `routine-data-etf/history.jsonl` - uma linha JSON por dia (a execucao
  mais recente desse dia, se tiver corrido mais do que uma vez)
- `routine-data-etf/latest.csv` - o mesmo snapshot em CSV, uma linha por activo
- `routine-data-etf/history.csv` - historico em CSV, formato longo (uma
  linha por activo por dia) - o mais indicado para graficos

Acesso directo (sem autenticacao, repositorio publico):

```
https://raw.githubusercontent.com/dblxpt/data-published/main/routine-data-etf/latest.json
https://raw.githubusercontent.com/dblxpt/data-published/main/routine-data-etf/history.jsonl
https://raw.githubusercontent.com/dblxpt/data-published/main/routine-data-etf/latest.csv
https://raw.githubusercontent.com/dblxpt/data-published/main/routine-data-etf/history.csv
```

Para Google Sheets, `=IMPORTDATA(url)` com um dos URLs `.csv` acima
carrega a tabela directamente numa folha, pronta para graficos.

### crawler-cred

Preçário (TAN/TAEG/MTIC/prestação) de um simulador de crédito pessoal
português, para várias combinações de montante, prazo e finalidade.
Atualizado nos dias úteis. Codigo fonte num repositorio privado
separado.

- `crawler-cred/cof.csv` - snapshot mais recente
- `crawler-cred/cof.json` - a mesma recolha em bruto (JSON completo)
- `crawler-cred/cof_historico.csv` - histórico cumulativo (uma linha por
  combinação por dia de recolha)
- `crawler-cred/cof-dash.html` - painel visual (tabelas de TAN e
  prestação mensal por montante/prazo). Uma finalidade de cada vez,
  escolhida por botão: preçário atual, alterações face à última recolha
  em que o preçário foi diferente (pode ter sido há dias ou meses) e
  esse preçário anterior. Lê `cof_historico.csv` diretamente ao carregar
  a página (não tem dados embutidos - fica sempre atualizado sozinho).
  Só funciona servido por http(s) (ex.: GitHub Pages); aberto como ficheiro
  local (`file://`) o browser bloqueia esse pedido e a página mostra um
  aviso a explicar.

Acesso directo (sem autenticacao, repositorio publico):

```
https://raw.githubusercontent.com/dblxpt/data-published/main/crawler-cred/cof.csv
https://raw.githubusercontent.com/dblxpt/data-published/main/crawler-cred/cof.json
https://raw.githubusercontent.com/dblxpt/data-published/main/crawler-cred/cof_historico.csv
```

Para ver `cof-dash.html` como página (não como texto em bruto), ativar o
GitHub Pages neste repositório (Settings → Pages → Deploy from branch →
`main` → `/ (root)`) e abrir:

```
https://dblxpt.github.io/data-published/crawler-cred/cof-dash.html
```

## Painel partilhado (`_shared/bank-dash.html`)

O painel visual (tabelas TAN/prestação por finalidade) não vive duplicado
em `crawler-cred/` — o código real está todo em **`_shared/bank-dash.html`**,
uma página genérica que não sabe nada sobre "COF" ou Cofidis. Ela lê dois
parâmetros da URL:

- `src` — caminho (relativo a `_shared/`) para o `..._historico.csv` do
  projeto a mostrar
- `nome` — título a mostrar na página

`crawler-cred/cof-dash.html` é só um redirecionamento de 3 linhas para
`_shared/bank-dash.html?src=../crawler-cred/cof_historico.csv&nome=COF` —
mantém o link antigo a funcionar sem duplicar código.

**Para dar o mesmo visual a um novo crawler** (outro banco), o histórico
desse crawler tem de sair com as mesmas colunas de `cof_historico.csv`
(`data_recolha, finalidade, montante_eur, prazo_meses,
prestacao_mensal_eur, taeg_pct, tan_pct, mtic_eur`). Feito isso, não é
preciso copiar nem editar `_shared/bank-dash.html` — basta:

1. Publicar o `..._historico.csv` desse banco numa pasta nova aqui (ex.
   `outro-banco/historico.csv`), tal como o `crawler-cred` já faz.
2. Ligar para `_shared/bank-dash.html?src=../outro-banco/historico.csv&nome=XYZ`
   (ou criar um redirecionamento de 3 linhas como o de `cof-dash.html`,
   se quiseres manter um URL fixo por banco).

Qualquer alteração de aspeto visual (cores, fontes, layout) faz-se **uma
única vez** em `_shared/bank-dash.html` e aplica-se a todos os bancos
instantaneamente, sem tocar nas pastas de cada um.

## Nota

Estes dados sao publicados "as-is", tal como calculados pelos scripts de
origem. Ver o `source_notes` e o campo `notes` de cada activo em
`latest.json`/`latest.csv` para limitacoes, fallbacks e proxies aplicados.

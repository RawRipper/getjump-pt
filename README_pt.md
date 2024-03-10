# getjump

[![Versão PyPI](
  <https://badge.fury.io/py/getjump.svg>
  )](
  <https://badge.fury.io/py/getjump>
) [![Manutenibilidade](
  <https://api.codeclimate.com/v1/badges/8d8c16d52b49885dad8c/maintainability>
  )](
  <https://codeclimate.com/github/eggplants/getjump/maintainability>
) [![status pre-commit.ci](
  <https://results.pre-commit.ci/badge/github/eggplants/getjump/master.svg>
  )](
  <https://results.pre-commit.ci/latest/github/eggplants/getjump/master>
) [![Cobertura de Teste](
  <https://api.codeclimate.com/v1/badges/8d8c16d52b49885dad8c/test_coverage>
  )](
  <https://codeclimate.com/github/eggplants/getjump/test_coverage>
) [![Teste](
  <https://github.com/eggplants/getjump/actions/workflows/test.yml/badge.svg>
  )](
  <https://github.com/eggplants/getjump/actions/workflows/test.yml>
)

[![ghcr última](
  <https://ghcr-badge.deta.dev/eggplants/getjump/latest_tag?trim=major&label=latest>
 ) ![tamanho ghcr](
  <https://ghcr-badge.deta.dev/eggplants/getjump/size>
)](
  <https://github.com/eggplants/getjump/pkgs/container/getjump>
)

- Recuperar e salvar imagens de sites de distribuição de mangás usando [GigaViewer](https://hatena.co.jp/solutions/gigaviewer)
  - Se você ler quadrinhos recuperados como PDF combinado, use: [eggplants/mkbook](https://github.com/eggplants/mkbook)

_Nota: A redistribuição de dados de imagem baixados é proibida. Por favor, mantenha-o para uso privado._

## Captura de tela

![imagem](https://user-images.githubusercontent.com/42153744/175097993-c6a2e162-50ea-41d4-9590-19a09a61e053.png)

## Formatos de URL Válidos

- `<host>/(episode|magazine|volume)/<número>`
  - ex. <https://shonenjumpplus.com/episode/13932016480028799982>
- `<host>/(episode|magazine|volume)/<número>.json`
  - ex. <https://shonenjumpplus.com/episode/13932016480028799982.json>

## Hosts Disponíveis

- `https://www.corocoro.jp`
- `https://comic-action.com`
- `https://comic-days.com`
- `https://comic-earthstar.com`
- `https://comic-gardo.com`
- `https://comic-ogyaaa.com`
- `https://comic-trail.com`
- `https://comic-zenon.com`
- `https://comicborder.com`
- `https://comic-growl.com`
- `https://feelweb.jp`
- `https://kuragebunch.com`
- `https://magcomi.com`
- `https://pocket.shonenmagazine.com`
- `https://shonenjumpplus.com`
- `https://tonarinoyj.jp`
- `https://viewer.heros-web.com`

## Instalar

```bash
pip install getjump
```

## CLI

### Uso

```shellsession
$ jget https://shonenjumpplus.com/episode/13932016480028799982.json
get: https://shonenjumpplus.com/episode/13932016480028799982.json
...
salvo: ./阿波連さんははかれない/[1話]阿波連さんははかれない
feito.

$ jget -b https://shonenjumpplus.com/episode/10833519556325021912.json
get: https://shonenjumpplus.com/episode/10833519556325021912.json
...
salvo: ./こちら葛飾区亀有公園前派出所/[第1話]こちら葛飾区亀有公園前派出所
próximo: https://shonenjumpplus.com/episode/10833519556325022016.json
...
...
...
salvo: ./こちら葛飾区亀有公園前派出所/[第1953話]こちら葛飾区亀有公園前派出所
feito.
```

### Ajuda

```shellsession
$ jget -h
uso: jget [-h] [-b] [-d DIR] [-f] [-o] [-m] [-u ID] [-p PW] [-q] [-V] url

Obtenha imagens do visualizador web jump

argumentos posicionais:
  url                    url de destino

opções:
  -h, --help             mostrar esta mensagem de ajuda e sair
  -b, --bulk             baixar série em massa (padrão: False)
  -d DIR, --savedir DIR  diretório para salvar imagens baixadas (padrão: .)
  -f, --first            baixar apenas a primeira página (padrão: False)
  -o, --overwrite        sobrescrever (padrão: False)
  -m, --metadata         salvar metadados como json (padrão: False)
  -u ID, --username ID   nome de usuário se você deseja fazer login (padrão: None)
  -p PW, --password PW   senha se você deseja fazer login (padrão: None)
  -q, --quiet            desativar impressão no console (padrão: False)
  -V, --version          mostrar número da versão do programa e sair

urls disponíveis:
  - https://www.corocoro.jp
  - https://comic-action.com
  - https://comic-days.com
  - https://comic-gardo.com
  - https://comic-ogyaaa.com
  - https://comic-trail.com
  - https://comic-zenon.com
  - https://comicborder.com
  - https://comic-growl.com
  - https://feelweb.jp
  - https://kuragebunch.com
  - https://magcomi.com
  - https://pocket.shonenmagazine.com
  - https://shonenjumpplus.com
  - https://www.sunday-webry.com
  - https://tonarinoyj.jp
  - https://viewer.heros-web.com
```

## Biblioteca

### Visão Geral

```python
from getjump import GetJump
g = GetJump()  # criar sessão

g.get(
    url: str,
    save_path: str = ".",
    overwrite: bool = True,
    only_first: bool = False,
    username

: str | None = None,
    password: str | None = None,
)
# >>> (next_uri: str | None, prev_title: str, saved: bool)

g.login(
    url: str,
    username: str | None = None,
    password: str | None = None,
    overwrite: bool = False,
)
# >>> resposta logada: requests.Response | None

g.is_valid_uri(url: str)
# >>> é_uri_válida: bool
```

### Baixar todas as séries

Para baixar todas as séries de uma vez:

```python
from getjump import GetJump as g

G = g()
next_uri = "https://shonenjumpplus.com/episode/13932016480028799982.json"
while next_uri:
    next_uri, prev_title, saved = G.get(next_uri, overwrite=False)
    if saved:
        print("salvo:", prev_title)
    print("próximo:", next_uri)
```

### Login

Para obter obras compradas ou que requerem login:

```python
from getjump import GetJump as g

G = g()
G.login("https://shonenjumpplus.com", username="***", password="***")
G.login("https://comic-days.com", username="***", password="***")
...
G.get(...)
```

## Licença

MIT

---

## Referência

- [fa0311/jump-downloader](https://github.com/fa0311/jump-downloader)
- [少年ジャンププラスの漫画をダウンロードするライブラリ - yuki0311.com](https://blog.yuki0311.com/jumppuls_downloader/)
- [はてな開発の新マンガビューワを「少年ジャンプ＋」が採用。集英社と共同でサイト成長、マネタイズの両面を加速 - プレスリリース - 株式会社はてな](https://hatenacorp.jp/press/release/entry/2017/01/18/113000)
- [はてな、集英社「少年ジャンプ＋」ブラウザ版への機能提供を拡張。ブラウザ版への電子版「週刊少年ジャンプ」定期購読が可能に｜株式会社はてなのプレスリリース](https://prtimes.jp/main/html/rd/p/000000078.000006510.html)
- [GigaViewer の検索結果 - プレスリリース - 株式会社はてな](https://hatenacorp.jp/press/release/search?q=GigaViewer)
- [GigaViewer（ギガビューワー）を作るにあたって - daily thinking running](https://jusei.hatenablog.com/entry/2018/01/09/172026)

# Pelican Client — Updates

Repositório **somente de distribuição** (sem código-fonte). O launcher instalado (1.0.1+) lê daqui.

```
client/PelicanClient-1.8.9.jar              mod (baixado para <gamePath>/mods/PelicanClient 1.8.9.jar)
version/version.md                          versão do mod (formato: version-1.0)
changelog/changelog.json                    changelog exibido no launcher (sem rebuild)
launcher/latest.yml                         manifesto do auto-update do launcher
launcher/Pelican-Client-Setup-x.y.z.exe     instalador do update
launcher/Pelican-Client-Setup-x.y.z.exe.blockmap
```

## Atualizar o changelog (sem reinstalar)

Edite `changelog/changelog.json` direto no GitHub (ou localmente) e dê commit em `main`.
Não precisa rebuildar nem reinstalar nada: o launcher baixa esse arquivo ao abrir.

Formato: array de objetos, **mais novo primeiro** (o app também ordena por data):

```json
[
  {
    "date": "2026-10-22T00:00:00.000Z",
    "content": "**Pelican Client 1.0.3**\n\n- Novidade 1\n- Novidade 2"
  }
]
```

Regras:
- `date` em ISO 8601; `content` em Markdown (negrito, listas).
- Mantenha o histórico (não apague as entradas antigas).
- Se o arquivo ficar inválido ou a pessoa estiver offline, o launcher usa o último cache local e,
  na primeira execução, o fallback que vem dentro do app.
- O raw do GitHub tem cache de ~5 min; o launcher usa `?t=timestamp`, então normalmente pega na hora.

## Atualizar o mod

1. Substitua `client/PelicanClient-1.8.9.jar` (mantenha o mesmo nome).
2. Atualize `version/version.md` (ex.: `version-1.1`).
3. Commit + push em `main`.

Não precisa rebuildar o launcher: ele checa a versão ao abrir e antes de cada launch.

## Atualizar o launcher

No projeto (fonte, fora deste repo):

1. Suba a `version` do `package.json` (sempre maior que a publicada).
2. Rode `npm run dist`.
3. Copie para `launcher/`:
   - `Pelican-Client-Setup-<versão>.exe`
   - `Pelican-Client-Setup-<versão>.exe.blockmap`
   - `latest.yml`
4. Remova os instaladores antigos de `launcher/` (o histórico do git guarda).
5. Commit + push em `main`.

Conferir depois do push (raw tem cache de ~5 min; use `?x=timestamp`):

```powershell
curl.exe -s "https://raw.githubusercontent.com/Ryanabcraft/PelicanClient-Updates/main/launcher/latest.yml?x=1"
```

O `sha512`/`size` do `latest.yml` têm que bater com o `.exe` publicado.

## Regras

- **Nunca** commitar source aqui — só binário.
- Não renomear `client/PelicanClient-1.8.9.jar` nem `version/version.md` (o launcher procura esses caminhos).
- Não mudar `appId`, `productName` nem o padrão `artifactName` (quebra update/atalhos/userData).
- Nunca reutilizar um número de versão já publicado (`electron-updater` não faz downgrade).
- Instalador tem ~80 MB: GitHub avisa acima de 50 MB e bloqueia acima de 100 MB. Mantenha um só.
- O update roda silencioso, mas pede UAC (instalação per-machine em Program Files).

## Como o update funciona

- `electron-updater` com provider `generic`;
- feed: `https://raw.githubusercontent.com/Ryanabcraft/PelicanClient-Updates/main/launcher/`;
- o app checa ao abrir, mostra "Atualizar agora / Mais tarde", baixa validando o sha512, fecha e
  reinstala.

Detalhes de implementação e runbook completo ficam no README do projeto de origem
(`Pelican-Client/README.md`).

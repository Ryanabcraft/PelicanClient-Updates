# Pelican Client — Updates

Repositório **somente de distribuição** (sem código-fonte).

```
client/PelicanClient-1.8.9.jar         mod do cliente (baixado pro .pelicanclient/mods)
version/version.md                     versão do mod (formato: version-1.0)
launcher/latest.yml                    manifesto de update do launcher (gerado pelo electron-builder)
launcher/Pelican-Client-Setup-x.y.z.exe  instalador usado pelo auto-update
```

## Como soltar uma versão nova

### Mod
1. Substitua `client/PelicanClient-1.8.9.jar`.
2. Atualize `version/version.md` (ex.: `version-1.1`).
3. Commit + push em `main`.

### Launcher
1. No projeto, mude a `version` do `package.json` e rode `npm run dist`.
2. Copie para `launcher/`:
   - `Pelican-Client-Setup-<versão>.exe`
   - `Pelican-Client-Setup-<versão>.exe.blockmap`
   - `latest.yml`
3. Commit + push em `main`.

O launcher instalado checa `launcher/latest.yml` a cada abertura e pergunta se quer atualizar.
O download é validado pelo hash (sha512) que está no `latest.yml`.

# Como esconder o link do site no app gerado pelo PWA Builder

Quando usas o PWA Builder (pwabuilder.com) com o link do teu site para gerar
o `.apk`/`.aab` Android, ele cria uma "Trusted Web Activity" (TWA) — uma app
que abre o teu site em ecrã inteiro, sem barra de navegador. Mas o Android só
esconde essa barra (e o link) se conseguir **verificar que o site e a app
pertencem ao mesmo dono**. Sem essa verificação, aparece sempre uma barra
com o URL do site.

Segue estes passos:

## 1. Gera o app no PWA Builder primeiro
1. Vai a https://www.pwabuilder.com
2. Introduz o link do teu site (onde o `index.html` e o `manifest.json` desta
   app estão hospedados)
3. Escolhe "Android" e gera o pacote
4. O PWA Builder vai mostrar-te (ou incluir num ficheiro `assetlinks.json`
   pronto) dois valores que precisas:
   - **Package name** (ex: `com.exemplo.qrgenerator`)
   - **SHA256 fingerprint** do certificado usado para assinar a app

## 2. Preenche o ficheiro assetlinks.json
Abre o ficheiro `well-known/assetlinks.json` incluído neste pacote e
substitui:
- `COLOCA_AQUI_O_PACKAGE_NAME` → o package name do passo anterior
- `COLOCA_AQUI_A_SHA256_FINGERPRINT` → a fingerprint SHA256 do passo anterior

(O PWA Builder normalmente já te dá este ficheiro pronto para descarregar —
podes usar esse diretamente em vez de preencher o template à mão.)

## 3. Hospeda o ficheiro no local exato
O Android vai à procura deste ficheiro exatamente em:

```
https://o-teu-dominio.com/.well-known/assetlinks.json
```

Ou seja: renomeia a pasta `well-known` para `.well-known` (com o ponto no
início — não incluí o ponto no ZIP porque alguns sistemas escondem pastas
que começam por ponto) e coloca-a na **raiz do domínio**, não dentro da
pasta da app.

## 4. Confirma que está acessível
Abre `https://o-teu-dominio.com/.well-known/assetlinks.json` no browser —
deves ver o conteúdo JSON diretamente, sem erros 404.

## 5. Reinstala a app
Depois do ficheiro estar no ar, desinstala e volta a instalar a app gerada
pelo PWA Builder (ou espera até 24h — o Android por vezes cacheia a
verificação). O link do site deixa de aparecer.

---

**Nota:** este passo é feito no teu servidor/hosting, fora desta pasta da
app — o ficheiro `assetlinks.json` tem de estar acessível publicamente no
domínio onde a PWA está hospedada.

---
title: Utvickling på Posten PC
date: 2025-10-20 16:29:00
categories: [or_team, ny_ansatt, ny_pc]
tags: [or_team, ny_ansatt, ny_pc]
---

# Utvikling på Posten PC (Klientmaskin)

Denne veiledningen beskriver hvordan du setter opp en utviklings‑arbeidsflate på en Posten PC med begrensede rettigheter (uten lokal administrator). Den kombinerer innholdet fra den opprinnelige PDFen og TXT‑uttrekket, samt klikkbare lenker til verktøyene.

## Innholdsfortegnelse

1. [Oversikt](#oversikt)  
2. [Begrensninger](#begrensninger)  
3. [Verktøy](#verktøy)  
   1. [Tilgjengelig i Software Center](#tilgjengelig-i-software-center)  
   2. [Portabel / manuell nedlasting](#portabel--manuell-nedlasting)  
   3. [Installerbare uten admin](#installerbare-uten-admin)  
4. [Anbefalt katalogstruktur](#anbefalt-katalogstruktur)  
5. [Oppsett av miljøvariabler](#oppsett-av-miljøvariabler)  
6. [Proxy‑oppsett](#proxy-oppsett)  
   1. [Git](#git-proxy)  
   2. [npm (Node.js)](#npm-proxy)  
   3. [Maven](#maven-proxy)  
   4. [Andre verktøy / IDEer](#andre-verktøy--ideer)  
7. [Verifisering](#verifisering)  
8. [Notater / Merknader](#notater--merknader)  
9. [Lenkeregister](#lenkeregister)

---

## Oversikt

En Posten PC har begrensninger som gjør at det kreves ekstra oppsett og alternative løsninger for å kunne jobbe som utvikler.

## Begrensninger

| Begrensning | Konsekvens / Tiltak |
|-------------|--------------------|
| Ingen lokale administrator‑rettigheter | Bruk portable versjoner / ZIP‑pakker eller installerere som ikke krever admin. |
| HTTP proxy (`webguard.posten.no:8080`) | Må konfigureres i Git, npm, Maven og enkelte IDEer. |
| Mulige flere interne nettverksrestriksjoner | Verktøy må kunne fungere bak proxy, bruk HTTP i første omgang der det er mulig. |
| (Plassholder) "mere?" i originaltekst | Undersøk om sertifikater / lokal brannmur krever særskilt konfig. |

## Verktøy

### Tilgjengelig i Software Center

Disse kan installeres direkte (ingen manuell zip nødvendig):

- Office
- Acrobat Reader
- Firefox
- Chrome
- Notepad++
- PuTTY
- FileZilla
- 7-Zip

### Portabel / manuell nedlasting

Last ned ZIP / “portable” varianter og pakk ut til f.eks. `C:\devtools` (eksempelsti brukt videre):

| Kategori | Verktøy | Kommentar / Bruk | Lenke |
|----------|--------|------------------|-------|
| Kildekodekontroll | Git | Bruk stand‑alone / portable om tilgjengelig | [Git for Windows](https://git-scm.com/download/win) |
| Java | Java JDK 11 | Trengs for bygg / Maven / Ant | [OpenJDK / Oracle JDK 11](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html) |
| Java bygg | Apache Ant | Eldre byggeskript | [Ant](https://ant.apache.org/bindownload.cgi) |
| Java bygg | Apache Maven | Standard bygg/verktøy | [Maven](https://maven.apache.org/download.cgi) |
| Java IDE | NetBeans | IDE (kan ligge portable) | [NetBeans](http://netbeans.apache.org/download/index.html) |
| Java IDE | IntelliJ IDEA | Community / Ultimate | [IntelliJ IDEA](https://www.jetbrains.com/idea/download/#section=windows) |
| Python IDE | PyCharm | Community / Professional | [PyCharm](https://www.jetbrains.com/pycharm/download/#section=windows) |
| Database | DBeaver | Universell DB‑klient | [DBeaver](https://dbeaver.io/download/) |
| Editor / Web | Visual Studio Code | God allround editor | [VS Code](https://code.visualstudio.com/download) |
| Runtime | Node.js + npm | JS/TS verktøykjede | [Node.js](https://nodejs.org/en/download/) |
| Runtime | Python | Scripting / verktøy | [Python Windows](https://www.python.org/downloads/windows/) |
| Notater | Notable | Markdown / notater | [Notable](https://notable.md/) |
| Referanse | Markdown syntaksguide | Dokumentasjon | [Markdown Guide](https://www.markdownguide.org/basic-syntax/) |

### Installerbare uten admin

Noen installasjonsprogrammer (VS Code, Python) tilbyr brukermodus (“Install for current user”) som ikke krever admin. Velg det hvis ZIP ikke er nødvendig.

## Anbefalt katalogstruktur

```text
C:\
  devtools\
    git\
    jdk-11.0.x\
    node\
    python38\
    apache-maven-3.6.3\
    apache-ant-1.10.x\
    vscode\
    dbeaver\
    notable\
    netbeans\
    idea\
    pycharm\
```

Hold versjonskatalogene intakte (unngå mellomrom om mulig).

## Oppsett av miljøvariabler

Åpne “Edit environment variables for your account” (Windows søk). Legg inn (User variables):

| Variabel | Verdi (eksempel) |
|----------|------------------|
| `JAVA_HOME` | `C:\devtools\jdk-11.0.6` |
| (legg til i `PATH`) | `C:\devtools\jdk-11.0.6\bin` |
| (legg til i `PATH`) | `C:\devtools\apache-maven-3.6.3\bin` |
| (legg til i `PATH`) | `C:\devtools\apache-ant-1.10.x\bin` (hvis brukt) |
| (legg til i `PATH`) | `C:\devtools\node` |
| (legg til i `PATH`) | `C:\devtools\python38` |
| (legg til i `PATH`) | `C:\devtools\python38\Scripts` |
| (valgfritt) | `C:\devtools\git\cmd` (for `git.exe` om ikke allerede i PATH) |

Eksempel PowerShell engangskjøring (justér versjoner):

```powershell
[System.Environment]::SetEnvironmentVariable('JAVA_HOME','C:\devtools\jdk-11.0.6','User')
$add = @(
  'C:\devtools\jdk-11.0.6\bin',
  'C:\devtools\apache-maven-3.6.3\bin',
  'C:\devtools\node',
  'C:\devtools\python38',
  'C:\devtools\python38\Scripts'
)
$current = [System.Environment]::GetEnvironmentVariable('Path','User')
$new = ($current.Split(';') + $add) | Where-Object { $_ -and (Test-Path $_) } | Select-Object -Unique
[System.Environment]::SetEnvironmentVariable('Path', ($new -join ';'), 'User')
```

> NB: Versjonsnumre/stier kan avvike. Kontroller faktiske katalognavn.

## Proxy‑oppsett

**Proxy host:** `webguard.posten.no`  
**Port:** `8080`  
**Autentisering:** Ikke nødvendig (ingen brukernavn/passord)  
**HTTPS separat:** Ikke påkrevd; samme host/port brukes.

### Git (proxy)

```bash
git config --global http.proxy http://webguard.posten.no:8080
# Valgfritt å sette også for https (ofte ikke nødvendig):
git config --global https.proxy http://webguard.posten.no:8080
```

For å sjekke:

```bash
git config --global --get http.proxy
```

### npm (proxy)

Originalteksten du ga gjentar Git‑kommando; riktig for npm er:

```bash
npm config set proxy http://webguard.posten.no:8080
npm config set https-proxy http://webguard.posten.no:8080
```

Verifiser:

```bash
npm config get proxy
npm config get https-proxy
```

(Disse lagres normalt i `%APPDATA%\npm\etc\npmrc` eller en brukerspesifikk `.npmrc`.)

### Maven (proxy)

Rediger (eller opprett) `C:\Users\<brukernavn>\.m2\settings.xml`:

```xml
<settings>
  <proxies>
    <proxy>
      <id>posten-proxy</id>
      <active>true</active>
      <protocol>http</protocol>
      <host>webguard.posten.no</host>
      <port>8080</port>
    </proxy>
    <proxy>
      <id>posten-proxy-https</id>
      <active>true</active>
      <protocol>https</protocol>
      <host>webguard.posten.no</host>
      <port>8080</port>
    </proxy>
  </proxies>
</settings>
```

Test:

```bash
mvn -version
mvn help:effective-settings
```

### Andre verktøy / IDEer

I JOSM, IntelliJ IDEA, PyCharm, NetBeans, Visual Studio Code og DBeaver finnes proxy‑innstillinger:

- IntelliJ / PyCharm / IDEer: Settings → Appearance & Behavior → System Settings → HTTP Proxy.
- VS Code: `settings.json` kan bruke:
  ```json
  {
    "http.proxy": "http://webguard.posten.no:8080",
    "http.proxyStrictSSL": false
  }
  ```
- DBeaver: Preferences → General → Network Connections → Active Provider: Manual.

> NB: Ikke sett brukernavn/passord, og port er `8080`.

## Verifisering

Kjør disse etter oppsett:

```bash
git --version
java -version
javac -version
mvn -v
ant -version   # hvis relevant
node -v
npm -v
python --version
```

En enkel npm test bak proxy:

```bash
npm view lodash version
```

## Notater / Merknader

- Bruk portable ZIP hvor installasjon krever admin.
- Hold verktøy under en kort sti (unngå mellomrom) for å slippe path‑problemer.
- Oppgrader versjoner ved å legge til ny katalog og justere PATH / JAVA_HOME.
- For Notable / Markdown: lag en katalog `notes` i et synkronisert område om backup ønskes.
- Hvis interne Maven/NPM registries finnes, legg til repository / registry settings senere.

## Lenkeregister

| Navn | Lenke |
|------|-------|
| Visual Studio Code | https://code.visualstudio.com/download |
| Python (Windows) | https://www.python.org/downloads/windows/ |
| Notable | https://notable.md/ |
| Markdown Basic Syntax | https://www.markdownguide.org/basic-syntax/ |
| Git | https://git-scm.com/download/win |
| Java JDK 11 | https://www.oracle.com/java/technologies/javase-jdk11-downloads.html |
| Node.js | https://nodejs.org/en/download/ |
| Apache Ant | https://ant.apache.org/bindownload.cgi |
| Apache Maven | https://maven.apache.org/download.cgi |
| NetBeans | http://netbeans.apache.org/download/index.html |
| IntelliJ IDEA | https://www.jetbrains.com/idea/download/#section=windows |
| PyCharm | https://www.jetbrains.com/pycharm/download/#section=windows |
| DBeaver | https://dbeaver.io/download/ |

---

*Rekonstruert fra tilgjengelig TXT + PDF metadata og annoterte lenker. Gi beskjed om du ønsker en engelsk versjon, flere detaljer (pip/pipenv, Gradle, Docker‑workarounds, etc.), eller automatisk genererte skript.*
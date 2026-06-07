# Obsidian med GitHub — Installationsvejledning

Denne vejledning beskriver hvordan du installerer Obsidian og sætter det op med GitHub via obsidian-git pluginet. Vejledningen dækker Linux og Windows. På iPhone tilgås vault'en via GitHub-appen.

---

## Forudsætninger

- En GitHub-konto på [github.com](https://github.com)
- Et **privat** GitHub-repo oprettet til din vault
- Git installeret på din maskine

---

## Del 1: Installation af Git

### Linux

```bash
sudo apt update && sudo apt install git -y
```

### Windows

Download og installer Git fra [git-scm.com](https://git-scm.com/download/win). Brug standardindstillingerne under installationen.

---

## Del 2: Konfigurer Git

Kør følgende i terminalen (Linux) eller Git Bash (Windows):

```bash
git config --global user.email "din@email.dk"
git config --global user.name "DitBrugernavn"
```

---

## Del 3: SSH-nøgle til GitHub

### Opret nøgle

**Linux:**
```bash
ssh-keygen -t ed25519 -C "din@email.dk"
cat ~/.ssh/id_ed25519.pub
```

**Windows (Git Bash):**
```bash
ssh-keygen -t ed25519 -C "din@email.dk"
cat ~/.ssh/id_ed25519.pub
```

### Tilføj nøgle til GitHub

1. Gå til [github.com](https://github.com)
2. Klik på dit **profilbillede** øverst til højre
3. Vælg **Settings**
4. I venstre sidebar under **Access** → klik **SSH and GPG keys**
5. Klik **New SSH key**
6. Indsæt outputtet fra `cat`-kommandoen i **Key**-feltet
7. Giv nøglen et navn (f.eks. "Laptop") og klik **Add SSH key**

### Test forbindelsen

```bash
ssh -T git@github.com
```

Du bør se: `Hi BRUGERNAVN! You've successfully authenticated.`

---

## Del 4: Installation af Obsidian

### Linux

Download AppImage fra [obsidian.md](https://obsidian.md):

```bash
chmod +x Obsidian-*.AppImage
./Obsidian-*.AppImage
```

Alternativt via Snap:

```bash
sudo snap install obsidian --classic
```

### Windows

Download installationsfilen fra [obsidian.md](https://obsidian.md) og kør den. Følg installationsguiden.

---

## Del 5: Opret og initialiser din vault

Åbn Obsidian og opret en ny vault — vælg den mappe du vil bruge lokalt, f.eks.:

- **Linux:** `/home/BRUGERNAVN/Documents/obsidian-vault`
- **Windows:** `C:\Users\BRUGERNAVN\Documents\obsidian-vault`

Åbn derefter en terminal i vault-mappen og kør:

```bash
git init
git remote add origin git@github.com:GITHUBBRUGERNAVN/obsidian-vault.git
```

Tilføj en `.gitignore` så Obsidians interne filer ikke synkroniseres:

```bash
cat > .gitignore << 'EOF'
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.trash/
EOF
```

Lav første commit og push:

```bash
git add .
git commit -m "Initial vault commit"
git branch -m master main
git push -u origin main
```

---

## Del 6: Installer obsidian-git pluginet

1. Åbn Obsidian → **Settings** (tandhjul nederst til venstre)
2. Gå til **Community plugins**
3. Klik **Turn on community plugins** hvis Restricted mode er aktiv
4. Klik **Browse** og søg efter `obsidian-git`
5. Klik **Install** og derefter **Enable**

---

## Del 7: Konfigurer obsidian-git

Gå til **Settings → obsidian-git** og sæt følgende:

| Indstilling | Anbefalet værdi |
|---|---|
| Auto pull interval (minutes) | 10 |
| Auto commit-and-sync interval (minutes) | 10 |
| Commit message | `vault backup: {{date}}` |
| Pull updates on startup | Aktiveret |

### Test pluginet

Tryk `Ctrl+P` og søg efter:

```
obsidian-git: Commit all changes
```

Kør kommandoen og verificér at der ikke opstår fejl.

---

## Del 8: Adgang fra iPhone

På iPhone tilgås vault'en via **GitHub-appen**:

1. Download **GitHub** fra App Store
2. Log ind med din GitHub-konto
3. Find dit `obsidian-vault` repo
4. Gennemse og læs dine markdown-noter direkte i appen

> GitHub-appen renderer markdown og er velegnet til at læse noter på farten. Redigering anbefales foretaget på desktop.

---

## Daglig brug

Obsidian-git kører automatisk i baggrunden og committer og synkroniserer dine noter med GitHub hvert 10. minut. Du behøver ikke gøre noget manuelt.

Hvis du vil synkronisere manuelt, tryk `Ctrl+P` og kør:

```
obsidian-git: Commit all changes and push
```

---

## Brug på flere maskiner

På en ny maskine kloner du repo'et:

**Linux:**
```bash
git clone git@github.com:GITHUBBRUGERNAVN/obsidian-vault.git ~/Documents/obsidian-vault
```

**Windows (Git Bash):**
```bash
git clone git@github.com:GITHUBBRUGERNAVN/obsidian-vault.git "C:\Users\BRUGERNAVN\Documents\obsidian-vault"
```

Åbn derefter mappen som vault i Obsidian og installer obsidian-git som beskrevet i Del 6 og 7.

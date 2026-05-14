# Kurs GIT dla koła naukowego SOIP

## Co to git?

Rozproszony system kontroli wersji!

### Instalacja

- [git-scm.com](https://git-scm.com/install/)
- [github.com](https://github.com/git-guides/install-git)

### Konfiguracja

- [GitHub CLI](https://cli.github.com/manual/gh)
Połączenie konta GitHub i Git - `gh auth login`
Sprawdzenie statusu - `gh auth status`

## Podstawy

### `git init`

Polecenie tworzy puste repozytorium git we wskazanym katalogu. Tworzony jest ukryty plik `.git`, który przechowuje wszystkie informacje o repozytorium. Git przechowuje pliki całościowo, aby łatwiej było je odtworzyć. 
Git opiera się na wskaźnikach na konkretne migawki - `HEAD` pokazuje na jakiej zmianie jest obecnie użytkownik, natomiast np. `feat/new-page` informuje nas o gałęzi (serii migawek). Każda migawka zawiera wskaźnik do poprzedniej migawki.

### `git add`

Domyślnie pliki nie są śledzone. Programista musi wskazać systemowi, które pliki ma śledzić. Do tego służy polecenie `git add`.

### `git commit`

Pliki z poczekalni są utrwalane w migawce. 

### `git status`

Sprawdzenie statusu repozytorium.

### `git branch`

Sprawdzenie gałęzi w projekcie. 

### `git checkout`

Przełączenie się na gałąź w projekcie.

### `git push | git pull | git fetch`

#### `git push --force`

### `git reset`

## Zdalne repozytoria

### GitHub

GitHub to nie jest Git! Jest to platforma do zarządzania repozytoriami .git. Git jest narzędziem, działającym lokalnie. 

GitHub Umożliwia:
- przechowywanie repozytoriów zdalnie
- współpracowanie nad projektem
- udzielanie dostępu do repozytoriów - Kontrola dostępu
- wdrażanie i łączenie zmian w projekcie - Pull Request
- GitHub Actions - automatyczne testy, wdrażanie
- Forki - kopiowanie repozytoriów i niezależna praca nad nim 

#### Stworzenie publicznego repozytorium
- New Repository
- Widoczność: Public
- Wymagana jest unikalna nazwa
- Publiczne repozytorium jest widoczne dla każdego. 
UWAGA! Widoczność zawsze można zmienić. 

#### Stworzenie prywatnego repozytorium i dodanie użytkowników
- Widoczność: Private
- Po utworzeniu repozytorium należy wejść:
    - Settings
    - Collaborators/Access
    - Add people
    - Wpisz nazwę użytkownika do zaproszenia

Aby móc zklonować prywatne repozytorium, należy zalogować się poprzez narzędzie `gh`. Prywatne repozytorium nie jest widoczne (można to sprawdzić np. poprzez tryb incognito) i wymaga odpowiednich poświadczeń. 

#### Wstawianie zmian na zdalny serwer
```bash
# inicjalizacja repozytorium
git init

# dodanie treści do pliku
vim README.md

# dodanie wszystkich zmian do poczekalni
git add .

# zatwierdzenie zmian
git commit -am "init"

# dodanie zdalnego repozytorium
git remote add origin https://github.com/USERNAME/REPO.git

# wysłanie zmian
git push -u origin main

git push # wystarczy, jeżeli mamy jedno origin
```

#### Praca na gałęziach

### Origin

Wyżej wspomniana "gałąź" origin nie jest wcale gałęźią. Jest to wskaźnik na repozytorium GitHub. Aby wskazać konkretną gałąź w repozytorium zdalnym, musimy połączyć ją z tym wskaźnikiem: `origin/main`. 

`feat/test-1` - gałąź lokalna, na której pracujemy nad testami
`origin/feat/test-1` - gałąź zdalna, stan gałęzi `feat/test-1` na GitHubie.

### GitLab

Alternatywa dla GitHub, nastawiona na DevOps - bardziej rozbudowane procesy CI/CD. GitLab jest bardziej kontrolowany, można go samemu zahostować na właśnej infrastrukturze. GitHub jest popularniejszy, bardziej społecznościowy. 

### GitGud.io'

GitLab i GitHub to nie są jedyne 2 hosty Git. Jest to zahostowany GitLab, który pozwala na darmowe CI/CD. Jest to jedna z alternatyw dla dużych graczy, o której warto wspomnieć. 

### Co to jest Pull Request?

(Merge Konflikty w Świecie Według PHP)[https://www.youtube.com/watch?v=sDQvMaFTiWY]

Pull Request nie jest operacją Git, jest funkcją platformy GitHub. 
Przed operacją połączenia gałęzi, możemy zaproponować zmiany, które mają zostać wdrożony. Inny programista dzięki temu może przejrzeć zmiany i je przedyskutować z resztą zespołu. Jest to narzędzie, pozwalające sprawnie kontrolować wprowadzane zmiany w dużej organizacji. 

Dodatkowo, można zintegrować procesy CI/CD. GitHub Actions pozwala np. uruchomić aplikację i zweryfikować jakość kodu dzięki linterom. Można też uruchomić automatyczne testowanie aplikacji. Daje to informację dla reszty zespołu o jakości wprowadzanego kodu. 
Idąc z duchem czasu, w GitHub Actions można nawet uruchomić model AI!

```bash
# stworzenie brancha 
git checkout -b feature/login

# wystawienie zmian do repozytorium zdalnego
git push origin feature/login

# Na GitHub tworzymy PR:
# - Zakładka Pull Requests
# - New pull request
# - base - gałąź do której trafią zmiany
# - compare - gałaź z której pobieramy zmiany. 
```

### `git merge` vs `git rebase`

#### Sytacja wyjściowa

```
A---B---C (main)
     \
      D---E (feature)
```

#### `git merge`

```
git checkout main
git merge feature
```

```
A---B---C-------F (main)
     \         /
      D---E---
```

#### `git rebase`

```
git checkout feature
git rebase main
```

```
A---B---C---D---E (feature)
```

```
git checkout main
git merge feature
```

```
A---B---C---D---E (main)
```

#### Jaka jest różnica?

Merge dodaje nowy commit, ale nie zmienia historii. Ten commit jest efektem połączenia 3 migawek: 
- ostatniej wspólnej dla 2 gałęzi
- ostatniej na gałęzi głównej
- ostatniej na gałęzi dołączanej

Rebase zmienia historię, wstawiając zmiany z gałęzi macierzystej na gałaź poboczną. Stosuje się to, gdy chcemy wprowadzić zmiany na gałaź, na której już pracujemy.

Dobrą praktyką jest stosować i rebase i merge, aby zachować liniowość historii. 

## GitFlow

Jest to organizacja pracy z branchami w Git. Strategia została zaprojektowana pod duże zespoły, z podziałem na wiele środowisk (np. development, staging, production). To jest kowencja pracy z której korzysta się w profesjonalnych zespołach. 

Nie wrzuca się wszystkiego do `main`, przynajmniej bezpośrednio. GitFlow zakłada kilka typów branchy: - `main` - stabilna produkcja, poprawnie działająca wersja aplikacji, która jest przetestowana
- `develop` - główny branch, na który wprowadza się zmiany 
- `feature/*` - branch, na którym programista pracuje nad jedną funkcjonalnością/zadaniem
- `release/*` - przygotowanie nowej wersji aplikacji 
- `hotfix/*` - naprawianie błędów

Nie są to jedyne typy branchy, nazewnictwo też może się różnić, jednak idea jest podobna. 

```
main
  ^
  |
release/*
  ^
  |
develop
  ^
  |
feature/*
```

Nie wolno wprowadzać zmian bezpośrednio. Aby zmienić coś w projekcie, trzeba utworzyć nowy `feature/*`, który trzeba połączyć z gałęzią `develop`. Dopiero, kiedy wersja deweloperska wydaje się stabilna i skończona, wprowadza się gałaź `release/*` (może być pomijana), na której programiści eliminują występujące błędy. Następnie, gałąź ta jest łączona z gałęzią `main`. 

```
                                   ┌────────────────────┐
                                   │   hotfix/payment   │
                                   └─────────┬──────────┘
                                             │
                                             ▼
MAIN ───────────────────────────────●────────●──────────────
                                    ▲        ▲
                                    │        │
                                    │        │
                         merge release   merge hotfix
                                    │        │
                                    │        │
DEVELOP ─────●────────●─────────────●────────●──────────────
             ▲        ▲
             │        │
             │        │
     ┌───────┘        └──────────┐
     │                           │
     ▼                           ▼

feature/auth              feature/docker
     │                           │
     └──────── merge ────────────┘
```

## Aplikacje graficzne

### GitHub Desktop

### Gitk

### LazyGit

### JetBrains/VSCode

# Autorzy

- Kamil "Kamyk2003" Kosior
- Kacper "DScraftPL" Wiącek

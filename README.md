# Hasnapló – Android alkalmazás

Étkezés-, tünet- és székletnapló gastritis, reflux és Crohn-betegség követésére. Önálló, offline alkalmazás. Nem kell hozzá fiók, és nem küld adatot sehova. A napló a telefonon marad.

## APK készítése GitHubon (nem kell hozzá semmit telepíteni)

1. Hozz létre egy **privát** repót a GitHubon, például `hasnaplo` néven.
2. Töltsd fel ennek a mappának a teljes tartalmát. A `.github` mappa is kell, mert abban van a fordítás leírása:
   ```bash
   cd hasnaplo-android
   git init -b main
   git add .
   git commit -m "Hasnapló"
   git remote add origin git@github.com:<felhasznalonev>/hasnaplo.git
   git push -u origin main
   ```
3. A feltöltés után az **Actions** fülön elindul az „APK készítése” folyamat, ami kb. 3–5 percig tart.
4. Ha kész, nyisd meg a futást, és lent az **Artifacts** résznél töltsd le a `Hasnaplo-apk` csomagot. Ez egy zip, benne van a `Hasnaplo.apk`.

Kézzel is elindíthatod: Actions → APK készítése → **Run workflow**.

## APK készítése a saját gépen

Szükséged lesz Java 17-re és az Android SDK-ra (például Android Studióval együtt települ).

```bash
cd hasnaplo-android
echo "sdk.dir=$HOME/Android/Sdk" > local.properties
./gradlew assembleRelease
# kész: app/build/outputs/apk/release/app-release.apk
```

Android Studióban egyszerűen nyisd meg a mappát, és válaszd a Build → Build APK(s) menüpontot.

## Telepítés a telefonra

1. Másold a `Hasnaplo.apk` fájlt a telefonra, vagy töltsd le rá közvetlenül.
2. Nyisd meg. Az Android megkérdezi, hogy engedélyezed-e a telepítést ebből a forrásból (például a Fájlok alkalmazásból vagy a Chrome-ból). Engedélyezd.
3. Android 10 vagy újabb kell hozzá.

Frissítéskor az új APK-t a régire telepítheted, a napló megmarad. Ez azért működik, mert minden változat ugyanazzal a kulccsal van aláírva (`app/hasnaplo.keystore`). Ezért legyen a repó privát. **Ne töröld az alkalmazást**, mert azzal a napló is törlődik.

## Biztonsági mentés és átköltöztetés

Az Összefüggések fül alján:

- **Mentés fájlba:** a teljes naplót egy `.json` fájlba menti a Letöltések mappába. Rendszeresen érdemes, mert ha a telefon elveszik, ebből tudod visszaállítani.
- **Betöltés fájlból:** visszatölt egy ilyen mentést. A meglévő bejegyzések megmaradnak, csak az újak kerülnek be.
- **Napló mentése CSV-ként:** táblázat az orvosi vizithez, a Letöltések mappába kerül.

Ha a claude.ai-os Hasnaplóban már vannak bejegyzéseid, ott is van „Mentés fájlba” gomb. Az így kapott fájlt itt be tudod tölteni.

## Felépítés

- `app/src/main/assets/index.html`: maga az alkalmazás (HTML, CSS, JavaScript), a betűtípusokkal együtt offline.
- `app/src/main/java/.../MainActivity.java`: a WebView, ami megjeleníti. Ez kezeli a vissza gombot, a fájlmentést a Letöltésekbe és a mentésfájl kiválasztását.
- Az adatok a WebView helyi tárolójában (localStorage) vannak, az alkalmazás saját, privát területén.

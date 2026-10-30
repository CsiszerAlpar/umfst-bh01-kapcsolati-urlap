# A dékáni hivatal „működő” kapcsolati űrlapja – hibariport

Repó: https://github.com/CsiszerAlpar/umfst-bh01-kapcsolati-urlap
Készítette: Csiszer Alpar, Info_LM 3. ev

## 1. hiba
- **Hiba leírása és reprodukálása (lépések, böngésző, segédtechnológia):** 
  A képernyőolvasó (pl. NVDA) a mezőknél csak „szerkesztés, üres” szöveget olvas fel, és gépelés közben elveszíti a mező nevét, mert hiányoznak a szabványos `<label>` címkék.
- **Gyökérok (melyik sor, melyik szabály sérül):** 
  Az input/select/textarea elemeknél kizárólag `placeholder` attribútum szerepelt, ami szemantikailag nem helyettesíti a `<label>` elemet (WCAG 1.3.1 és 3.3.2).
- **Javítás (commit link, előtte/utána kódrészlet):** 
  Hozzáadtuk a `<label for="...">` elemeket a form mezőihez.
  Előtte: `<input type="text" name="nev" placeholder="Teljes név" required>`
  Utána: `<label for="nev">Teljes név:</label><input type="text" id="nev" name="nev" placeholder="Teljes név" required>`
- **Hogyan tesztelted (W3C validátor, Lighthouse, Tab-bejárás, képernyőolvasó):** 
  Képernyőolvasóval és Lighthouse Accessibility audit segítésével ellenőrizve.
- **Tanulság / érintett alapelv:** 
  *Szemantika előbb, stílus később.* A placeholder vizuális segédlet, nem akadálymentes címke.

## 2. hiba
- **Hiba leírása és reprodukálása (lépések, böngésző, segédtechnológia):** 
  A főcím fekete maradt böngészőben (Chrome/Firefox), holott zöld színűnek kellett volna lennie.
- **Gyökérok (melyik sor, melyik szabály sérül):** 
  A CSS `.cim` osztályában el lett gépelve a tulajdonság neve: `colour: #0e7c66;` helyett `color: #0e7c66;` szükséges.
- **Javítás (commit link, előtte/utána kódrészlet):** 
  Előtte: `.cim { colour: #0e7c66; font-size: 2rem; }`
  Utána: `.cim { color: #0e7c66; font-size: 2rem; }`
- **Hogyan tesztelted (W3C validátor, Lighthouse, Tab-bejárás, képernyőolvasó):** 
  Böngésző DevTools (Elements -> Styles panel).
- **Tanulság / érintett alapelv:** 
  A böngésző a hibás tulajdonságneveket csendben ignorálja, érdemes lint eszközöket használni.

## 3. hiba
- **Hiba leírása és reprodukálása (lépések, böngésző, segédtechnológia):** 
  Mobilon az e-mail mezőre kattintva a hagyományos szöveges billentyűzet nyílt meg (nincs speciális @ karakter), és érvénytelen formátumot (pl. „alma”) is elfogadott az űrlap.
- **Gyökérok (melyik sor, melyik szabály sérül):** 
  Az e-mail mező `type="text"` attribútummal rendelkezett a `type="email"` helyett.
- **Javítás (commit link, előtte/utána kódrészlet):** 
  Előtte: `<input type="text" name="email" placeholder="E-mail cím" required>`
  Utána: `<input type="email" id="email" name="email" placeholder="E-mail cím" required>`
- **Hogyan tesztelted (W3C validátor, Lighthouse, Tab-bejárás, képernyőolvasó):** 
  Mobilos billentyűzet teszt és HTML5 kliens oldali validáció ellenőrzése.
- **Tanulság / érintett alapelv:** 
  A megfelelő HTML5 input típusok használata alapvető a mobilos felhasználói élmény és az automatikus validáció szempontjából.

## 4. hiba
- **Hiba leírása és reprodukálása (lépések, böngésző, segédtechnológia):** 
  Billentyűzetes navigáció (Tab gomb) során nem látszódott, hogy melyik űrlapmezőn van éppen a fókusz.
- **Gyökérok (melyik sor, melyik szabály sérül):** 
  A CSS-ben az `input:focus, select:focus, textarea:focus, button:focus { outline: none; }` szabály teljesen eltávolította a fókuszjelzést vizuális alternatíva nélkül (WCAG 2.4.7).
- **Javítás (commit link, előtte/utána kódrészlet):** 
  Előtte: `input:focus, select:focus, textarea:focus, button:focus { outline: none; }`
  Utána: `input:focus, select:focus, textarea:focus, button:focus { outline: 2px solid #0e7c66; outline-offset: 2px; }`
- **Hogyan tesztelted (W3C validátor, Lighthouse, Tab-bejárás, képernyőolvasó):** 
  Billentyűzetes (Tab) bejárás tesztelése asztali böngészőben.
- **Tanulság / érintett alapelv:** 
  Soha ne szüntessük meg a fókuszkeretet vizuális pótlás nélkül, mert ellehetetleníti a billentyűzetes navigációt.

## 5. hiba
- **Hiba leírása és reprodukálása (lépések, böngésző, segédtechnológia):** 
  A Küldés gombra navigálva a fókusz viselkedése furcsa volt, kétszer kellett Tab-olni rajta.
- **Gyökérok (melyik sor, melyik szabály sérül):** 
  A gomb egy hivatkozásba volt ágyazva (`<a href="#"><button>Küldés</button></a>`), ami szemantikailag érvénytelen HTML, és duplikált fókuszpontot hozott létre (WCAG 4.1.2).
- **Javítás (commit link, előtte/utána kódrészlet):** 
  Előtte: `<a href="#"><button>Küldés</button></a>`
  Utána: `<button type="submit">Küldés</button>`
- **Hogyan tesztelted (W3C validátor, Lighthouse, Tab-bejárás, képernyőolvasó):** 
  Tab-bejárás és W3C validator futtatása.
- **Tanulság / érintett alapelv:** 
  Interaktív elemeket (linkek és gombok) tilos egymásba ágyazni.

## 6. hiba
- **Hiba leírása és reprodukálása (lépések, böngésző, segédtechnológia):** 
  A képernyőolvasó a logó képnél a fájlnevet („logo.png”) olvasta fel szöveges alternatíva helyett.
- **Gyökérok (melyik sor, melyik szabály sérül):** 
  Az `<img>` elemből hiányzott az `alt` attribútum (WCAG 1.1.1).
- **Javítás (commit link, előtte/utána kódrészlet):** 
  Előtte: `<img src="logo.png" width="120" height="40">`
  Utána: `<img src="logo.png" alt="ÁOK Dékáni Hivatal logó" width="120" height="40">`
- **Hogyan tesztelted (W3C validátor, Lighthouse, Tab-bejárás, képernyőolvasó):** 
  Képernyőolvasó teszt és W3C validator.
- **Tanulság / érintett alapelv:** 
  Minden tartalmi kép esetében kötelező leíró `alt` attribútumot megadni.

## 7. hiba
- **Hiba leírása és reprodukálása (lépések, böngésző, segédtechnológia):** 
  A képernyőolvasó angol fonetikai kiejtéssel olvasta fel a magyar nyelvű szöveget.
- **Gyökérok (melyik sor, melyik szabály sérül):** 
  A gyökér `<html>` címkéből hiányzott a `lang="hu"` attribútum (WCAG 3.1.1).
- **Javítás (commit link, előtte/utána kódrészlet):** 
  Előtte: `<html>`
  Utána: `<html lang="hu">`
- **Hogyan tesztelted (W3C validátor, Lighthouse, Tab-bejárás, képernyőolvasó):** 
  Képernyőolvasó beszédmotor hangalapú ellenőrzése.
- **Tanulság / alapelv:** 
  A dokumentum nyelvének pontos beállítása alapvető követelmény a helyes akadálymentes felolvasáshoz.

## Összegzés
- Mit tanultam: A webes akadálymentesség (a11y) és a szemantikus HTML alapvető fontosságú a valós felhasználói élményhez; a vizuális hibák mellett a képernyőolvasók és a billentyűzetes navigáció tesztelése elengedhetetlen.
- Melyik alapelv sérült a leggyakrabban: A szemantikus elemek helytelen használata és az akadálymentességi attribútumok (label, alt, lang) hiánya.

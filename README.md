# JavaScript – Tahák pro práci s DOM

## 1. Výběr prvku na stránce – `querySelector`

Vrací **první** odpovídající prvek (nebo `null`, pokud nic nenajde). Používá se CSS syntaxe.

```js
// podle značky (tagu)
const nadpis = document.querySelector("h1");

// podle ID (mřížka #)
const tlacitko = document.querySelector("#mojeTlacitko");

// podle třídy (tečka .)
const polozka = document.querySelector(".polozka");

// kombinace – první <p> uvnitř .obsah
const odstavec = document.querySelector(".obsah p");
```

---

## 2. Výběr více prvků – `querySelectorAll` + `forEach`

Vrací **NodeList** (kolekci) všech odpovídajících prvků.

```js
const polozky = document.querySelectorAll(".polozka");

polozky.forEach((polozka) => {
  console.log(polozka.textContent);
});

// s indexem
polozky.forEach((polozka, index) => {
  polozka.textContent = `Položka číslo ${index + 1}`;
});
```

---

## 3. Úprava prvku – text, styly, třídy

```js
const prvek = document.querySelector("#box");

// textový obsah
prvek.textContent = "Nový text";

// CSS vlastnosti (camelCase místo pomlček)
prvek.style.color = "red";
prvek.style.backgroundColor = "yellow";
prvek.style.fontSize = "20px";

// CSS třídy
prvek.classList.add("aktivni");        // přidat
prvek.classList.remove("aktivni");     // odebrat
prvek.classList.toggle("aktivni");     // přepnout (přidat/odebrat)
prvek.classList.contains("aktivni");   // true / false
```

---

## 4. Události – `addEventListener`

```js
prvek.addEventListener("nazevUdalosti", funkce);
```

### Nejdůležitější události myši
| Událost | Kdy se spustí |
|---|---|
| `click` | kliknutí |
| `dblclick` | dvojklik |
| `mousedown` / `mouseup` | stisknutí / uvolnění tlačítka |
| `mousemove` | pohyb myší nad prvkem |
| `mouseover` / `mouseout` | najetí / opuštění prvku |
| `contextmenu` | pravé tlačítko |

### Nejdůležitější události klávesnice
| Událost | Kdy se spustí |
|---|---|
| `keydown` | stisk klávesy (i opakovaně při držení) |
| `keyup` | uvolnění klávesy |
| `keypress` | (zastaralé, nepoužívat) |

### Příklad – kliknutí
```js
const tlacitko = document.querySelector("#tl");

tlacitko.addEventListener("click", () => {
  alert("Kliknuto!");
});
```

### Příklad – stisk klávesy
```js
document.addEventListener("keydown", (event) => {
  console.log("Stisknuto:", event.key);

  if (event.key === "Enter") {
    console.log("Stisknul jsi Enter");
  }
  if (event.key === "ArrowRight") {
    console.log("Šipka doprava");
  }
});
```

> `event.key` = název klávesy (např. `"a"`, `"Enter"`, `"ArrowUp"`, `" "` pro mezerník)

---

## 5. Vytváření nových prvků – `createElement` + `appendChild`

```js
// 1. vytvoříme prvek (zatím existuje jen v paměti)
const novy = document.createElement("li");

// 2. nastavíme mu obsah / styl / třídu
novy.textContent = "Nová položka";
novy.classList.add("polozka");

// 3. vložíme ho do stránky jako potomka jiného prvku
const seznam = document.querySelector("#seznam");
seznam.appendChild(novy);
```

Užitečné související metody:
```js
rodic.removeChild(potomek);   // odebrat
prvek.remove();               // odebere sám sebe
```

---

## 6. Hodnoty z formulářových polí – `value`

U `<input>`, `<textarea>` i `<select>` se zapsaná hodnota čte (a nastavuje) přes vlastnost `.value`. Je to **vždy řetězec (string)**, i když uživatel napíše číslo.

```html
<input type="text" id="jmeno">
<input type="number" id="vek">
<button id="ok">Odeslat</button>
```

```js
const jmeno = document.querySelector("#jmeno");
const vek = document.querySelector("#vek");
const tlacitko = document.querySelector("#ok");

tlacitko.addEventListener("click", () => {
  console.log(jmeno.value); // "Petr"  (string)
  console.log(vek.value);   // "20"    (string – pozor!)

  // nastavit hodnotu (např. vyčistit pole)
  jmeno.value = "";
});
```

### Převod hodnoty na číslo

```js
const text = vek.value;       // "20"
const cislo = Number(text);   // 20  (number)

// alternativa
const cislo2 = parseInt(text, 10);    // celé číslo
const cislo3 = parseFloat(text);      // desetinné číslo
```

### Ošetření, že vstup je opravdu číslo

Pokud uživatel napíše něco, co se nedá převést (např. `"abc"` nebo nechá pole prázdné), `Number(...)` vrátí `NaN` (Not a Number). To se ověří funkcí `isNaN()`:

```js
const cislo = Number(vek.value);

if (isNaN(cislo)) {
  alert("Zadej platné číslo!");
} else {
  console.log("Za rok ti bude:", cislo + 1);
}
```

> **Časté úskalí:** Bez převodu se `+` chová jako spojování textů.
> `"20" + 1` → `"201"`, kdežto `Number("20") + 1` → `21`.

### Checkbox – čtení přes `.checked`

```html
<input type="checkbox" id="souhlas">
```
```js
const souhlas = document.querySelector("#souhlas");
console.log(souhlas.checked); // true / false
```

---

## 7. Časovače – `setTimeout` a `setInterval`

### `setTimeout` – spustí jednou po určité době
```js
setTimeout(() => {
  console.log("Uplynuly 2 vteřiny");
}, 2000); // čas v milisekundách
```

### `setInterval` – spouští opakovaně
```js
const id = setInterval(() => {
  console.log("Tik");
}, 1000);

// zastavení
clearInterval(id);
```

Analogicky pro setTimeout:
```js
const id = setTimeout(...);
clearTimeout(id);
```

---

## 8. `localStorage` – ukládání dat v prohlížeči

Data zůstanou uložená i po zavření prohlížeče. Ukládají se **vždy jako řetězce (string)**.

```js
// uložit
localStorage.setItem("jmeno", "Petr");

// načíst (vrací null, pokud klíč neexistuje)
const jmeno = localStorage.getItem("jmeno");

// odstranit jeden klíč
localStorage.removeItem("jmeno");

// vymazat vše
localStorage.clear();
```

### Ukládání objektů / polí – přes JSON
```js
const uzivatel = { jmeno: "Petr", vek: 20 };

// uložit
localStorage.setItem("uzivatel", JSON.stringify(uzivatel));

// načíst
const data = JSON.parse(localStorage.getItem("uzivatel"));
console.log(data.jmeno);
```

---

## 9. Cookies – základy

Cookies jsou malé textové záznamy uložené v prohlížeči. Na rozdíl od `localStorage` se **odesílají na server** s každým HTTP požadavkem a lze jim nastavit **dobu platnosti**.

### Zápis cookie
```js
// jednoduchá cookie (zmizí po zavření prohlížeče)
document.cookie = "jmeno=Petr";

// cookie s dobou platnosti (7 dní)
const dny = 7;
const datum = new Date();
datum.setTime(datum.getTime() + dny * 24 * 60 * 60 * 1000);
document.cookie = "jmeno=Petr; expires=" + datum.toUTCString() + "; path=/";
```

> **Pozor:** `document.cookie = ...` nepřepisuje všechny cookies – přidává/aktualizuje jen tu konkrétní.

### Čtení cookies

`document.cookie` vrací **jeden dlouhý řetězec** se všemi cookies:

```js
console.log(document.cookie);
// "jmeno=Petr; barva=modra; vek=20"
```

Pomocná funkce pro získání jedné hodnoty:
```js
function getCookie(nazev) {
  const cookies = document.cookie.split("; ");
  for (const cookie of cookies) {
    const [klic, hodnota] = cookie.split("=");
    if (klic === nazev) return hodnota;
  }
  return null;
}

console.log(getCookie("jmeno")); // "Petr"
```

### Smazání cookie

Cookie se smaže nastavením `expires` do minulosti:
```js
document.cookie = "jmeno=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/";
```

### Cookies vs. localStorage
| | Cookies | localStorage |
|---|---|---|
| Kapacita | cca 4 KB | cca 5 MB |
| Odesílá se na server | ano | ne |
| Platnost | nastavitelná (expires) | trvalá (dokud se nesmaže) |

---

## 10. Drag & Drop – základy

Funguje na principu událostí. Prvek, který chceme **táhnout**, musí mít atribut `draggable="true"`.

```html
<div id="zdroj" draggable="true">Táhni mě</div>
<div id="cil">Sem mě pusť</div>
```

### Události na taženém prvku
| Událost | Kdy |
|---|---|
| `dragstart` | začátek tažení |
| `dragend` | konec tažení (puštění) |

### Události na cílovém prvku
| Událost | Kdy |
|---|---|
| `dragover` | prvek je tažen nad cílem (**musí se zavolat `event.preventDefault()`**, jinak `drop` nefunguje) |
| `dragenter` | prvek vstoupil nad cíl |
| `dragleave` | prvek opustil cíl |
| `drop` | puštění nad cílem |

### Minimální příklad
```js
const zdroj = document.querySelector("#zdroj");
const cil = document.querySelector("#cil");

zdroj.addEventListener("dragstart", (event) => {
  event.dataTransfer.setData("text/plain", "ahoj");
});

cil.addEventListener("dragover", (event) => {
  event.preventDefault(); // DŮLEŽITÉ – povolí drop
});

cil.addEventListener("drop", (event) => {
  event.preventDefault();
  const data = event.dataTransfer.getData("text/plain");
  console.log("Puštěno, předaná data:", data);
  cil.appendChild(zdroj); // přesun prvku do cíle
});
```

> **Trik:** Přes `event.dataTransfer` lze předávat informace mezi `dragstart` a `drop` (typicky ID taženého prvku).

---

## 11. Fetch – načítání dat ze serveru

`fetch` slouží k odesílání HTTP požadavků (typicky na API). Vrací **Promise** – výsledek, který bude k dispozici až v budoucnu.

### Varianta 1 – Promise (`.then`)

```js
fetch("https://jsonplaceholder.typicode.com/users/1")
  .then((response) => {
    if (!response.ok) {
      throw new Error("Chyba: " + response.status);
    }
    return response.json(); // převede tělo odpovědi na JS objekt
  })
  .then((data) => {
    console.log(data.name);  // "Leanne Graham"
    console.log(data.email); // "Sincere@april.biz"
  })
  .catch((error) => {
    console.error("Něco se pokazilo:", error);
  });
```

**Jak to číst:** Zavolej `fetch` → **pak** (`.then`) zpracuj odpověď → **pak** pracuj s daty → **kdyby se cokoliv pokazilo** (`.catch`), vypiš chybu.

### Varianta 2 – `async` / `await`

Totéž, ale čitelnějším zápisem:

```js
async function nactiUzivatele() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users/1");

    if (!response.ok) {
      throw new Error("Chyba: " + response.status);
    }

    const data = await response.json();
    console.log(data.name);
    console.log(data.email);
  } catch (error) {
    console.error("Něco se pokazilo:", error);
  }
}

nactiUzivatele();
```

> `await` říká: „počkej, až se tohle dokončí, a teprve pak pokračuj na další řádek". Funguje **jen uvnitř funkce označené jako `async`**.

### Výpis dat do stránky – typický vzor

```js
async function zobrazUzivatele() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const uzivatele = await response.json();

  const seznam = document.querySelector("#seznam");

  uzivatele.forEach((uzivatel) => {
    const li = document.createElement("li");
    li.textContent = uzivatel.name;
    seznam.appendChild(li);
  });
}

zobrazUzivatele();
```

> Toto je kombinace **fetch + createElement + appendChild** – propojení více naučených témat dohromady.

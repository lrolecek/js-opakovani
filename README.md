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

## 9. Drag & Drop – základy

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

## Bonus – propojení JS se stránkou

V HTML se skript načítá nejlépe takto (na konci `<body>` nebo s `defer`):

```html
<script src="script.js" defer></script>
```

`defer` zajistí, že se skript spustí **až po načtení DOM** – jinak by `querySelector` nic nenašel.

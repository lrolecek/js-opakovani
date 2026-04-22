# Cvičení 11 – Fetch

1. Po načtení stránky stáhni seznam uživatelů z `https://jsonplaceholder.typicode.com/users` a pro každého uživatele vytvoř `<div class="karta">` s jeho jménem a e-mailem. Karty vlož do `#seznam`.
2. Po kliknutí na tlačítko `"Načíst příspěvky"` stáhni příspěvky z `https://jsonplaceholder.typicode.com/posts?_limit=5` a vypiš je jako `<li>` do `#prispevky`.
3. Bonus: Během načítání zobraz text `"Načítám..."` v `#stav`, po dokončení ho skryj.

> Tip: Použij `async/await` variantu. Nezapomeň na `await response.json()`.

# Ćwiczenie 1

1. Stwórz plik `.gitignore` i dodaj do niego zawartość:

```
/node_modules
.env
```

2. Z poziomu terminala zainicjalizuj projekt. Użyj do tego polecania `npm init`. Uzupełnij informacje o projekcie.

3. Wewnątrz pliku `package.json` dodaj pole `type` ustawione na `module`. Dzięki tej konfiguracji możemy używać składni `import/export`.

4. Zainstaluj zależności:

- `express`
- `helmet`
- `cors`
- `dotenv`
- `cross-env`

5. Zainstaluj zależności jako `devDependency`:

- `nodemon`
- `@techni-tools/postman`

6. Stwórz folder `src`, a w nim plik `app.js`.

7. Wewnątrz pliku `app.js` zaimportuj bibliotekę `dotenv` oraz skonfiguruj ją wywołując metodę `config()`.

Dzięki temu w kodzie będzie można używać zmiennych środowiskowych.

8. Zaimportuj bibliotekę `express`, następnie stwórz stałą o nazwie `app` i przypisz do niej aplikację utworzoną za pomocą tej biblioteki (należy wywołać express).

9. Zaimportuj bliotekę `cors`. Ta biblioteka posiada funkcję wprowadzającą dodatkową konfigurację do aplikacji.

Aby ją zaaplikować użyj metody `app.use()`:

```js
import cors from "cors";
// ...
const app = express();
app.use(cors())
```

10. Analogicznie jak w punkcie 9. zaaplikuj funkcję z biblioteki `helmet`. Biblioteka zabezpiecza nasz serwer przed różnymi atakami.
    Ze względu na to, że stackblitz uruchamia aplikację w kontenerze, a `helemt` domyślnie to blokuje musimy wprowadzić dodatkowe opcje konfiguracyjne:

```js
import helmet from "helmet";
// ...
const app = express();
const isDev = process.env.NODE_ENV === 'development';
app.use(
  helmet({
    contentSecurityPolicy: !isDev,
    crossOriginResourcePolicy: !isDev,
    frameguard: !isDev,
  })
);
```

11. Zaaplikuj również wbudowane funkcje:

```js
const app = express();
app.use(express.json({ limit: "10mb" }));
app.use(express.urlencoded({ extended: true }));
```

12. Uruchom serwer za pomocą metody `app.listen()`.

Metoda w pierwszym argumencie otrzymuje `port`, na którym ma zostać uruchomiony, a w drugim `callback`, który zostanie wywołany, gdy serwer zostanie poprawnie uruchomiony.

Jako `port` podaj wartość ze zmiennej środowiskowej `PORT`, alternatywnie liczbę `8080`, gdy zmienna środowiskowa jest niezdefiniowana.

Jako `callback` podaj funkcję stworzoną za pomocą słowa kluczowego `function` (Ważne: Nie może być to funkcja strzałkowa!):

```js
function () {
  const { port } = this.address();
  console.log(`Server listening on port: ${port}`);
}
```

13. Przed wywołaniem `app.listen(...)` stwórz endpoint `GET /healthcheck`.

```js
const app = express();

app.get("/healthcheck", (req, res) => {
  res.status(200).json({ status: "ok", timestamp: Date.now()});
});
```

14. Analogicznie - stwórz endpoint `GET /`, który zwraca jako json pole "name", wartość to nazwa aplikacji.

15. Do pliku `package.json` dodaj dwa skrypty oraz konfigurację `nodemon`:

```json
{
  "scripts": {
    "start": "cross-env NODE_ENV=production node --experimental-specifier-resolution=node src/app.js",
    "dev": "cross-env NODE_ENV=development nodemon   --experimental-specifier-resolution=node src/app.js"
  },
  "nodemonConfig": {
    "ignore": ["technipostman"],
    "delay": 2500
  }
}
```

16. W folderze nadrzędnym stwórz plik `.env` i ustaw zmienną środowiskową `PORT` na wybraną wartość np. `5050`.

17. Uruchom serwer w wersji developerskiej.

18. Przetestuj ręcznie endpoint `/healtcheck`.

Możesz wejść do niego dodając do adresu URL uruchomionego kontenera ścieżkę: `/healtcheck`, np:

`https://stackblitzstarterslwuj8t-zbke--9999--d0228c83.local-credentialless.webcontainer.io/healthcheck`

---

### Dodatkowa konfiguracja - Techni Postman

Standardowa aplikacja Postman nie może być używana w zadaniach uruchamianych na stackblitz ze względu na to, że taka aplikacja jest przechowywana w odizolowanym kontenerze, do którego zewnętrzne aplikacje nie mają dostępu.

W związku z tym:

19. W `package.json` Dodaj nowy skrypt:

```json
{
  "scripts": {
    "postman": "techni-postman"
  }
}
```

20. Otwórz drugi terminal, a w nim uruchom skrypt: `npm run postman`

21. Otwórz link, który wyskoczył w terminalu.

22. W aplikacji Techni Postman stwórz request testujący Twój endpoint.

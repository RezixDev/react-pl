# Poradnik React (2025)

Jeżeli chcecie testować funkcje Reacta bez potrzeby tworzenia kolejnych projektów na dysku, to przydatną w tym przypadku platformą może być StackBlitz:
https://stackblitz.com/

Na niej możecie tworzyć własne Snippety, Projekty i dzielić się nimi ze znajomymi, bądź innymi programistami np: na Discordzie, aby pokazać jak coś działa, coś wytłumaczyć, bądź wskazać z czym macie problemy i poprosić innych o pomoc.

# Co się zmieniło w Reacie na przestrzeni Lat?

## Co wciąż zobaczycie w poradnikach, a czego nie musicie już robić?

Nie ma już potrzeby importowania Reacta do waszych komponentów.
Kiedyś, React był kompilowany przez Babel (przed React 17), z tego względu był wymagany React w zakresie Componentu, obecnie nie jest to wymagane, dzięki "new JSX transform". Babel już nie używa React.createElement, tylko importuje specjalne funkcje z pakietu react/jsx-runtime automatycznie.

https://legacy.reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html

# Hooki

Warto na początek wiedzieć, że nie można używać Hooków wewnątrz instrukcji warunkowych takich jak "if", "else", czy "switch". Najlepiej definiować je na początku componentu. 
## useState

useState to hook w React'cie, który pozwala komponentom funkcyjnym mieć swój własny stan (czyli przechowywać dane, które mogą się zmienić w czasie).

Takim klasycznym przykładem będzie funkcja "count".

```ts
const [count, setCount] = useState(0);
```

- count – to aktualna wartość stanu (tutaj: 0).
- setCount – to funkcja, która zmienia stan.
- useState(0) – 0 to wartość początkowa.

przykład w użyciu jako komponent:

```ts 
import { useState } from 'react';

export function Counter() {
  const [count, setCount] = useState(0);

  function increase() {
    setCount(count + 1)
  }

  return (
    <div>
      <h2>Licznik: {count}</h2>
      <button onClick={increase}>Zwiększ</button>
    </div>
  );
}
```

Aby było jeszcze bardziej współczesnie, możemy użyć funkcji strzałkowej:

```ts
import { useState } from 'react';

export function Counter() {
  const [count, setCount] = useState(0);

  const increase = () => setCount(count + 1);

  return (
    <div>
      <h2>Licznik: {count}</h2>
      <button onClick={increase}>Zwiększ</button>
    </div>
  );
}
```

Inny przykład użycia useState to lista Todo:


https://react.dev/reference/react/useState

## useEffect

Jeden z bardziej niezrozumiałych hooków w React. Wydaje mi się też, że nie jest on też często tłumaczony poprawnie w poradnikach, czy filmach na YouTube. Często tłumaczący zbyt mocno opiera się na zbyt zwięzłym przykładzie, używając abstrakcyjnych pojęć.

useEffect umożliwia komponentom Reactowym reagowanie na zmiany w aplikacji oraz na zdarzenia zewnętrzne. Efekt ten może być wykonywany:
- po każdym renderze (jeśli nie podano tablicy zależności),
- tylko raz (onMount, na początku ładowania komponentu, jeśli podano pustą tablicę []),
- lub po zmianie określonych wartości (jeśli podano zależności w tablicy).

Przykładem może być aktualizacja informacji, gdy coś się zmieni, bądź dynamiczna odpowiedź, gdy coś się stanie w prawdziwym świecie np: zmiana wyniku meczu w piłce nożnej. 

W skrócie: useEffect pozwala uruchamiać efekty uboczne (ang. side effects) — czyli wszystko to, co wykracza poza czysty rendering komponentu.

Ale czym właściwie są "efekty uboczne"? Są to zdarzenia w twoim kodzie, które mają miejsce poza renderingem twojej aplikacji.

Składnia useEffect wygląda tak:

```
useEffect(() => {
  // kod, który ma się wykonać
}, []);
```

"useEffect" a w nim funkcja strzałkowa wraz blokiem kodu oraz z tablicą []. W tej tablicy definiujemy zależności. 
Hook ten ma zareagować na pewne zmiany w kodzie, np: jak z naszm przykładem piłki nożnej, gdy wynik meczu się zmieni, przykładowo z 0 na 1, to nasze UI powinno się też zmienić i pokazać zaktualizowany wynik. 

Jeżeli wynik zmieni się ponownie, to funkcja ta również powinna się ponownie wykonać. 

Innym przykładem może być reagowanie na zmiany pogodowe w aplikacji pokazując aktualny stan pogody w danym rejonie. czy mieście. 
Albo aktualizacja ceny kursu akcji, bądź wartości danej waluty.

I to wszystko bez konieczności aktualizacji aplikacji!

Poza aktualizacją danych, useEffect może również zwracać funkcję czyszczącą (cleanup function), która służy do sprzątania po efektach ubocznych.
Taka funkcja uruchamiana jest:
- przy odmontowaniu komponentu (czyli gdy znika z widoku DOM),
- lub przed ponownym uruchomieniem efektu (jeśli zmienią się zależności w tablicy []).
  
Jest to szczególnie ważne, aby:
- zapobiec wyciekom pamięci,
- przerwać niepotrzebne fetchowanie danych,
- usunąć nasłuchiwacze zdarzeń lub
- zatrzymać interwały i timeouty.

### Niebezpieczeństwa związane z useEffect 

Mimo wielu zalet useEffect, ta funkcja, niepoprawnie użyta może spowodować wiele problemów, a nawet narobić wam niepotrzebnych kosztów.

Jeżeli macie podpięte API, za które płacicie, to zbyt częste fetchowanie poprzez useEffect może spowodować zwiększenie kosztów. Dlatego korzystając z tego Hooka ważnym jest poprawne napisanie Timera, funkcji debounce, czy wcześniej wspomnianej funkcji czyszczącej.

Dlatego zawsze sprawdzajcie waszą tablicę zależności, czyście wasze funkcje z efektów ubocznych, a jeżeli zajdzie taka potrzeba zróbcie debounce().

https://react.dev/reference/react/useEffect

## useContext (od React 19 use)

Context.Provider


# Wzorce

## Fasada


Facade (Fasada) to wzorzec projektowy, który tworzy pośrednią warstwę między zewnętrzną biblioteką a resztą aplikacji. Zamiast bezpośrednio używać np. axios lub localStorage w wielu komponentach, tworzysz jedną, ujednoliconą „fasadę” (np. plik z funkcjami), która to wszystko ukryw

Załóżmy, że używasz axios w 30 plikach. Jeśli zmieni się API biblioteki, musisz poprawić 30 plików. Ale jeśli wszystko przechodzi przez jedną fasadę, zmieniasz tylko 1 plik.

### 📦 Przykład w React z Axios

#### 🔴 Złe podejście – bez fasady

```tsx
import axios from 'axios';

export function getUser(id) {
  return axios.get(`/api/users/${id}`);
}
```

Powyższe powielone w wielu plikach = problem.

---

#### ✅ Dobre podejście – z fasadą

**📁 `api/userApi.ts` – fasada**

```ts
import axios from 'axios';

const apiClient = axios.create({
  baseURL: '/api',
  timeout: 5000,
});

export const getUser = (id: string) => apiClient.get(`/users/${id}`);
export const createUser = (data: any) => apiClient.post(`/users`, data);
```

**📁 `components/User.tsx`**

```tsx
import { getUser } from '../api/userApi';

useEffect(() => {
  getUser('123').then(res => console.log(res.data));
}, []);
```

https://pl.wikipedia.org/wiki/Fasada_(wzorzec_projektowy)

https://maxtsh.medium.com/what-is-a-facade-pattern-and-how-to-use-it-in-react-js-cb8332eb1b92


# Performance

Największym wpływem na szybkość aplikacji jest zbyt częste re-renderowanie i wolne Komponenty. 
W zniwelowaniu tych problemów może wam pomóc React Scan:
https://react-scan.com/

Narzędzie to pokaże wam, które komponenty będą wymagały poprawy 

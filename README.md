# Poradnik React (2025)


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

count – to aktualna wartość stanu (tutaj: 0).
setCount – to funkcja, która zmienia stan.
useState(0) – 0 to wartość początkowa.

https://react.dev/reference/react/useState

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

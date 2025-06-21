# react-pl
React Pl



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

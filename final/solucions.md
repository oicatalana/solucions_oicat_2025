# Solucions Final oiCat 2025

## Taula de continguts
* [Problema C1. Morra](#C1)
* [Problema Q1. Data quadrada](#Q1)
* [Problema C2. Cromos](#C2)
* [Problema G1. Diana](#G1)
* [Problema C3. Set i mig](#C3)
* [Problema Q2. Puntuacions](#Q2)
* [Problema C4. Obres](#C4)
* [Problema G2. Fractal deformat](#G2)
* [Problema C5. Eles](#C5)
* [Problema C6. Aminoàcids alienígenes](#C6)


## [Problema C1. Morra](https://jutge.org/problems/U54675) <a name="C1"/>

En aquest problema se'ns descriuen les regles del joc de la morra: dos jugadors estenen entre 0 i 5 dits d'una mà, i l'ensenyen a la vegada que diuen un número del 0 al 10. Guanya el joc qui endevina la suma dels dits que han ensenyat entre els dos. 

Sabent el número que ha dit cada jugador, ens demanen calcular la mínima i la màxima suma de dits possible. A primera vista, pot semblar que no hi hagi relació entre el nombre de dits que ensenya el jugador i el número que diu. Cal adonar-se però que si el jugador ensenya $k$ dits, aleshores no té sentit que digui un número menor que $k-5$ (ja que llavors mai l'encertaria, ni que l'altre jugador ensenyés 5 dits). Així doncs, si el primer jugador diu $a$, com a mínim haurà ensenyat $a - 5$ dits. Igualment, si el segon jugador diu $b$, com a mínim haurà ensenyat $b - 5$ dits.

Així doncs, el mínim nombre de dits ensenyats entre els dos jugadors serà 

$$\max(a - 5, \\\,\\\, 0) + \max(b - 5, \\\,\\\, 0)$$ 

Observeu que hem de prendre màxim entre $a - 5$ i $0$ perquè si un jugador diu un número $a < 5$ de dits, això no ens dona cap informació sobre el mínim de dits que ha ensenyat.

Amb un argument semblant podem trobar la màxima suma de dits ensenyats: si el primer jugador diu $a$, aleshores com a molt haurà ensenyat $a$ dits (altrament es passaria sempre, encara que l'altre jugador ensenyés 0 dits). Igualment, si el segon jugador diu $b$, com a molt haurà ensenyat $b$ dits. Per tant, el màxim nombre de dits ensenyats és

$$\min(a, \\\,\\\, 5) + \min(b, \\\,\\\, 5)$$ 

Novament, prenem mínim amb $5$ perquè si $a > 5$ no tenim cap informació sobre el màxim de dits que ha ensenyat el primer jugador (podria haver ensenyat fins a 5 dits). 

<details>
<summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
  int a, b;
  while(cin >> a >> b) {
    int minsuma = max(a-5, 0) + max(b-5, 0);
    int maxsuma = min(a, 5) + min(b, 5);
    cout << minsuma << endl << maxsuma << endl;
  }
}
```
</details>


<details>
  <summary><b>Codi (Python3)</b></summary>

```py
from easyinput import read

a, b = read(int, int)
minsuma = max(a-5, 0) + max(b-5, 0)
maxsuma = min(a, 5) + min(b, 5)
print(minsuma)
print(maxsuma)
```
</details>


## [Problema Q1. Data quadrada](https://jutge.org/problems/P47788) <a name="Q1"/>

En aquest problema se'ns demana trobar el primer dia tal que, tant si s'escriu en format europeu (`DDMMAAAA`) com en format americà (`MMDDAAAA`), dona un quadrat perfecte.

Per resoldre-ho, iterem sobre tots els dies en ordre creixent i, per a cadascun, verifiquem si compleix aquesta condició. Per simplificar el codi, assumim inicialment que tots els mesos tenen 31 dies; més endavant podem comprovar si els dies trobats són vàlids segons el calendari real.

Per saber si un nombre és un quadrat perfecte, podríem fer servir la funció `sqrt(x)`, que calcula l’arrel quadrada de `x`. El problema és que `sqrt(x)` retorna un valor de tipus `float`, i això pot introduir errors d’arrodoniment. Per evitar aquest tipus de problemes, en el codi següent calculem l’arrel quadrada mitjançant una cerca binària.

<details>
  <summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std; 
 
// Retorna true si x és un quadrat perfecte.
// Requereix: 1 <= x < 400'000'000
bool EsQuadrat(int x) {
  int l = 1; // invariant: l*l <= x
  int r = 20000; // invariant: r*r > x
  while(r - l > 1) {
    int m = (l + r) / 2;
    if(m * m > x) r = m;
    else l = m;
  }
  // Al sortir del bucle, es compleix que r == l + 1, i que l*l <= x < r * r,
  // de manera que o bé x == l*l, o bé x no pot ser un quadrat perfecte.
  return l*l == x;
}


int main(){
  for(int a = 2025; a <= 3000; ++a) { // any
    for(int m = 1; m <= 12; ++m) { // mes
      for(int d = 1; d <= 31; ++d) { // dia
        int europeu = d; // data en format europeu
        europeu *= 100;
        europeu += m;
        europeu *= 10000;
        europeu += a;

        int america = m; // data en format americà
        america *= 100;
        america += d;
        america *= 10000;
        america += a;
        
        if(EsQuadrat(europeu) and EsQuadrat(america)) {
          cout << d << " / " << m << " / " << a << endl;
        }
      }
    }
  }
}
```
</details>



## [Problema C2. Cromos](https://jutge.org/problems/X38308) <a name="C2"/> 

En aquest problema ens donen una llista de cromos de diversos tipus (cada tipus ve representat per una lletra de la 'a' a la 'z') i n'hem de triar $n$ per comprar de manera que maximitzem el benefici que obtinguem al revendre'ls, sabent que podem revendre un grup de $m$ cromos del mateix tipus per $m^2$ euros. Quin és el màxim benefici que podem aconseguir?

La clau del problema és adonar-se que $m^2$ creix molt ràpidament en funció de $m$, de manera que sempre serà òptim comprar el màxim nombre de cromos del mateix tipus. Així doncs, la solució consistirà en ordenar els tipus de més a menys cromos, i anar comprant els cromos en aquest ordre fins a haver-ne comprat $n$.

<details><summary><b>Demostració de que aquesta és la tria òptima</b></summary> 

Per demostrar que aquesta solució és la que ens dona el màxim benefici, utilitzarem una idea típica en aquest tipus de problemes: veurem que si tenim una solució que no és la que hem donat anteriorment, podem fer un canvi en aquesta solució que augmenta el benefici obtingut. Així doncs, cap solució diferent de l'anterior pot donar el benefici òptim.

Vegem-ho amb més detall. Suposem que ordenem els tipus de cromos de més a menys cromos disponibles. Sigui $m_i$ el nombre de cromos disponibles del tipus $i$-èssim, i sigui $a_i$ el nombre de cromos que comprem d'aquell tipus. Com que hem ordenat els tipus de més a menys cromos, tenim que $m_i \geq m_{i+1}$ per tot $i$. A més, podem suposar que $a_{i} \geq a_{i+1}$ (com el tipus del cromo no influeix en el preu, podem ordenar els valors de $a_i$ de manera que comprem més cromos dels tipus que més n'hi ha).

El que ens cal demostrar ara és que per tot $i$ es compleix que o bé $a_i = m_i$ (és a dir, estem comprant el màxim de cromos d'aquest tipus) o bé $a_{i+1} = 0$ (és a dir, no comprem cap cromo del tipus següent). Intuïtivament, el que això vol dir és que prioritzem comprar els cromos dels tipus que en tenen més, i que no comprem cromos del tipus següent si no hem esgotat ja tots els dels tipus anteriors.

Per demostrar-ho, suposem que és fals, i que existeix un $i$ tal que $a_i < m_i$ però $a_{i+1} > 0$. Aleshores, veurem que la solució on comprem $a_i + 1$ cromos de tipus $i$, i $a_{i+1}-1$ cromos del tipus $i+1$ ens dona un major benefici, de manera que la solució actual no pot ser la òptima.

Venent $a_i$ cromos de tipus $i$ i $a_{i+1}$ de tipus $i+1$ obtenim un benefici de $a_i^2 + a_{i+1}^2$. Per contra, venent $a_i + 1$ cromos de tipus $i$ i $a_{i+1}-1$ de tipus $i+1$ obtenim un benefici de $(a_{i} + 1)^2 + (a_{i+1}-1)^2$. La diferència entre aquestes dues quantitats és

$$\left( (a_{i} + 1)^2 + (a_{i+1}-1)^2 \right) - \left( a_i^2 + a_{i+1}^2 \right)  = 2a_i + 1 - 2 a_{i+1} + 1 = 2 + 2(a_i - a_{i+1}) \geq 2$$

Observeu que, com $a_i \geq a_{i+1}$, la diferència entre les dues quantitats ens surt positiva. Això vol dir que la solució actual no era la òptima.
</details>

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std; 
 
int main(){
  int n;
  string s;
  while(cin >> n >> s) {
    int const N = 'z' - 'a' + 1; // Mida de l'alfabet.
    vector<int> count(N, 0); // count[i] := nombre de cromos de tipus i. 
    for(char c : s) {
      count[c - 'a']++;
    }
    sort(count.rbegin(), count.rend()); // Ordenem de gran a petit.
    int ans = 0; // Màxim benefici possible.

    // Per maximitzar el benefici, volem comprar dels tipus de cromos que n'hi ha més.
    for(int i = 0; i < N; ++i) {
      int mida = min(count[i], n); // Màxim de cromos que podem comprar del tipus actual.
      n -= mida;
      ans += mida*mida; // Benefici que obtenim venent els cromos d'aquest tipus.
    }
    cout << ans << endl;
  }
}
```
</details>

<details><summary><b>Codi (Python3)</b></summary>

```py
from easyinput import read

n, s = read(int, str)
while n is not None:
  N = ord('z') - ord('a') + 1
  v = [0 for _ in range(N)]
  for c in s:
    v[ord(c) - ord('a')] += 1
  ans = 0
  for x in reversed(sorted(v)):
    inc = min(x, n)
    n -= inc
    ans += inc * inc
  print(ans)
  n, s = read(int, str)
  ```
</details>

## [Problema G1. Diana](https://jutge.org/problems/U35129) <a name="G1"/>

En aquest problema ens demanen dibuixar una diana amb franges de diferents colors. La dificultat és que el color de cada franja dependrà del nombre de tirades que ha rebut, de manera que necessitem comptar quantes tirades han caigut a cada regió. Per fer-ho, calculem la distància de cada tirada al centre de la diana, i dividim per l'amplada de les franges, obtenint l'índex de la franja en la que ha caigut.

Per dibuixar les franges, utilitzem el mètode `dib.ellipse([x1, y1, x2, y2], color)`, que ens dibuixa una el·lipse continguda al rectangle amb cantonada superior esquerra `(x1, y1)` i cantonada inferior dreta `(x2, y2)`. Per a més informació sobre aquesta i altres funcions gràfiques, podeu consultar el següent <a href="https://lliçons.jutge.org/python/grafics/">enllaç</a>. 

<details><summary><b>Codi (Python3)</b></summary>

```py
from PIL import Image, ImageDraw
from easyinput import read
import math

# Calcula la distància entre dos punts
def Dist(p, q):
  dx = p[0] - q[0]
  dy = p[1] - q[1]
  return math.sqrt(dx*dx + dy*dy)


# Llegim l'entrada.
d, f = read(int, int)
franges = [read(int, int, int) for _ in range(f)]
t = read(int)
tirades = [read(int, int) for _ in range(t)]

# Creem la imatge.
img = Image.new('RGB', (d, d), 'White')
dib = ImageDraw.Draw(img)

# Comptem el nombre d'encerts en cada franja.
encerts = [0 for _ in range(f)]
for p in tirades:
  franja = int(Dist(p, (d/2, d/2)) / (d//(2*f)))
  if franja < f:
    encerts[franja] += 1

# Dibuixem les franges de color.
for i in range(f-1, -1, -1):
  color = map(lambda x : x*(1 + encerts[i]), franges[i])
  inici = (f - i - 1)*d//(2*f)
  final = (f + i + 1)*d//(2*f) - 1
  dib.ellipse([inici, inici, final, final], tuple(color))

# Dibuixem les tirades.
for p in tirades:
  dib.point(tuple(p), 'Black')

# Guardem la imatge.
img.save('output.png')
```
</details>

## [Problema C3. Set i mig](https://jutge.org/problems/Z12317) <a name="C3"/>

En aquest problema es defineixen les normes del joc del "Set i mig" i ens demanen determinar el resultat d'una partida, sabent les cartes aparegudes i l'estratègia que segueix cada jugadora. La dificultat del problema consisteix en implementar correctament la simulació de la partida i en identificar quina informació cal mantenir per trobar la guanyadora de cada ronda.

Donat que hi ha cartes que valen mig punt, és recomanable multiplicar els valors de cada carta per 2. Així podem fer tots els càlculs amb nombres enters i no ens hem de preocupar de problemes de precisió en les comparacions (que poden aparèixer si utilitzem variables de tipus `float`).

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;
 
int main(){
  string s;
  while(cin >> s) {
    int total_anna = 0; // partides guanyades per l'Anna
    int total_ivet = 0; // partides guanyades per l'Ivet

    // Variables sobre l'estat de la ronda actual:
    int torn = 0; // 0 -> juga l'Anna, 1 -> juga la Ivet.
    int punts_anna = 0; // Punts actuals de l'Anna.
    int carta_amagada = -1; // Valor de la carta amagada de l'Anna.
                            // Ã‰s -1 si encara no ha estat assignada.
    int punts_ivet = 0; // Punts actuals de la Ivet.
    
    // Neteja els valors de les variables al final d'una ronda.
    function<void()> Reset = [&]() {
      torn = punts_anna = punts_ivet = 0;
      carta_amagada = -1;
    };

    for(char c : s) {
      // Valor de la carta (multiplicat per 2).
      int valor = (c >= '1' and c <= '7' ? 2*int(c - '0') : 1);

      // Actualitzem recompte de punts:
      if(torn == 0) {
        punts_anna += valor;
        if(carta_amagada == -1) 
          carta_amagada = valor;
      }
      else
        punts_ivet += valor;

      // Comprovem si hem acabat el torn:
      if(punts_anna > 15) { // L'Anna s'ha passat de 7 i mig.
        ++total_ivet;
        Reset();
      }
      else if(torn == 0 and punts_anna >= 12) // L'Anna es planta.
        torn = 1;
      else if(punts_ivet > 15) { // La Ivet s'ha passat de 7 i mig.
        ++total_anna;
        Reset();
      }
      else if(torn == 1 and punts_ivet >= 10 and punts_ivet >= punts_anna - carta_amagada + 1) { // La Ivet es planta.
        if(punts_anna > punts_ivet)
          ++total_anna;
        else
          ++total_ivet;
        Reset();
      }
    }
    cout << total_anna << " " << total_ivet << endl;
  }
}
```
</details>

<details><summary><b>Codi (Python3)</b></summary>

```py
from easyinput import read

# Neteja els valors de les variables al final d'una ronda.
def Reset():
  global punts_anna, punts_ivet, torn, carta_amagada
  punts_anna = punts_ivet = torn = 0
  carta_amagada = -1


s = read(str)
while s is not None:
  total_anna = 0 # Partides guanyades per l'Anna.
  total_ivet = 0 # Partides guanyades per la Ivet.

  # Variables sobre l'estat de la ronda actual:
  torn = 0 # 0 -> juga l'Anna, 1 -> juga la Ivet.
  punts_anna = 0 # Punts actuals de l'Anna.
  carta_amagada = -1 # Valor de la carta amagada de l'Anna.
                     # És -1 si encara no ha estat assignada.
  punts_ivet = 0 # Punts actuals de la Ivet.

  for c in s:
    # Valor de la carta (multiplicat per 2).
    valor = (2*int(c) if c.isnumeric() else 1)

    # Actualitzem recompte de punts:
    if torn == 0:
      punts_anna += valor
      if carta_amagada == -1:
        carta_amagada = valor
    else:
      punts_ivet += valor

    # Comprovem si hem acabat el torn:
    if punts_anna > 15:
      total_ivet += 1
      Reset()
    elif punts_ivet > 15:
      total_anna += 1
      Reset()
    elif torn == 0 and punts_anna >= 12:
      torn = 1
    elif torn == 1 and punts_ivet >= 10 and punts_ivet >= punts_anna - carta_amagada + 1:
      if punts_anna > punts_ivet:
        total_anna += 1
      else:
        total_ivet += 1
      Reset()

  print(total_anna, total_ivet, sep=' ')
  s = read(str)
```
</details>

## [Problema Q2. Puntuacions](https://jutge.org/problems/P39082) <a name="Q2"/>

Tenim un concurs amb 7 problemes i volem assignar una puntuació a cadascun de manera que si dos concursants resolen subconjunts de problemes diferents, no puguin empatar a punts. Quina és la manera de fer-ho que minimitza la màxima puntuació assignada?

En altres paraules, necessitem trobar 7 enters positius tals que la suma de cada un dels $2^7$ possibles subconjunts doni un resultat diferent. Una possibilitat és prendre les potències de 2: $\\\{1, 2, 4, 8, 16, 32, 64\\\}$. Sabríeu veure per què funciona?

<details><summary><b>Resposta</b></summary>

> Les potències de 2 compleixen la propietat que cada potència és més gran que la suma de les anteriors (és a dir, $2^k > 2^{k-1} + 2^{k-2} + \dots + 2 + 1$). 

> Suposem que tenim dos subconjunts de problemes $S, T \subseteq \\\{1, 2, 4, 8, 16, 32, 64\\\}$ amb la mateixa suma. Podem assumir que $S \cap T = \emptyset$ (és a dir, $S$ i $T$ no tenen cap element en comú), ja que en cas contrari podríem eliminar aquest element tant de $S$ com de $T$ mantenint la igualtat de les sumes. Així doncs, si no tenen cap element en comú (i no són els dos buits), algun dels dos (diem que $S$) tindrà una potència (diem-li $2^k$) més gran que totes les de l'altre subconjunt. Això implica que la suma dels elements de $S$ és com a mínim $2^k$, i la suma dels elements de $T$ és com a molt $1 + 2 + \dots + 2^{k-1}$. Per la propietat que mencionàvem al principi, les sumes no poden ser iguals.   
</details>

El que potser resulta sorprenent és que aquesta no és la assignació òptima. Per trobar-la, fem un codi que iteri per totes les possibles assignacions (en ordre creixent del màxim element), i que per cada una comprovi si hi ha alguna suma repetida. Per tal de fer aquesta comprovació, necessitem iterar per tots els possibles subconjunts, calcular-ne la suma, i comprovar si aquesta suma ha aparegut abans.

Una manera pràctica d'implementar-ho és codificar cada subconjunt com un vector de zeros i uns de mida 7. Així, per exemple, el subconjunt que conté el 1r i el 3r element seria el `1010000`, i el conjunt que conté el 1r el 2n i el 7è element seria el `1100001`. Aquests vectors es corresponen amb les representacions en binari dels nombres entre 0 (=`0000000`) i $2^7 - 1$ (=`1111111`). Això simplifica el codi, ja que simplement hem d'iterar per tots els nombres enters entre 0 i $2^7 - 1$.

Finalment, per calcular la suma d'un subconjunt, representat com l'expansió binària d'un nombre $x$, per cada $i$ de 0 fins a 6 calculem `x & (1 << i)`. Aquesta expressió és un `AND` de l'expressió binària de $x$ amb l'expressió binària de `1 << i` (que és el nombre que és tot zeros amb un `1` a la posició $i$-èssima). Així doncs `x & (1 << i)` donarà $0$ si $x$ té un `0` a la posició $i$-èssima, i $\neq 0$ si $x$ té un `1` a la posició $i$-èssima. 

Aquest tipus de trucs amb operacions bit a bit (en anglès: <i>bit-wise operations</i>) poden resultar una mica críptics per als principiants, però són molt útils en programació a baix nivell. En aquest <a href="https://codeforces.com/blog/entry/73490" target="_blank" rel="noopener noreferrer">enllaç</a> podeu trobar-ne un petit tutorial.

<details> <summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

// Retorna true si el vector de puntuacions no té cap suma repetida.
bool NoTeSumesRepetides(vector<int> const& puntuacions) {
  int n = puntuacions.size();
  // Calculem la màxima suma de puntuacions.
  int maxima_suma = 0;
  for(int x : puntuacions)
    maxima_suma += x;

  // sumes[x] serà true si ja hem trobat un subconjunt de puntuacions que sumen x.
  vector<bool> sumes(maxima_suma + 1, false);

  // Iterem per tots els subconjunts de puntuacions. Cada subconjunt el codifiquem com un enter entre 0 i 2^n - 1,
  // de manera que l'i-èssim dígit en binari és 1 si l'element 'i' està en el subconjunt, i 0 si no hi està.
  //
  // Per exemple: 0 = 0000000 representa el subconjunt buit, 3 = 0000011 representa el conjunt format pel 1r i el 2n elements.
  for(int mask = 0; mask < (1 << n); ++mask) {
    int suma = 0; // suma dels elements del subconjunt.
    for(int b = 0; b < n; ++b) {
      if(mask & (1<<b)) // és true si el b-èssim dígit en binari de 'mask' és un 1.
        suma += puntuacions[b];
    }
    if(sumes[suma]) // comprovem si ja hem vist aquesta suma abans.
      return false;
    sumes[suma] = true;
  }
  // Si no hem trobat cap suma repetida, retornem true.
  return true;
}

int main() {
  // Iterem per totes les possibles puntuacions, on N és la màxima, 
  // i la resta compleixen que a < b < c < d < e < f < N.  
  for(int N = 1; ; ++N) {
    cerr << "N: " << N << endl;
    for(int f = N - 1; f >= 1; --f) {
      for(int e = f - 1; e >= 1; --e) {
        for(int d = e - 1; d >= 1; --d) {
          for(int c = d - 1; c >= 1; --c) {
            for(int b = c - 1; b >= 1; --b) {
              for(int a = b - 1; a >= 1; --a) {
                vector<int> puntuacions = {a, b, c, d, e, f, N};
                if(NoTeSumesRepetides(puntuacions)) {
                  cout << "RESPOSTA: ";
                  for(int x : puntuacions) 
                    cout << x << "-";
                  cout << endl;
                  return 0;
                }
              }
            }
          }
        }
      }
    }
  }
}
```
</details>

## [Problema C4. Obres](https://jutge.org/problems/V78601) <a name="C4"/>

Tenim unes obres, de les quals mesurem l'estat diàriament mitjançant una puntuació numèrica. Al dia 0 considerem que les obres tenen 0 punts. Cada dia, s'incrementa o disminueix la puntuació de les obres seguint una seqüència donada, que es repeteix cíclicament. Volem trobar el primer dia tal que el valor total mai baixarà de $p$.

Per resoldre aquest problema cal adonar-se de dues idees clau:
- És complicat trobar la resposta simulant el problema "cap endavant" (és a dir, tot i que la puntuació en el dia actual sigui $>= p$, com garantim que més endavant no tornem a baixar de $p$?), però és fàcil simulant-ho "cap enrere" (si sabem que la resposta és com a molt $d$, aleshores anem des del dia $d$ cap a enrere, i si el primer dia on baixem de $p$ és el dia $x$, la resposta serà $x + 1$).
- Tot i que no podem simular el procés dia a dia (amb els valors donats, podria trigar fins a $\sim 10^{10}$ dies a acabar), podem trobar una resposta aproximada (i.e. un dia $d$ tal que garantim que la resposta és com a molt $d$, i que $d$ no està "massa lluny" de la resposta), i simular des d'allà.

Anem ara a explicar amb més detall com funciona la nostra solució. Si calculem la suma de tots els increments al llarg d'un cicle, i li diem $s$, aleshores tenim que la diferència de puntuació entre el dia $d$ i el dia $d + n$ serà de $s$. A l'enunciat ens garanteixen que $s$ és positiva, així que la puntuació a cada dia del cicle anirà augmentant fins que en tots els dies del cicle tenim un valor $\geq p$. Així doncs, podem trobar una fita superior de la resposta amb el procediment següent:
- Trobem el dia $1 \leq d \leq n$ amb la puntuació mínima (és a dir, el dia amb puntuació mínima dins la primera iteració del cicle).
- Calculem quin valor de $i$ necessitem per tal que la puntuació al dia $d + i \cdot n$ sigui $\geq p$ (és a dir, quantes vegades hem d'iterar el cicle per tal que el valor al dia $d$ sigui $\geq p$).

Això ens garanteix que els valors als dies $j + i \cdot n$, per tot $1 \leq j \leq n$ seran tots $\geq p$ (altrament $d$ no seria el dia amb la puntuació mínima de la primera iteració).

Així doncs, sabem que la resposta serà com a molt $i \cdot n$. A més, sabem que la resposta serà com a mínim $(i-1) \cdot n$, ja que altrament $d + (i-1)\cdot n \geq p$. Per tant, simulant el procés cap enrere a partir del dia $i \cdot n$, com a molt trigarem $n$ iteracions en trobar un dia amb puntuació $< p$.

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

using ll = long long;

int main() {
  int p, n; // p := puntuació desitjada; n:= mida del cicle.
  while(cin >> p >> n) {
    vector<int> v(n); // v[i] := increment el i-èssim dia del cicle.
    for(int& x : v)
      cin >> x;
    ll punts_minims = 0; // Minima puntuació en la primera iteració del cicle.
    ll punts_actuals = 0;
    for(int i = 0; i < n; ++i) {
      punts_actuals += v[i];
      punts_minims = min(punts_minims, punts_actuals);
    }
    ll increment_cicle = punts_actuals; // Increment en una iteració del cicle.

    // Sabem que la resposta estarà entre la iteració 'i-1' i la 'i', on 'i' és
    // el valor més petit que compleix que: 
    //
    // punts_minims + increment_cicle * i  >=  p

    // Aïllem 'i' de la fórmula anterior, arrodonint cap amunt:
    ll i = (p - punts_minims + increment_cicle - 1) / increment_cicle; 
    ll dia = i * n; // Dia que acaba la iteració 'i' del cicle.
    punts_actuals *= i; // Puntuació en aquell moment.

    // Iterem cap enrere:
    while(punts_actuals >= p) {
      --dia;
      punts_actuals -= v[dia%n];
    }

    // 'dia' és l'últim dia que tindrà una puntuació inferior a p.
    cout << dia + 1 << endl;
  }
}
```
</details>


## [Problema G2. Fractal deformat](https://jutge.org/problems/V43125) <a name="G2"/>

En aquests problema ens demanen implementar un procediment que genera imatges semblants al Triangle de Sierpinski, però deformades.

La dificultat principal rau en saber enumerar eficientment els subconjunts de mida $\leq k$ de $\\\{1, 2, \dots, n\\\}$, i de calcular-ne la mida de la intersecció.

Igual que en el problema Q2, resultarà útil representar un subconjunt $S \subseteq \\\{1, \dots, n \\\}$ com un vector $v$ de mida $n$ format per zeros i uns, on $v_i = 1$ si $i \in S$. Aquests vectors es corresponen bijectivament amb els nombres de $0$ fins a $2^n - 1$ (pensant-los com la seva expansió binària). Aleshores, els subconjunts de mida $\leq k$ vindran codificats pels nombres $x$ entre $0$ i $2^n - 1$ que tinguin com a molt $k$ uns a la seva expansió binària.

Calcular-ne la intersecció és també senzill amb aquesta representació: es correspon a fer-ne un `AND` bit a bit (ja que el vector corresponent a la intersecció tindrà un 1 en les posicions on els dos vectors originals tenien un 1). Igual que en C++, a Python3 es pot fer el `AND` bit a bit de dos enters $x$ i $y$ mitjançant `x & y`.

<details><summary><b>Codi (Python3)</b></summary>

```py
from PIL import Image, ImageDraw
from easyinput import read

# Retorna el nombre d'uns en l'expansió binària de 'x' 
def Mida(x):
    ans = 0
    while x:
        ans += x%2
        x //= 2
    return ans


n, k = read(int,int)
subconjunts = [x for x in range(2**n) if Mida(x) <= k]
N = len(subconjunts)

img = Image.new('RGB', (N, N), 'Black')
dib = ImageDraw.Draw(img)

for i in range(N):
    for j in range(N):
        m = Mida(subconjunts[i] & subconjunts[j])
        dib.point((i, j), (255 // (m + 1), 0, 0))

img.save('output.png')
```
</details>



## [Problema C5. Eles](https://jutge.org/problems/T99685) <a name="C5"/>

En construcció...

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;
 
int main(){
  int n, m;
  while(cin >> n >> m) {
    // Llegim el tauler:
    vector<string> tauler(n);
    for(string& s : tauler) 
      cin >> s;

    // Retorna true si es pot col·locar una 'L' amb l'extrem superior esquerre a la posició (r, c)
    // (del revés si 'girada' == True).
    function<bool(int,int,bool)> EsPotColocar = [&](int r, int c, bool girada) {
      if(r + 2 >= n or c + 1 >= m or tauler[r][c] == 'X' or tauler[r+2][c+1] == 'X')
        return false;
      if(girada) 
        return tauler[r][c+1] == '.' and tauler[r+1][c+1] == '.';
      return tauler[r+1][c] == '.' and tauler[r+2][c] == '.';
    };

    // Col·loca una 'L' amb l'extrem superior esquerre a la posició (r, c)
    // (del revés, si 'girada' == True).
    function<void(int,int,bool)> Coloca = [&](int r, int c, bool girada) {
      tauler[r][c] = tauler[r+2][c+1] = 'X';
      if(girada) 
        tauler[r][c+1] = tauler[r+1][c+1] = 'X';
      else
        tauler[r+1][c] = tauler[r+2][c] = 'X';
    };

    bool es_possible = true; // True si es pot omplir el tauler amb 'L's.
    for(int i = 0; i < n and es_possible; ++i) {
      for(int j = 0; j < m and es_possible; ++j) {
        if(tauler[i][j] == '.') {
          // Si podem col·locar la 'L' del dret, la col·loquem.
          if(EsPotColocar(i, j, true))
            Coloca(i, j, true);
          // Altrament, mirem de col·locar-la del revés.
          else if(EsPotColocar(i, j, false))
            Coloca(i, j, false);
          else 
            es_possible = false;
        }
      }
    }
    cout << (es_possible? "si" : "no") << endl;
  }
}
```
</details>




## [Problema C6. Aminoàcids alienígenes](https://jutge.org/problems/T50529) <a name="C6"/>

En aquest problema ens demanen comptar el nombre de maneres de dividir una paraula en segments consecutius, sota la restricció que cada segment ha de començar i acabar per una parella de caràcters "vàlida" (és a dir, pertanyent a una certa llista que se'ns dona al principi).

Suposem que llegim la paraula com una string `s`. Per guardar-nos les parelles vàlides, tenim diverses opcions (un `std::set`, un `std::map`, etc.) però el més eficient a l'hora d'accedir-hi és guardar-nos-ho en un vector de vectors de booleans de mida $26 \times 26$, que anomenarem `es_valida`, on tindrem que `es_valida[i][j]` serà `true` si la parella formada per la $i$-èssima i la $j$-èssima lletra és vàlida. Per exemple, `es_valida[0][2]` correspondria a la parella `ac`.  

Com resolem el problema? L'estructura del problema ens suggereix una solució recursiva, on definim $f(i)$ com el nombre de maneres de dividir la paraula fins a la posició $i$-èssima, i llavors calculem cada $f(i)$ en funció dels anteriors mitjançant la fórmula:

$$f(i) = \sum_{j = 0}^{i} f(j-1) \cdot \text{es\\\_valida}[s_j][s_i]$$

És a dir, per dividir la paraula fins a la posició $i$-èssima, iterem per totes les possibles posicions $j$ on pot començar l'últim segment, i sumem $f(j-1)$ (el nombre de maneres de dividir la paraula fins a la posició $(j-1)$-èssima). Tingueu en compte que per simplificar l'expressió estem definint $f(-1) := 1$.

Per evitar calcular de nou valors de $f$ que ja haguem calculat, utilitzem la tècnica de la <a href="https://aprende.olimpiada-informatica.org/algoritmia-dinamica-1">programació dinàmica</a>. El cost total d'aquesta solució seria $\mathcal O(n^2)$ (hem de calcular $\mathcal O(n)$ valors de $f$, i cada un ens costa $\mathcal O(n)$ ).

Pels valors de $n$ que ens dona el problema ($n \leq 10^5$), la solució anterior trigarà massa, així que hem de buscar la manera d'optimitzar-la.

La clau consisteix en adonar-se que podem expressar la fórmula per calcular $f(i)$ com:

$$ f(i) = \sum_{c = \text{'a'}}^{\text{'z'}} \text{acumulat}(c) \cdot \text{es\\\_valida}[c][s_i] $$

on definim $\text{acumulat}(c)$ com la suma de $f(j-1)$ per les $j$ tals que $s_j = c$.

Donat que ara només hem d'iterar pels 26 caràcters diferents (en lloc de per totes les posicions anteriors), això ens redueix la complexitat de la solució a $\mathcal O(26 \cdot n) = \mathcal O(n)$.

Només falta veure que podem mantenir els valors de $\text{acumulat}(c)$ actualitzats eficientment. Però això és senzill: quan processem el caràcter $i$-èssim, afegim $f(i-1)$ a $\text{acumulat}(s_i)$. Simplement aneu amb compte de fer-ho abans de calcular $f(i)$, ja que és vàlid començar i acabar un segment a la posició $i$-èssima).

A continuació us donem una possible implementació. També ho podeu fer amb una funció recursiva si ho preferiu, però recordeu utilitzar programació dinàmica per evitar repetir càlculs!

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    string s;
    while(cin >> s) {
        int k;
        cin >> k;
        int N = 'z' - 'a' + 1;
        vector<vector<bool>> valid(N, vector<bool>(N, false));
        for(int i = 0; i < k; ++i){
            char a, b;
            cin >> a >> b;
            valid[a-'a'][b-'a'] = true;
        }
        int const MOD = 1e8 + 7;
        int const n = s.size();
        vector<int> dp(n, 0); // dp[i] := maneres de dividir s[0..i]
        vector<int> acumulat(N, 0); // acumulat[c] := suma de dp[j-1] per tots els j tals que s[j] == c
        for(int i = 0; i < n; ++i) {
            acumulat[s[i]-'a'] += (i == 0? 1 : dp[i-1]);
            acumulat[s[i]-'a'] %= MOD;
            for(int c = 0; c < N; ++c) {
                if(valid[c][s[i]-'a']) {
                    dp[i] += acumulat[c];
                    dp[i] %= MOD;
                }
            }
        }
        cout << dp[n-1] << endl;
    }
}
```
</details>

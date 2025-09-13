# Solucions Classificatori OICat 2025

## Taula de continguts
* [Problema C1. Només un igual a la suma dels altres](#C1)
* [Problema C2. Sumant en romà](#C2)
* [Problema Q1. Any inusual](#Q1)
* [Problema C3. Xifratge de Vigenère](#C3)
* [Problema G1. Art decimal](#G1)
* [Problema C4. Trobada de religiosos](#C4)
* [Problema Q2. Graf especial](#Q2)
* [Problema G2. Dissenyant estovalles](#G2)
* [Problema C5. Saltant amunt](#C5)
* [Problema C6. Camins màxims en un graf](#C6)
* [Problema C7. Estacions de radar](#C7)


## [Problema C1. Només un igual a la suma dels altres](https://jutge.org/problems/P20891) <a name="C1"/>

En aquest problema ens donen 4 nombres enters i hem de dir si n'hi ha només un que sigui igual a la suma dels altres. Per resoldre-ho de manera senzilla, podem iterar per cada un dels nombres, comprovar si la condició es compleix, i en cas que així sigui incrementar en 1 un comptador. Al final, la resposta serà `sí` si el comptador és igual a 1.

<details>
<summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;
 
int main(){
  vector<int> v(4);
  for(int i = 0; i < 4; ++i)
    cin >> v[i];

  int count = 0;
  for(int i = 0; i < 4; ++i) {
    int suma = 0;
    for(int j = 0; j < 4; ++j) {
      if(i != j) suma += v[j];
    }
    if(suma == v[i]) count++;
  }
  cout << (count == 1 ? "si" : "no") << endl;
}
```
</details>

Per evitar haver de recalcular la suma cada cop, podríem calcular la suma total, i utilitzar que un nombre `x` és igual a la suma dels altres si es compleix que `total - x == x`. En aquest cas això no afecta massa al temps d'execució, perquè només tenim 4 nombres, però si tinguessim $n$ nombres, això faria que el codi passés de fer $\mathcal O(n^2)$ operacions a $\mathcal O(n)$ operacions.
<details>
  <summary><b>Codi alternatiu (C++)</b></summary>
  
  ```cpp
#include<bits/stdc++.h>
using namespace std;
 
int main(){
  vector<int> v(4);
  int suma = 0;
  for(int i = 0; i < 4; ++i) {
    cin >> v[i];
    suma += v[i];
  }

  int count = 0;
  for(int i = 0; i < 4; ++i) {
    if(suma - v[i] == v[i]) count++;
  }
  cout << (count == 1 ? "si" : "no") << endl;
}
  ```
</details>

Per acabar, una solució més concisa en Python:

<details>
  <summary><b>Codi (Python3)</b></summary>

```py
from easyinput import read

v = [read(int) for _ in range(4)]
ans = [sum(v)-x == x for x in v]
print("si" if sum(ans) == 1 else "no")
```
</details>

Observeu que al fer `ans = [sum(v)-x == x for x in v]` estem creant una llista de variables booleanes (és a dir, `True` / `False`) i al fer `sum(ans)` els valors de `True` es converteixen a `1` i els valors de `False` es converteixen a `0`. Així doncs, `sum(ans) == 1` equival a dir que només hi ha un `x` en `v` tal que `sum(v)-x == x`.


## [Problema C2. Sumant en romà](https://jutge.org/problems/P71583) <a name="C2"/>

En aquest problema ens demanen calcular la suma de dos nombres romans. Per resoldre-ho, necessitem implementar una funció que ens passi de nombres romans (donats com una `string`) a nombres "normals" i una altra funció que ens passi de nombres "normals" a nombres romans.

Hi ha moltes maneres de fer-ho. Per exemple, podem implementar la primera amb un `map<string,int>`, i la segona amb un `vector<string>` (on a la posició $n$-èssima hi guardem el nombre romà corresponent a `n`).

<details>
  <summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;
 
int main(){
  vector<string> nums_a_romans = {"?", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX", "X", "XI", "XII", "XIII", "XIV", "XV", "XVI", "XVII", "XVIII", "XIX", "XX"};
  map<string,int> romans_a_nums;
  for(int i = 0; i < int(nums_a_romans.size()); ++i) {
    romans_a_nums[nums_a_romans[i]] = i;
  }

  string a, b;
  while(cin >> a >> b >> b) {
    int suma = romans_a_nums[a] + romans_a_nums[b];
    cout << a << " + " << b << " = " << nums_a_romans[suma] << endl;
  }
}
```
</details>

Observeu que, per no haver d'inicialitzar els elements del mapa d'un en un (i.e. fent `romans_a_nums["I"] = 1`, `romans_a_nums["II"] = 2`, ...), hem fet un bucle on utilitzem `nums_a_romans` per inicialitzar `romans_a_nums`.


## [Problema Q1. Any inusual](https://jutge.org/problems/P80489) <a name="Q1"/>

Per resoldre aquest problema hem d'iterar per tots els nombres majors a 2025 fins a trobar-ne un que compleixi la propietat. Per "partir en dos trossos" un nombre de manera senzilla, el podem convertir a `string` amb la funció `to_string()` i després utilitzar la funció `s.substr()` per seleccionar la part de la `string` que volguem. Al final, per tornar a convertir-ho a un enter, utilitzem la funció `stoi()`. 

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;
 
int main(){
  for(int n = 2025; n <= 10000; ++n) {
    string s = to_string(n);
    int k = s.size();
    for(int i = 1; i < k; ++i) {
      // partim el nombre en 2 trossos:
      // a = s[0..i-1], b = s[i..k-1]
      string a = s.substr(0,i); // des de posició 0, longitud = i
      string b = s.substr(i); // des de posició i, fins al final
      int na = stoi(a);
      int nb = stoi(b);
      if((na + nb)*(na + nb) == n) {
        cout <<"(" << a << " + " << b << ")^2 = " << n << endl;
      } 
    }
  }
}
```
</details>

## [Problema C3. Xifratge de Vigenère](https://jutge.org/problems/P51849) <a name="C3"/>

Aquest problema està basat en un mètode de xifratge real, que es va utilitzar en diferents variants al llarg de l'Edat Moderna i fins i tot a la Guerra Civil Americana (<a href=https://ca.wikipedia.org/wiki/Xifratge_de_Vigen%C3%A8re>enllaç</a>).

Per resoldre'l, va bé saber que C++ ens permet operar amb els caràcters com si fossin enters. Per exemple, `ìnt('c'-'a')` retorna `2`, ja que la lletra `'c'` està 2 posicions per davant de la `'a'`, i de la mateixa manera `char('a' + 2)` ens retorna `'c'`. Així doncs, per xifrar el caràcter `x` amb el caràcter `y` podem fer `y += (x - 'a')`. El problema és que aleshores ens podem passar de llarg (podem acabar amb un caràcter més enllà de la `'z'`), així que hem de restar-li `'a'`, prendre mòdul per la mida de l'alfabet, i tornar-li a sumar `'a'`. Al final obtenim la implementació següent:

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;
 
int main() {
  string s, t;
  while(cin >> s >> t) {
    int n = s.size();
    int m = t.size();
    for(int i = 0; i < m; ++i) {
      if(t[i] == '_') cout << " ";
      else {
        int x = t[i] - 'a' + s[i%n] - 'a';
        x %= 26;
        cout << char('a' + x);
      }
    }
    cout << endl;
  }
}
```
</details>

## [Problema G1. Art decimal](https://jutge.org/problems/P43122) <a name="G1"/>

En aquest problema ens demanen representar els decimals d'una fracció amb diferents colors. La clau és que si treballem amb nombres de coma flotant (és a dir, si calculem `a/b`) no obtindrem un resultat prou precís com per poder resoldre correctament els casos grans. Així doncs, hem de treballar només amb enters, utilitzant l'algorisme habitual de divisió per anar trobant els dígits un a un. 

<details><summary><b>Codi (Python3)</b></summary>

```py
from PIL import Image, ImageDraw
from easyinput import read

m, n = read(int, int)
colors = [read(str) for _ in range(10)]
a, b = read(int, int)

img = Image.new('RGB', (m, n), 'White')
dib = ImageDraw.Draw(img)

for y in range(n):
  for x in range(m):
    a *= 10
    digit = a // b
    a %= b
    dib.point((x, y), colors[digit])

img.save('output.png')
```
</details>

## [Problema C4. Trobada de religiosos](https://jutge.org/problems/P41094) <a name="C4"/>

Per resoldre aquest problema cal adonar-se que tot i que estigui formulat en termes d'una única cua, on els religiosos s'ordenen depenent del seu rang, en realitat això és equivalent a tenir 3 cues diferents, una per cada tipus de religiós.

Així doncs, mantenim una cua per cada un dels 3 tipus de religiós i cada cop que arriba una nova persona l'afegim a la cua que pertoqui. Quan marxa una persona, busquem quina és la primera cua no buida (de major a menor rang) i n'extreiem el primer element.

Per implementar-ho, podem utilitzar l'estructura `std::queue`, que té una operació `pop()`, que extreu el primer element de la cua, i una operació `push()`, que afegeix un element al final de la cua. Aquestes dues operacions triguen $\mathcal O(1)$ cadascuna, així que la complexitat total serà $\mathcal O(n)$.

<details> <summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;
 
int main() {
  char tipus;
  vector<queue<string>> v(3);
  string const rangs = "mbc";
  while(cin >> tipus and tipus != 'f') {
    if(tipus == 's') {
      bool fet = false;
      for(int i = 2; i >= 0 and not fet; --i) {
        if(not v[i].empty()) {
          cout << v[i].front() << endl;
          v[i].pop();
          fet = true;
        }
      }
      if(not fet) cout << "ERROR" << endl;
    }
    else {
      string nom;
      char rang;
      cin >> nom >> rang;
      for(int i = 0; i < 3; ++i) {
        if(rang == rangs[i]) {
          v[i].push(nom);
        }
      }
    }
  }
  cout << string(10, '-') << endl;
  for(int i = 2; i >= 0; --i) {
    while(not v[i].empty()) {
      cout << v[i].front() << endl;
      v[i].pop();
    }
  }
}
```
</details>

Existeix una solució alternativa utilitzant una estructura de dades més complexa que es coneix com a *cua de prioritat* (a C++, `std::priority_queue`). Aquesta estructura té també una operació `push()`, que afegeix un element a la cua, i una operació `pop()`, que extreu l'element més gran que hi hagi a la cua (sense importar l'ordre d'arribada). Per resoldre el nostre problema, podem codificar cada religiós com una parella `{x, nom}`, on `nom` és la string amb el nom del religiós i `x = n * t - i`, on `t` és el tipus del religiós (el de menor rang és un 0, el de major rang un 2) i `i` és la posició d'arribada. Al codificar-ho així, aconseguim que es compleixi que el religiós amb major valor de `x` és el que ha de sortir abans de la cua.

La cua de prioritat triga $\mathcal O(\log n)$ operacions per cada inserció/extracció, així que la complexitat total d'aquesta solució és $\mathcal O(n \log n)$. Això és pitjor que la solució anterior però serà acceptat pel Jutge si s'implementa de manera prou eficient.

## [Problema Q2. Graf especial](https://jutge.org/problems/P78645) <a name="Q2"/>

Un graf amb 7 vèrtexos pot tenir com a molt $\binom{7}{2} = 21$ arestes. Això vol dir que hi ha $2^{21} \cong 2\cdot 10^6$ grafs possibles. Una solució possible és iterar per tots ells i comprovar per cadascun si contenen algun cicle de longitud 6. Depenent de com de bé s'implementi la solució (és a dir, depenent de quant s'aprofitin les simetries del problema per reduir el nombre de grafs a comprovar), el codi pot trigar des d'uns pocs segons fins a uns quants minuts a executar-se. Tanmateix, el problema només ens demana entrar la resposta final, així que podem deixar el programa executant-se en segon pla mentre resolem altres problemes.

A continuació us donem una implementació relativament ràpida:

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n = 7; // nombre de vèrtexs
    int m = n * (n-1) / 2; // nombre màxim d'arestes
    vector<int> best_deg(n, 0); // millor seqüència de graus trobada fins ara
    int best_mask = 0; // graf que assoleix la millor seqüència de graus
    vector<bool> vist(n);
    for(int mask = 0; mask < 1<<m; ++mask) {
        // El bit i-èssim de la representació binària de 'mask' ens diu si la
        // aresta i-èssima està present al graf (ordenant les arestes primer 
        // segons el vèrtex més petit, i després segons el vèrtex més gran).

        // Imprimim el percentatge de grafs que hem comprovat ja.
        if(mask%10000 == 0) 
            cout << mask*100 / (1 << m)  << "\% completat" << endl;

        // Construim el graf.
        vector<vector<int>> G(n); // graf actual
        vector<int> deg(n, 0); // seqüència de graus de G
        int b = 0; // índex de la següent aresta a processar
        bool invalid = false;
        for(int i = 0; i < n and not invalid; ++i) {
            for(int j = i + 1; j < n and not invalid; ++j) {
                if((1 << b)&mask) {
                    // afegim aresta i <-> j
                    G[i].push_back(j);
                    G[j].push_back(i);
                    deg[i]++;
                    deg[j]++;
                }
                ++b;

                // només considerem els grafs amb seqüència de graus decreixent
                // (així ens estalviem de comprovar grafs que, si reordenem els 
                //  vèrtexos, són equivalents a grafs que ja hem vist)
                if(i and deg[i] > deg[i-1]) invalid = true;
            }
        }

        // v := vèrtex que estem processant actualment
        // len := longitud del camí que portem (en nombre d'arestes)
        // inici := vèrtex on hem començat el camí
        function<void(int,int,int)> BuscaCicle = [&](int v, int len, int inici) {
            if(invalid) return;
            if(len == 5) {
                // mirem si podem completar el cicle tornant a l'inici.
                for(int u : G[v]) {
                    if(u == inici) {
                        invalid = true;
                    }
                }
                return;
            }
            vist[v] = true;
            for(int u : G[v]) {
                if(not vist[u]) {
                    // provem d'ampliar el camí anant cap a 'u'
                    BuscaCicle(u, len + 1, inici);
                    
                    // desmarquem 'u' com a visitat, abans de provar el següent vèrtex
                    vist[u] = false;
                }
            }
        };

        // Com que el cicle ha de passar per 6 dels 7 vèrtexos, sabem que ha de contenir per
        // força el vèrtex 0 o el vèrtex 1. 
        fill(vist.begin(), vist.end(), false); // marquem tots els vèrtexos com a no visitats.
        BuscaCicle(0, 0, 0); // Busquem cicles que comencin pel vèrtex 0.
        fill(vist.begin(), vist.end(), false); // marquem tots els vèrtexos com a no visitats.
        BuscaCicle(1, 0, 1); // Busquem cicles que comencin pel vèrtex 1.
        if(not invalid and deg > best_deg) {
            best_deg = deg;
            best_mask = mask;
        }
    }   

    // Imprimim la solució
    cout << endl << endl << " solucio: ";
    for(int x : best_deg) 
        cout << x << " ";
    cout << endl << endl;

    // Imprimim el graf buscat
    cout << "graf:" << endl;
    int b = 0;
    for(int i = 0; i < n; ++i) {
        for(int j = i + 1; j < n; ++j) {
            if((1<<b)&best_mask)
                cout << i << " - " << j << endl;
            ++b;
        }
    }
}
```
</details>

El problema també es podia resoldre raonant-ho per casos, sense cap eina informàtica. Està clar que els dos primers vèrtexos han de tenir grau 6, ja que el graf amb graus `6-6-2-2-2-2-2` no té cap cicle de longitud 6 (sabríeu justificar per què?).

<details><summary><b>Resposta</b></summary>

Suposem que tenim un cicle de longitud 6 en el graf amb graus `6-6-2-2-2-2-2` (és a dir, els vèrtexos 1 i 2 connectats amb tots els altres vèrtexos). Totes les arestes del graf són adjacents al vèrtex 1 o al vèrtex 2. Així doncs, cada 2 vèrtexos consecutius del cicle, almenys un ha de ser el 1 o el 2. Com el cicle té longitud 6, això voldria dir que tres vèrtexos del cicle són el 1 o el 2, així que un dels dos hauria d'estar repetit.
</details>

 Provant d'afegir una aresta addicional, es pot veure que el graf `6-6-3-3-2-2-2` continua sense tenir cap cicle de longitud 6. Per veure que és òptim, cal comprovar que les dues maneres d'afegir una altra aresta (`6-6-4-3-3-2-2` i `6-6-4-4-2-2-2`) contenen cadascuna un cicle de longitud 6, així que no hi ha cap solució millor.

## [Problema G2. Dissenyant estovalles](https://jutge.org/problems/P28763) <a name="G2"/>

Per resoldre el problema, podem guardar-nos una llista amb 3 nombres per cada píxel, representant els valors `(r, g, b)` que portem acumulats. Per cada franja, iterem per tots els píxels que inclou i augmentem els valors de `(r, g, b)` per cadascun d'ells. Al final, prenem el mínim entre el valor que tenim i 255, per tal de no passar-nos del màxim. 

<details><summary><b>Codi (Python3)</b></summary>

```py
from PIL import Image, ImageDraw
from easyinput import read

m, n = read(int, int)
img = Image.new('RGB', (m, n), 'Black')
dib = ImageDraw.Draw(img)

k = read(int)
color = [[[0,0,0] for _ in range(m)] for _ in range(n)]
for _ in range(k):
  tipus, e, d, r, g, b = read(chr, int, int, int, int, int)
  increment = [r, g, b]
  if tipus == 'V':
    for x in range(e, d+1):
      for y in range(n):
        for z in range(3):
          color[y][x][z] += increment[z]
  else:
    for x in range(m):
      for y in range(e, d+1):
        for z in range(3):
          color[y][x][z] += increment[z]

for x in range(m):
  for y in range(n):
    for z in range(3):
      color[y][x][z] = min(color[y][x][z], 255)
    dib.point((x, y), tuple(color[y][x]))

img.save('output.png')
```
</details>

Tot i que la implementació anterior ja dona AC, podem resoldre el problema de manera més eficient si en lloc d'iterar per tots els píxels que conté cada franja, iterem per les coordenades verticals i/o horitzontals, i només al final (quan estem pintant els píxels), calculem la suma entre les aportacions de les franges verticals i horitzontals. Això redueix la complexitat asimptòtica de $\mathcal O(f \cdot m \cdot n)$ a $\mathcal O(f \cdot m + f \cdot n + m \cdot n)$.

<details><summary><b>Codi millorat (Python3)</b></summary>

```py
from PIL import Image, ImageDraw
from easyinput import read

m, n = read(int, int)
img = Image.new('RGB', (m, n), 'Black')
dib = ImageDraw.Draw(img)

k = read(int)
color_vertical = [[0, 0, 0] for _ in range(m)]
color_horitzontal = [[0, 0, 0] for _ in range(n)]
for _ in range(k):
  tipus, e, d, r, g, b = read(chr, int, int, int, int, int)
  color = color_vertical if tipus == 'V' else color_horitzontal
  for x in range(e, d+1):
    color[x] = [sum(z) for z in zip(color[x], [r, g, b])]

for x in range(m):
  for y in range(n):
    color = map(lambda z : min(z, 255), map(sum, zip(color_vertical[x], color_horitzontal[y])))
    dib.point((x, y), tuple(color))


img.save('output.png')
```
</details>

Encara es pot reduir més la complexitat fins a $\mathcal O(f + m \cdot n)$. Sabríeu com fer-ho?

<details><summary><b>Pista</b></summary>
Proveu de plantejar el problema a partir de l'array de diferències.
</details>

## [Problema C5. Saltant amunt](https://jutge.org/problems/P57741) <a name="C5"/>

Sigui $f(i)$ el mínim cost d'arribar a l'esglaó $i$-èssim. Si sempre poguéssim fer salts de longitud entre 1 i $s$, aleshores podríem calcular $f(i)$ a partir dels anteriors, fent
$$f(i) = c(i) + \min \left\\\{ f(i-1), f(i-2), \dots, f(i-s) \right\\\}$$,
on $c(i)$ és el cost de trepitjar l'esglaó $i$-èssim.

El problema és que si l'últim pas ha estat de longitud $k$, aleshores el següent només pot ser com a màxim de longitud $s + 1 - k$. Per tant, per calcular els $f(i)$ necessitem controlar d'alguna manera quina és la longitud de l'últim pas que hem fet. La manera típica de fer-ho és afegir una variable extra: ara tindrem una funció $f(i, k)$ que ens donarà el mínim cost d'arribar a l'esglaó $i$-èssim, on l'últim pas que hem fet és de longitud $k$. Aleshores:

$$f(i,k) = c(i) + \min \left\\\{ f(i-k, 1), f(i-k, 2), \dots, f(i-k, s + 1 - k) \right\\\}$$

Un cop calculats els valors de $f(i,k)$ per tot $i$ i per tot $k$, la solució és fàcil de trobar. Podem afegir un esglaó de cost 0 al final, de manera que la resposta és

$$ \min \left\\\{ f(n, 1), f(n, 2), \dots, f(n, s) \right\\\}$$

Observeu que hem de prendre el mínim entre tots els possibles valors de $k$, ja que no sabem quina serà la longitud òptima per a l'últim salt.

Si proveu d'implementar el càlcul de $f(i,k)$ amb una funció recursiva, veureu que el programa triga molt a executar-se. El que passa és que per calcular $f(i,k)$ necessitem calcular fins a $s$ altres valors de $f$, de manera que el nombre total de crides a la funció creix exponencialment a mesura que augmenta $n$ (el nombre d'esglaons).

Per resoldre-ho, utilitzem la tècnica coneguda com a <a href="https://aprende.olimpiada-informatica.org/algoritmia-dinamica-1">programació dinàmica</a>. La idea és anar-nos guardant els valors de $f(i,k)$ que calculem per tal que el proper cop que volem calcular el mateix $f(i,k)$, poguem retornar directament el valor que tenim guardat. Així ens garantim que no farem més de $n \cdot s$ crides a la funció que calcula $f$ (una per cada parella de $ (i,k) $).

En el codi següent ho hem implementat amb una matriu $dp$ de mida $n \times s$, que al principi té tots els valors inicialitzats a -1. Quan calculem $f(i,k)$, ens guardem el resultat a $dp[i][k]$. Així doncs, el proper cop que volguem calcular el mateix $f(i,k)$, accedirem a $dp[i][k]$, veurem que és diferent de -1, i retornarem directament el valor que tenim guardat.

Amb aquest mètode la complexitat total és $\mathcal O(n \cdot s^2)$, ja que hem de calcular $n \cdot s$ valors de $f$, i cadascun ens requereix fer $\mathcal O(s)$ operacions. 

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std; 

int n, s; // nombre d'esglaons i màxima longitud de salt.
vector<int> cost; // cost[i] := cost que hem de pagar per trepitjar l'esglaó i-èssim
vector<vector<int>> dp; // dp[i][j] := minim cost per arribar a l'esglaó i-èssim, 
                        // on a l'últim pas hem saltat j esglaons.
                        // Si dp[i][j] == -1, vol dir que encara no ho hem calculat.
int const INF = 1e9;

// Calcula dp[i][salt].
// Si ja l'hem calculat abans, en retorna el valor directament, per evitar repetir
// càlculs innecessàriament.
int GetDP(int i, int salt) {
    if(dp[i][salt] != -1) return dp[i][salt]; // si ja l'hem calculat anteriorment.

    // Cas base de la recursió (quan estem en el primer esglaó). 
    if(i == 0)
      return cost[0];

    // És impossible que l'últim salt sigui més gran que el nombre d'esglaó on ens trobem. 
    if(i < salt)
      return INF;

    // Calculem dp[i][salt]:
    dp[i][salt] = INF;
    for(int salt_previ = 1; salt_previ <= s + 1 - salt; ++salt_previ) {
        dp[i][salt] = min(dp[i][salt], cost[i] + GetDP(i - salt, salt_previ));
    }
    return dp[i][salt];
}


int main(){
  while(cin >> s >> n) {
    cost = vector<int>(n);
    for(int& x : cost) 
      cin >> x;
    cost.push_back(0); // Afegim un esglaó extra al final de cost 0, per simplificar el càlcul de la DP.

    dp = vector<vector<int>>(n+1, vector<int>(s+1, -1));

    int ans = INF;
    for(int salt = 1; salt <= s; ++salt)
      ans = min(ans, GetDP(n, salt));
    cout << ans << endl;
  }
}
```
</details>

En els problemes de programació dinàmica, sovint (però no sempre!) és més ràpid implementar la solució sense fer servir funcions recursives, sinó amb un bucle que itera sobre totes les parelles $(i,k)$ i calcula $f(i,k)$. Per tal de fer-ho així, necessitem iterar en algun ordre que ens garanteixi haver calculat anteriorment tots els valors de $f$ que necessitem per obtenir el valor actual. En aquest cas, per calcular $f(i,k)$ només necessitem els valors de $f(i-k, x)$, amb $1 \leq x \leq s + 1 - k$, així que podem iterar en ordre creixent de $i$. 

<details><summary><b>Codi iteratiu (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std; 
 
int main(){
  int s, n;
  while(cin >> s >> n) {
    vector<int> cost(n);
    for(int& x : cost) 
      cin >> x;
    cost.push_back(0); // Afegim un esglaó extra al final de cost 0, per simplificar el càlcul de la DP.

    int const INF = 1e9;
    // dp[i][j] := minim cost per arribar a l'esglaó i-èssim, on a l'últim pas hem saltat j esglaons.
    vector<vector<int>> dp(n+1, vector<int>(s+1, INF));
    // Podem suposar que al principi venim d'un salt de mida 1, ja que llavors al proper podem saltar
    // qualsevol longitud des de 1 fins a s.
    dp[0][1] = cost[0];
    for(int i = 1; i <= n; ++i) {
      for(int last = 1; last <= s; ++last) {
        for(int salt = 1; salt <= min(i, s + 1 - last); ++salt) {
          dp[i][salt] = min(dp[i][salt], dp[i - salt][last] + cost[i]);
        }
      }
    }
    int ans = INF;
    for(int salt = 1; salt <= s; ++salt)
      ans = min(ans, dp[n][salt]);
    cout << ans << endl;
  }
}
```
</details>

<details><summary><b>Repte</b></summary>
Sabries optimitzar la solució anterior per tal que la complexitat total sigui $\mathcal O(n \cdot s)$?
</details>


## [Problema C6. Camins màxims en un graf](https://jutge.org/problems/P51388) <a name="C6"/>

Trobar el camí més llarg en un graf és un problema NP-complet (tècnicament, estem parlant de la versió decisional del problema, que consisteix en dir si existeix un camí de mida $\geq k$ donat un enter $k$ (<a href="https://en.wikipedia.org/wiki/Longest_path_problem">vegeu</a>)). Això vol dir que si es trobés un algorisme polinòmic que resolgui aquest problema, es podria resoldre també qualsevol altre problema de NP en temps polinòmic. Com que actualment es creu que P $\neq$ NP (encara que no està demostrat), això vol dir que no és probable que el problema del camí més llarg es pugui resoldre en temps polinòmic. 

Ara bé, en aquest problema ens demanen trobar els camins més llargs des de cada vèrtex en un graf *sense cicles*. Això simplifica el problema enormement, ja que en un graf sense cicles sempre hi ha com a molt un camí entre cada parell de vèrtexos (com a exercici, convenceu-vos que si hi hagués dos camins diferents entre un cert parell de vèrtexos, això implicaria que el graf ha de contenir un cicle).

Així doncs, una possible solució és la següent: per cada vèrtex, calculem la distància a la resta de vèrtexos amb un BFS, i ens quedem amb la distància més gran. Això funciona perquè el camí més llarg entre dos vèrtexos és també el camí més curt entre els dos vèrtexos (per això de que hi ha un únic camí entre cada parell de vèrtexos), i això és precisament el que podem calcular amb el BFS.

La complexitat de la solució és $\mathcal O(n^2)$, ja que per cada un dels $n$ vèrtexos hem de fer un BFS de cost $\mathcal O(n)$. Això hauria de ser suficient per obtenir la puntuació parcial.
<details><summary><b>Codi - Solució parcial (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n, m;
    while(cin >> n >> m) {
        vector<vector<int>> G(n);
        while(m--) {
            int a, b;
            cin >> a >> b;
            G[a].push_back(b);
            G[b].push_back(a);
        }
        vector<bool> vist(n);

        function<int(int)> DFS = [&](int v) {
            vist[v] = true;
            int ans = 0;
            for(int u : G[v]) {
                if(not vist[u]) {
                    ans = max(ans, 1 + DFS(u));
                }
            }
            return ans;
        };

        for(int v = 0; v < n; ++v) {
            fill(vist.begin(), vist.end(), false);
            int ans = DFS(v);
            cout << ans;
            cout << (v == n-1? '\n' : ' ');
        }
    }
}
```
</details>

Observeu que en el codi anterior utilitzem un DFS en lloc d'un BFS. En un graf general, per calcular la mínima distància entre un vèrtex i la resta hauríem d'utilitzar un BFS. En un graf sense cicles, però, ho podem fer tant amb un BFS com amb un DFS, ja que l'ordre en que visitem els vèrtexos no importa (novament, degut al fet que hi ha un únic camí entre cada parell de vèrtexos).

Per rebre la puntuació completa cal millorar l'algorisme fins a $\mathcal O(n)$. Una possible manera és la següent:

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

int main(){
    cin.tie(NULL);
    ios_base::sync_with_stdio(false);
    int n, m;
    while(cin >> n >> m) {
        vector<vector<int>> G(n);
        while(m--) {
            int u, v;
            cin >> u >> v;
            G[u].push_back(v);
            G[v].push_back(u);
        }

        // dist_fill[v] := distància més gran que podem aconseguir anant cap a un fill de v
        vector<int> dist_fill(n, 0); 
        
        // millor_fill[v] := fill que assoleix dist_fill[v]         
        vector<int> millor_fill(n, -1); 
        
        // segon_dist_fill[v] := distància més gran que podem aconseguir anant cap a un fill
        //                       de v diferent de millor_fill[v]
        vector<int> segon_dist_fill(n, 0);            
        vector<int> pare(n, -1); // pare[v] := pare de v en l'arbre
        vector<bool> vist(n, false); // vist[v] := true si ja hem visitat el vèrtex v

        // Calcula els valors de dist_fill[v] i segon_dist_fill[v]
        function<void(int)> DFS_Down = [&](int v) {
            vist[v] = true;
            for(int u : G[v]) {
                if(u == pare[v]) continue;
                pare[u] = v;
                DFS_Down(u);
                if(dist_fill[v] < dist_fill[u] + 1) {
                    segon_dist_fill[v] = dist_fill[v];
                    dist_fill[v] = dist_fill[u] + 1;
                    millor_fill[v] = u;
                }
                else if(segon_dist_fill[v] < dist_fill[u] + 1) {
                    segon_dist_fill[v] = dist_fill[u] + 1;
                }
            }
        };

        for(int i = 0; i < n; ++i) {
            if(not vist[i])
                DFS_Down(i);
        }

        // dist_pare[v] := distància més gran que podem aconseguir anant cap al pare de v
        vector<int> dist_pare(n, 0); 

        // Calcula el valor de dist_pare[v]
        function<void(int)> DFS_Up = [&](int v) {
            vist[v] = true;
            if(pare[v] == -1) return;
            if(not vist[pare[v]])
                DFS_Up(pare[v]);
            dist_pare[v] = 1 + max(dist_pare[pare[v]], (millor_fill[pare[v]] == v? segon_dist_fill[pare[v]] : dist_fill[pare[v]]));
        };

        vist = vector<bool>(n, false);
        for(int i = 0; i < n; ++i) {
            if(not vist[i])
                DFS_Up(i);
        }
        for(int i = 0; i < n; ++i) {
            if(i) cout << ' ';
            cout << max(dist_fill[i], dist_pare[i]);
        }
        cout << endl;
    }
}
```
</details>

Per entendre el que fa aquest codi, cal primer una mica de terminologia sobre grafs. Un graf connex i sense cicles és el que es coneix com a un *arbre*. En els arbres generalment triem un vèrtex especial que anomenem *arrel*, i ordenem la resta de vèrtexos per nivells segons la seva distància a l'arrel. Cada vèrtex diferent de l'arrel està connectat a un únic vèrtex de nivell inferior (el seu *pare*), i a zero o més vèrtexos de nivell superior (els seus *fills*). Si considerem un graf sense cicles però traiem la condició que hagi de ser connex, obtenim una unió d'arbres disjunts, que s'anomena (per raons òbvies) *bosc*.

En aquest problema ens donen un bosc i hem de trobar la longitud del camí més llarg que surt de cada vèrtex. Òbviament, podem tractar cada un dels arbres que forma el bosc per separat, ja que els camins no poden creuar d'un arbre a un altre. Donat un arbre, triarem arbitràriament un vèrtex com a arrel, i ordenarem la resta de vèrtexos per nivells.

Observeu que el camí més llarg des d'un vèrtex pot ser de dos tipus: o bé va cap a un fill del vèrtex, després cap a un fill del fill, i així successivament, fins a arribar a un vèrtex sense fills (el que es coneix com a *fulla*); o bé va cap al pare del vèrtex (i des d'allà pot o bé anar a un fill del pare diferent del propi vèrtex, o continuar enrere cap al pare del pare).

La longitud dels camins del primer tipus és fàcil de calcular. Per cada vèrtex $v$, definim $\text{DistFill}(v)$ com la màxima longitud d'un camí que comenci a $v$ i vagi fins a una fulla. Si $v$ és una fulla, sabem que $\text{DistFill}(v) = 0$. Aleshores, processant la resta de vèrtexos de major a menor distància a l'arrel, tenim que

$$
\text{DistFill}(v) = 1 + \max\limits_{u \text{ fill de } v} \text{DistFill}(u)
$$

Un cop hem calculat $\text{DistFill}(v)$ per cada vèrtex $v$, procedim a calcular $\text{DistPare}(v)$, que és la màxima longitud d'un camí que comenci a $v$ i vagi cap al pare de $v$ (i des d'allà, cap a on sigui). A primera vista, se'ns pot acudir calcular-ho com en el cas anterior, usant que $\text{DistPare}(v) = 0$ si $v$ és l'arrel, i processant la resta de vèrtexos de menor a major distància a l'arrel, fent

$$
\text{DistPare}(v) = 1 + \max \left\\\{ \text{DistFill}(\text{pare}(v)), \text{DistPare}(\text{pare}(v)) \right\\\}
$$

El problema d'això és que si el camí més llarg des del pare de $v$ fins a una fulla passa per $v$, aleshores quan calculem $\text{DistPare}(v)$ estem anant de $v$ al pare, i llavors tornant a baixar cap a $v$. Això no ho volem permetre, ja que el camí estaria repetint vèrtexos.

Per resoldre-ho, en el primer pas no només calculem $\text{DistFill}(v)$ (la major distància des de $v$ fins a una fulla), sinó que també ens guardem $\text{MillorFill}(v)$ (per quin fill de $v$ passa aquest camí), i $\text{SegonDistFill}(v)$, quina és la major distància des de $v$ a una fulla sense passar per $\text{MillorFill}(v)$.

Aleshores, per calcular $\text{DistPare}(v)$, comprovem si $\text{MillorFill}(\text{pare}(v)) == v$, i en cas que així sigui utilitzem $\text{SegonDistFill}(\text{pare}(v))$ en lloc de $\text{DistFill}(\text{pare}(v))$ per fer els càlculs.

Per acabar, el camí més llarg des de cada vèrtex $v$ serà el màxim entre $\text{DistFill}(v)$ i $\text{DistPare}(v)$.

<details><summary><b>Repte</b></summary>
Existeix una solució alternativa basada en trobar el diàmetre de cada arbre. Sabríeu com fer-ho? 
</details>

## [Problema C7. Estacions de radar](https://jutge.org/problems/P45007) <a name="C7"/>

En aquest problema ens donen fins a 1000 punts amb coordenades enteres en el pla, i ens demanen comptar el nombre de triangles acutangles (és a dir, amb els tres angles menors que 90 graus) que podem formar amb ells.

Una solució senzilla seria iterar per tots els possibles trios de punts i comprovar per cadascun si forma un triangle acutangle. Això és massa lent, ja que és $\mathcal O(n^3)$. 

Per millorar-ho, observem primer que per comptar el nombre de triangles acutangles, en tenim prou amb comptar el nombre d'angles aguts. Això és degut a que cada triangle acutangle tindrà 3 angles aguts, mentre que cada triangle no acutangle tindrà 2 angles aguts (ja que només pot tenir un angle $\geq 90$ graus). Així doncs, donat que el nombre total de triangles és $\binom{n}{3} = n(n-1)(n-2)/6$, tenim que

```math
\begin{align*}
\text{nombre d'angles aguts} &= 3 \cdot \text{nombre de triangles acutangles} + 2 \cdot \text{nombre de triangles no acutangles} \\
&= 3 \cdot \text{nombre de triangles acutangles} + 2 \cdot (\text{nombre total de triangles} -\text{nombre de triangles acutangles}) \\
&= \text{nombre de triangles acutangles} + 2\binom{n}{3}
\end{align*}
```

de manera que

$$ 
\text{nombre de triangles acutangles} = \text{nombre d'angles aguts} - 2\binom{n}{3}
$$

Per calcular el nombre d'angles aguts, fixem un punt $P$ (el vèrtex central de l'angle) i ordenem la resta de punts angularment al seu voltant. Per cada altre punt $Q$, volem trobar el nombre de punts $R$ tals que l'angle $QPR$ és agut. Una manera trivial de fer-ho seria començar amb $R \gets Q$ i mentre que l'angle $QPR$ és agut, avançar $R$ una posició (en el vector de punts ordenats per angle). Un cop arribats a un $QPR$ que no és agut, restem l'índex de $R$ i l'índex de $Q$ en el vector, i això ens donarà el nombre de punts que formen un angle agut juntament amb $P$ i $Q$.

Per evitar haver de recórrer tots els punts cada cop, podem utilitzar que si ja hem calculat el $R$ per un cert $Q$, aleshores pel següent punt $Q'$, el $R'$ corresponent no es pot trobar abans que $R$. Així doncs, en lloc de tornar a començar des de $Q'$, en tenim prou amb comprovar a partir de $R$. Aquesta tècnica es coneix habitualment com a <a href="https://usaco.guide/silver/two-pointers?lang=cpp">two-pointers method</a>.

Per a la implementació, és recomanable fer totes les operacions amb enters (anant amb compte de fer servir `long long` per les multiplicacions), en lloc de nombres de coma flotant. Així s'eviten errors de precisió i es redueix el factor constant de la complexitat asimptòtica, que és de $\mathcal O(n^2)$.

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

using ll = long long;

struct Punt {
    int x, y;
    Punt operator-(Punt const& other) const {
        return Punt{x - other.x, y - other.y};
    }
    Punt Rota90Horari() const {
        return Punt{y, -x};
    }
};

ll CrossProduct(Punt const& p, Punt const& q) {
    return ll(p.x) * q.y - ll(p.y) * q.x;
}

// Retorna true si Angle(p) es troba entre [0, 180) graus.
// (En cas que p == (0,0), retorna true).
bool Up (Punt p) {
  return p.y > 0 or (p.y == 0 and p.x >= 0);
}

// Retorna true si Angle(a) < Angle(b), ordenant els angles en l'interval [-180, 180).
bool Compara(Punt a, Punt b){
    if (Up(a) == Up(b)) 
        return CrossProduct(a, b) > 0;
    return Up(a) < Up(b);    
}

// Retorna true si l'angle des de p fins a q en sentit antihorari és menor de 90 graus.
bool AngleMenysDe90(Punt const& p, Punt const& q) {
    return CrossProduct(p, q) > 0 and CrossProduct(q.Rota90Horari(), p) > 0;
}

int main() {
    int n;
    while(cin >> n) {
        vector<Punt> points(n);
        for (int i = 0; i < n; ++i) 
            cin >> points[i].x >> points[i].y;

        ll total_triangles = ll(n) * (n - 1) * (n - 2) / 6;
        ll angles_bons = 0; // nombre de angles < 90 graus

        for (int i = 0; i < n; ++i){
            vector<Punt> v; // vectors entre el punt i-èssim i la resta de punts.
            for (int j = 0; j < n; ++j){
                if (i != j) 
                    v.push_back(points[j] - points[i]);
            }
            sort(v.begin(), v.end(), Compara);  // ordenem els vectors per angle.

            int m = v.size();
            int k = 1; // primer index tal que l'angle entre v[j] i v[k] es >= 90 graus.
            for(int j = 0; j < m; ++j) {
                k = max(k, j + 1);
                while(k%m != j and AngleMenysDe90(v[j], v[k%m])) {
                    ++k;
                }
                angles_bons += k - j - 1;
            }
        }
        // angles_bons = 3 * triangles_acutangles + 2 * (total_triangles - triangles_acutangles)
        // ==>  triangles_acutangles = angles_bons - 2 * total_triangles
        cout << angles_bons - 2 * total_triangles << endl;
    }
}
```
</details>

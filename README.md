# Sistema de Targeting Espacial

Projecte de programació en C++ que simula un sistema de targeting assistit per a un videojoc de combats espacials. El programa utilitza punters i memòria dinàmica per gestionar enemics i identificar l'objectiu més proper al jugador.

## Descripció del Projecte

Aquest programa implementa un mòdul de "targeting" que permet:
- Generar enemics de forma dinàmica amb posicions i característiques aleatòries
- Identificar automàticament l'enemic més proper al jugador
- Aplicar dany a l'enemic seleccionat fins a la seva destrucció
- Gestionar memòria dinàmica de forma segura

El projecte fa ús intensiu de:
- Estructures de dades (`struct`)
- Punters i pas de paràmetres per referència
- Memòria dinàmica (`new` i `delete[]`)
- Càlculs matemàtics (distància euclidiana)

## Explicació del Sistema de Targeting

### Funcionament
1. **Generació d'enemics**: Es creen enemics amb coordenades aleatòries en un espai 2D (rang: -100 a 100 en ambdós eixos)
2. **Posició del jugador**: El jugador sempre es troba a les coordenades (0, 0)
3. **Càlcul de distàncies**: S'utilitza la fórmula de distància euclidiana:
```
   distància = √((x₂-x₁)² + (y₂-y₁)²)
```
4. **Selecció de target**: Es retorna un punter a l'enemic amb la distància mínima
5. **Aplicació de dany**: S'ataca l'enemic seleccionat fins a reduir els seus HP a 0

### Estructura de Dades
```cpp
struct plantillaNau {
    int id;        // Identificador únic
    float x, y;    // Coordenades en l'espai
    int hp;        // Punts de vida (30-80)
};
```

### Funcions Principals
- `plantillaNau* trobarEnemic(plantillaNau* enemiesAUX, int totalEnemics)`: Retorna un punter a l'enemic més proper
- `void dany(plantillaNau* en)`: Aplica dany a un enemic fins a destruir-lo

## Com Compilar i Provar

### Requisits
- Compilador de C++ (g++, clang++, etc.)
- Sistema operatiu: Windows, Linux o macOS

### Compilació

**Linux/macOS:**
```bash
g++ -o targeting main.cpp
./targeting
```

**Windows:**
```bash
g++ -o targeting.exe main.cpp
targeting.exe
```

### Execució
1. El programa demanarà quants enemics vols generar (mínim 3)
2. Es generaran enemics amb posicions i HP aleatoris
3. El sistema identificarà l'enemic més proper
4. S'aplicarà dany fins a destruir l'enemic

## Exemple de Sortida
```
Quants enemics vols generar? (Minim ha de haver-hi 3)
El total de enemics ara meteix es: 0
5
Generant 5 enemics

Enemics generats: 5
[Targeting System Activated]
Enemy més proper → ID 3 (addr: 0x7ffeefbff5a0)
Distància: 5.29

Aplicant dany...
Enemy 3 HP: 45 → 25
Enemy 3 HP: 25 → 5
Enemy 3 HP: 5 → 0
Enemy destroyed!
```

### Generació d'Enemics
```
Quants enemics vols generar? (Minim ha de haver-hi 3)
El total de enemics ara meteix es: 0
2
El total de enemics ara meteix es: 2
2
Generant 4 enemics
```

### Sistema de Targeting
```
[Targeting System Activated]
Enemy més proper → ID 1 (addr: 0x600003d8c020)
Distància: 12.65
```

### Aplicació de Dany
```
Aplicant dany...
Enemy 1 HP: 67 → 47
Enemy 1 HP: 47 → 27
Enemy 1 HP: 27 → 7
Enemy 1 HP: 7 → 0
Enemy destroyed!
```

## Dificultats i Resolució

### Dificultats Trobades

1. **Gestió de punters vs valors**
   - **Problema**: Inicialment la funció `dany` rebia l'enemic per valor, per tant els canvis en HP no es reflectien a l'array original
   - **Solució**: Modificar la funció per rebre un punter (`plantillaNau*`) i accedir als membres amb l'operador `->`

2. **Càlcul de distàncies**
   - **Problema**: Confusió amb les funcions `pow()` i `powf()` per a float vs double
   - **Solució**: Utilitzar `pow()` de forma consistent i assegurar conversions de tipus correctes

3. **Alliberament de memòria**
   - **Problema**: Risc de memory leaks si no s'allibera la memòria reservada dinàmicament
   - **Solució**: Afegir `delete[] enemies` al final del `main()` abans de retornar

4. **Validació d'entrada**
   - **Problema**: L'usuari podria introduir menys de 3 enemics
   - **Solució**: Implementar un bucle `while` que continua demanant enemics fins arribar al mínim de 3

5. **Retorn de punters**
   - **Problema**: Comprendre la diferència entre retornar un ID (int) i retornar un punter a l'enemic
   - **Solució**: Canviar el tipus de retorn de `trobarEnemic` de `int` a `plantillaNau*` i utilitzar l'operador `&` per obtenir l'adreça

### Conceptes Clau Apresos

- **Punters**: Permeten accedir i modificar dades directament en memòria
- **Memòria dinàmica**: Necessària quan el nombre d'elements no es coneix en temps de compilació
- **Operador `->` vs `.`**: `->` s'utilitza amb punters, `.` amb variables directes
- **Pas per referència**: Permet que les funcions modifiquin les dades originals

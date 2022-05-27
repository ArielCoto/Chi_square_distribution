# Chi_square_distribution
Programa que genera números pseudoaleatorios con distribución χ2  partiendo de números pseudoaleatorios con distribución uniforme en el intervalo (0,1). 

#### 1. Realice un programa que genere números pseudoaleatorios con distribución χ2 (distribución Ji-cuadrada con 2 grados de libertad) partiendo de números pseudoaleatorios con distribución uniforme en el intervalo (0,1). Haga un histograma y compárelo con la función de densidad de una variable aleatoria con densidad Ji-cuadrada con 2 grados de libertad.

```
from math import log, e, sqrt, sin, cos, pi
from random import uniform
from scipy.stats import chi2
import matplotlib.pyplot as plt
import numpy as np
import random
```
### 1. Primero generaremos nuestras variables a transformar.

Si $U_{1}$, $U_{2}$ son v.a.i.i.d con distribución Unif(0,1), entonces

$$X=\sqrt{-2logU_{1}cos(2\pi U_{2})}$$

y

$$Y=\sqrt{-2logU_{1}sin(2\pi U_{2})}$$

son v.a.i.d.d con distribución $N(0,1)$

##### Realizando la transformación 

Números pseudoaleatorios con distribución uniforme en el intervalo (0,1)
 Si $U_{1}$, $U_{2}$ son v.a.i.i.d con distribución unif entonces
 
```
def phi(u1,u2):
    aux=sqrt(-2 * log(u1))
    
    return aux * cos(2*pi*u2), aux*sin(2*pi*u2)
```
 ```
u1,u2=0.1, 0.7
x,y=phi(u1,u2)

print(x,y)
  ```
  
  
```
  def generator(N=100000):
    x_values=[]
    y_values=[]
    
    for i in range(N):
        u1=uniform(0,1)
        u2=uniform(0,1)
        x,y=phi(u1,u2)
        x_values.append(x)
        y_values.append(y)
    
    return x_values, y_values
    
```
     
```
     x_values, y_values=generator()
plt.hist(x_values) # Verificamos...Obtenemos las distribuciones uniformes
    
```
    
### 2. De acuerdo con el siguiente lema generaremos la distribución χ2 con dos grados de libertad

Si $Z_{1},...,Z_{n}$ es una muestra aleatoria de una población normal estándar, entonces:

$$ \sum_{i=1}^{n}Z_{i}^{2} \sim \chi_{(n)}^{2}$$

Observemos que en paso $\textbf{1}$ , generamos una población aleatoria normal estandar, ya sea x_values o  y_values. Entonces, solo agarraremos una muestra aleatoria de la población generada, y los grados de libertad dependerá de la cantidad de sumandos.

Como nos piden solo dos grados de libertad entonces tomamos:

$$Z_{i}^2+Z_{i}^2 $$
```
sample_size = 90000
sample_1 = [
    x_values[i] for i in (random.sample(range(len(x_values)), sample_size))
]

sample_2= [
    x_values[i] for i in (random.sample(range(len(x_values)), sample_size))
]
```
```
Z_1=[n**2 for n in sample_1]
Z_2=[n**2 for n in sample_2]

Ji_cuadrada=Z_1+Z_1
```
```
plt.hist(x=Ji_cuadrada, density=True,color='black',bins=100)
plt.xlabel("Gráfica 1")
plt.ylabel('Frecuencia')
plt.title('Distribución $\chi_{(2)}^{2}$')
plt.show()
```
[![chi-1.png](https://i.postimg.cc/j5gLYJLp/chi-1.png)](https://postimg.cc/r048SsGj)


### 3. Ahora compararemos la gráfica 1  con la función de densidad de una variable aleatoria con densidad Ji-cuadrada con 2 grados de libertad.

```
x = np.linspace(0,20,180000)

y=stats.chi2.pdf(x,df=2)
plt.plot(x,y,c="black")
plt.title('Función de densidad de probabilidad de la distribución $\chi_{(2)}^{2}$')
plt.tight_layout()

```
[![chi-2.png](https://i.postimg.cc/fTgpSW9M/chi-2.png)](https://postimg.cc/ykcvwHjG)

Concluimos que tienen la misma similitud y que nuestro programa que genera números pseudoaleatorios con distribución $\chi^{2}$ a partir de una distribución uniforme en el intervalo (0,1) es correcto. $\blacksquare$




    
    
    
    
    
    
    
    
    

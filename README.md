# Deciciones, decisiones, decisiones ... .

## Introducción

En este artículo voy a comentar un caso que me ha pasado y para ello voy a ver cómo funciona un navegador por dentro y que consideraciones deberíamos tener a la hora de coficar una determinada solución.

## ¿Por qué estamos aquí?

Pues bien, todo viene a raiz de una *code review* de un compañero de algo totalmente trivial. El planteó lo siguiente.

```javascript
side: computed('align', function() {
    const align = this.get('align');

    if (align === 'left') {
        return 'input';
    } else if (align === 'center') {
        return '';
    }
    return 'output';
})
```

Aquí no se está haciendo un *early exit* así que se le propuso hacer lo siguiente:

```javascript
side: computed('align', function() {
    const align = this.get('align');

    if (align === 'left') {
        return 'input';
    }
    if (align === 'center') {
        return '';
    }

    return 'output';
})
```

De esta forma debería de ser mas optima ya que permite salir de la función cuando se cumple el criterio.

Tengo que decir que yo propuse algo más "retorcido". ¿Por qué no usar algo de programación funcional?. Y propuse lo siguiente:

```javascript
side: computed('align', function() {
    const align = this.get('align');
    return (({'left': 'input', 'center': ' '})[align] || 'output').trim();
})
```

Bueno, ya sabía que mi solución no sería muy popular, pero ... me hizo preguntarme, ¿Por qué una solución es mejor que otra?, ¿Por qué no hacer las cosas de otra forma?, ¿Qué hay, por jemplo de usar un *switch*?.
Así que lo primero fue hacer unas pruebas de caja negra en el que medir las diferentes opciones. La idea era ver cúal de las soluciones es la optima desde un criterio de rendimiento. El código está en el index.html de este repositorio y podeis ejecutarlo en Chrome, está escrito en ES6 y hace uso de async, worker y blob.

Casos a considerar:

1. Para pocas ejecuciones < 100:

> _TODO pegar salida de la consola_

2. Para > 100.000:

> _TODO_

3. Peor caso para *early exit*:

> _TODO_

Y qué conclusiones podemos sacar. Bueno, en rendimiento no tenemos grandes diferencias:

1. El algoritmo de A ... 
2. B
3. C

## Correpción y claridad

Viendo que desde el punto de vista de rendimiento cualquiera de las soluciones podría ser buena. Veamos el criterio de la concisión, de la claridad, de la forma y no del fondo.
Aquí entramos en el terreno de las opiniones personales. Yo he recopilado los siguientes puntos de vista a favor y en contra de cada una de las soluciones. 

_TODO desarollar el tema viendo los pros y contras de cada una de las soluciones_

## Cruzando la línea

Vale, ahora mismo, yo estoy completamente perdido, tengo varios argumentos y creo que puedo defender todos. Pero quiero ir algo mas lejos, ver que "narices" está pasando de verdad. ¿Se pueden hacer las cosas mejor?, ¿Estoy dejando pasar algo?. Siento que tenemos que dar un paso mas y hacer como decía Peter Gabriel en su canción ["Digging in the dirt"](https://www.youtube.com/watch?v=X0C3DHp36zc).

_Ver como funciona V8, TurboFan, Babel, AST y cosas que hacen que nuestras optimizaciones no tengan sentido._

## Conclusión

La conclusión es: "(Creo que) No hay conclusión". He intentado exponer hechos y no conclusiones. Ser dogmático en cosas tan complejas y a la vez tan triviales me hace sentir deshonesto. Me parece que me meto un terreno cenagoso donde diferentes opiniones pueden ser las acertadas, en el que las cosas son demasiado triviales e innecesarias como para siquiera plantearlas, en el que es todo tan complejo y variable que puede cambiar de un día para otro y ser cierto lo contrario mañana.
Cosas como; a quién pertence el código, quién tiene la potestad para decidir que está bien y que está mal, que sentimientos podemos despertar en el equipo con nuestras opiniones y hasta dónde debemos llegar exponiendo nuestro criterio.
En definitiva, que no tengo ni idea de qué solución es la mejor.

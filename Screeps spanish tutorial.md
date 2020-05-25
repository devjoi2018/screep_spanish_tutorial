<head><link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.6.1/css/all.css" integrity="sha384-gfdkjb5BdAXd+lj+gudLWI+BXq4IuLW5IT+brZEZsLFm++aCMlF1V92rMkPaX4PP" crossorigin="anonymous"></head>

**<center><h1>Screeps Spanish Tutorial</h1></center>**

# Interfaz de usuario del juego y secuencias de comandos básicas

Bienvenido a Screeps!

Este tutorial te ayudará a aprender conceptos básicos del juego paso a paso. Puedes tomarlo más tarde, pero te recomendamos encarecidamente que lo hagas ahora, antes de comenzar un juego real.

Screeps es un juego para programadores. Si no sabe cómo codificar en JavaScript, consulte este [curso interactivo gratuito.]([https://link](https://www.codecademy.com/learn/introduction-to-javascript))

Recuerde que si cierras accidentalmente la ventana de sugerencias en el tutorial, siempre puede abrirla nuevamente con este botón <i class="fas fa-question-circle"></i>. 

Vamos a empezar. Este es un campo de juego llamado "sala". En el juego real, las habitaciones están conectadas entre sí con salidas, pero en el modo de simulación solo hay una habitación disponible para ti.

El objeto en el centro de la pantalla es tu primer Spawn, el centro de tu colonia.

- Documentación:
  - [Game world]([https://link](https://docs.screeps.com/introduction.html#Game-world))

Juegas escribiendo código real en la **consola** que esta en la parte inferior de la pantalla.

Haga click en la pestaña **Console**.

Puede ingresar tu código en este campo. Se ejecutará una vez.

Escribe cualquier cosa en este campo y presione **Enter**.

Su comando devuelve una respuesta (o error de ejecución) en la consola. Todas las salidas se duplican en la consola de su navegador, presiona (Ctrl + Shift + J) para expandir la consola en tu navegador con fines de depuración. Dentro del juego puede abrir y cerrar el panel inferior presionando **Alt + Enter**.

Ahora escribiremos algo real.

Tu Spawn crea nuevas unidades llamadas "creeps" con el método ```spawnCreep```. El uso de este método se describe en la [documentación](https://docs.screeps.com/). Cada creep tiene un nombre y ciertas partes del cuerpo que le otorgan diversas habilidades.

Puedes localizar a tu screep por su nombre de la siguiente manera: ```Game.spawns['Spawn1']```

Crea un creep trabajador con estos valores en el array de su cuerpo ```[WORK,CARRY,MOVE]``` y nombralo ```Harvester1```  (¡El nombre es importante para el tutorial, pero puedes darle el nombre que quieras en un juego real!). Puedes escribir el código en la consola por tu propia cuenta o copiar y pegar el código a continuación en la consola.

- Documentación:
  - [Your Colony]([https://link](https://docs.screeps.com/introduction.html#Your-colony))
  - [Creeps]([https://link](https://docs.screeps.com/creeps.html))
  - [Game Object]([https://link](https://docs.screeps.com/global-objects.html#Game-object))
  - [StructureSpawn.spawnCreep]([https://link](https://docs.screeps.com/api/#StructureSpawn.spawnCreep))

Código

```javascript
Game.spawns['Spawn1'].spawnCreep( [WORK, CARRY, MOVE], 'Harvester1' );
```

¡Excelente! Ahora tienes un creep con el nombre "Harvester1" que puedes controlar.

Puedes ver todas las características de tu creep (u otros objetos) utilizando la acción "Ver" <i class="fas fa-eye"></i>.

Para ocultar el panel del editor presione **Alt+Enter** y selecciona a tu creep con la ayuda del botón o la acción "Ver".

Aquí puede ver las características del objeto que seleccionaste. Los valores de cada característica y funciones de las partes del cuerpo de un creep se describen en la documentación.

¡Es hora de poner a trabajar al screep! El cuadrado amarillo parpadeante dentro del mapa, es una fuente de energía, un recurso valioso del juego. Puede ser cosechado por creeps con una o más partes del cuerpo ```WORK```, y la energía puede ser transportada al spawn por creeps con partes de ```CARRY```.

Para darle a tu creep un comando de funcionamiento permanente, la consola no es suficiente, ya que queremos que el creep funcione todo el tiempo. Entonces usaremos la pestaña Script en la parte inferior izquierda de la pantalla en lugar de la consola.

Haga clic en la pestaña "Script".

En la pestaña Scripts puedes escribir scripts que se ejecutarán de forma permanente, cada tick del juego en un bucle. Permite escribir programas que funcionan constantemente para controlar el comportamiento de tus creep que funcionarán incluso mientras estás desconectado (solo en el juego real, no en el modo Sala de simulación).

Para enviar un script al juego para que pueda ejecutarse, use este botón <i class="fas fa-check"></i> o **Ctrl+Enter**.

El código para cada sección del Tutorial se crea en su propia rama. Puedes ver el código de estas ramas para usarlo en un futuro en sus scripts.

- Documentación:
  - [Scripting basics](https://docs.screeps.com/scripting-basics.html)

Para enviar a un creep a cosechar energía, debe utilizar los métodos descritos en la sección de la documentación a continuación. Los comandos se pasarán en cada tick del juego. El método de cosecha requiere que la fuente de energía sea adyacente al creep.

Le puedes dar órdenes a un creep por su nombre de esta manera: ```Game.creeps['Harvester1']```. Use la constante ```FIND_SOURCES ```como argumento para el método ```Room.find```.

Envía a tu creep a cosechar energía escribiendo el siguiente código en la pestaña "Script" o copiando y pegando este código.

- Documentación:
  - [Game.creeps](https://docs.screeps.com/api/#Game.creeps)
  - [RoomObjects.room](https://docs.screeps.com/api/#RoomObject.room)
  - [Room.find](https://docs.screeps.com/api/#Room.find)
  - [Creep.moveTo](https://docs.screeps.com/api/#Creep.moveTo)
  - [Creep.harvest](https://docs.screeps.com/api/#Creep.harvest)


Código
```javascript
module.exports.loop = function () {
    var creep = Game.creeps['Harvester1'];
    var sources = creep.room.find(FIND_SOURCES);
    if(creep.harvest(sources[0]) == ERR_NOT_IN_RANGE) {
        creep.moveTo(sources[0]);
    }
}
```

Puedes ver como el creep se dirige a la fuente de energia y comienza a cosecharla, si una mancha amarilla burbujeante crece dentro del creep significa que ha comenzado a recolectar energía de la fuente.

Para hacer que el creep transfiera energía de vuelta al spawn, debes usar el método ```Creep.transfer```. Sin embargo, recuerda que debe hacerse cuando el creep está al lado del spawn, por lo que el creep debe caminar de regreso al spawn.

Si modificas el código agregando el ```check.store.getFreeCapacity() > 0 ``` al creep, podrá ir y venir por sí solo, dando energía al spawn y regresando a la fuente nuevamente.

Extenderemos el código del creep para que pueda transferir la energía cosechada al spawn y volver al trabajo.

- Documentación:
  - [Creep.transfer](https://docs.screeps.com/api/#Creep.transfer)
  - [Creep.store](https://docs.screeps.com/api/#Creep.store)

Código

```javascript
module.exports.loop = function () {
    var creep = Game.creeps['Harvester1'];

    if(creep.store.getFreeCapacity() > 0) {
        var sources = creep.room.find(FIND_SOURCES);
        if(creep.harvest(sources[0]) == ERR_NOT_IN_RANGE) {
            creep.moveTo(sources[0]);
        }
    }
    else {
        if( creep.transfer(Game.spawns['Spawn1'], RESOURCE_ENERGY) == ERR_NOT_IN_RANGE ) {
            creep.moveTo(Game.spawns['Spawn1']);
        }
    }
}
```

¡Excelente! Este creep ahora funcionará como cosechador hasta que muera. Recuerde que casi cualquier creep tiene un ciclo de vida de 1500 ticks de juego, luego "envejece" y muere (este comportamiento está desactivado en el Tutorial).

Creemos otro creep trabajador para ayudar al primero. Te costará otras 200 unidades de energía, por lo que es posible que debas esperar hasta que su cosechadora recolecte suficiente energía. El método ```spawnCreep``` devolverá un código de error ```ERR_NOT_ENOUGH_ENERGY (-6)``` en caso de no contar con la suficiente energía, hasta entonces debemos esperar a tener las 200 unidades.

Recuerda: para ejecutar el código una vez, simplemente escríbelo en la pestaña "Consola".

Genera un segundo creep con los siguientes valores en el array del cuerpo ```[WORK,CARRY,MOVE]``` y nombralo ```Harvester2```, recuerda que solo es por el tutorial, pero puedes darle el nombre que quieras en el juego real.

- Documentación:
  - [StructureSpawn.spawnCreep](https://docs.screeps.com/api/#StructureSpawn.spawnCreep)

Código

```javascript
Game.spawns['Spawn1'].spawnCreep( [WORK, CARRY, MOVE], 'Harvester2' );
```

El segundo creep está listo, pero no se moverá hasta que lo incluyamos en el programa.

Para establecer el comportamiento de ambos creeps, podríamos duplicar todo el script para el segundo, pero es mucho mejor usar el bucle ```for``` para todos los screeps en el metodo ```Game.creeps.```

Es hora de expandir aún mas tu programa para que funcionen todos los creeps.

- Documentación:
  - JavaScript Reference: [for..in loops](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in)


Código

```javascript
module.exports.loop = function () {
    for(var name in Game.creeps) {
        var creep = Game.creeps[name];

        if(creep.store.getFreeCapacity() > 0) {
            var sources = creep.room.find(FIND_SOURCES);
            if(creep.harvest(sources[0]) == ERR_NOT_IN_RANGE) {
                creep.moveTo(sources[0]);
            }
        }
        else {
            if(creep.transfer(Game.spawns['Spawn1'], RESOURCE_ENERGY) == ERR_NOT_IN_RANGE) {
                creep.moveTo(Game.spawns['Spawn1']);
            }
        }
    }
}
```

Ahora vamos a mejorar nuestro código llevando el comportamiento de los creeps trabajadores a un módulo separado. Crea un módulo llamado ```role.harvester``` con la ayuda de la sección Módulos a la izquierda del editor de scripts y defina una función de ejecución dentro del objeto ```module.exports```, que contenga el comportamiento de creep.

Cree un módulo ```role.harvester.```

- Documentación:
  - [Organizing scripts using modules](https://docs.screeps.com/modules.html)

Código (role.harvester)

```javascript
var roleHarvester = {

    /** @param {Creep} creep **/
    run: function(creep) {
	    if(creep.store.getFreeCapacity() > 0) {
            var sources = creep.room.find(FIND_SOURCES);
            if(creep.harvest(sources[0]) == ERR_NOT_IN_RANGE) {
                creep.moveTo(sources[0]);
            }
        }
        else {
            if(creep.transfer(Game.spawns['Spawn1'], RESOURCE_ENERGY) == ERR_NOT_IN_RANGE) {
                creep.moveTo(Game.spawns['Spawn1']);
            }
        }
	}
};

module.exports = roleHarvester;
```

Ahora puedes volver a escribir el código del módulo principal, dejando solo el bucle y una llamada a su nuevo módulo por el método ```require('role.harvester').```

Incluye el módulo ```role.harvester``` en el módulo principal(main).

Código (main)

```javascript
var roleHarvester = require('role.harvester');

module.exports.loop = function () {

    for(var name in Game.creeps) {
        var creep = Game.creeps[name];
        roleHarvester.run(creep);
    }
}
```

¡Es mucho mejor ahora!

Al agregar nuevas funciones y módulos a tus creeps de esta manera, puede controlar y administrar el trabajo de muchos creeps. En la siguiente sección del Tutorial, desarrollaremos un nuevo rol de creep.

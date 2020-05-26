<head><link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.6.1/css/all.css" integrity="sha384-gfdkjb5BdAXd+lj+gudLWI+BXq4IuLW5IT+brZEZsLFm++aCMlF1V92rMkPaX4PP" crossorigin="anonymous"></head>

**<center><h1>Screeps Spanish Tutorial</h1></center>**

# 1. Interfaz de usuario del juego y secuencias de comandos básicas

Bienvenido a Screeps!

Este tutorial te ayudará a aprender conceptos básicos del juego paso a paso. Puedes tomarlo más tarde, pero te recomendamos encarecidamente que lo hagas ahora, antes de comenzar un juego real.

Screeps es un juego para programadores. Si no sabe cómo codificar en JavaScript, consulte este [curso interactivo gratuito.](https://www.codecademy.com/learn/introduction-to-javascript)

Recuerde que si cierras accidentalmente la ventana de sugerencias en el tutorial, siempre puede abrirla nuevamente con este botón que tiene un signo de interrogación <i class="fas fa-question-circle"></i>. 

Vamos a empezar. Este es un campo de juego llamado "sala". En el juego real, las habitaciones están conectadas entre sí con salidas, pero en el modo de simulación solo hay una habitación disponible para ti.

El objeto en el centro de la pantalla es tu primer Spawn, el centro de tu colonia.

- Documentación:
  - [Game world](https://docs.screeps.com/introduction.html#Game-world)

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
  - [Your Colony](https://docs.screeps.com/introduction.html#Your-colony)
  - [Creeps](https://docs.screeps.com/creeps.html)
  - [Game Object](https://docs.screeps.com/global-objects.html#Game-object)
  - [StructureSpawn.spawnCreep](https://docs.screeps.com/api/#StructureSpawn.spawnCreep)

Código

```javascript
Game.spawns['Spawn1'].spawnCreep( [WORK, CARRY, MOVE], 'Harvester1' );
```

¡Excelente! Ahora tienes un creep con el nombre "Harvester1" que puedes controlar.

Puedes ver todas las características de tu creep (u otros objetos) utilizando la acción "Ver" (botón en forma de ojo) <i class="fas fa-eye"></i>.

Para ocultar el panel del editor presione **Alt+Enter** y selecciona a tu creep con la ayuda del botón o la acción "Ver".

Aquí puede ver las características del objeto que seleccionaste. Los valores de cada característica y funciones de las partes del cuerpo de un creep se describen en la documentación.

¡Es hora de poner a trabajar al screep! El cuadrado amarillo parpadeante dentro del mapa, es una fuente de energía, un recurso valioso del juego. Puede ser cosechado por creeps con una o más partes del cuerpo ```WORK```, y la energía puede ser transportada al spawn por creeps con partes de ```CARRY```.

Para darle a tu creep un comando de funcionamiento permanente, la consola no es suficiente, ya que queremos que el creep funcione todo el tiempo. Entonces usaremos la pestaña Script en la parte inferior izquierda de la pantalla en lugar de la consola.

Haga clic en la pestaña "Script".

En la pestaña Scripts puedes escribir scripts que se ejecutarán de forma permanente, cada tick del juego en un bucle. Permite escribir programas que funcionan constantemente para controlar el comportamiento de tus creep que funcionarán incluso mientras estás desconectado (solo en el juego real, no en el modo Sala de simulación).

Para enviar un script al juego para que pueda ejecutarse, usa el botón con simbolo de check <i class="fas fa-check"></i> o **Ctrl+Enter**.

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

# 2. Actualización de controlador

En esta sección del Tutorial, hablaremos sobre un objeto clave estratégico en tu habitación: **Romm Controller**. Al controlar esta invencible estructura, puedes construir instalaciones en la habitación. Cuanto más alto sea el nivel del controlador, más estructuras estarán disponibles para construir.

Necesitarás un nuevo creep trabajador para actualizar a tu controlador. Llamémoslo "Upgrader1". En las siguientes secciones discutiremos cómo crear creeps automáticamente, pero por ahora enviemos un comando manualmente a la consola.

Genera un creep con el cuerpo ```[WORK,CARRY,MOVE]``` y el nombre ```Upgrader1.```

- Documentación:
  - [Control](https://docs.screeps.com/control.html)
  - [Game.spawns](https://docs.screeps.com/api/#Game.spawns)
  - [StructureSpawn.spawnCreep](https://docs.screeps.com/api/#StructureSpawn.spawnCreep)

Código

```javascript
Game.spawns['Spawn1'].spawnCreep( [WORK, CARRY, MOVE], 'Upgrader1' );
```

El creep "Upgrader1" realizará la misma tarea que el creep cosechador de energía, pero no queremos que lo haga. Necesitamos diferenciar los roles de los creeps.

Para hacer eso, necesitamos utilizar la propiedad de ```memory``` de cada creep que permite escribir información personalizada en la "memoria" del creep. Hagamos esto para asignar diferentes roles a nuestros creeps.

Se puede acceder a toda su memoria almacenada a través del objeto de memoria global. Puedes usarlo como quieras.

Escriba la propiedad ```role='harvester'``` en la memoria del creep cosechador de energía, y escribe la propiedad ```role='upgrader'```- en el creep actualizador con la ayuda de la consola.

- Documentación:
  - [Memory object](https://docs.screeps.com/global-objects#Memory-object)
  - [Creep.memory](https://docs.screeps.com/api/#Creep.memory)

Código

```javascript
Game.creeps['Harvester1'].memory.role = 'harvester';
Game.creeps['Upgrader1'].memory.role = 'upgrader';
```

Puedes verificar la memoria de tus creeps en el panel de información de creeps a la izquierda o en la pestaña "Memory".

Ahora vamos a definir el comportamiento del nuevo creep. Ambos creeps deberían recolectar energía, pero el creep con el rol de recolector ```(harvester)``` debería llevarlo al spawn, mientras que el creep con el rol de actualizador ```(upgrader)``` debería ir al Controlador y aplicarle la función ```upgradeController``` (puede obtener el objeto Controlador con la ayuda de la propiedad ```Creep.room.controller```).

Para hacer esto, crearemos un nuevo módulo llamado ```role.upgrader.```

Crea un nuevo módulo y nombralo ```role.upgrader``` y agrega la lógica de comportamiento de tu nuevo creep.

- Documentación:
  - [RoomObject.room](https://docs.screeps.com/api/#RoomObject.room)
  - [Room.Controller](https://docs.screeps.com/api/#Room.controller)
  - [Creep.upgradeController](https://docs.screeps.com/api/#Creep.upgradeController)


Código (role.upgrader)

```javascript
var roleUpgrader = {

    /** @param {Creep} creep **/
    run: function(creep) {
	    if(creep.store[RESOURCE_ENERGY] == 0) {
            var sources = creep.room.find(FIND_SOURCES);
            if(creep.harvest(sources[0]) == ERR_NOT_IN_RANGE) {
                creep.moveTo(sources[0]);
            }
        }
        else {
            if(creep.upgradeController(creep.room.controller) == ERR_NOT_IN_RANGE) {
                creep.moveTo(creep.room.controller);
            }
        }
	}
};

module.exports = roleUpgrader;
```

Ahora en nuestro módulo principal, todos los creeps tienen el mismo rol. Necesitamos dividir su comportamiento dependiendo de la propiedad ```Creep.memory.role``` previamente definida conectando el nuevo módulo.

Aplicaremos la lógica del módulo ```role.upgrader``` al creep con el actualizador de roles y verifica cómo funciona.

Código (main)

```javascript
var roleHarvester = require('role.harvester');
var roleUpgrader = require('role.upgrader');

module.exports.loop = function () {

    for(var name in Game.creeps) {
        var creep = Game.creeps[name];
        if(creep.memory.role == 'harvester') {
            roleHarvester.run(creep);
        }
        if(creep.memory.role == 'upgrader') {
            roleUpgrader.run(creep);
        }
    }
}
```

¡Perfecto, has mejorado el nivel de tu controlador!

**Importante:** si no actualizas tu controlador dentro de los 20,000 ticks de juego, pierdes un nivel. Al alcanzar el nivel 0, perderás el control sobre la sala y otro jugador podrá capturarlo libremente. Asegúrese de que al menos uno de tus creeps realice regularmente la función ```upgradeController.```

# 3. Contruyendo estructuras

La actualización del controlador da acceso a algunas estructuras nuevas: muros, murallas y extensiones. Discutiremos muros y murallas en la próxima sección del Tutorial, por ahora hablemos de extensiones.

Se requieren extensiones para construir creeps más grandes. Un creep con solo una parte del cuerpo de un solo tipo funciona mal. Darle varias partes de ```WORKs``` (trabajos) lo hará trabajar proporcionalmente más rápido.

Sin embargo, tal desplazamiento será costoso y un solo spawn solo puede contener 300 unidades de energía. Para construir creeps que cuestan más de 300 unidades de energía, necesitas extensiones de spawn.

El segundo nivel de Controlador tiene **5 extensiones** disponibles para que construyas. Este número aumenta con cada nuevo nivel.

Puedes colocar extensiones en cualquier lugar de tu habitación, y un creep puede usarlas independientemente de la distancia. En este tutorial ya hemos colocado los sitios de construcción correspondientes para tu conveniencia.

Vamos a crear un nuevo creep cuyo propósito es construir estructuras. Este proceso será similar a las secciones de Tutoriales anteriores. Pero esta vez establezcamos ```memoria``` para el nuevo creep directamente en el método ```Spawn.spawnCreep``` pasándolo en el tercer argumento.

Crea un creep con el cuerpo ```[WORK,CARRY,MOVE]```, y el nombre ```Builder1``` y ```{role: 'builder'}``` como su memoria.

- Documentación:
  - [StructureSpawn.spawnCreep](https://docs.screeps.com/api/#StructureSpawn.spawnCreep)

Código

```javascript
Game.spawns['Spawn1'].spawnCreep( [WORK, CARRY, MOVE], 'Builder1',
    { memory: { role: 'builder' } } );
```

Nuestro nuevo creep no se moverá hasta que definamos el comportamiento en el role ```builder.```

Como antes, vamos a pasar este rol a un módulo separado con el nombre ```role.builder.``` La construcción es llevada a cabo aplicando el método ```Creep.build``` a los sitios de construcción que se pueden buscar por ```Room.find (FIND_CONSTRUCTION_SITES).``` La estructura requiere energía que el creep puede cosechar por sí solo.

Para evitar que el creep corra de un lado a otro con demasiada frecuencia, sin que se agote la carga, complicaremos nuestra lógica creando una nueva variable booleana ```creep.memory.building``` que le dirá al creep cuándo cambiar de tarea. También agregaremos la nueva opción ```creep.say``` y ```visualizePathStyle``` al método ```moveTo``` para visualizar las intenciones del creep.

Crearemos el módulo ```role.builder``` con la lógica de comportamiento para el nuevo creep.

- Documentation:
  - [RoomObject.room](https://docs.screeps.com/api/#RoomObject.room)
  - [Room.find](https://docs.screeps.com/api/#Room.find)
  - [Creep.build](https://docs.screeps.com/api/#Creep.build)
  - [Creep.say](https://docs.screeps.com/api/#Creep.say)

Código (role.builder)

```javascript
var roleBuilder = {

    /** @param {Creep} creep **/
    run: function(creep) {

	    if(creep.memory.building && creep.store[RESOURCE_ENERGY] == 0) {
            creep.memory.building = false;
            creep.say('🔄 harvest');
	    }
	    if(!creep.memory.building && creep.store.getFreeCapacity() == 0) {
	        creep.memory.building = true;
	        creep.say('🚧 build');
	    }

	    if(creep.memory.building) {
	        var targets = creep.room.find(FIND_CONSTRUCTION_SITES);
            if(targets.length) {
                if(creep.build(targets[0]) == ERR_NOT_IN_RANGE) {
                    creep.moveTo(targets[0], {visualizePathStyle: {stroke: '#ffffff'}});
                }
            }
	    }
	    else {
	        var sources = creep.room.find(FIND_SOURCES);
            if(creep.harvest(sources[0]) == ERR_NOT_IN_RANGE) {
                creep.moveTo(sources[0], {visualizePathStyle: {stroke: '#ffaa00'}});
            }
	    }
	}
};

module.exports = roleBuilder;
```

Crearemos una llamada del nuevo rol en el módulo principal y esperemos el resultado.

Para usar el módulo ```role.builder``` en el nuevo creep, construye las 5 extensiones.

Código (main)

```javascript
var roleHarvester = require('role.harvester');
var roleBuilder = require('role.builder');

module.exports.loop = function () {

    for(var name in Game.creeps) {
        var creep = Game.creeps[name];
        if(creep.memory.role == 'harvester') {
            roleHarvester.run(creep);
        }
        if(creep.memory.role == 'builder') {
            roleBuilder.run(creep);
        }
    }
}
```

Tus extensiones deberian de haber sido construidas. Ahora aprendamos a trabajar con ellas.

Mantener las extensiones requiere que enseñes a tus creeps a llevar energía no solo a un spawn sino también a las extensiones. Para hacer esto, puedes usar el objeto ```Game.structures``` o buscar dentro de la habitación con la ayuda de ```Room.find (FIND_STRUCTURES).``` En ambos casos, deberás filtrar la lista de elementos en la condición ```structure.structureType == STRUCTURE_EXTENSION``` (o, alternativamente, ```structure instanceof StructureExtension```) y también verificarlos para la carga de energía, como antes.

Mejoremos la lógica en el módulo ```role.harvester.```

- Documentación:
  - [Game.structures](https://docs.screeps.com/api/#Game.structures)
  - [Room.find](https://docs.screeps.com/api/#Room.find)
  - [StructureExtension](https://docs.screeps.com/api/#StructureExtension)

Código (role.harvester)

```javascript
var roleHarvester = {

    /** @param {Creep} creep **/
    run: function(creep) {
	    if(creep.store.getFreeCapacity() > 0) {
            var sources = creep.room.find(FIND_SOURCES);
            if(creep.harvest(sources[0]) == ERR_NOT_IN_RANGE) {
                creep.moveTo(sources[0], {visualizePathStyle: {stroke: '#ffaa00'}});
            }
        }
        else {
            var targets = creep.room.find(FIND_STRUCTURES, {
                    filter: (structure) => {
                        return (structure.structureType == STRUCTURE_EXTENSION || structure.structureType == STRUCTURE_SPAWN) &&
                            structure.store.getFreeCapacity(RESOURCE_ENERGY) > 0;
                    }
            });
            if(targets.length > 0) {
                if(creep.transfer(targets[0], RESOURCE_ENERGY) == ERR_NOT_IN_RANGE) {
                    creep.moveTo(targets[0], {visualizePathStyle: {stroke: '#ffffff'}});
                }
            }
        }
	}
};

module.exports = roleHarvester;
```

Para conocer la cantidad total de energía en la habitación, puedes usar la propiedad ```Room.energyAvailable.``` Agreguemos la salida de esta propiedad a la consola para rastrearla durante el llenado de extensiones.

Rellene todas las 5 extensiones y el spawn con energía.

- Documentación:
  - [Room.energyAvailable](https://docs.screeps.com/api/#Room.energyAvailable)

Código (main)

```javascript
var roleHarvester = require('role.harvester');
var roleBuilder = require('role.builder');

module.exports.loop = function () {

    for(var name in Game.rooms) {
        console.log('Room "'+name+'" has '+Game.rooms[name].energyAvailable+' energy');
    }

    for(var name in Game.creeps) {
        var creep = Game.creeps[name];
        if(creep.memory.role == 'harvester') {
            roleHarvester.run(creep);
        }
        if(creep.memory.role == 'builder') {
            roleBuilder.run(creep);
        }
    }
}
```

Excelente, todas las estructuras despues de un rato deberian de estar llenas de energía. ¡Es hora de construir a un creep grande!

En total, tenemos 550 unidades de energía en nuestro spawn y extensiones. Es suficiente para crear un creep con el cuerpo ```[WORK,WORK,WORK, WORK,CARRY,MOVE,MOVE].``` Este creep funcionará 4 veces más rápido que un creep trabajador normal. Su cuerpo es más pesado, por eso le agregamos otro ```MOVE.``` Sin embargo, dos partes aún no son suficientes para moverlo a la velocidad de un creep pequeño, para poder avanzar rápido requeriría 4x```MOVE``` o la construcción de una carretera.

Genera un creep con el cuerpo ```[WORK,WORK,WORK,CARRY,MOVE,MOVE]```, y dale el nombre de HarvesterBig y el rol de cosechador (harvester).

Código

```javascript
Game.spawns['Spawn1'].spawnCreep( [WORK,WORK,WORK,WORK,CARRY,MOVE,MOVE],
    'HarvesterBig',
    { memory: { role: 'harvester' } } );
```

La construcción de este creep tomó energía de todos los almacenes y los drenó por completo.

Ahora, seleccionemos nuestro creep y veamos cómo funciona.

Has clic en el creep ```Harvester2.```

Como puedes ver en el panel derecho, este poderoso creep cosecha 8 unidades de energía por tick. Algunos de los creep pueden drenar por completo una fuente de energía antes de que se vuelva a llenar, lo que le da a tu colonia un impulso de energía máximo.

Por lo tanto, al actualizar a tu controlador, podras construir nuevas extensiones y creeps más potentes, mejorarás considerablemente la efectividad del trabajo de tu colonia. Además, al reemplazar muchos creeps pequeños con menos grandes, ahorras recursos de CPU para controlarlos, lo cual es un requisito previo importante para jugar en el modo en línea.

En la siguiente sección, hablaremos sobre cómo configurar la fabricación automática de nuevos creeps.


# 4. Auto creación de creeps

Hasta ahora, hemos creado nuevos creeps directamente en la consola. No es una buena idea hacerlo constantemente ya que la idea misma de Screeps es hacer que tu colonia se controle a sí misma. Te irá bien si le enseñas a tu spawn a producir creeps en la habitación por sí solo.

Este es un tema bastante complicado y muchos jugadores pasan meses perfeccionando y refinando su código de generación automática. Pero intentemos al menos algo simple y dominemos algunos principios básicos para comenzar.

Tendrás que crear nuevos creeps cuando los viejos mueran por la edad o por otras razones. Dado que no hay eventos en el juego para informar la muerte de un creep particular, la forma más fácil es contar el número de creeps requeridos y, si llega a ser inferior a un valor definido, comenzará a generar creeps automaticamente.

Hay varias formas de contar la cantidad de creeps. Uno de ellos es filtrar ```Game.creeps``` con la ayuda de la función ```_.filter``` y usar el rol en su memoria. Intentemos hacer eso y llevar el número de creeps a la consola.

Agrega la salida de la cantidad de creeps con el rol de recolector (```harvester```) en la consola.

- Documentación:
  - [Game.creeps](https://docs.screeps.com/api/#Game.creeps)
  - [lodash.filter](https://lodash.com/docs#filter)

Código (main)

```javascript
var roleHarvester = require('role.harvester');
var roleUpgrader = require('role.upgrader');

module.exports.loop = function () {

    var harvesters = _.filter(Game.creeps, (creep) => creep.memory.role == 'harvester');
    console.log('Harvesters: ' + harvesters.length);

    for(var name in Game.creeps) {
        var creep = Game.creeps[name];
        if(creep.memory.role == 'harvester') {
            roleHarvester.run(creep);
        }
        if(creep.memory.role == 'upgrader') {
            roleUpgrader.run(creep);
        }
    }
}
```

Digamos que queremos tener siempre al menos dos creeps. La forma más fácil de lograr esto es ejecutar ```StructureSpawn.spawnCreep``` cada vez que dla cantidad de creeps en la sala sea menor a dos. Es posible que no sea necesario definir su nombre (se le dará automáticamente en este caso), pero no olvides definir la función necesaria.

También podemos agregar una nueva llamada ```RoomVisual``` para visualizar qué creep se está generando.

Agrega la lógica para ```StructureSpawn.spawnCreep``` en su módulo principal.

- Documentación:
  - [StructureSpawn.spawnCreep](https://docs.screeps.com/api/#StructureSpawn.spawnCreep)
  - [RoomVisual](https://docs.screeps.com/api/#RoomVisual)

Código (main)

```javascript
var roleHarvester = require('role.harvester');
var roleUpgrader = require('role.upgrader');

module.exports.loop = function () {

    var harvesters = _.filter(Game.creeps, (creep) => creep.memory.role == 'harvester');
    console.log('Harvesters: ' + harvesters.length);

    if(harvesters.length < 2) {
        var newName = 'Harvester' + Game.time;
        console.log('Spawning new harvester: ' + newName);
        Game.spawns['Spawn1'].spawnCreep([WORK,CARRY,MOVE], newName, 
            {memory: {role: 'harvester'}});        
    }
    
    if(Game.spawns['Spawn1'].spawning) { 
        var spawningCreep = Game.creeps[Game.spawns['Spawn1'].spawning.name];
        Game.spawns['Spawn1'].room.visual.text(
            '🛠️' + spawningCreep.memory.role,
            Game.spawns['Spawn1'].pos.x + 1, 
            Game.spawns['Spawn1'].pos.y, 
            {align: 'left', opacity: 0.8});
    }

    for(var name in Game.creeps) {
        var creep = Game.creeps[name];
        if(creep.memory.role == 'harvester') {
            roleHarvester.run(creep);
        }
        if(creep.memory.role == 'upgrader') {
            roleUpgrader.run(creep);
        }
    }
}
```

Ahora tratemos de emular una situación en la que uno de nuestros creeps muere. Ahora puedes dar el comando suicidio al creep a través de la consola o tu panel de propiedades a la derecha.

Haz que uno de los creeps se suicide.

- Documentación:
  - [Creep.suicide](https://docs.screeps.com/api/#Creep.suicide)

Código 

```javascript
Game.creeps['Harvester1'].suicide()
```

Como puedes ver desde la consola, después de que nos faltó un creep, comenzó a construirse un nuevo creep con un nuevo nombre.

Un punto importante aquí es que la memoria de los creeps muertos no se borra sino que se guarda para su posterior reutilización. Si creas creeps con nombres aleatorios cada vez, puedes provocar una saturación de la memoria, por lo que debes borrarlo al comienzo de cada tick (antes del código de creación del creep).

Agrega el siguiente código para borrar la memoria.

Código (main)

```javascript
var roleHarvester = require('role.harvester');
var roleUpgrader = require('role.upgrader');

module.exports.loop = function () {

    for(var name in Memory.creeps) {
        if(!Game.creeps[name]) {
            delete Memory.creeps[name];
            console.log('Clearing non-existing creep memory:', name);
        }
    }

    var harvesters = _.filter(Game.creeps, (creep) => creep.memory.role == 'harvester');
    console.log('Harvesters: ' + harvesters.length);

    if(harvesters.length < 2) {
        var newName = 'Harvester' + Game.time;
        console.log('Spawning new harvester: ' + newName);
        Game.spawns['Spawn1'].spawnCreep([WORK,CARRY,MOVE], newName, 
            {memory: {role: 'harvester'}});
    }
    
    if(Game.spawns['Spawn1'].spawning) { 
        var spawningCreep = Game.creeps[Game.spawns['Spawn1'].spawning.name];
        Game.spawns['Spawn1'].room.visual.text(
            '🛠️' + spawningCreep.memory.role,
            Game.spawns['Spawn1'].pos.x + 1, 
            Game.spawns['Spawn1'].pos.y, 
            {align: 'left', opacity: 0.8});
    }

    for(var name in Game.creeps) {
        var creep = Game.creeps[name];
        if(creep.memory.role == 'harvester') {
            roleHarvester.run(creep);
        }
        if(creep.memory.role == 'upgrader') {
            roleUpgrader.run(creep);
        }
    }
}
```

Ahora la memoria de los creeps muertos queda relegada al olvido, lo que nos ahorra recursos.

Además de crear nuevos creeps después de la muerte de los antiguos, hay otra forma de mantener la cantidad necesaria de creeps: con el método ```StructureSpawn.renewCreep.``` El envejecimiento progresivo está deshabilitado en el Tutorial, por lo que te recomendamos que te familiarices con él por tu cuenta.

- Documentación:
  - [StructureSpawn.renewCreep](https://docs.screeps.com/api/#StructureSpawn.renewCreep)


# 5. Defendiendo tu habitación

El mundo de Screeps no es el lugar más seguro. Otros jugadores pueden reclamar tu territorio. Además, tu habitación puede ser asaltada ocasionalmente por creeps de NPC neutrales. Por lo tanto, debes pensar en la defensa de tu colonia para desarrollarla con éxito.

- Documentanción:
  - [Defending your room](https://docs.screeps.com/defense.html)

En la habitación hay un creep de color rojo hostil que vino de la entrada izquierda y atacó a tu colonia. Es bueno que tengamos muros para frenarlo temporalmente. Pero caerán tarde o temprano, por lo que debemos lidiar con el problema.

La forma más segura de defenderte de un ataque es usar el modo seguro de la habitación. En el modo seguro, ningún otro creep podrá usar algun método dañino en la habitación (pero aún tendrás que defenderte de extraños).

El modo seguro se activa a través del controlador de la sala, que debe tener activaciones disponibles para su uso. Pasemos una activación para encenderlo en nuestra habitación.

Activa el modo seguro.

- Documentación:
  - [StructureController.activeSafeMode](https://docs.screeps.com/api/#StructureController.activateSafeMode)

Código

```javascript
Game.spawns['Spawn1'].room.controller.activateSafeMode();
```

Como puedes ver, el creep enemigo dejó de atacar el muro: sus métodos dañinos están bloqueados. Recomendamos que actives el modo seguro cuando tus defensas fallen.

Ahora limpiemos la habitación de los invitados no deseados.

Las torres son la forma más fácil de defender activamente una habitación. Usan energía y pueden apuntar a cualquier creep en una habitación para atacarlo o curarlo. El efecto depende de la distancia entre la torre y el objetivo.

Para empezar, vamos a sentar las bases de nuestra nueva torre. Puedes configurar cualquier lugar que desees dentro de las paredes y colocar el sitio de construcción con la ayuda del botón "Construir" en el panel superior (botón con simbolo en forma de caja) <i class="fas fa-cube"></i>.

Coloca el sitio de construcción para la torre (manualmente o usando el código a continuación).

- Documentación:
  - [StructureTower](https://docs.screeps.com/api/#StructureTower)
  - [Room.createConstructionSite](https://docs.screeps.com/api/#Room.createConstructionSite)


Código

```javascript
Game.spawns['Spawn1'].room.createConstructionSite( 23, 22, STRUCTURE_TOWER );
```

Como puedes ver el creep Builder1 ha comenzado inmediatamente la construcción. Espera hasta que termine.

Una torre usa energía, así que modifiquemos el el rol de recolector (```harvester```) para llevar energía a la torre junto con otras estructuras. Para hacer esto, debe agregar la constante ```STRUCTURE_TOWER``` al filtro de estructuras a las que apunta su creep recolector.

Agrega ```STRUCTURE_TOWER``` al módulo ```role.harvester``` y espere a que aparezca la energía en la torre.

Código (role.harvester)

```javascript
var roleHarvester = {

    /** @param {Creep} creep **/
    run: function(creep) {
	    if(creep.store.getFreeCapacity() > 0) {
            var sources = creep.room.find(FIND_SOURCES);
            if(creep.harvest(sources[0]) == ERR_NOT_IN_RANGE) {
                creep.moveTo(sources[0], {visualizePathStyle: {stroke: '#ffaa00'}});
            }
        }
        else {
            var targets = creep.room.find(FIND_STRUCTURES, {
                    filter: (structure) => {
                        return (structure.structureType == STRUCTURE_EXTENSION ||
                                structure.structureType == STRUCTURE_SPAWN ||
                                structure.structureType == STRUCTURE_TOWER) && 
                                structure.store.getFreeCapacity(RESOURCE_ENERGY) > 0;
                    }
            });
            if(targets.length > 0) {
                if(creep.transfer(targets[0], RESOURCE_ENERGY) == ERR_NOT_IN_RANGE) {
                    creep.moveTo(targets[0], {visualizePathStyle: {stroke: '#ffffff'}});
                }
            }
        }
	}
};

module.exports = roleHarvester;
```

¡Excelente, tu torre deberia estar lista para usarse!

Como un creep, una torre tiene varios métodos similares: atacar(```attack```), curar (```heal```) y reparar (```repair```). Cada acción gasta 10 unidades de energía. Necesitamos usar el ataque contra el enemigo más cercano para que se descubra. Recuerda que la distancia es vital: ¡el efecto puede ser más fuerte algunas veces con el mismo costo de energía!

Para obtener el objeto de la torre directamente, puedes usar su ID desde el panel derecho y el método ```Game.getObjectById.```

Destruye al creep enemigo con la ayuda de la torre.

- Documentación:
  - [Game.getObjectById](https://docs.screeps.com/api/#Game.getObjectById)
  - [RoomObject.pos](https://docs.screeps.com/api/#RoomObject.pos)
  - [RoomPosition.findClosestByRange](https://docs.screeps.com/api/#RoomPosition.findClosestByRange)
  - [StructureTower.attack](https://docs.screeps.com/api/#StructureTower.attack)

Código (main)

```javascript
var roleHarvester = require('role.harvester');
var roleUpgrader = require('role.upgrader');
var roleBuilder = require('role.builder');

module.exports.loop = function () {

    var tower = Game.getObjectById('321a7961043f40fdb57c9348');
    if(tower) {
        var closestHostile = tower.pos.findClosestByRange(FIND_HOSTILE_CREEPS);
        if(closestHostile) {
            tower.attack(closestHostile);
        }
    }

    for(var name in Game.creeps) {
        var creep = Game.creeps[name];
        if(creep.memory.role == 'harvester') {
            roleHarvester.run(creep);
        }
        if(creep.memory.role == 'upgrader') {
            roleUpgrader.run(creep);
        }
        if(creep.memory.role == 'builder') {
            roleBuilder.run(creep);
        }
    }
}
```

Como puedes ver el creep enemigo fué eliminado y nuestra colonia puede respirar fácilmente. Sin embargo, el invasor ha dañado algunas paredes durante el breve ataque. Será mejor que configure la reparación automática.

Las estructuras dañadas pueden ser reparadas tanto por creeps como por torres. Intentemos usar una torre para eso. Necesitaremos el método de reparación (```repair```). También necesitarás el método ```Room.find``` y un filtro para ubicar las paredes dañadas.

Ten en cuenta que, dado que los muros no pertenecen a ningún jugador, encontrarlos requiere la constante ```FIND_STRUCTURES``` en lugar de ```FIND_MY_STRUCTURES.```

Repara todas las paredes dañadas.

- Documentación:
  - [Room.find](https://docs.screeps.com/api/#Room.find)
  - [StructureTower.repair](https://docs.screeps.com/api/#StructureTower.repair)

Código (main)

```javascript
var roleHarvester = require('role.harvester');
var roleUpgrader = require('role.upgrader');
var roleBuilder = require('role.builder');

module.exports.loop = function () {

    var tower = Game.getObjectById('321a7961043f40fdb57c9348');
    if(tower) {
        var closestDamagedStructure = tower.pos.findClosestByRange(FIND_STRUCTURES, {
            filter: (structure) => structure.hits < structure.hitsMax
        });
        if(closestDamagedStructure) {
            tower.repair(closestDamagedStructure);
        }

        var closestHostile = tower.pos.findClosestByRange(FIND_HOSTILE_CREEPS);
        if(closestHostile) {
            tower.attack(closestHostile);
        }
    }

    for(var name in Game.creeps) {
        var creep = Game.creeps[name];
        if(creep.memory.role == 'harvester') {
            roleHarvester.run(creep);
        }
        if(creep.memory.role == 'upgrader') {
            roleUpgrader.run(creep);
        }
        if(creep.memory.role == 'builder') {
            roleBuilder.run(creep);
        }
    }
}
```

¡Todo el daño del ataque ha sido reparado!

¡Felicitaciones, has completado el Tutorial! Ahora tienes suficiente conocimiento y código para comenzar a jugar en el modo en línea. ¡Elige tu habitación, encuentra una colonia y emprende tu propia búsqueda de dominación en el mundo de Screeps!

Si desea profundizar en las sutilezas del juego o tienes alguna pregunta, no dudes en consultar:

- [Documentation](https://docs.screeps.com/)
- [Forum](https://screeps.com/forum/)
- [Slack chat](https://chat.screeps.com/)

Feliz codificación.

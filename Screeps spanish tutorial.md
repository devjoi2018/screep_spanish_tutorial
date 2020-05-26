<head><link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.6.1/css/all.css" integrity="sha384-gfdkjb5BdAXd+lj+gudLWI+BXq4IuLW5IT+brZEZsLFm++aCMlF1V92rMkPaX4PP" crossorigin="anonymous"></head>

**<center><h1>Screeps Spanish Tutorial</h1></center>**

# 1. Interfaz de usuario del juego y secuencias de comandos básicas

Bienvenido a Screeps!

Este tutorial te ayudará a aprender conceptos básicos del juego paso a paso. Puedes tomarlo más tarde, pero te recomendamos encarecidamente que lo hagas ahora, antes de comenzar un juego real.

Screeps es un juego para programadores. Si no sabe cómo codificar en JavaScript, consulte este [curso interactivo gratuito.]([https://link](https://www.codecademy.com/learn/introduction-to-javascript))

Recuerde que si cierras accidentalmente la ventana de sugerencias en el tutorial, siempre puede abrirla nuevamente con este botón <i class="fas fa-question-circle"></i>. 

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

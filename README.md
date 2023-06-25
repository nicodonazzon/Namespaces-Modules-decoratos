# Namespaces-Modules-decoratos

Namespaces:

Los Namespaces en TypeScript son una forma de organizar y agrupar código relacionado en un ámbito específico. Permiten evitar colisiones de nombres y proporcionan un mecanismo para estructurar y modularizar tu código.

Aquí tienes un ejemplo de cómo crear un Namespace en TypeScript:

```typescript
namespace MyNamespace {
  export interface Person {
    name: string;
    age: number;
  }

  export function sayHello(person: Person) {
    console.log(`Hello, ${person.name}! You are ${person.age} years old.`);
  }
}

const person: MyNamespace.Person = { name: "John", age: 25 };
MyNamespace.sayHello(person); // Hello, John! You are 25 years old.
```

En este ejemplo, hemos creado un Namespace llamado `MyNamespace` que contiene una interfaz `Person` y una función `sayHello`. La interfaz y la función están exportadas para que puedan ser utilizadas fuera del Namespace. Luego, creamos un objeto `person` que cumple con la interfaz `Person` y llamamos a la función `sayHello` para saludar a la persona.

Aunque en los ejemplos anteriores los namespaces se definieron en el mismo archivo, los namespaces también se pueden dividir en varios archivos para organizar y modularizar el código de manera más eficiente.

Aquí tienes un ejemplo de cómo utilizar namespaces en múltiples archivos:

**myNamespace.ts:**
```typescript
namespace MyNamespace {
  export interface Person {
    name: string;
    age: number;
  }
}
```

**greetings.ts:**
```typescript
namespace MyNamespace {
  export function sayHello(person: Person) {
    console.log(`Hello, ${person.name}! You are ${person.age} years old.`);
  }
}
```

**main.ts:**
```typescript
/// <reference path="myNamespace.ts" />
/// <reference path="greetings.ts" />

const person: MyNamespace.Person = { name: "John", age: 25 };
MyNamespace.sayHello(person); // Hello, John! You are 25 years old.
```

En este ejemplo, hemos dividido el namespace `MyNamespace` en dos archivos: `myNamespace.ts` y `greetings.ts`. Luego, en el archivo `main.ts`, utilizamos las directivas de referencia (`/// <reference path="...">`) para indicar que necesitamos cargar los archivos de definición de namespace. De esta manera, podemos acceder a las interfaces y funciones del namespace en el archivo `main.ts`.

Utilizar namespaces en múltiples archivos puede ayudar a mantener el código más organizado y modular, especialmente en proyectos más grandes. Sin embargo, es importante tener en cuenta que los modules (módulos) son una opción más recomendada y moderna para organizar el código en TypeScript, ya que ofrecen una mayor flexibilidad y soporte para sistemas de módulos estándar como CommonJS o ES Modules.

Modules:

Los Modules en TypeScript son una forma más moderna de organizar y compartir código. A diferencia de los Namespaces, los Modules se basan en el sistema de módulos de JavaScript (CommonJS o ES Modules) y proporcionan una forma más robusta y flexible de trabajar con módulos.

Aquí tienes un ejemplo de cómo crear un Module en TypeScript basada en ES Modules:

```typescript
// myModule.ts
export interface Person {
  name: string;
  age: number;
}

export function sayHello(person: Person) {
  console.log(`Hello, ${person.name}! You are ${person.age} years old.`);
}
```

```typescript
// main.ts
import { Person, sayHello } from "./myModule";

const person: Person = { name: "John", age: 25 };
sayHello(person); // Hello, John! You are 25 years old.
```

En este ejemplo, hemos creado un archivo `myModule.ts` que contiene una interfaz `Person` y una función `sayHello`. Utilizamos la palabra clave `export` para exportar estas entidades y hacerlas accesibles desde otros archivos. Luego, en el archivo `main.ts`, importamos las entidades necesarias desde el módulo y las utilizamos.

Para utilizar el sistema de módulos ES Modules en TypeScript, también necesitarás ajustar tu archivo `tsconfig.json` y establecer la opción `"module": "es6"`.

Otro ejemplo pero utilizando CommonJS:

```javascript
// myModule.js
exports.Person = function(name, age) {
  this.name = name;
  this.age = age;
};

exports.sayHello = function(person) {
  console.log("Hello, " + person.name + "! You are " + person.age + " years old.");
};
```

En CommonJS, en lugar de utilizar las palabras clave `export` e `import`, se utilizan las asignaciones a `exports` para exportar y `require` para importar.

Para utilizar este módulo en otro archivo con CommonJS, puedes hacer lo siguiente:

```javascript
const myModule = require("./myModule");

const person = new myModule.Person("John", 25);

myModule.sayHello(person);
```

En este ejemplo, utilizamos `require` para importar el módulo `myModule.js` desde el archivo actual. Luego, creamos un objeto `person` utilizando la función `Person` del módulo y llamamos a la función `sayHello` pasándole ese objeto.

Recuerda que para utilizar el sistema de módulos CommonJS en TypeScript, también necesitarás ajustar tu archivo `tsconfig.json` y establecer la opción `"module": "commonjs"`.

Decorators:

Los Decorators en TypeScript son una característica experimental que permite modificar o extender el comportamiento de una clase, método, propiedad o parámetro de función. Los decoradores se aplican utilizando la sintaxis `@decorator` justo antes de la declaración de la entidad a la que se desea aplicar.

Aquí tienes un ejemplo de cómo utilizar un Decorator en TypeScript:

```typescript
function log(target: any, key: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;

  descriptor.value = function (...args: any[]) {
    console.log(`Calling method ${key} with arguments ${args}`);
    return originalMethod.apply(this, args);
  };

  return descriptor;
}

class MyClass {
  @log
  greet(name: string) {
    console.log(`Hello, ${name}!`);
  }
}

const instance = new MyClass();
instance.greet("John"); // Calling method greet with arguments John // Hello, John!
```

En este ejemplo, hemos creado un Decorator llamado `log` que intercepta y registra la llamada a un método. El Decorator modifica la implementación original del método para que registre el nombre del método y los argumentos antes de ejecutar el código original.

Más ejemplos de decorator para una mayor comprensión:

**Ejemplo 1: Decorador de clase**
```typescript
function logClass(target: any) {
  console.log(`Class ${target.name} is being logged.`);
}

@logClass
class MyClass {
  // ...
}
```

En este ejemplo, el decorador `logClass` se aplica a la clase `MyClass`. Cuando se crea una instancia de `MyClass`, el decorador imprimirá en la consola el mensaje "Class MyClass is being logged." Esto permite registrar y realizar acciones adicionales cuando se crea una instancia de la clase.

**Ejemplo 2: Decorador de método**
```typescript
function logMethod(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  console.log(`Method ${propertyKey} is being logged.`);
}

class MyClass {
  @logMethod
  myMethod() {
    // ...
  }
}
```

En este caso, el decorador `logMethod` se aplica al método `myMethod` dentro de la clase `MyClass`. Cada vez que se invoca `myMethod`, el decorador imprimirá en la consola el mensaje "Method myMethod is being logged." Esto permite registrar y realizar acciones adicionales antes o después de la ejecución del método.

**Ejemplo 3: Decorador de parámetro**
```typescript
function logParameter(target: any, propertyKey: string, parameterIndex: number) {
  console.log(`Parameter ${parameterIndex} of ${propertyKey} is being logged.`);
}

class MyClass {
  myMethod(@logParameter param1: string, @logParameter param2: number) {
    // ...
  }
}
```

En este ejemplo, el decorador `logParameter` se aplica a los parámetros `param1` y `param2` del método `myMethod` en la clase `MyClass`. Cada vez que se llama a `myMethod`, el decorador imprimirá en la consola un mensaje indicando el índice del parámetro que está siendo logueado. Esto puede ser útil para registrar información adicional sobre los parámetros utilizados en un método.

Estos son solo algunos ejemplos básicos de cómo se pueden usar los decoradores en TypeScript. Los decoradores ofrecen una forma flexible de extender y modificar el comportamiento de las clases, métodos, propiedades y parámetros. Puedes combinar múltiples decoradores y crear decoradores personalizados para adaptarlos a tus necesidades específicas en tu aplicación o proyecto.

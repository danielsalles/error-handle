# error-handle
Estava iniciando começando um novo projeto, percebi o quão chato e impreciso é lidar com erros.
Geralmente em muitas libs, ou exemplo que vejo no github, o tratamento de erro utilizado é um simples `Error("msg")` e pronto.
Sinceramente não está errado, mas também você não irá conseguir ter um maior controle, alias até consegue se invés da mensagem você passar código, e no handle do catch você trata esse erro. Mas isso ai já está ficando um pouco chato de mais, e provavelmente você em alguma hora esquecerá tratar esse erro.

Pensando exatamente nisso, tive uma ideia de uma lib para fazer tudo isso.

Tive a ideia de criar 

##Níveis

A ideia é o programador ter controle e poder total de tudo, sem muito esforço. 

> Calma, eu explico isso.

Existem vários tipos de erros, logo alguns erros tem que ser melhor tratados que outros. Alguns tem que enviar logs por email ou colocado no DB.
Se você usar da forma `Error("msg")` você terá que tratar no `catch`.
Com os níveis novos quando for carregar os erros na sua aplicação você poderá configurar isso.
Existem 3 tipos de erros e eles são.

**1. BaseError**

`BaseError` será herdado de `Error`. 

**2. MediumError**

`MediumError` será herdado de `BaseError`.

**2. CriticalError**

`CriticalError` será herdado de `MediumError`.

Quando você iniciar o `error-handle` você terá a opção de passar algumas opções customizadas.

**Exemplo:**
```js
const Error = require("error-handle")

const fnEmail = () => {
  //Função que envia email para o administrador
}

const ActionsCriticalError = {
  {
    name: 'db-fail',
    msg: "DB connection failure",
    fn: [fnEmail]
  }
}
Error.setCritical(ActionsCriticalError);
```
>A cada novo erro vou ter que criar isso, ai? Não!

Existe uma forma também de criar um `global` para cada um deles.

**Exemplo:**
```js
const CriticalError = require("error-handle");

const fnEmail = () => {
  //Função que envia email para o administrador
}

const CriticalGlobal = {
  {
    fn: [fnEmail]
  }
}
Error.setCriticalAll(CriticalGlobal);
```
>E tem como eu escolher isso na hora que estiver dando a excessão? Claro!

Existe uma forma de criar esse objeto no momento que for chamar o error.

**Exemplo:**
```js
const CriticalError = require("error-handle").CriticalError;
const fnDB = () => {
  //Função que grava no DB.
}
try{
  throw new CriticalError({
    name: 'fail-request',
    msg: 'Could not send request.',
    fn: [fnDB]
  })
}
catch(err){

}
```

##CONTINUA!

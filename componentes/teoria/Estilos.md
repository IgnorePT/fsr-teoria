# Estilos em React

Existem neste momento 3 formas de estilizar os nossos componentes em React

1. Inline Styles
2. Css Regular (com recurso a propriedade className)
3. CSS in JS

## Style Prop

Em HTML:

```html
<!-- HTML -->
<div style="margin: 20px; background-color: red;"></div>
```

Em REACT:

```jsx
//JSX
const div = <div style={{ margin: 20, backgroundColor: "red" }} />;
```

Reparem que em React utilizamos `{{ ... }}` que é uma combinação entre uma expressão de JSX e um objeto em JS. Ou seja o que estamos a passar é um objeto dinamicamente para a expressao JSX. Poderiamos ter escrito o mesmo exemplo da seguinte forma:

```jsx
//JSX
const divStyle = {
	margin: 20,
	backgroundColor: "red",
};

const div = <div style={divStyle} />;
```

Temos de ter de ter em atenção que ao escrevermos as propriedades que queremos passar deixamos de utilizar o `kebab-cased (margin-top: xxx)` e passamos a utilizar `camelCased (marginTop: xxx)`. Esta prop de estilo é mapeada para a reperentação correspondente em DOM Nodes.

Mais informação em [CSSStyleDeclaration](https://developer.mozilla.org/en-US/docs/Web/API/CSSStyleDeclaration)

## ClassName prop

Como discutimos anteriormente em HTML nos utilizamos o sistema de classes para identificarmos um elemento para mais tarde ser modificado com recurso a `CSS`.

Em `JSX` implementamos o mesmo sistema mas com recurso a prop `className` de React,isto porque como foi falado a palavra `class` é reservada em JS.

`HTML`

```CSS
 /* Ficheiro CSS - style.css */
 .box{
        width: 100px;
        height: 100px;
        background-color: blue;
    }

```

```HTML
<!-- Utilização em HTML -->
<div class="box"></div>
```

`JSX`

```CSS
 /* Ficheiro CSS - style.css */
 .box{
        width: 100px;
        height: 100px;
        background-color: blue;
    }

```

```JSX
// Utilização em JSX
const div = <div className="box"></div>
```

### **Quais são os inconvenientes desta forma de trabalhar?**

Neste formato teremos que garantir que todas as classes são únicas e não se repetem ao logo da nossa aplicação ou seja temos as mesmas preocupações que quando estamos a estruturar aplicações em HTML e CSS puro porque os estilos gerados são partilhados pela nossa aplicação.

Mas existe alguma forma de garantirmos que as classes geradas pelos componentes sejam única e excusivas do componente?

## CSS Modules

É aqui que entra em acção os CSS Modules, mas o que são `CSS Modules`.

Os novos ambientes de desenvolvimeno (CRA e VITE) possuem um conjunto de ferramentas que nos permitem ir muito alem a criação de sistema de componentes em pastas para serem importados e exportados eles permitem compilar trechos de codigo no momento do Build que nos permitem simplificar todos o processo de desenvolvimento.

`CSS Modules` é uma abordagem para estilizar componentes em React de forma isolada e modular. Com `CSS Modules`, podemos escrever estilos em ficheiros separados e aplicá-los a componentes específicos, garantindo que os estilos sejam exclusivos para cada componente e não afetem outros elementos da página.

### Mas como usamos esta funcionalidade

1. Primeiro é necessario criarmos a nossa arbore de componente que pretendemos utilizar neste exemplo vamos criar um componente `Box` que não é nada mais nem nada menos do que uma `div` com a classe `box` e um texto a dizer `"this is a box component!"`

```JSX
// Nosso componente Box
const Box = () => {

    return (
        <div className="box">
            This is a box component!
        </div>
    )
}

export default Box;
```

2. Criamos o ficheiro `style.module.css` com os estilos prerendidos no nosso caso criamos uma classe `.box` com a sua configuração `css`

```CSS
/* CSS para o nosso elemento box */
.box{
    display: flex;
    background-red: red;
}
```

3. Depois de criarmos o ficheiro com a estrutura do passo 2. Passamos a importar o ficheiro no nosso componenete para podermos utilizar os seus recursos.

```JSX
// Importação dos modules em css
import style from "./style.module.css"

const Box = () => {

    return (
        <div className={style.box}>
            This is a box component!
        </div>
    )
}

export default Box;
```

### Porque criamos o ficheiro desta forma:

Para o nossos sistema de Modules funcionar temos que indicar ao nosso sistema de desenvolvimento que queremos que processe o ficheiro css de uma forma diferente.

No nosso caso fazemos essa indicação com recurso a nomearmos o ficheiro com a terminação `.module.css`.

Desta forma a componente do nome onde inserimos o `style` é completamente opcional e podemos colocar o nome que quisermos desde que seja respeitada a regra de colocarmos o prefixo `.module.css` em vez de so `.css`.

Assim sendo o que irá acontecer é que o sistema ira processar o nosso ficheiro `CSS` e por cada `classe` ou `id` que encontrar no ficheiro ira retornar um objeto contendo uma chave com cada elemento criado no nosso caso no objeto `style` recebemos uma chave `box`

```JSX
// Importação dos modules em css
import style from "./style.module.css"

//Accedermos a Chave com a nossa classe CSS
style.box

//Usamos desta forma
<div className={style.box}>

// Com destructuring
import { box } from "./style.module.css"

//Usamos desta forma
<div className={box}>
```

E é assim que conseguimos criar classes unicas reaproveitaveis que não irão poluir nem afetar o resto da nossa aplicação

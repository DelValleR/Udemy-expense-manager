The idea is to build a component tree, having a root component wich one is render with React-DOM

React detects when is an HTML element and when is from React becuase of the Capital letters that we have to use to name the components in React

En JSX solo se debe de tener un root element por cada "return" que se utilice

## 28/06/2022
comienzo la seccion 5 del curso de Udemy, la clase 63 es una introduccion breve sobre lo que veremos en la seccion, a esta altura del curso, sabemos como construir componentes, trabajar con multiples componentes como trabjas el "State" y como manejar los eventos de los usuarios

En esta seccion se trabjará la renderizacion de listas (rendering list) y renderizacion de contenido condicional (Conditional content), asi que veremos como podemos generar matrices de datos en nuestra pagina y como podemos mostrar contenido diferente basado en diferentes condiciones

### Clase 64: REndering lists of data
Lo que se hara es dejar a un lado el hard coding y empezaremos con los datos dinamicos, asi que por eso debemos de cambiar nuestro codigo, por que en las aplicaciones reales no sabremos que datos hay que renderizar, asi que en el archivo de Expenses.js vamos a dejar de tener un array de objetos para tener un array de JSX, un array que tenga los gastos, esto se hara a traves del metodo .map de JavaScript (.map toma una funcion como argumento y esa funcion se ejecuta para cada elemento en el array y obtiene ese elemento para el que se esta ejecutando actualmente como parametro), asi que se dara el "expense" como parametro y despues en el cuerpo de la funcion tenemos que devolver el elemento de JSX en el que se quiere mapear ese "expense", es decir se transforma el objeto normal en un objeto JSX especial, entonces se borra ese array de 4 elementos predeterminado para dejar el objeto JSX solamente, para que quede asi:
{props.items.map((expense) => (
          <ExpenseItem
          title={expense.title}
          amount={expense.amount}
          date={expense.date}
          />
        ))}
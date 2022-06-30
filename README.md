The idea is to build a component tree, having a root component wich one is render with React-DOM

React detects when is an HTML element and when is from React becuase of the Capital letters that we have to use to name the components in React

En JSX solo se debe de tener un root element por cada "return" que se utilice

## 28/06/2022
comienzo la seccion 5 del curso de Udemy, la clase 63 es una introduccion breve sobre lo que veremos en la seccion, a esta altura del curso, sabemos como construir componentes, trabajar con multiples componentes como trabjas el "State" y como manejar los eventos de los usuarios

En esta seccion se trabjará la renderizacion de listas (rendering list) y renderizacion de contenido condicional (Conditional content), asi que veremos como podemos generar matrices de datos en nuestra pagina y como podemos mostrar contenido diferente basado en diferentes condiciones

### Clase 64: Rendering lists of data
Lo que se hara es dejar a un lado el hard coding y empezaremos con los datos dinamicos, asi que por eso debemos de cambiar nuestro codigo, por que en las aplicaciones reales no sabremos que datos hay que renderizar, asi que en el archivo de Expenses.js vamos a dejar de tener un array de objetos para tener un array de JSX, un array que tenga los gastos, esto se hara a traves del metodo .map de JavaScript (.map toma una funcion como argumento y esa funcion se ejecuta para cada elemento en el array y obtiene ese elemento para el que se esta ejecutando actualmente como parametro), asi que se dara el "expense" como parametro y despues en el cuerpo de la funcion tenemos que devolver el elemento de JSX en el que se quiere mapear ese "expense", es decir se transforma el objeto normal en un objeto JSX especial, entonces se borra ese array de 4 elementos predeterminado para dejar el objeto JSX solamente, para que quede asi:

```JSX
<{props.items.map((expense) => (
    <ExpenseItem
    title={expense.title}
    amount={expense.amount}
    date={expense.date}
    />
  ))}
```

### Clase 65: Using stateful lists
En App.js importamos el "useState" para declararlo en la funcion princpial y poder dejar de tener ese array de objetos estatico, asi que sacamos ese array, lo nombramos como "DUMMY_EXPENSES" y creamos el nuevo array:

`const [expenses, setExpenses] = useState(DUMMY_EXPENSES)`

Recodar que la forma mas limpia para actualizar el estado seria asi:
```JSX
const [expenses, setExpenses] = useState(DUMMY_EXPENSES)
  
  const addExpenseHandler = expense => {
    setExpenses(prevExpenses => {
      return [expense, ...prevExpenses];
    });
  }
  ```

Ahora con esto se convierte en una lista dinamica.

### Clase 66: Understanding "keys"
Como tenemos la aplicacion hasta ahora, en la consola nos aparece una "Key warning" la cual esta para asegurarse de que React pueda actualizar y renderizar dichas listas de manera eficiente sin perdidas de rendmiento o errores que pueden ocurrir. Lo que esta pasando en este momento es que cuando estamos agragando un gasto, es decir un nuevo elemento, React presenta este nuevo elemento como el ultimo elemento en la lista de los <div> y actualiza todos los elementos y reemplaza su contenido de modo que nuevamente coincia con el orden de los elementos en el array, esto puede llevar a problemas en el rendimiento y hasta a errores, por eso es que tenemos una manera de decirle a React donde queremos agregar un nuevo elemento, al identificarlos con ID unicas con esta propiedad: `key={expense.id}`

### Assignment 3: Working with list
Pusimos a funcionar el filtro por año, asi que hay que transformar la fecha a un string para poder filtrarlo y hacer una funcion de esta manera:
```JSX
const filteredExpenses = props.items.filter(expense => {
  return expense.date.getFullYear().toString() === filteredYear;
});
```

### Clase 67: Outputting Conditional Content
Se pondra un mensaje cuando no haya nada que mostrar cuando se aplique el filtro por año.
El contenido condicional consiste en generar diferentes resultqados en diferentes condiciones, en esta caso este seria el resultado en codigo:
```JS
{filteredExpenses.length === 0 ? (
    <p>No expenses found.</p>
  ) : (
    filteredExpenses.map((expense) => (
      <ExpenseItem
        key={expense.id}
        title={expense.title}
        amount={expense.amount}
        date={expense.date}
      />
    ))
  )}
```
Aunque lo que podemos hacer para mantener el codigo aun mas limpio es generar una variable con un condicional para que este genere el mismo resultado que estamos haciendo arriba de este, pero teniendo un elemento mas reutilizable:
```JS
let expensesContent = <p>No expenses found.</p>;

  if (filteredExpenses.length > 0) {
    expensesContent = filteredExpenses.map((expense) => (
      <ExpenseItem
        key={expense.id}
        title={expense.title}
        amount={expense.amount}
        date={expense.date}
      />
    ));
  }
```
Y asi dentro del JSX return solo llamariamos la variable: `{expensesContent}`

### Clase 68: Adding conditional return statements
Se agrega un nuevo componente para el filtro de la lista de gastos, para que quede mas limpio el espacio de trabajo en Expenses.js, el componente se llama ExpensesList (.js y .css) cambiamos la logica de este filtro, al eliminar la variable y encerrar los gastos en una lista por que le estamos pidiendo a toda la lista que se renderice de nuevo

## 29/06/2022

### Assignment 4
Se va a renderizar el form condicionalmente y que no aparezca inmediatamente, que primero aparezca un boton que abra el form.
Se agrega el boton en el JSX del archivo NewExpense.js, hay que agregar el state para que muestre condicionalmente o el boton o el formulario en este archivo, se necesita un true or false para que se decida si el estado se muestra o no, entonces asi declaramos el state primer: `const [isEditing, setIsEditing] = useState(false);`
Ahora agregamos otra nueva funcion, la de start editing handler function que es la que dice que setIsEditing true y se aplica cuando el boton de Add New Expense se activa, lo que lleva a que se muestre el form.
Al boton se le agrega el onClickcon el startEditingHandler, ahora hayq ue controlar cual se muestra con la funcion del useState, queremos mostrar el boton si no se esta editando, para lo que nos queda esta sintaxis: `{!isEditing && <button onClick={startEditingHandler}>Add New Expense</button>}` con el signo de exlamacion significa que si es falso el isEditing aparece el boton y cuando no es flaso aparece el form, por lo que se debe escribir asi: `{isEditing && <ExpenseForm onSaveExpenseData={saveExpenseDataHandler} />}`
Teniendo listo esto, agregamos el boton de "Cancel" dentro de ExpenseForm.js al lado del de "Add Expense", con el tipo de boton "button" para que no haga el submit predeterminado y agregarle el onClick para hacer una funcion cuando se le presiona, pero no queremos que sea una funcion que se ejecuta dentro del archivo de ExpenseForm.js, si no del archivo de NewExpense.js por lo que creamos esta funcion:
```JSX
const stopEditingHandler = () => {
  setIsEditing(false);
}
```
y crear un pointer dentro del componente de ExpenseForm que quede asi: 
```JSX  
{isEditing && (
  <ExpenseForm 
    onSaveExpenseData={saveExpenseDataHandler} 
    onCancel={stopEditingHandler} 
  />
)}
```
Y asi pasarle el props dentro del ExpenseForm.js al boton de "Cancel": `<button type='button' onClick={props.onCancel}>Cancel</button>`

Tambien queremos cerrar el form cuando ha sido enviado, entonces en el saveExpenseDataHandler, debemos poner el setIsEditing falso

### Clase 69: Demo App: Adding a Chart
Se creara un grafico de barras, por lo que agregamos un nuevo componente llamado Chart, dentro del cual crearemos los archivos "Chart" y "ChartBar" con sus respectivos .js y .css.
Comenzamos con el archivo de Chart, dentro del cual llamaremos con props al ChartBar dentro del JSX, entonces se generan las chartbar dinamicamente para pasar por una matriz de puntos de datos y mapear cada punto de datos a una chartbar, asi que se crearan chartbars tanto como se tengan dataPoints, esperamos que el valor del chartbar sea el valor del dataPoint y que asi mismo se pueda crear su maximo valor, tambien le agregamos la propiedad "key" por que ayuda a React a renderizar los elementos de una lista de manera eficiente, entonces queda escrito en codigo asi:
```JSX
const Chart = props => {
  return <div className="chart">
    {props.dataPoints.map(dataPoint => 
    <ChartBar 
    key={dataPoint.label}
    value={dataPoint.value} 
    maxValue={null}
    label={dataPoint.label}
    />
    )}
  </div>
};
```
## 30/06/2022

### Clase 70: Adding Dynamic Styles
Se hara el estilo del chartbar, en el cual el grafico es dinamico, asi que comenzamos con el archivo ChartBar.js, dentro del cual creamos algunos div para dividir los componentes del grafico y tenemos que agregar una variable que dfenimos asi: ` let barFillHeight = '0%';` y despues hacemos una logica de JavaScript para que la grafica quede realmente dinamica, asi:
```JS
if (props.max < 0) {
  barFillHeight = Math.round((props.value / props.maxValue) * 100) + '%';
}
```
Despues agregamos el estilo de un elemento de forma dinamica y esto se puede hacer agregando el atributo de style añadiendo la funcion de JavaScript: 
```JS
<div className="chart-bar__fill" style={{ height: barFillHeight }}></div>
```
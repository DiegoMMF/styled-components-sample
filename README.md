# Ejemplos de uso de Styled-Components
(Fuente: Platzi)

El ejemplo consiste en dos contadores de películas, cada uno de ellos posee un color asignado y una cantidad predeterminada de valores a ser sumados/restados con botones.

Cada vez que pasamos con el mouse por arriba de uno de los contadores, el color de fondo de la página cambia al color asignado a dicho contador.

Además, cuando llegamos a los tope inferior o superior de las cantidades, los botones se deshabilitan. 

## Caso 1: Elementos HTML envueltos fuera del archivo JSX

1. Creamos un archivo separado para el componente que vamos a crear.

```javascript
import styled from 'styled-components';

export const Title = styled.h2`
  padding-bottom: 10px;
  border-bottom: 1px solid ${(p) => p.theme.color2};
`;
```

2. Importamos el componente en el archivo donde lo vamos a usar.

```javascript
import { Title } from './Title';
```

3. Usamos el componente en el archivo donde lo importamos.

```javascript
<Title>Styled Components</Title>
```

## Caso 2: Estilización de elementos dentro de un mismo archivo

```javascript
import React from 'react';
import styled from 'styled-components';

const StyledForm = styled.form`
  font-family: courier;
  margin: 0 50px 25px;
  padding: 10px 25px 25px;
  text-align: center;
  transform: scale(1);
  transition: transform 0.3s;

  &:hover {
    transform: scale(1.2);
  }
`;

const StyledButton = styled.button`
  cursor: pointer;
  padding: 5px 10px;
  border: 1px solid transparent;
  transition: 0.15s border-color;

  &:hover {
    border-color: ${(p) => p.theme.color2};
  }

  &[disabled] {
    background: ${(p) => p.theme.color2};
  }
`;

export default function Form(props) {
  const [quantity, setQuantity] = React.useState(0);
  const { movie } = props;

  return (
    <StyledForm onMouseEnter={() => props.updateTheme()}>
      <h3>{movie.name}</h3>
      <StyledButton
        type="button"
        onClick={() => setQuantity(quantity - 1)}
        disabled={quantity <= 0}>
        -
      </StyledButton>
      {quantity}
      <StyledButton
        type="button"
        onClick={() => setQuantity(quantity + 1)}
        disabled={quantity >= movie.available}>
        +
      </StyledButton>
    </StyledForm>
  );
}
```

## Caso 3: Seteamos themes de forma dinámica con sólo :hover sobre un componenente determinado.

Para lo cual, debemos crear un ThemeProvider y un GlobalStyle (que incluiremos en alguna parte de nuestro proyecto)

```javascript
import { ThemeProvider } from 'styled-components';
import { GlobalStyle } from './GlobalStyle';

const themes = {
  avengers: {
    bg: '#ceceff',
    color: '#335',
    color2: 'rgba(5, 5, 100, 0.5)',
  },
  terminator: {
    bg: '#ccffcc',
    color: '#010',
    color2: 'rgba(50, 100, 50, 0.5)',
  },
};

export const Theme = (props) => (
  <ThemeProvider theme={themes[props.theme.toLowerCase()]}>
    <GlobalStyle />
    {props.children}
  </ThemeProvider>
);
```

Finalmente, creamos la lógica en el GlobalStyle:

```javascript
import { createGlobalStyle } from 'styled-components';

export const GlobalStyle = createGlobalStyle`
  html,
  body {
    background: ${(p) => p.theme.bg};
  }
`;
```
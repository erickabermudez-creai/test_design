# test_design

Galería de componentes para compartir con el equipo de desarrollo lo que se diseña en
**Claude Design**. Cada componente vive en su propia carpeta con su archivo de Storybook,
para que los devs puedan verlo, interactuar con sus props, y copiarlo a su proyecto.

## Requisitos

- Node.js 20+
- npm

## Uso

```bash
npm install
npm run storybook
```

Abre Storybook en `http://localhost:6006`.

Otros comandos útiles:

```bash
npm run build-storybook   # genera una versión estática en storybook-static/ (para publicar/compartir)
npm run dev               # levanta la app de Vite normal (no es lo importante en este repo)
```

## Stack

- **React + TypeScript + Vite**
- **Tailwind CSS v4** — para poder pegar directo el JSX/HTML con clases de Tailwind que
  exporta Claude Design, sin tener que traducir estilos.
- **Storybook 10** — vitrina de componentes con controles interactivos.

## Cómo agregar un componente nuevo

1. Crea una carpeta dentro de `src/components/` con el nombre del componente (PascalCase),
   por ejemplo `src/components/Card/`.
2. Dentro, agrega dos archivos:
   - `ComponentName.tsx` — el componente en sí.
   - `ComponentName.stories.tsx` — su historia de Storybook.
3. Pega el JSX que generaste en Claude Design dentro del `.tsx`, ajustando solo lo
   necesario para que sea un componente de React válido (props, imports, etc.).

Ejemplo mínimo de story, basado en el componente `Button` que ya está en este repo
(`src/components/Button/`):

```tsx
// src/components/Card/Card.stories.tsx
import type { Meta, StoryObj } from '@storybook/react-vite';
import { Card } from './Card';

const meta = {
  title: 'Components/Card',
  component: Card,
  parameters: { layout: 'centered' },
  tags: ['autodocs'],
} satisfies Meta<typeof Card>;

export default meta;
type Story = StoryObj<typeof meta>;

export const Default: Story = {
  args: {
    title: 'Título de ejemplo',
  },
};
```

4. Corre `npm run storybook` y verifica que tu componente aparezca en el sidebar bajo
   **Components**.

Storybook detecta automáticamente cualquier archivo `*.stories.tsx` dentro de
`src/components/`, así que no hace falta registrar nada más a mano.

## Convenciones

- Un componente por carpeta, con el mismo nombre para la carpeta y el archivo principal.
- Usa `tags: ['autodocs']` en el `meta` de cada story para que Storybook genere la
  documentación de props automáticamente.
- Prefiere props tipadas con una interfaz `ComponentNameProps` exportada, para que los
  controles de Storybook y el autocompletado funcionen bien.
- Si el componente necesita datos o estado más complejo, agrega variantes como stories
  adicionales (`Primary`, `Disabled`, `WithLongText`, etc.) en vez de lógica condicional
  dentro del `.tsx`.

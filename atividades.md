# Backstage: Aula 2 - Customizações Visuais e Funcionais

## 📝 Atividades

**Objetivo:** Realizar customizações visuais e funcionais na instância local do Backstage.

### \[ ] 1. Alterar nome do produto

* Atualizar `app-config.yaml`
* Atualizar `manifest.json`

### \[ ] 2. Alterar logos

* Substituir arquivos de ícones e logos
* Atualizar componentes `LogoFull.tsx` e `LogoIcon.tsx`

### \[ ] 3. Customização de cores

* Criar arquivo de tema
* Aplicar novo tema em `App.tsx`

### \[ ] 4. Customização de componentes

* Adicionar `styleOverrides` nos componentes desejados
* Exemplo com `MuiCard`

---

## ⚠️ Pontos de Atenção

### 🪺 Customizar com Segurança

* Evite alterar fork do Backstage e plugins nativos se não for para contribuir
* Use o comando oficial:

  ```bash
  npx @backstage/create-app@latest
  ```
* Prefira *overrides* nos arquivos de configuração e temas

---

## 🎨 Passo a Passo das Customizações

### 1. Alteração de Nome do Produto

#### Arquivo: `app-config.yaml`

```yaml
app:
  title: Minha Plataforma
  baseUrl: http://localhost:3000
organization:
  name: Minha Empresa
```

#### Arquivo: `packages/app/public/manifest.json`

```json
{
  "short_name": "MinhaPlataforma",
  "name": "Minha Plataforma",
  "icons": [
    {
      "src": "favicon.ico",
      "sizes": "48x48",
      "type": "image/png"
    }
  ],
  "start_url": "./index.html",
  "display": "standalone",
  "theme_color": "#000000",
  "background_color": "#ffffff"
}
```

Esse arquivo é essencial para que o PWA funcione corretamente.

---

### 2. Alteração de Logos

Substitua os arquivos listados abaixo na pasta `packages/app/public` com os seus logos personalizados.

| Arquivo                      | Formato | Tamanho Recomendado |
| ---------------------------- | ------- | ------------------- |
| `favicon.ico`                | ICO     | 32x32               |
| `favicon-16x16.png`          | PNG     | 16x16               |
| `favicon-32x32.png`          | PNG     | 32x32               |
| `android-chrome-192x192.png` | PNG     | 192x192             |
| `apple-touch-icon.png`       | PNG     | 192x192             |

#### Componentes

* `packages/app/src/components/Root/LogoFull.tsx`
* `packages/app/src/components/Root/LogoIcon.tsx`

Substitua os SVGs por suas versões personalizadas ou use imagens.

---

### 3. Customização de Cores

#### Criar tema personalizado

Crie o arquivo `packages/app/src/themes/customTheme.ts`:

```ts
import {
  createBaseThemeOptions,
  createUnifiedTheme,
  palettes,
} from '@backstage/theme';

export const darkTheme = createUnifiedTheme({
  ...createBaseThemeOptions({
    palette: {
      ...palettes.dark,
      primary: { main: '#00ff96' },
      secondary: { main: '#8991dd' },
      background: { default: '#000000', paper: '#121212' },
      navigation: {
        background: '#121212',
        indicator: '#00ff96',
        color: '#c0c0c0',
        selectedColor: '#00ff96',
      },
    },
  }),
});
```

#### Aplicar tema em `App.tsx`

```tsx
import { darkTheme } from './themes/customTheme';

const themes = [
  {
    id: 'dark',
    title: 'Dark Mode',
    variant: 'dark',
    icon: <Brightness2Icon />,
    Provider: ({ children }) => (
      <UnifiedThemeProvider theme={darkTheme}>
        {children}
      </UnifiedThemeProvider>
    ),
  },
];
```

---

### 4. Customização de Componentes

#### Exemplo: `MuiCard`

```ts
components: {
  MuiCard: {
    styleOverrides: {
      root: ({ theme }) => ({
        borderRadius: '7px',
        backgroundColor: 'transparent',
        border: `1px solid ${theme.palette.divider}`,
      }),
    },
  },
},
```

#### Exemplo: Header

```ts
defaultPageTheme: 'home',
pageTheme: {
  home: genPageTheme({
    colors: ['#8c4351', '#343b58'],
    shape: `url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg"><rect width="100%" height="100%" fill="%23111C1B"/></svg>')`,
  }),
  app: genPageTheme({
    colors: ['#8c4351', '#343b58'],
    shape: shapes.wave,
  }),
},
```

---

## ⛏ Troubleshooting

* **Erro de dependências:** verifique se `@backstage/theme` está atualizado.
* **No Windows sem WSL:**
  * Instale o Visual Studio Build Tools (incluindo C++ Build Tools)
 
* **Erro com yarn:**

  ```bash
  rm -rf node_modules && yarn cache clean && yarn install
  ```

---

## 🔗 Referências Oficiais e Complementares

Backstage Theme:
  * [https://backstage.io/docs/getting-started/app-custom-theme/](https://backstage.io/docs/getting-started/app-custom-theme/)
  * [https://github.com/backstage/backstage/tree/master/packages/theme/src/base](https://github.com/backstage/backstage/tree/master/packages/theme/src/base)
  * [https://github.com/backstage/backstage/blob/master/packages/theme/src/base/palettes.ts](https://github.com/backstage/backstage/blob/master/packages/theme/src/base/palettes.ts)
 
App Customization:
* Gerador de formas SVG: [https://app.haikei.app/](https://app.haikei.app/)
* Guia PWA/Manifest: [https://developer.mozilla.org/en-US/docs/Web/Manifest](https://developer.mozilla.org/en-US/docs/Web/Manifest)

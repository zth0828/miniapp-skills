# TypeScript Setup

## tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ES2015",
    "module": "CommonJS",
    "strict": true,
    "jsx": "preserve",
    "allowJs": true,
    "outDir": "./",
    "rootDir": "./",
    "typeRoots": ["./node_modules/miniprogram-api-typings"]
  },
  "include": ["**/*"]
}
```

## Install types

```bash
npm install miniprogram-api-typings
```

## Page TS example

```typescript
import { IAppOption } from '../../app';

interface IData {
  title: string;
}

Page<IData, {}>({
  data: { title: 'Hello TS' },
  onLoad(options: Record<string, string>) {
    console.log(options);
  }
});
```

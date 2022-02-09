# setup-graphql-codegen
A place I can copy scripts faster

## 1. Install dependencies:
I use TypeScript as much as I can so it will be here as well.
```bash
yarn add -D -E \
    @graphql-codegen/cli \
    @graphql-codegen/introspection \
    @graphql-codegen/typescript \
    @graphql-codegen/typescript-graphql-files-modules \
    @graphql-codegen/typescript-operations \
    @graphql-codegen/typescript-react-apollo \
    @graphql-eslint/eslint-plugin
```

## 2. Create a file `codegen.yaml` at root folder:
```yaml
overwrite: true
watch: true
schema:
  - http://localhost:1337/v1/graphql:
      headers:
        x-hasura-admin-secret: "nhost-admin-secret"
documents: "graphqls/**/*.graphql"
generates:
  ./graphqls/index.ts:
    hooks:
      afterOneFileWrite:
        - "eslint --config .eslintrc.json --fix"
    plugins:
      - "typescript"
      - "typescript-operations"
      - "typescript-react-apollo"
    config:
      withHooks: false
      useTypeImports: true
      namingConvention:
        typeNames: "change-case#pascalCase"
        enumValues: "change-case#pascalCase"
        transformUnderscore: true
      scalars:
        uuid: string
        timestamptz: string
        geography: "utils/types#Geography"

  ./graphqls/schema.json:
    plugins:
      - "introspection"
    config:
      minify: true
```

## 3. Config ESLint for GraphQL files

```json
{
  "overrides": [
    {
      "files": ["*.graphql"],
      "parser": "@graphql-eslint/eslint-plugin",
      "plugins": ["@graphql-eslint"],
      "rules": {
        "@graphql-eslint/known-type-names": "error"
      },
      "parserOptions": {
        "schema": "./graphqls/schema.json"
      }
    },
    {
      "other": "configs here"
    }
  ]
}
```

Enjoy! <3

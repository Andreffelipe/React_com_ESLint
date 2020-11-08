## Configurando ESLint e Prettier em projetos React com TypeScript

Para iniciar a configuração, primeiro vamos iniciar um projeto ReactJS com um template TypeScript, este ponto é muito importante, pois vamos aproveitar da própria CLI do React, para configurar o projeto.

```sh
yarn create react-app react-com-eslint --template=typescript
#or
npx create-react-app react-com-eslint --template=typescript
```

Note que estamos criando um projeto com o nome react-com-eslint, o comando que vem depois do nome do projecto, informa a CLI que iremos adicionar um template TypeScript. agora já podemos entrar dentro do diretório do projeto, para instalar as dependências do ESLint, entretanto antes de iniciarmos essa parte, precisamos remover a configuração padrão do ESLint que já vem instalado no ReactJS.

Acesse o arquivo package.json e remova o trecho de código abaixo, não esqueça de salvar as alterações.

```javascript
"eslintConfig": {
  "extends": "react-app"
},
```

Depois das Linhas removidas, já estamos pronto para instalar as dependências do ESLint, abra o terminal na raiz do projeto e execute o seguinte comando.

```sh
yarn add eslint -D
#or
npm install eslint -D
```

Note que estamos usando o -D para instalar o ESLint como dependência de desenvolvimento, pois iremos usar ele apenas quando estivermos escrevendo nossos códigos.

## Iniciando o ESLint

Agora que o ESLint já está instalado, vamos iniciar as configurações, ainda na raiz do projeto execute o seguinte comando.

```sh
yarn eslint --init
#or
npm eslint --init
```

Em seguida você deve selecionar exatamente as mesmas configurações abaixo.

## How would you like to use ESLint? …

```sh
  To check syntax only
  To check syntax and find problems
▸ To check syntax, find problems, and enforce code style
```

## What type of modules does your project use? …

```sh
▸ JavaScript modules (import/export)
  CommonJS (require/exports)
  None of these
```

## Which framework does your project use? …
```sh
▸ React
  Vue.js
  None of these
```

## Does your project use TypeScript? ‣ No / Yes

```sh
‣ Yes
```

## Where does your code run? …

```sh
‣ ✔ Browser
✔ Node
```

## How would you like to define a style for your project? …

```sh
▸ Use a popular style guide
  Answer questions about your style
  Inspect your JavaScript file(s)
```

## Which style guide do you want to follow? …

```sh
▸ Airbnb: https://github.com/airbnb/javascript
  Standard: https://github.com/standard/standard
  Google: https://github.com/google/eslint-config-google
```

## What format do you want your config file to be in? …

```sh
  JavaScript
  YAML
▸ JSON
```

Este ponto é um dos mais importantes, o próprio ESLint vai sugerir a instalação de algumas dependências de configuração, se você estiver usando npm simplesmente aceite a instalação nesta opção.

## Would you like to install them now with npm? ‣ No / Yes

```sh
▸ Yes
```

No caso do yarn.

```sh
yarn add -D eslint-plugin-react@^7.20.0 @typescript-eslint/eslint-plugin@latest eslint-config-airbnb@latest eslint@^5.16.0 || ^6.8.0 || ^7.2.0 eslint-plugin-import@^2.21.2 eslint-plugin-jsx-a11y@^6.3.0 eslint-plugin-react-hooks@^4 || ^3 || ^2.3.0 || ^1.7.0 @typescript-eslint/parser@latest
```

Alterando o arquivo .eslintrc.json
Agora precisamos adicionar algumas alterações no arquivo `.eslintrc.json`, essas alterações serão necessárias para que o ESLint trabalha em conjunto com TypeScript, certifique-se de que esses trecho de códigos tenha exatamente a mesma estrutura ou algo parecido.

```JSON
"extends": [
  "plugin:react/recommended",
  "airbnb",
  "plugin:@typescript-eslint/recommended",
  "prettier/@typescript-eslint",
  "plugin:prettier/recommended"
],

"plugins": [
  "react",
  "react-hooks",
  "@typescript-eslint",
  "prettier"
],

"rules": {
  "no-use-before-define": "off",
  "prettier/prettier": "error",
  "react-hooks/rules-of-hooks": "error",
  "react-hooks/exhaustive-deps": "warn",
  "react/jsx-filename-extension": [ 1, {"extensions": [".tsx"]} ],
  "import/prefer-default-export": "off",
  "@typescript-eslint/explicit-function-return-type": [
    "error",
    {
      "allowExpressions": true
    }
  ],

  "import/extensions": [
    "error",
    "ignorePackages",
    {
      "ts": "never",
      "tsx": "never"
    }
  ]
},

"settings": {
  "import/resolver": {
    "typescript": {}
  }
}
```
Com as dependências instaladas vamos criar na raiz do projeto um arquivo `.eslintignore` com o conteúdo abaixo para ignorar o Linting em alguns arquivos:

```
**/*.js
node_modules
build
/src/react-app-env.d.ts
/src/reportWebVitals.ts
```

Observe que apesar de termos adicionar algumas configurações do Prettier, ainda não fizemos a instalação das dependências, isso é o que vamos fazer agora.

```bash
yarn add prettier eslint-config-prettier eslint-plugin-prettier eslint-import-resolver-typescript -D
#or
npm install prettier eslint-config-prettier eslint-plugin-prettier eslint-import-resolver-typescript -D
```

Observe que além do Prettier, estamos adicionando mais uma dependência do typescript eslint-import-resolver-typescript, por padrão o ESlint não entende arquivos .ts e .tsx, por isso se faz necessário adicionar essa dependência que já deixamos configurada no nosso arquivo .eslintrc.json na sessão settings.

Corrigindo configuração do Prettier
Por padrão algumas configurações do Prettier, consegue subscrever algumas regras do ESLint, por esse motivo iremos criar um novo arquivo da raiz do nosso projeto chamado prettier.config.js, com o arquivo criado adicione as seguintes configurações.

```javascript
module.exports = {
  singleQuote: true,
  trailingComa: "all",
  allowParens: "avoid"
}
```

Pronto, agora só falta uma coisinha, o ESLint fica ouvindo todos os nossos diretórios e arquivos dentro do projeto, porém alguns arquivos não serão necessário que ele verifique, por isso vamos ignorar alguns deles, ainda na raiz do projeto crie um novo arquivo chamado .eslintignore, em seguida adicione as seguintes configurações.

```javascript
**/*.js
node_modules
build
```

Agora sim, todas as configurações estão concluídas.

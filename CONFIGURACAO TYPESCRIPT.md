<h6>Este projeto visa criar uma aplicação para criação, alteração, e exlusão de usuario e senha a partir do node e biblioteca express/h6>



<h1>1 -Criando aplicação com node</h1>

<h5>comando: npm init </h5>

//configuração basica package.json

{
  "name": "ms-authentication",
  "version": "1.0.0",
  "description": "Microservice criado na aula da DIO",
  "main": "./dist/index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node ./",
    "build":"tsc -p ."

​	"dev": "ts-node-dev --respawn --transpile-only --ignore-watch node_modules --no-notify src/index.ts"

  },
  "repository": {
    "type": "git",
    "url": "Janaina-Costa"
  },
  "author": "Janaina Costa",
  "license": "ISC",
  "devDependencies": {
    "@types/node": "^16.10.1",
    "typescript": "^4.4.3"
  }
}



<h1>2 -Criando projeto com typescript</h1>

<h5>comando: npm install -g typescript</h5>

<h5>comando: tsc --init</h5>

//consfiguração basica tsconfig
{
  "compilerOptions": {
    "target": "es2019",                                   
    "module": "commonjs", 
    "moduleResolution": "node",                              
    "rootDir": "src",
    "typeRoots": [
      "./src/@types",
      "./node_modules/@types"
    ],
    "outDir": "./dist",
    "removeComments": true,
    "esModuleInterop": true,                             
    "forceConsistentCasingInFileNames": true,          
    "strict": true,                                     
    "skipLibCheck": true                                
}
}

//criar pastas:

> pasta: src -> pasta: @types -->arquivo: index. ts

> pasta: dist

<h1>3 - Converter arquivo index.ts em index.js</h1>

<h5>comando: npm install --save-dev typescript</h5>

<h5>comando: npm install --save-dev @types/node</h5>

<h5>comando: npm run build</h5>

//o arquivo index.ts é convertido e criado na pasta dist como index.js



<h1>4 - Criando um servidor - express</h1>

//Baixando a dependência express

<h5>comando: npm install --save express</h5>

//Baixando o types express

<h5>comando: npm install --save-dev @types/express</h5>



<h4>no arquivo index.ts</h4>

import express, {Request, Response, NextFunction} from 'express'

const app = express()

const port = 3000

const host = `http://localhost:${port}/status`



app.get('/status', (*req*:*Request*, *res*: *Response*, *next*: *NextFunction*)=>{

  *res*.status(200).send({obj:'bar'}) //200 é padrão  para status send ok

})



app.listen(port,()=>{

  console.log(`Aplicação executando na porta ${host}`)

})



<h1>5 - Automatizando o servidor</h1>

<h5>comando : npm install --save-dev ts-node-dev</h5>

> Incluir o código no script do package.json: 
>
>  "dev": "ts-node-dev --respawn --transpile-only --ignore-watch node_modules --no-notify src/index.ts"

<h5>comando : npm run dev</h5>

<h1>6 -Criando rotas apartadas para o usuário</h1>

> Criar pasta: routes  --> arquivo:users.route.ts
>
> Importar no arquivo users.route.ts: 
>
> import { Router, Request, Response, NextFunction } from "express";
>
> <b>//criar uma instancia para o Router:</b>
>
> const usersRoute = Router()
>
> <b>//configurando get/users :</b>
>
> userRoute.get('/users', ((*req*: *Request*, *res*: *Response*, *next*: *NextFunction* )=>{
>
>   const users = [{userName:' '}]
>
>   *res*.status(200).send(users)
>
> })
>
> <b>//exportando</b>
>
> export default usersRoute

> <b>Após isso importar no arquivo index.ts</b>
>
> import usersRoute from './routes/users.route'
>
> app.use(userRoute)

<p style = 'blue'><b >//configurando get/users/:uuid :</b></p>

​	userRoute.get('/users/:uuid', (*req*: *Request*<{uuid : *string*}>, *res*: *Response*, *next*: *NextFunction*) =>{

​	 const uuid = req.params.uuid

​	res.status(200).send({uuid})

})


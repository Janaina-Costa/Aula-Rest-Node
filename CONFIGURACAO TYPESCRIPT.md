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

==>app.use(express.json())

==>app.use(express.urlencoded({extended:true}))

const port = 3000

const host = `http://localhost:${port}/status`



~~*app.get('/status', (req:Request, res: Response, next: NextFunction)=>{*~~

  ~~*res.status(200).send({obj:'bar'}) //200 é padrão  para status send ok*~~

~~*})*~~

<b>A linha acima será substituida pela linha abaixo</b>

==> app.user(statusRoute)



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
> <h6>Buscador e todos os  Usuario</h6>
>
> <b>//configurando get/users :</b>
>
> userRoute.get('/users', ((*req*: *Request*, *res*: *Response*, *next*: *NextFunction* )=>{
>
> const users = [{userName:' '}]
>
> *res*.status(200).send(users)
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

<h6>Buscando usuario por id</h6>

<p style.color = 'blue'><b >//configurando get/users/:uuid :</b></p>

​	userRoute.get('/users/:uuid', (*req*: *Request*<{uuid : *string*}>, *res*: *Response*, *next*: *NextFunction*) =>{

​	 const uuid = req.params.uuid

​	res.status(200).send({uuid})

})

<h4>o que é status code?</h4>

padronização do que acontece com uma requisição. 200= ok

alternativamente a digitar os números pode ser criada constantes a partir de um pacote:

<h5>comando : npm install --save http-status-codes </h5>

import {StatusCodes} from 'http-status-codes'

***no lugar de (200) colocar ''*StatusCodes.OK":

> *res*.status(*StatusCodes*.OK).send(users)



<h6>Criando Usuario</h6>


<p style = 'blue'><b >//configurando post/users : </b></p>

usersRoute.post('/users', (*req*: *Request*, *res*: *Response*, *next*: *NextFunction* )=>{

​		const newUser = req.body

​		res.status(StatusCodes.CREATED).send(newUser)

​		<b>***testar se deu certo : console.log(req.body)</b>

})

==>paralelo a isso deve ser configurado no arquivo index.ts para que o app transforme em json a informação vinda do insomnia ou postman

  ==>app.use(express.json())

​	==>app.use(express.urlencoded({extended:true}))

> No Postman
>
> . jogar a URL
>
> .POST
>
> . Em 'Body' selecionat x-www-form-urlencoded
>
> .Digitar os elementos do objeto
>
> .Sen
>
> d
>
> 

<h6>Alterando Usuario</h6>

<p style = 'blue'><b >//configurando put/users/:uuid : </b></p>

usersRoute.put('/users/:uuid', (*req*: *Request*<{uuid : *string*}>, *res*: *Response*, *next*: *NextFunction*)=>{

​	const uuid = req.params.uuid

​	const modifiUser = *req*.body

  	modifiUser.uuid = uuid

 ****para verificar se está rodando certo :  console.log(modifiUser)

  *res*.status(*StatusCodes*.OK).send(modifiUser)

}



> altera o usuario pelo numero de id no postman para conferir



<h6>Deletando Usuario</h6>

<p style = 'blue'><b >//configurando delete/users/:uuid : </b></p>

<h1>7 -Melhorando a rota de status</h1>

Pasta routes --> novo arquivo: status.routes.ts

<b>configurando o arquivo status.routes.ts</b>

import {Router, NextFunction, Response, request} from 'express'

import {StatusCodes} from 'http-status-codes'



const statusRoute = Router()



statusRoute.get('/status', ((*req*:*Request*, *res*: *Response*, *next*:*NextFunction*)=>{

res.sendStatus(StatusCodes.OK)

})



export default statusRoute

> a seguir, na pasta index.ts, apaga  alinha:
>
> *<u>"app.get('/status', (req:Request, res: Response, next: NextFunction)=>{</u>*
>
>   *<u>res.status(200).send({obj:'bar'}) //200 é padrão  para status send ok</u>*
>
> *<u>})"</u>*
>
> 

pela linha : app.user(statusRoute)

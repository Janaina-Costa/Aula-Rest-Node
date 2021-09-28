<h6>Este projeto visa criar uma aplicação para busca, criação, alteração, e exlusão de usuario e senha a partir do node e biblioteca express/h6>



<h1 style='background-color: yellow'>1 -Configuração geral</h1>

<p style = 'color:blue'>//configuração basica package.json</p>

<h5 style = 'color : red'>comando: npm init </h5>

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



<h4 style = 'color:green'>Instalando typescript</h4>

<h5 style='color:red'>comando: npm install -g typescript</h5>

<h5 style='color:red'>comando: tsc --init</h5>

<p style = 'color:blue'>//consfiguração basica tsconfig</p>

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

<p style='color:orange' >//criar pastas:</p>

> pasta: src ->  criar arquivo: index. ts 
>
> na pasta :src =>  pasta: @types

> pasta: dist



<h4 style='color:green'> Converter arquivo index.ts em index.js</h4>

<h5 style='color:red'>comando: npm install --save-dev typescript</h5>

<h5 style='color:red'>comando: npm install --save-dev @types/node</h5>

<u>==> escrever no package json -> script :  "build":"tsc -p ."</u>

<h5 style='color:red'>comando: npm run build</h5>

<u>//o arquivo index.ts é convertido e criado na pasta dist como index.js</u>



<h4 style="color:green"> Criando um servidor com express</h4>

<p style = 'color:blue'>//Baixando a dependência express</p>

<h5 style='color:red'>comando: npm install --save express</h5>

<p style = 'color:blue'>//Baixando o types express</p>

<h5 style='color:red'>comando: npm install --save-dev @types/express</h5>



<h4 style='color:green'>No arquivo index.ts digitar ...</h4>

import express, {Request, Response, NextFunction} from 'express'

const app = express()

==>app.use(express.json())

==>app.use(express.urlencoded({extended:true}))

<!--*app.get('/status', (req:Request, res: Response, next: NextFunction)=>{*-->

  <!--*res.status(200).send({obj:'bar'}) //200 é padrão  para status send ok*-->

<!--*})-->*

<b>A linha acima será substituida pela linha abaixo quando for feira a refatoração do users.status</b>

<u>==> app.user(statusRoute)</u>



app.listen(3000,()=>{

  console.log(`Aplicação executando na porta 3000`)

})



<h4 style='color:green'> Automatizando o servidor</h4>

<h5 style='color:red'>comando : npm install --save-dev ts-node-dev</h5>

Incluir o código no script do package.json: 

"dev": "ts-node-dev --respawn --transpile-only --ignore-watch node_modules --no-notify src/index.ts"

<h5 style='color:red'>comando : npm run dev</h5>





<h1 style='background-color: yellow'>2 -Criando rotas apartadas para os usuários</h1>

> <p style='color:orange'>Criar na pasta src ==> pasta :routes  --> arquivo:users.route.ts</p>
>
> <h4 style='color:green'>No arquivo users.route.ts digitar...</h4>
>
> import { Router, Request, Response, NextFunction } from "express";
>
> import {StatusCodes} from 'http-status-codes'
>
> const usersRoute = Router()



<h4 style='background-color:pink'>o que é status code?</h4>

Padronização do que acontece com uma requisição http. ex: 200= OK

alternativamente a digitar os números pode ser criada constantes a partir de um pacote:

<h5>comando : npm install --save http-status-codes </h5>

importa :import {StatusCodes} from 'http-status-codes'

*** e no lugar de (200) colocar ''*StatusCodes.OK":

ex: *res*.status(*StatusCodes*.OK).send(users)





> <h6 style='color:purple'>Buscador de todos os  Usuario</h6>
>
> <p style='color:blue'>//configurando get/users :</p>
>
> userRoute.get('/users', ((*req*: *Request*, *res*: *Response*, *next*: *NextFunction* )=>{
>
> const users = [{userName:' '}]
>
> *res*.status(*StatusCodes*.OK).send(users)
>
> })

> <u><b>Após isso importar no arquivo index.ts</b></u>
>
> import usersRoute from './routes/users.route'
>
> app.use(userRoute)





> <h6 style='color:purple'>Buscando usuario por id</h6>

<p style = 'color:blue'><b >//configurando get/users/:uuid :</b></p>

​	userRoute.get('/users/:uuid', (*req*: *Request*<{uuid : *string*}>, *res*: *Response*, *next*: *NextFunction*) =>{

​	 const uuid = req.params.uuid

​	res.status(*StatusCodes*.OK).send({uuid})

})



<h6 style='color:purple'>Criando Usuario</h6>

<p style = 'color:blue'><b >//configurando post/users : </b></p>

usersRoute.post('/users', (*req*: *Request*, *res*: *Response*, *next*: *NextFunction* )=>{

​		const newUser = req.body

​		res.status(StatusCodes.CREATED).send(newUser)

​		<b>***testar se deu certo : console.log(req.body)</b>

})

<u>==>Paralelo a isso deve ser configurado no arquivo index.ts para que o app transforme em json a informação vinda do insomnia ou postman</u>

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
> .Send
>
> 

<h6 style='color:purple'>Alterando Usuario</h6>

<p style = 'color:blue'><b >//configurando put/users/:uuid : </b></p>

usersRoute.put('/users/:uuid', (*req*: *Request*<{uuid : *string*}>, *res*: *Response*, *next*: *NextFunction*)=>{

​	const uuid = req.params.uuid

​	const modifiUser = *req*.body

  	modifiUser.uuid = uuid

 *****para verificar se está rodando certo :  console.log(modifiUser)*

  *res*.status(*StatusCodes*.OK).send(modifiUser)

}

> *altera o usuario pelo numero de id no postman para conferir*



<h6 style='color:purple'>Deletando Usuario</h6>

<p style = 'color:blue'><b >//configurando delete/users/:uuid : </b></p>

usersRoute.delete('/users/:uuid', (req:Request, res:Response, next:NextFunction)=>{

​	const uuid = req.params.uuid

​	res.sendStatus(StatusCode.OK)

})



<p style='color:blue'><b>//Exportando</b></p>

export default usersRoute





<h1 style='background-color:pink'>Melhorando a rota de status</h1>

<p style='color:orange'>Pasta routes --> novo arquivo: status.routes.ts</p>

<p style = 'color:blue'><b>//configurando o arquivo status.routes.ts</b></p>

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

Inclui a  linha : app.user(statusRoute)

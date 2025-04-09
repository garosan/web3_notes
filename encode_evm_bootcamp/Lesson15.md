# Lesson 15 - NodeJS API using NestJS framework

Once again, having a backend for your dapp is not about centralizing assets and information, is about making your whole system more efficient by being able to cache things, store some parts that don't need to be onchain more securely and efficiently, etc.

I did not follow this lesson 100%, as I am not interested in adding a backend right now, so I just pasted the main notes along with some notes of my own.

## NestJS and controllers and services

**NestJS in a Nutshell**

- **Built with TypeScript** (but supports JavaScript too)
- **Inspired by Angular**:

  - Uses decorators, modules, services, and dependency injection (DI)
  - Makes code structured and modular

- **Opinionated architecture**:

  - Encourages a clean separation of concerns
  - Promotes testability and scalability

- **Built on top of Express (default)**, but also supports **Fastify** for better performance

- **Great for building**:

  - REST APIs
  - GraphQL APIs
  - WebSockets
  - Microservices (with built-in transport layer support)

- **Features**:
  - Dependency injection
  - Middleware, Pipes, Guards, and Interceptors
  - Powerful CLI (`nest g resource`, etc.)
  - Ecosystem for authentication, configuration, validation, etc.

Dividing the code into controllers and services is about separating responsibilities so your app is clean, modular, and easy to maintain.

### 📦 **Controller** = "The Request Handler"

Think of it like:

> "Hey, someone made a request to our API — what do we do?"

- Handles incoming **HTTP requests** (`GET`, `POST`, etc.)
- Calls the appropriate **service** to process the logic
- Sends back the **response**

Example:

```ts
// user.controller.ts
@Controller("users")
export class UserController {
  constructor(private userService: UserService) {}

  @Get()
  getAllUsers() {
    return this.userService.findAll();
  }
}
```

---

### 🔧 **Service** = "The Business Logic"

Think of it like:

> "Here’s where the actual work happens — database calls, calculations, etc."

- Contains your **core logic**
- Reusable across multiple controllers
- Easy to test

Example:

```ts
// user.service.ts
@Injectable()
export class UserService {
  private users = [{ name: "Alice" }, { name: "Bob" }];

  findAll() {
    return this.users;
  }
}
```

---

### 🎯 Why separate controllers and services?

- **Clean code**: Your controller focuses on handling routes, not logic.
- **Reusable logic**: Services can be used by other parts of the app (or even other services).
- **Easy to test**: You can test services without needing to hit an API endpoint.
- **Scalable**: As your app grows, you keep logic modular and maintainable.

## Implementing the API

- The NestJS CLI
- Creating Resources
- Controllers, Services and Routes in NestJS
- Modules and injections (overview)
- Server configuration
- Serving script operations as API services
- Params, DTOs and Payloads
- HTTP errors and messages
- (Review) Environment
- Implementing the features

## Read-only data

- GET methods:
  - GET contract address
  - GET token name
  - GET total supply
  - GET balance of a given address
  - GET transaction receipt of a transaction by transaction hash

### Example GET Method for Contract Address

- Controller method for GET `contract-address`:

```typescript
  @Get('contract-address')
  getContractAddress(){
    return {result: this.appService.getContractAddress()};
  }
```

- Implementing `getContractAddress` service at `app.service.ts`:

```typescript
  getContractAddress(): string {
    return "0x2282A77eC5577365333fc71adE0b4154e25Bb2fa";
  }
```

### Example GET Method for Token Name

- Installing Viem

```bash
npm i viem
```

- Controller method for GET `token-name`:

```typescript
  @Get('token-name')
  async getTokenName() {
    return {result: await this.appService.getTokenName()};
  }
```

- Fetching the ABI for creating a `Contract` with Viem:
  - Pick your `MyToken.json` from any ERC20 that you compiled before
  - Copy it and paste at `src/assets/`
  - Import the file at `app.service.ts`:

```typescript
import * as tokenJson from "./assets/MyToken.json";
```

- Add the `resolveJsonModule` configuration to `compilerOptions` inside `tsconfig.json`:

```json
"resolveJsonModule": true
```

- Implementing `getTokenName` service at `app.service.ts`:

```typescript
  async getTokenName(): Promise<string> {
    const publicClient = createPublicClient({
      chain: sepolia,
      transport: http(`>Put your RPC endpoint URL here<`),
    });
    const name = await publicClient.readContract({
      address: this.getContractAddress(),
      abi: tokenJson.abi,
      functionName: "name"
    });
    return name;
  }
```

### Using environment variables

- Add `dotenv` to your project and configure it at `main.ts`:

```bash
npm i --save dotenv
```

```typescript
...
import 'dotenv/config';

async function bootstrap() {
...
```

- Create your `.env` file at the root of your project:

```txt
PRIVATE_KEY=****************************************************************
RPC_ENDPOINT_URL="https://****************************************************************"
TOKEN_ADDRESS="0x****************************************"
```

- Modify the `getTokenName` service:

```typescript
  getContractAddress(): string {
    return process.env.TOKEN_ADDRESS;
  }

  async getTokenName(): Promise<string> {
    const publicClient = createPublicClient({
      chain: sepolia,
      transport: http(process.env.RPC_ENDPOINT_URL),
    });
    const name = await publicClient.readContract({
      address: this.getContractAddress(),
      abi: tokenJson.abi,
      functionName: "name"
    });
    return name;
  }
```

### Using NestJS Configurations Module

- Remove `dotenv` configurations at `main.ts`:

```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
...
```

- Add `@nestjs/config` to your project

```bash
npm i --save @nestjs/config
```

- Import `ConfigModule` in `app.module.ts`

```typescript
import { Module } from "@nestjs/common";
import { AppController } from "./app.controller";
import { AppService } from "./app.service";
import { ConfigModule } from "@nestjs/config";

@Module({
  imports: [ConfigModule.forRoot()],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

- Import `ConfigService` inside `app.service.ts`:

```typescript
  constructor(private configService: ConfigService) {}
```

- Refactor `getContractAddress` and `getTokenName`:

```typescript
  getContractAddress(): string {
    return this.configService.get<string>('TOKEN_ADDRESS');
  }

  async getTokenName(): Promise<string> {
    const publicClient = createPublicClient({
      chain: sepolia,
      transport: http(this.configService.get<string>('RPC_ENDPOINT_URL')),
    });
    const name = await publicClient.readContract({
      address: this.getContractAddress(),
      abi: tokenJson.abi,
      functionName: "name"
    });
    return name;
  }
```

### Adding Swagger Module

- Install `@nestjs/swagger` to your project

```bash
npm install --save @nestjs/swagger
```

- Configure your `main.ts` file:

```typescript
import { NestFactory } from "@nestjs/core";
import { SwaggerModule, DocumentBuilder } from "@nestjs/swagger";
import { AppModule } from "./app.module";

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  const config = new DocumentBuilder()
    .setTitle("API example")
    .setDescription("The API description")
    .setVersion("1.0")
    .addTag("example")
    .build();
  const document = SwaggerModule.createDocument(app, config);
  SwaggerModule.setup("api", app, document);

  await app.listen(3000);
}
bootstrap();
```

### Other Controller GET Methods

- Adding the GET methods at `app.controller.ts`:

```typescript
  @Get('total-supply')
  async getTotalSupply() {
    return {result: await this.appService.getTotalSupply()};
  }

  @Get('token-balance/:address')
  async getTokenBalance(@Param('address') address: string) {
    return {result: await this.appService.getTokenBalance(address)};
  }

  @Get('transaction-receipt')
  async getTransactionReceipt(@Query('hash') hash: string) {
    return {result: await this.appService.getTransactionReceipt(hash)};
  }
```

- Organizing the `AppService` class:

```typescript
export class AppService {
  publicClient;

  constructor(private configService: ConfigService) {
    this.publicClient = createPublicClient({
      chain: sepolia,
      transport: http(this.configService.get<string>('RPC_ENDPOINT_URL')),
    });
  }
  ...
```

## Minting tokens

- GET methods:
  - Get the address of the server wallet
  - Check if address has MINTER_ROLE role
- POST methods:
  - Request tokens to be minted

### Implementation

- Controller methods:

```typescript
  @Get('server-wallet-address')
  getServerWalletAddress() {
    return {result: this.appService.getServerWalletAddress()};
  }

  @Get('check-minter-role')
  checkMinterRole(@Query('address') address: string) {
    return {result: await this.appService.checkMinterRole(address)};
  }

  @Post('mint-tokens')
  async mintTokens(@Body() body: any) {
    return {result: await this.appService.mintTokens(body.address)};
  }
```

- Creating the `MintTokenDto` class at `src/dtos/mintToken.dto.ts`:

```typescript
import { ApiProperty } from "@nestjs/swagger";

export class MintTokenDto {
  @ApiProperty({ type: String, required: true, default: "My Address" })
  address: string;
}
```

- Using `MintTokenDto` for the `mintTokens` method:

```typescript
...
import { MintTokenDto } from './dtos/mintToken.dto';

@Controller()
export class AppController {
  ...

  @Post('mint-tokens')
  async mintTokens(@Body() body: MintTokenDto) {
    return {result: await this.appService.mintTokens(body.address)};
  }
}
```

- Implementing the `WalletClient` in the `AppService` constructor:

```typescript
export class AppService {
  publicClient;
  walletClient;

  constructor() {
    const account = privateKeyToAccount(`0x${process.env.PRIVATE_KEY}`);
    this.publicClient = createPublicClient({
      chain: sepolia,
      transport: http(process.env.RPC_ENDPOINT_URL),
    });
    this.walletClient = createWalletClient({
      transport: http(process.env.RPC_ENDPOINT_URL),
      chain: sepolia,
      account: account,
    });
  }
```

- Checking the wallet address and minter role:

```typescript
  getServerWalletAddress(): string {
    return this.walletClient.account.address;
  }

  async checkMinterRole(address: string): Promise<boolean> {
    const MINTER_ROLE = "0x9f2df0fed2c77648de5860a4cc508cd0818c85b8b8a1ab4ceeef8d981c8956a6";
    // const MINTER_ROLE =  await this.publicClient.readContract({
    //   address: this.getContractAddress(),
    //   abi: tokenJson.abi,
    //   functionName: 'MINTER_ROLE'
    // });
    const hasRole = await this.publicClient.readContract({
      address: this.getContractAddress(),
      abi: tokenJson.abi,
      functionName: 'hasRole',
      args: [MINTER_ROLE, address],
    });
    return hasRole;
  }
```

## ❓ Questions and 💪 Exercises

## 🛠️ Links and Resources

- [Notes](https://github.com/Encode-Club-Solidity-Bootcamp/Lesson-15)
- [Video](https://www.youtube.com/watch?v=Tm7LoFRSAps)

<https://docs.nestjs.com/>

<https://docs.nestjs.com/openapi/introduction>

<https://www.coreycleary.me/what-is-the-difference-between-controllers-and-services-in-node-rest-apis>

<https://restfulapi.net/idempotent-rest-apis/>

<https://docs.nestjs.com/openapi/types-and-parameters>

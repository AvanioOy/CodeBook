# CodeBook Typescript / Javascript

## Common basic blocks which can be used in Typescript and Javascript projects

### Logging: [@avanio/logger-like](https://www.npmjs.com/package/@avanio/logger-like) (typescript)

- **_important_** : **ILoggerLike** interface type which matches with console/log4js/winston
- **MapLogger** for complex setups, this extends normal logger with pre-defined keys and log levels for those. (extendable class)
- **LevelLogger** where you can setup base log level. (extendable class)

```typescript
import {ILoggerLike} from '@avanio/logger-like';
function doSomething(logger?: ILoggerLike) {
  logger?.info("doing something");
}
```

### Config/Env Variables: [@avanio/variable-util](https://www.npmjs.com/package/@avanio/variable-util)

- Multiple ways to get variables from environment, config files, fetch loadable json, Azure KV, etc.
- This module can be used on both frontend and backend.
- [@avanio/variable-util-node](https://www.npmjs.com/package/@avanio/variable-util-node) for nodejs specific features.
- [@avanio/variable-util-azurekv](https://www.npmjs.com/package/@avanio/variable-util-azurekv) for Azure KV accesss

```typescript
const urlParse = new UrlParser({ urlSanitize: true });
const dockerEnv = new DockerSecretsConfigLoader({
  fileLowerCase: true,
  isSilent: true,
}).getLoader;
const fileEnv = new FileConfigLoader({
  fileName: "./settings.json",
  type: "json",
}).getLoader;
// default loaders, can be defined also on per variable basis
const loaders = [env(), fileEnv(), dockerEnv()];

type RootVariables = {
  PORT: number;
  MONGODB_URI: URL;
  JWT_SECRET: string;
};

export const rootConfig = new ConfigMap<RootVariables>({
  PORT: {loaders, parser: integerParser(), defaultValue: 6000, params: { showValue: true }},
  MONGODB_URI: {loaders,parser: urlParse, defaultValue: new URL("mongodb://localhost/db"), params: { showValue: true }},
  JWT_SECRET: {loaders, parser: stringParser(), undefinedThrowsError: true},
});
const port: number = await rootConfig.get("PORT");
const mongoUri: URL = await rootConfig.get("MONGODB_URI");
const jwtSecret: string = await rootConfig.get("JWT_SECRET"); // or throws error if not found
```

### JWT Auth/Authz modules
- [mharj-jwt-util](https://www.npmjs.com/package/mharj-jwt-util) Simple Asymmetric JWT verification setup (loads automatically public key based on issuer discovery)
```typescript
const {isCached, body} = await jwtVerify(token, {issuer: process.env.TOKEN_ISSUER, audience: process.env.TOKEN_AUDIENCE});
```

- [@avanio/jwt-util](https://www.npmjs.com/package/@avanio/jwt-util) More complex verification which supports multiple symmetric/asymmetric JWT verifications same time. (based on issuer and kid)

```typescript
const jwt = new JwtManager(new IssuerManager([new JwtAzureMultitenantTokenIssuer({allowedIssuers: [`https://sts.windows.net/${process.env.AZ_TENANT_ID}/`]})]));
// validate with jwt manager
const {isCached, body} = await jwt.verify(token);
```

### Cache [@avanio/expire-cache](https://www.npmjs.com/package/@avanio/expire-cache)
- **ExpireCache** which works like JS Map, but have expiration time for each key. (expires on read operation)
- **ExpireTimeoutCache** similar to ExpireCache, but expires with timeout.
- This module is heavily used on JWT token caching/expiration.
```typescript
const cache = new ExpireCache<string>();
cache.set('key','value', new Date(Date.now() + 1000)); // expires in 1 second
const value = cache.get('key'); // returns 'value'
```

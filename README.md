# Template SDK

<div align="left">
    <a href="https://speakeasyapi.dev/"><img src="https://custom-icon-badges.demolab.com/badge/-Built%20By%20Speakeasy-212015?style=for-the-badge&logoColor=FBE331&logo=speakeasy&labelColor=545454" /></a>   
</div>

## How to use this repository

**👀** This template repository is designed to bootstrap a [Speakeasy managed SDK repository](https://speakeasyapi.dev/docs/create-client-sdks/) using Github's repository clone feature. Once this repository is setup it will automatically keep your SDK up to date and published to a package manager. 


### Creating an SDK

1. To get started, simply clone the repository by clicking on the "Use template" button and give it a name.
   
![Screenshot 2023-09-06 at 09 20 52](https://github.com/speakeasy-sdks/template-sdk/assets/68016351/b4cf4e43-db4e-455a-9359-0f09f942b997)

3. Configure the Speakeasy workflow to generate the SDK. Go to the [generation workflow file](https://github.com/speakeasy-sdks/template-sdk/blob/main/.github/workflows/speakeasy_sdk_generation.yml) and configure the `language`, `mode` and `location` of your openapi document. For complete documentation on all the available generation configurations, see [here](https://www.speakeasyapi.dev/docs/advanced-setup/sdk-automation). You will also need to add a `SPEAKEASY_API_KEY` as a repository secret. If you don't already have a key you can get one by making a workspace on Speakeasy [here](https://app.speakeasyapi.dev/).

4. Configure the Speakeasy workflow to publish the SDK. Go to the [publishing workflow file](https://github.com/speakeasy-sdks/template-sdk/blob/main/.github/workflows/speakeasy_sdk_publish.yml) and configure any relevant package manager credentials as repository secrets. For complete documentation on all the available publishing configurations, see [here](https://www.speakeasyapi.dev/docs/productionize-sdks/publish-sdks).

5. Configure the generation by editing the `gen.yaml` file in the root of the repo. This file controls the generator and determines various attributes of the SDK like `packageName`, `sdkClassName`, inlining of parameters, and other ergonomics.

6. Finally go to the Actions tab, choose the generation workflow and click "Force Generate". This will trigger a new generation of your SDK using the configuration you provided above. Depending on whether you configured `pr` or `direct` mode above your updated SDK will appear in PR or in the main branch.

![Screenshot 2023-09-06 at 10 01 46](https://github.com/speakeasy-sdks/template-sdk/assets/68016351/35828982-c6de-4a5c-84f5-ae2b4224cece)

🚀 You should have a working SDK for your API 🙂 . To check out all the features of the SDK please see our docs [site](https://speakeasyapi.dev/docs/create-client-sdks/).

### Local development

Once you have the SDK setup you may want to iterate on the SDK. Speakeasy supports OpenAPI vendor extensions that can be added to your spec to customize the SDK ergonomics (method names, namespacing resources etc.) and functionality (adding retries, pagination, multiple server support etc)

To get started install the Speakeasy CLI.

In your terminal, run:

```bash
brew install speakeasy-api/homebrew-tap/speakeasy
```
Once you annonate your spec with an extension you will want to run `speakeasy validate` to check the spec for correctness and `speakeasy generate` to recreate the SDK locally. More documentation on OpenAPI extensions [here](https://speakeasyapi.dev/docs/customize-sdks/namespaces/). Here's an example of adding a multiple server support to the spec so that your SDK supports production and sandbox versions of your API. 

```yaml
info:
  title: Example
  version: 0.0.1
servers:
  - url: https://prod.example.com # Used as the default URL by the SDK
    description: Our production environment
    x-speakeasy-server-id: prod
  - url: https://sandbox.example.com
    description: Our sandbox environment
    x-speakeasy-server-id: sandbox
```

Once you're finished iterating and happy with the output push only the latest version of spec into the repo and regenerate the SDK using step 6 above.

<!-- Start SDK Installation [installation] -->
## SDK Installation

### NPM

```bash
npm add https://github.com/speakeasy-sdks/bar-ts-2
```

### Yarn

```bash
yarn add https://github.com/speakeasy-sdks/bar-ts-2
```
<!-- End SDK Installation [installation] -->

<!-- Start SDK Example Usage [usage] -->
## SDK Example Usage

### Sign in

First you need to send an authentication request to the API by providing your username and password.
In the request body, you should specify the type of token you would like to receive: API key or JSON Web Token.
If your credentials are valid, you will receive a token in the response object: `res.object.token: str`.

```typescript
import { TheSpeakeasyBar } from "The-Speakeasy-Bar";
import { LoginSecurity, TypeT } from "The-Speakeasy-Bar/dist/sdk/models/operations";

async function run() {
    const sdk = new TheSpeakeasyBar();
    const operationSecurity: LoginSecurity = {
        password: "<PASSWORD>",
        username: "<USERNAME>",
    };

    const res = await sdk.authentication.login(
        {
            type: TypeT.ApiKey,
        },
        operationSecurity
    );

    if (res.statusCode == 200) {
        // handle response
    }
}

run();

```

### Browse available drinks

Once you are authenticated, you can use the token you received for all other authenticated endpoints.
For example, you can filter the list of available drinks by type.

```typescript
import { TheSpeakeasyBar } from "The-Speakeasy-Bar";
import { DrinkType } from "The-Speakeasy-Bar/dist/sdk/models/shared";

async function run() {
    const sdk = new TheSpeakeasyBar({
        security: {
            apiKey: "<YOUR_API_KEY>",
        },
    });

    const res = await sdk.drinks.listDrinks({});

    if (res.statusCode == 200) {
        // handle response
    }
}

run();

```

### Create an order

When you submit an order, you can include a callback URL along your request.
This URL will get called whenever the supplier updates the status of your order.

```typescript
import { TheSpeakeasyBar } from "The-Speakeasy-Bar";
import { OrderType } from "The-Speakeasy-Bar/dist/sdk/models/shared";

async function run() {
    const sdk = new TheSpeakeasyBar({
        security: {
            apiKey: "<YOUR_API_KEY>",
        },
    });

    const res = await sdk.orders.createOrder({
        requestBody: [
            {
                productCode: "APM-1F2D3",
                quantity: 26535,
                type: OrderType.Drink,
            },
        ],
    });

    if (res.statusCode == 200) {
        // handle response
    }
}

run();

```

### Subscribe to webhooks to receive stock updates

```typescript
import { TheSpeakeasyBar } from "The-Speakeasy-Bar";
import { Webhook } from "The-Speakeasy-Bar/dist/sdk/models/operations";

async function run() {
    const sdk = new TheSpeakeasyBar({
        security: {
            apiKey: "<YOUR_API_KEY>",
        },
    });

    const res = await sdk.config.subscribeToWebhooks([{}]);

    if (res.statusCode == 200) {
        // handle response
    }
}

run();

```
<!-- End SDK Example Usage [usage] -->

<!-- Start Available Resources and Operations [operations] -->
## Available Resources and Operations

### [authentication](docs/sdks/authentication/README.md)

* [login](docs/sdks/authentication/README.md#login) - Authenticate with the API by providing a username and password.

### [drinks](docs/sdks/drinks/README.md)

* [getDrink](docs/sdks/drinks/README.md#getdrink) - Get a drink.
* [listDrinks](docs/sdks/drinks/README.md#listdrinks) - Get a list of drinks.

### [ingredients](docs/sdks/ingredients/README.md)

* [listIngredients](docs/sdks/ingredients/README.md#listingredients) - Get a list of ingredients.

### [orders](docs/sdks/orders/README.md)

* [createOrder](docs/sdks/orders/README.md#createorder) - Create an order.

### [config](docs/sdks/config/README.md)

* [subscribeToWebhooks](docs/sdks/config/README.md#subscribetowebhooks) - Subscribe to webhooks.
<!-- End Available Resources and Operations [operations] -->





<!-- Start Error Handling [errors] -->
## Error Handling

Handling errors in this SDK should largely match your expectations.  All operations return a response object or throw an error.  If Error objects are specified in your OpenAPI Spec, the SDK will throw the appropriate Error type.

| Error Object      | Status Code       | Content Type      |
| ----------------- | ----------------- | ----------------- |
| errors.BadRequest | 400               | application/json  |
| errors.APIError   | 5XX               | application/json  |
| errors.SDKError   | 4xx-5xx           | */*               |

Example

```typescript
import { TheSpeakeasyBar } from "The-Speakeasy-Bar";
import { Webhook } from "The-Speakeasy-Bar/dist/sdk/models/operations";

async function run() {
    const sdk = new TheSpeakeasyBar({
        security: {
            apiKey: "<YOUR_API_KEY>",
        },
    });

    let res;
    try {
        res = await sdk.config.subscribeToWebhooks([{}]);
    } catch (err) {
        if (err instanceof errors.BadRequest) {
            console.error(err); // handle exception
            throw err;
        } else if (err instanceof errors.APIError) {
            console.error(err); // handle exception
            throw err;
        } else if (err instanceof errors.SDKError) {
            console.error(err); // handle exception
            throw err;
        }
    }

    if (res.statusCode == 200) {
        // handle response
    }
}

run();

```
<!-- End Error Handling [errors] -->



<!-- Start Server Selection [server] -->
## Server Selection

### Select Server by Name

You can override the default server globally by passing a server name to the `server: string` optional parameter when initializing the SDK client instance. The selected server will then be used as the default on the operations that use it. This table lists the names associated with the available servers:

| Name | Server | Variables |
| ----- | ------ | --------- |
| `prod` | `https://speakeasy.bar` | None |
| `staging` | `https://staging.speakeasy.bar` | None |
| `customer` | `https://{organization}.{environment}.speakeasy.bar` | `environment` (default is `prod`), `organization` (default is `api`) |

#### Example

```typescript
import { TheSpeakeasyBar } from "The-Speakeasy-Bar";

async function run() {
    const sdk = new TheSpeakeasyBar({
        server: "customer",
        security: {
            apiKey: "<YOUR_API_KEY>",
        },
    });

    const res = await sdk.ingredients.listIngredients({
        ingredients: ["string"],
    });

    if (res.statusCode == 200) {
        // handle response
    }
}

run();

```

#### Variables

Some of the server options above contain variables. If you want to set the values of those variables, the following optional parameters are available when initializing the SDK client instance:
 * `environment: models.ServerEnvironment`
 * `organization: string`

### Override Server URL Per-Client

The default server can also be overridden globally by passing a URL to the `serverURL: str` optional parameter when initializing the SDK client instance. For example:
```typescript
import { TheSpeakeasyBar } from "The-Speakeasy-Bar";

async function run() {
    const sdk = new TheSpeakeasyBar({
        serverURL: "https://speakeasy.bar",
        security: {
            apiKey: "<YOUR_API_KEY>",
        },
    });

    const res = await sdk.ingredients.listIngredients({
        ingredients: ["string"],
    });

    if (res.statusCode == 200) {
        // handle response
    }
}

run();

```

### Override Server URL Per-Operation

The server URL can also be overridden on a per-operation basis, provided a server list was specified for the operation. For example:
```typescript
import { TheSpeakeasyBar } from "The-Speakeasy-Bar";
import { DrinkType } from "The-Speakeasy-Bar/dist/sdk/models/shared";

async function run() {
    const sdk = new TheSpeakeasyBar({
        security: {
            apiKey: "<YOUR_API_KEY>",
        },
    });

    const res = await sdk.drinks.listDrinks({}, "https://speakeasy.bar");

    if (res.statusCode == 200) {
        // handle response
    }
}

run();

```
<!-- End Server Selection [server] -->



<!-- Start Custom HTTP Client [http-client] -->
## Custom HTTP Client

The Typescript SDK makes API calls using the [axios](https://axios-http.com/docs/intro) HTTP library.  In order to provide a convenient way to configure timeouts, cookies, proxies, custom headers, and other low-level configuration, you can initialize the SDK client with a custom `AxiosInstance` object.

For example, you could specify a header for every request that your sdk makes as follows:

```typescript
import { The-Speakeasy-Bar } from "TheSpeakeasyBar";
import axios from "axios";

const httpClient = axios.create({
    headers: {'x-custom-header': 'someValue'}
})

const sdk = new TheSpeakeasyBar({defaultClient: httpClient});
```
<!-- End Custom HTTP Client [http-client] -->



<!-- Start Authentication [security] -->
## Authentication

### Per-Client Security Schemes

This SDK supports the following security schemes globally:

| Name         | Type         | Scheme       |
| ------------ | ------------ | ------------ |
| `apiKey`     | apiKey       | API key      |
| `bearerAuth` | http         | HTTP Bearer  |

You can set the security parameters through the `security` optional parameter when initializing the SDK client instance. The selected scheme will be used by default to authenticate with the API for all operations that support it. For example:
```typescript
import { TheSpeakeasyBar } from "The-Speakeasy-Bar";

async function run() {
    const sdk = new TheSpeakeasyBar({
        security: {
            apiKey: "<YOUR_API_KEY>",
        },
    });

    const res = await sdk.ingredients.listIngredients({
        ingredients: ["string"],
    });

    if (res.statusCode == 200) {
        // handle response
    }
}

run();

```

### Per-Operation Security Schemes

Some operations in this SDK require the security scheme to be specified at the request level. For example:
```typescript
import { TheSpeakeasyBar } from "The-Speakeasy-Bar";
import { LoginSecurity, TypeT } from "The-Speakeasy-Bar/dist/sdk/models/operations";

async function run() {
    const sdk = new TheSpeakeasyBar();
    const operationSecurity: LoginSecurity = {
        password: "<PASSWORD>",
        username: "<USERNAME>",
    };

    const res = await sdk.authentication.login(
        {
            type: TypeT.ApiKey,
        },
        operationSecurity
    );

    if (res.statusCode == 200) {
        // handle response
    }
}

run();

```
<!-- End Authentication [security] -->



<!-- Start Retries [retries] -->
## Retries

Some of the endpoints in this SDK support retries.  If you use the SDK without any configuration, it will fall back to the default retry strategy provided by the API.  However, the default retry strategy can be overridden on a per-operation basis, or across the entire SDK.

To change the default retry strategy for a single API call, simply provide a retryConfig object to the call:
```typescript
import { TheSpeakeasyBar } from "The-Speakeasy-Bar";
import { Webhook } from "The-Speakeasy-Bar/dist/sdk/models/operations";

async function run() {
    const sdk = new TheSpeakeasyBar({
        security: {
            apiKey: "<YOUR_API_KEY>",
        },
    });

    const res = await sdk.config.subscribeToWebhooks([{}], {
        strategy: "backoff",
        backoff: {
            initialInterval: 1,
            maxInterval: 50,
            exponent: 1.1,
            maxElapsedTime: 100,
        },
        retryConnectionErrors: false,
    });

    if (res.statusCode == 200) {
        // handle response
    }
}

run();

```

If you'd like to override the default retry strategy for all operations that support retries, you can provide a retryConfig at SDK initialization:
```typescript
import { TheSpeakeasyBar } from "The-Speakeasy-Bar";
import { Webhook } from "The-Speakeasy-Bar/dist/sdk/models/operations";

async function run() {
    const sdk = new TheSpeakeasyBar({
        retry_config: {
            strategy: "backoff",
            backoff: {
                initialInterval: 1,
                maxInterval: 50,
                exponent: 1.1,
                maxElapsedTime: 100,
            },
            retryConnectionErrors: false,
        },
        security: {
            apiKey: "<YOUR_API_KEY>",
        },
    });

    const res = await sdk.config.subscribeToWebhooks([{}]);

    if (res.statusCode == 200) {
        // handle response
    }
}

run();

```
<!-- End Retries [retries] -->

<!-- Placeholder for Future Speakeasy SDK Sections -->



### Maturity

This SDK is in beta, and there may be breaking changes between versions without a major version update. Therefore, we recommend pinning usage
to a specific package version. This way, you can install the same version each time without breaking changes unless you are intentionally
looking for the latest version.

### Contributions

While we value open-source contributions to this SDK, this library is generated programmatically.
Feel free to open a PR or a Github issue as a proof of concept and we'll do our best to include it in a future release !

### SDK Created by [Speakeasy](https://docs.speakeasyapi.dev/docs/using-speakeasy/client-sdks)

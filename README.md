# Demo Serverless (REST API)

## Get started

### Install dependencies

- Nodejs v6.5.0 or later
- Serverless CLI v1.9.0 or later (`npm i -g serverless`)
- Choose your computer provider (AWS on my case)
  Give serverless access to our cloud provider account so that it can create and manage resources on our behalf. For this we need to create a policy with the appropriate permissions and a user to assign those.

### Create Policy

- AWS Services -> IAM -> Policies -> Create Policy -> JSON -> Copy & Paste the following gist: https://gist.github.com/ServerlessBot/7618156b8671840a539f405dea2704c8 (apply changes needed)
- Add name and description.

### Create User

- AWS Services -> IAM -> Users -> Create User
- Add name and enable programmatic access
- Link permissions to the previously created policy
- Download the credentials csv

### Setup Credentials

> ⚠️ **Warning**: This action will modify your *~/.aws/credentials* file.

```bash
sls config credentials --provider aws --key AWS_ACCESS_KEY_ID --secret AWS_ACCESS_SECRET_KEY --profile AWS_PROFILE_NAME
```

### Create config.yml

```yml
dev:
  DYNAMODB_LOCAL_PORT: 9200
  CRYPTO_SECRET_KEY: CRYPTO_SECRET_KEY
  JWT_SECRET: JWT_SECRET
  PROFILE: AWS_PROFILE_NAME
```

#### Generate secret using Node.js Crypto

```bash
node -e "require('crypto').randomBytes(20, (err, buffer) => {
  if (err) {
    console.error(err);
  } else {
    const token = buffer.toString('hex')
    console.log(token)
  }
})"

```

### Deploy

```bash
sls deploy
```

### Remove

```bash
sls remove
```

Note: when using `DeletionPolicy: Retain` on the db tables as on this case, after remove, we need to clean up the tables also. Otherwise we would see an error when trying to deploy again.

### Logs

```bash
  sls logs --function functionName
```

## Local environment configuration

```bash
sls dynamodb install
```

```bash
sls dynamodb start
```

```bash
sls offline
```

## Postman collection

https://www.getpostman.com/collections/61f4f550292769ccc83a

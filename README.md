# quarkus-graalvm-aws-lambda
Skeleton for a Quarkus app that compiles as a GraalVM native image and deploys to AWS Lambda, based on the NLJUG events app backend.

## Getting started
Prerequisites:
- Java 11
- Maven
- AWS CLI
- AWS SAM CLI
- AWS account + configured AWS CLI (via `aws configure`)

## Howto
This skeleton is based on the following guide: https://quarkus.io/guides/amazon-lambda-http

## Generation
This skeleton was generated using the `quarkus-amazon-lambda-rest-archetype`:

```
mvn archetype:generate \
-DarchetypeGroupId=io.quarkus \
-DarchetypeArtifactId=quarkus-amazon-lambda-rest-archetype \
-DarchetypeVersion=2.10.1.Final
```


## Build & native image generation
`scripts/build.sh`
or:
`mvn package -Dnative -Dquarkus.native.container-build=true`

Note: if you're running this on an M1 mac this will take a long time because the Docker image used to build the native image is emulated via Rosetta.  
A workaround can be to develop on a mac and build/deploy on an Intel machine/build server.


## Running the native image locally for testing purposes
Running the lambda:
`scripts/run-local.sh`
or:
`sam local start-api --template target/sam.native.yaml`

Testing the lambda:
`curl localhost:3000/hello`
should output:
`Hi from NLJUG!`

## Lambda configuration
Edit `target/sam.native.yaml` to suit your needs, for example tweak the `MemorySize`

## Deploying
`scripts/deploy.sh`
or:
`sam deploy -t target/sam.native.yaml -g`

The `sam deploy` command will return the URL of the deployed lambda+API gateway.

## Testing the production deployment
Use the URL from the `sam deploy` command output.
Test the lambda via `curl <url>/hello`

This should output `Hi from NLJUG!`.
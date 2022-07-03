# quarkus-graalvm-aws-lambda
Skeleton for a Quarkus app that compiles as a GraalVM native image and deploys to AWS Lambda.
This skeleton is based on the NLJUG events app backend.

## Getting started
Prerequisites:
- Java 11
- Maven
- AWS CLI (see https://github.com/bertjan/quarkus-graalvm-aws-lambda)
- AWS SAM CLI (see https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
- AWS account
- Configured AWS CLI (via `aws configure`)

## Howto
This skeleton is based on the following guide: https://quarkus.io/guides/amazon-lambda-http - kudos to the Quarkus team for writing it!

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

## Lambda deployment configuration
Edit `target/sam.native.yaml` to suit your needs, for example tweak the `MemorySize`

## Deploying
`scripts/deploy.sh`
or:
`sam deploy -t target/sam.native.yaml -g`

Go with the default values:
```
	Setting default arguments for 'sam deploy'
	=========================================
	Stack Name [sam-app]: 
	AWS Region [eu-west-1]: 
	#Shows you resources changes to be deployed and require a 'Y' to initiate deploy
	Confirm changes before deploy [y/N]: y
	#SAM needs permission to be able to create roles to connect to the resources in your template
	Allow SAM CLI IAM role creation [Y/n]: y
	#Preserves the state of previously provisioned resources when an operation fails
	Disable rollback [y/N]: n
	QuarkusGraalvmAwsLambdaNative may not have authorization defined, Is this okay? [y/N]: y
	Save arguments to configuration file [Y/n]: y
	SAM configuration file [samconfig.toml]: 
	SAM configuration environment [default]: 
```
Note: the stack that will be deployed will be named `sam-app` if you go for the defaults offered by `sam`.  
If you already have a lambda deployed with name `sam-app`, it will be replaced by this lambda! Update the name accordingly if you don't want to overwrite an existing `sam-app`.

The `sam deploy` command will return the URL of the deployed lambda+API gateway.

## Testing the production deployment
Use the URL from the `sam deploy` command output.
Test the lambda via `curl <url>/hello`

This should output `Hi from NLJUG!`.

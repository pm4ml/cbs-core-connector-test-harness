# Core Connector Golden Path Test Collection
The core connector golden path test collection is a set of test cases that are executed against a core connector inside a mojaloop testing toolkit.  The test collection has an accompanying environment file that needs to be in imported into the ttk instance where it is being run such that it can run appropriately.

# Pre-requisites
- Docker installed
- 16GB Ram for local runs.
- Knowledge of docker and compose
- Internet connection to download images

# Getting Started
Clone the repository
```bash
git clone https://github.com/pm4ml/cbs-core-connector-test-harness.git
```
Move into the directory.
```bash
cd cbs-core-connector-test-harness/sdk-based-test-harness
```
Follow these instructions while in the `cbs-core-connector-test-harness` folder.

# Running the golden path test collection
Running the core connector golden path test collection requires configuration of a run environment. Before running the golden path, some prerequisites should be in place.

## Overview
The golden path test collection can run in one of 2 configuration environments
- Local Configuration
- Deployed environment configuration

# Local Configuration
To locally test a core connector using the connector golden path test collection in the test harness, you can use the sdk based test harness that provides a pre-configured setup where you can just configure the connector to test and run it without any additional configuration.

To configure the test harness for locally testing a core connector. You will need to do the following
1. Obtain the core banking system api BASE_URL and make sure it can be accessed from your local machine or mock it using the TTK. 
    - If you are going to mock, ask the developer of the core connector to provide an api specification for the DFSP core banking system API.
2. Configure the core connector service inside the [`./core-connector.yaml`](./core-connector.yaml)
3. Set up the appropriate `env` file entries inside the [`core-connector.env`](./core-connector.env)
4. Run the test harness 
5. Load the test collection [`./collections/cc_golden_path.json`](./collections/cc_golden_path.json)
6. Load the environment file 
7. Run the test cases.

After you have obtained the BASE_URL or setup a mock for the DFSP core banking system, you will need to go through the steps from `2` through `7`.

Configure the core connector service inside the connector service configuration [file](./core-connector.yaml). Inside the file you will need to set the connector image name and version. Ensure the image can be accessed by the docker instance on your machine even if it is in a private docker repository.

After configuring the service configuration, you will need to setup the configuration environment variables for the core connector in the configuration file at [`./core-connector.env`](./core-connector.env). Typically, you would just copy the contents of the connector's `.env` file into this file. You can get these configurations from the core connector developer.

After setting up the service and environment variables, you will then be in position to run the test harness with the core connector to be tested.

## CAUTION ! Before you run locally
Before running the test collection locally, make sure to configure the currencies appropriately in the following files.
 - DFSP 1 SDK [Env file](./config/sdk-dfsp1/api-svc.env) 
    - Set `SUPPORTED_CURRENCIES=<YOUR-CONNECTOR-CURRENCY>`
-  FXP 1 SDK [Env file](./config/sdk-fxp1/api-svc.env)
    - Set `SUPPORTED_CURRENCIES="<YOUR-CONNECTOR-CURRENCY>,XTS"`
- HUB Input [file](./config/ttk-dfsp1/environments/hub_local_environment.json)
    - Set  `inputValues.DFSP1_CURRENCY=<YOUR-CONNECTOR-CURRENCY>`
    - Something like this 
        ```json
        {
            "options": {
                ...
            },
            "inputValues": {
                ...
                "DFSP1_CURRENCY": "YOUR-CONNECTOR-CURRENCY>",
                ...
            }
        }
        ```
This ensures that the right currencies are returned in the responses to the core connector being tested

## Running locally
- Make sure you are in `sdk-based-test-harness` directory
- Run the command `docker compose build`
- Run the command `docker compose --profile debug up` (wait till all the services are up)
- Open TTK UI at [http://localhost:16060](http://localhost:16060) in a browser. If TTK UI does not open, give it some more time to make sure all the services are up and running.
- Go to Test Runner, Click on Collections Manager and choose `collections` folder
- Load the Golden Path Test Collection at [`./collections/cc_golden_path.json`](./collections/cc_golden_path.json)
- Still under the Test Runner load the environment file by clicking on the Environment Manager and add the environment file at [`./environments/cc_golden_path_env.json`](./environments/cc_golden_path_env.json)
- If you want to run all the test cases, there is a `Run` button in Red color at the top right of the page that you can click and run all the test cases
- You can then download the Test Report by clicking on `Download Report` in the top right corner of the Testing Toolkit Screen

## Running with CLI or even in CI
- Make sure you are in `sdk-based-test-harness` directory
- Run the command `docker compose build`
- Run the command `docker compose up` (wait till all the services are up)
- Wait for some time to get all the services running
- Execute the following command in a separate terminal from `sdk-based-test-harness` folder to run the test cases using CLI
    ```
    docker compose -f ./ttk-tests-docker-compose.yml up
    ```
- You should get the html report in `sdk-based-test-harness/reports` folder
- For cleanup, execute the following commands
    ```
    docker compose -f ./ttk-tests-docker-compose.yml down
    docker compose down
    ```

# Deployed Environment Configuration - On Prem/Cloud
In a deployed environment the core connector golden path test collection will require the following.

1. A TTK instance where to run the test collection and the environment file
3. A running Mojaloop Connector Service (SDK) to which the core connector will make outbound transfer requests.

## Steps to run the tests
When deploying the core connector make sure to configure the following through the connector environment variables.

You will need to configure the BASE_URL of the DFSP Core Banking System API to either a mocked instance or a live DFSP Sandbox

You will also need to set the SDK_BASE_URL env variable of the core connector to the deployed instance of the SDK service in the environment of the DFSP being tested.

After setting up the configuration of the core connector please follow these steps to run the core connector golden path test collection.

1. Get access to the ttk instance that you will be using to run the core connector golden path test collection.
2. Go to Test Runner, Click on Collections Manager and choose `collections` folder
3. Load the Golden Path Test Collection at [`./collections/cc_golden_path.json`](./collections/cc_golden_path.json)
4. Still under the Test Runner load the environment file by clicking on the Environment Manager and add the environment file at [`./environments/cc_golden_path_env.json`](./environments/cc_golden_path_env.json)
5. Inside the loaded environment variables, edit the `connectorHost` variable and set it to the either the k8s service name, FDQN or docker compose service name of the connector. 


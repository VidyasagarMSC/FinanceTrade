#  Finance Trade

[![Build Status](https://api.travis-ci.org/IBM-Bluemix/investment-insights-for-asset-managers.svg?branch=master)](https://travis-ci.org/VidyasagarMSC/FinanceTrade)


**Finance Trade** is a Node.js application that uses [IBM Financial services](https://www.ibm.com/watson/financial-services/) and Watson services.  

The application is a modern portfolio manager that provides real-time insights into how news all around the world can impact any given investment portfolio.

# Overview

The project deploys a node.js Cloud Foundry application and uses the following services:
   * [Investment Portfolio](https://console.bluemix.net/catalog/services/fss-portfolio-service)
   * [Discovery](https://console.bluemix.net/catalog/services/discovery) and the pre-enriched News dataset
   * [Predictive Market Scenarios](https://console.bluemix.net/catalog/services/fss-predictive-scenario-analytics-service)
   * [Simulated Instrument Analytics](https://console.bluemix.net/catalog/services/fss-scenario-analytics-service)

   ![architecture](./architecture.png)

The application uses the financial services to analyze a stock portfolio in regards to various risk factors. Risk factors include things like currency fluctuations or changes in the price of oil and gold.

   ![flow](./flow.png)

1. User selects a risk factor to consider.
1. Using Watson Discovery, the app looks for articles related to the risk factor.
1. The app computes a shock value based on the sentiment of the articles.
1. The app calls the Predictive Market Scenarios service to create conditional scenarios to model how, given a change to a subset of factors the broader set of market factors are expected to change.
1. Finally it computes analytics on the portfolio stocks under the given scenarios.

   
## Deploy to Bluemix

1. If you do not already have a Bluemix account, [sign up here][bluemix_signup_url]

1. Download and install the [Cloud Foundry CLI][cloud_foundry_url] tool

1. Use this command to display or specify the URL of the API endpoint of Bluemix.

    ```
    cf api https://api.ng.bluemix.net
    ```
1. Connect to Bluemix in the command line tool and follow the prompts to log in

   ```
   cf login -a https://api.ng.bluemix.net
   ```
1. Clone the app to your local environment from your terminal using the following command:

   ```
   git clone https://github.com/VidyasagarMSC/FinanceTrade.git
   ```

1. `cd` into this newly created directory

1. Navigate to manifest.yml file and change the NAME "fintrade" to an unique name of your choice. The new name is your APP_NAME in the commands below.

1. Follow the above step for SERVICES as well.


1. Create services required for this app

1. Run the shell script "run.sh" to create the services in one go. 
   1.  Make your shell script executable by running this command 
   ```
     chmod +x run.sh
   ```
   1. Copy the shell file to /usr/local/bin with the below command
   ```
    cp run.sh /usr/local/bin
   ```
   
   On the terminal. just run the shell script.
   
   OR
   
   Manually create services
   
   ```
   cf create-service discovery lite <Discovery_Service_Name>
   ```
   _Discovery Service Name as mentioned in manifest.yml above_

   ```
   cf create-service fss-portfolio-service fss-portfolio-service-free-plan <Portfolio_Service_Name as in manifest.yml>
   ```

   ```
   cf create-service fss-predictive-scenario-analytics-service fss-predictive-scenario-analytics-service-free-plan <Predictive_Scenario_Name as in manifest.yml>
   ```
   ```
   cf create-service fss-scenario-analytics-service  fss-scenario-analytics-service-free-plan <Scenario_Analytics_Name as in manifest.yml>
   ```

1. Push the app to Bluemix

   ```
   cf push
   ```
 _This command uses the manifest.yml file in your directory to CREATE the app and BIND the services to the app_

And voila! You now have your very own application running on Bluemix.

## Run the app locally

1. If you do not already have a Bluemix account, [sign up here][bluemix_signup_url]

2. If you have not already, [download Node.js][download_node_url] and install it on your local machine.

3. In the checkout directory, create a file ```.env``` and paste the below snippet

	```
	INVESTMENT_PORFOLIO_BASE_URL=investment-portfolio.mybluemix.net
	INVESTMENT_PORFOLIO_USERNAME=
	INVESTMENT_PORFOLIO_PASSWORD=

	DISCOVERY_USERNAME=
	DISCOVERY_PASSWORD=

	PREDICTIVE_MARKET_SCENARIOS_URI=fss-analytics.mybluemix.net
	PREDICTIVE_MARKET_SCENARIOS_ACCESS_TOKEN=

	SIMULATED_INSTRUMENT_ANALYSIS_URI=fss-analytics.mybluemix.net
	SIMULATED_INSTRUMENT_ANALYSIS_ACCESS_TOKEN=
	```
1. For credentials and access tokens, run this command

    ```
    cf env APP_NAME
    ```


1. Run

   ```
   npm install
   ```

1. Run

   ```
   npm start
   ```

## Contribute

If you find a bug, please report it via the [Issues section][issues_url] or even better, fork the project and submit a pull request with your fix! We are more than happy to accept external contributions to this project if they address something noted in an existing issue.  In order to be considered, pull requests must pass the initial [Travis CI][travis_url] build and/or add substantial value to the sample application.

## Troubleshooting

The primary source of debugging information for your Bluemix app is the logs. To see them, run the following command using the Cloud Foundry CLI:

   ```
   $ cf logs APP_NAME --recent
   ```

For more detailed information on troubleshooting your application, see the [Troubleshooting section](https://www.ng.bluemix.net/docs/troubleshoot/tr.html) in the Bluemix documentation.

## License

See [License.txt](License.txt) for license information.

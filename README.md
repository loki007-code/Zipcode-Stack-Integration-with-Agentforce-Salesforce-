Zipcode Stack Integration with Agentforce (Salesforce)
📘 Overview

This project demonstrates how to integrate Agentforce with the Zipcode Stack API to retrieve city names for given zip codes.

Whenever an internal Salesforce employee or an external user asks the Agent for the city corresponding to a zip code, the Agent invokes an Apex Action that performs a callout to the external Zipcode Stack API and returns the city name in real-time.

⚙️ Use Case

When a user asks:

“What is the city for zip code 94040?”

The Agent invokes the configured Apex class, which calls the Zipcode Stack API, parses the response, and returns:

“The city for zip code 94040 is Mountain View.”

🧩 Prerequisites

Before starting, ensure the following:

You have a Salesforce org with Agentforce enabled.

You have API access and Apex callout permissions.

You have an active Zipcode Stack account and a valid API key.

🚀 Steps to Implement
1. Create an Account in Zipcode Stack

Visit https://zipcodestack.com
.

Sign up for a free or paid account.

Navigate to the Dashboard → API Keys section.

Copy your API Key, which will be required in the Apex callout.

2. Create Remote Site Settings in Salesforce

Go to Setup → Security → Remote Site Settings.

Click New Remote Site.

Enter the following details:

Remote Site Name: ZipcodeStack

Remote Site URL: https://api.zipcodestack.com

Active: ✅ Checked

Click Save.

This allows Salesforce to make callouts to the external API endpoint.

3. Create Apex Class for Callout

Create a new Apex class named ZipcodeStackService.cls:

public with sharing class ZipcodeStackService {
    
    @AuraEnabled(cacheable=false)
    public static String getCityName(String zipcode) {
        try {
            String apiKey = 'YOUR_API_KEY_HERE';
            String endpoint = 'https://api.zipcodestack.com/v1/search?codes=' + zipcode + '&country=US&apikey=' + apiKey;

            Http http = new Http();
            HttpRequest request = new HttpRequest();
            request.setEndpoint(endpoint);
            request.setMethod('GET');

            HttpResponse response = http.send(request);
            if (response.getStatusCode() == 200) {
                Map<String, Object> result = (Map<String, Object>) JSON.deserializeUntyped(response.getBody());

                if (result.containsKey('results')) {
                    Map<String, Object> results = (Map<String, Object>) ((List<Object>) result.get('results')).get(0);
                    if (results.containsKey('city')) {
                        return 'The city for zip code ' + zipcode + ' is ' + (String) results.get('city');
                    }
                }
                return 'City not found for the given zip code.';
            } else {
                return 'Error: Received status code ' + response.getStatusCode();
            }
        } catch (Exception e) {
            return 'Exception occurred: ' + e.getMessage();
        }
    }
}


Replace YOUR_API_KEY_HERE with your actual API key from Zipcode Stack.

4. Create Agentforce Topic and Action
a. Create a Topic

Navigate to Agentforce → Topics.

Click New Topic.

Enter:

Name: Zipcode Finder

Description: Helps users find the city name for a given zip code.

Save the topic.

b. Create an Action

Go to Agentforce → Actions.

Click New Action.

Provide the following details:

Name: Find City by Zipcode

Linked Apex Class: ZipcodeStackService

Method Name: getCityName

Parameters: zipcode

Add clear instructions:

“If a user asks for the city corresponding to a zip code, call the getCityName method in ZipcodeStackService with the provided zip code and return the response.”

Assign the Action to the Zipcode Finder topic.

Activate the topic and the action.

5. Test the Agent

Try asking your Agent:

“What is the city for zip code 30301?”

✅ Expected Response:

“The city for zip code 30301 is Atlanta.”

🧪 Common Errors and Fixes
Error	Cause	Fix
Unauthorized endpoint	Missing Remote Site Settings	Add the API URL in Setup → Remote Site Settings
Insufficient privileges	The executing user lacks access to Apex class	Go to Setup → Profiles → Apex Class Access and grant access to ZipcodeStackService
Invalid API key	Wrong or expired key used	Verify API key in Zipcode Stack dashboard
Malformed response	API response changed	Check JSON structure and update parsing logic
Timeout or Limit exceeded	Salesforce callout or API rate limit hit	Use a lower frequency or upgrade API plan
🧰 Additional Notes

You can extend this logic to include country, state, or timezone fields from the API.

Use Named Credentials instead of hardcoding the API key for better security.

Always test in Sandbox before deploying to Production.

📄 License

This project is licensed under the MIT License.

💬 Author

Logesh Aravindh
Built as part of a Salesforce Agentforce integration demo.

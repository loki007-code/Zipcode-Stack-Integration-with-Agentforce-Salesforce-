🏙️ Zipcode Stack Integration with Agentforce (Salesforce)
==========================================================

📘 Overview
-----------

This project demonstrates how to integrate **Agentforce** with the **Zipcode Stack API** to retrieve **city names** for given **zip codes**.

Whenever an internal Salesforce employee or an external user asks the Agent for the city corresponding to a zip code, the Agent invokes an **Apex Action** that performs a **callout** to the external Zipcode Stack API and returns the **city name** in real-time.

⚙️ Use Case
-----------

When a user asks:

> “What is the city for zip code 94040?”

The Agent invokes the configured **Apex class**, which calls the **Zipcode Stack API**, parses the response, and returns:

> “The city for zip code 94040 is Mountain View.”

🧩 Prerequisites
----------------

Before starting, ensure the following:

*   You have a **Salesforce org** with **Agentforce** enabled.
    
*   You have **API access** and **Apex callout** permissions.
    
*   You have an active **Zipcode Stack** account and a valid **API key**.
    

🚀 Steps to Implement
---------------------

### 1\. Create an Account in Zipcode Stack

1.  Visit [https://zipcodestack.com](https://zipcodestack.com).
    
2.  Sign up for a free or paid account.
    
3.  Navigate to the **Dashboard → API Keys** section.
    
4.  Copy your **API Key**, which will be required in the Apex callout.
    

### 2\. Create Remote Site Settings in Salesforce

1.  Go to **Setup → Security → Remote Site Settings**.
    
2.  Click **New Remote Site**.
    
3.  Enter the following details:
    
    *   **Remote Site Name:** ZipcodeStack
        
    *   **Remote Site URL:** https://api.zipcodestack.com
        
    *   **Active:** ✅ Checked
        
4.  Click **Save**.
    

> This allows Salesforce to make callouts to the external API endpoint.

### 3\. Create Apex Class for Callout

Create an Apex class (e.g., ZipcodeStackService.cls) that performs the external API callout and returns the city name for the given zip code.

👉 Add your Apex code file separately in the /force-app/main/default/classes directory.

### 4\. Create Agentforce Topic and Action

#### a. Create a Topic

1.  Navigate to **Agentforce → Topics**.
    
2.  Click **New Topic**.
    
3.  Enter:
    
    *   **Name:** Zipcode Finder
        
    *   **Description:** Helps users find the city name for a given zip code.
        
4.  Save the topic.
    

#### b. Create an Action

1.  Go to **Agentforce → Actions**.
    
2.  Click **New Action**.
    
3.  Provide the following details:
    
    *   **Name:** Find City by Zipcode
        
    *   **Linked Apex Class:** ZipcodeStackService
        
    *   **Method Name:** getCityName
        
    *   **Parameters:** zipcode
        
4.  “If a user asks for the city corresponding to a zip code, call the getCityName method in ZipcodeStackService with the provided zip code and return the response.”
    
5.  Assign the Action to the **Zipcode Finder** topic.
    
6.  Activate the topic and the action.
    

### 5\. Test the Agent

Try asking your Agent:

> “What is the city for zip code 30301?”

✅ Expected Response:

> “The city for zip code 30301 is Atlanta.”

🧪 Common Errors and Fixes
--------------------------

ErrorCauseFix**Unauthorized endpoint**Missing Remote Site SettingsAdd the API URL in **Setup → Remote Site SettingsInsufficient privileges**The executing user lacks access to Apex classGo to **Setup → Profiles → Apex Class Access** and grant access to ZipcodeStackService**Invalid API key**Wrong or expired key usedVerify API key in Zipcode Stack dashboard**Malformed response**API response changedCheck JSON structure and update parsing logic**Timeout or Limit exceeded**Salesforce callout or API rate limit hitUse a lower frequency or upgrade API plan

🧰 Additional Notes
-------------------

*   You can extend this logic to include **country**, **state**, or **timezone** fields from the API.
    
*   Use **Named Credentials** instead of hardcoding the API key for better security.
    
*   Always test in **Sandbox** before deploying to **Production**.
    

💬 Author
---------

**Logesh Aravindh**Built as part of a Salesforce Agentforce integration demo.

# RapidPro-WhatsApp-Integration
Integrating whatsapp Cloud Api using External api

# Required WhatsApp Business setup
Flow the steps [here](https://developers.facebook.com/docs/whatsapp/cloud-api/get-started) to setup WhatsApp Business

# RapidPro External API setup

1. When creating an External API Channel under URN Type leave as Phone Number

2. Under Phone Number put the phone number of your whatsapp chatbot starting with country code eg 2335xxxx566

3. Leave method as HTTP POST

4. Leave Encoding as Default Encoding

5. Change Content Type to Application/json

6. update Max Length to to suit your desired text length. max length allowed is 640

7. On send URL is the webhook that accepts POST request when RapidPro want to send a message. this is where our webhook conduit between RP and facebook's Graph API comes in. it accepts request from RP then post that data to https://graph.facebook.com/v17.0/. It also accepts request from WhatsApp then post that data to RP. example of send URL will be https://a65e-154-160-14-26.ngrok-free.app/webhook

8. On Request Body use this Json format {"text":{{text}}, "to_no_plus":{{to_no_plus}}}

9. On Reponse put the text 'OK'

10. Save the channel. It will take you to next step where it generates Receive URL, this URL is the one that receives data posted to RP and triggers a Flow.

11. add the webhook inside Meta dashboard and also create a **permanent access token** (see instructions below).

## Getting started

1. Clone the repository

2. Update environment variables
  - `VERIFY_TOKEN` a random string of your choosing
  - `RAPIDPRO_URL` is the url of your RapidPro deployment  
  - `WHATSAPP_TOKEN` copy the value of your **permanent** access token (see setup instructions below)
  - `PHONE_NUMBER_ID` copy the value of the Phone number ID from WhatsApp > API Setup in your App Dashboard
  - `RP_RECEIVE_URL` the generated Receive URL from RP
3. Run `npm install`
4. Run `npm start`


## How it works
- It accepts request from RP then post that data to https://graph.facebook.com/v17.0/. 
- It accepts request from WhatsApp then post that data to RP.

## Notes/known issues
1. ~~WhatsApp TOKEN refreshes every 24hours. We are yet to add the token refresh snippet. We  currently manually copy the token from Meta dashboard.~~ **SOLVED**: Use permanent access token (see setup instructions below)
2. Messages from RP flows needs to be saved as templates on Meta Dashboard. The template title is used as the flow message.

## Creating a Permanent WhatsApp Access Token

To avoid the 24-hour token expiration issue, create a permanent access token:

### Step 1: Access Meta Business Manager
1. Navigate to [Meta Business Manager](https://business.facebook.com/)
2. Log in with your business account
3. Click on **Business Settings** in the left sidebar

### Step 2: Create a System User
1. Go to **Users** > **System Users**
2. Click **Add** to create a new system user
3. Provide a descriptive name (e.g., "WhatsApp Integration User")
4. Set the role to **Admin**
5. Click **Create System User**

### Step 3: Assign Assets and Permissions
1. Select the newly created system user
2. Click on **Add Assets**
3. Choose your WhatsApp app from the list
4. Grant **Full Control** permissions
5. If your WhatsApp Business Account appears, assign it with full access
6. Click **Save Changes**

### Step 4: Generate Permanent Token
1. With the system user selected, click **Generate New Token**
2. Select your app from the dropdown
3. Under **Token Expiration**, choose **Never** (this creates a permanent token)
4. Ensure these permissions are checked:
   - `whatsapp_business_management`
   - `whatsapp_business_messaging`
5. Click **Generate Token**
6. **IMPORTANT**: Copy the token immediately - it won't be displayed again!

### Step 5: Update Your Environment
1. Replace `WHATSAPP_TOKEN` in your `.env` file with the permanent token
2. Restart your webhook server

**Benefits:**
- ✅ No more 24-hour expiration
- ✅ No manual token updates needed
- ✅ Stable, uninterrupted service
- ✅ Production-ready solution

## Reference
https://developers.facebook.com/docs/whatsapp/sample-app-endpoints
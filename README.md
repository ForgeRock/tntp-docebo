# Docebo SaaS REST Connector for Ping Advanced Identity Cloud

This repository contains a sample SaaS REST connector definition for integrating **Docebo** with **PingOne Advanced Identity Cloud** using the **SaaS REST connector**. The connector definition in `docebo_connector.json` uses the REST connector bundle `org.forgerock.openicf.connectors.rest.RestConnector` and defines CRUD plus query operations for the `__ACCOUNT__` object type.

## Overview

The SaaS REST connector lets Ping Advanced Identity Cloud interact with REST APIs for provisioning and managing external accounts. In Advanced Identity Cloud, the SaaS REST application template is intended for user, group, and similar object management through a REST API.

This example is configured for Docebo user management and currently includes:

- `CREATE` → `POST /user`
- `GET` → `GET /user/{uid}`
- `QUERY` → `GET /user`
- `UPDATE` → `PATCH /user/{uid}`
- `DELETE` → `DELETE /user/delete` 

## Prerequisites

Before configuring the connector, make sure you have:

- A Ping Advanced Identity Cloud tenant with access to connector configuration.
- A Docebo API base URL.
- A Docebo API authentication method and credentials.

## Configuration

### Create the SaaS REST Application

Before configuring the connector operations, you must first create a **SaaS REST application** in Ping Advanced Identity Cloud.

Follow these steps:

### 1. Navigate to Applications

- In the Ping Advanced Identity Cloud admin UI, go to **Applications**
- Click **Browse App Catalog**

### 2. Select the SaaS REST template

- Search for **SaaS REST**
- Select the **SaaS REST** application template (not “via Connector”)
- Click **Next**

### 3. Configure application details

Provide the required application details:

- **Name**: A unique name for your application (for example, `docebo-dev`)
- **Description**: (Optional) აღწ description of the integration
- **Owners**: Assign at least one application owner
- **App Logo URI**: (Optional)
- **Authoritative**: Leave unchecked unless this system is the source of truth for identities

Click **Create Application**

### 4. Open provisioning setup

- After the application is created, navigate to the **Provisioning** tab
- Click **Set up Provisioning**

At this point, the application is created and ready for connector configuration.

> The next step is to configure the connector by uploading the `docebo_connector.json` via the API.

To use this connector, you need to update the `docebo_connector.json` file with your Docebo API details and authentication settings.

### 5. Set authentication details

The connector is configured to use a **Refresh token **. Update the following fields with values from your Docebo API application:

- `authenticationMethod`: Leave as `TOKEN`
- `grantType`: Set to `refresh_token`
- `refreshToken`: Your Docebo refresh token
- `tokenEndpoint`: The Docebo OAuth token endpoint URL
- `clientId`: Your Docebo API client ID
- `clientSecret`: Your Docebo API client secret
- `useBasicAuthForOauthTokenNeg`: Usually `true`

### 6. Verify API base URL

Ensure the base URL in the connector matches your Docebo environment, for example:

    https://<your-docebo-domain>/manage/v1

### 7. Apply the configuration

Once you’ve updated the JSON file, deploy the connector by making the following API call:

    PUT https://openam-<FQDN>.forgeblocks.com/openidm/config/provisioner.openicf/<CONNECTOR_NAME>?waitForCompletion=true

- Replace `<FQDN>` with your Ping Advanced Identity Cloud tenant domain  
- Replace `<CONNECTOR_NAME>` with the name you want to assign to this connector  
- Use the contents of your updated `docebo_connector.json` as the request body  

### What happens next?

After the configuration is applied:

- The connector will use the refresh token to obtain access tokens automatically  
- All provisioning operations (create, update, delete, query) will use the Docebo REST API  
- You can begin mapping identities to the `__ACCOUNT__` object in Ping Advanced Identity Cloud  

## Supported schema

The connector currently defines the `__ACCOUNT__` object type with these attributes:

- `first_name`
- `firstname`
- `last_name`
- `lastname`
- `password`
- `user_id`
- `user_ids`
- `userid`
- `username`

Some of these are duplicated in different naming styles because the request and response payloads use different field names. For example:

- create/update requests use `firstname` and `lastname`
- read/query responses return `first_name` and `last_name`

## Operation mappings

### Create account

Creates a Docebo user with:

- Method: `POST`
- Path: `/user`
- ID path: `data.user_id`

Request attribute mapping:

- `firstname` → `firstname`
- `lastname` → `lastname`
- `password` → `password`
- `userid` → `userid`

Example request body sent by the connector:

```json
{
  "firstname": "Jane",
  "lastname": "Doe",
  "password": "",
  "userid": "jane.doe"
}

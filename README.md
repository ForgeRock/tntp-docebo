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
- Select the **SaaS REST** application template 
- Click **Next**

### 3. Configure application details

Provide the required application details:

- **Name**: A unique name for your application (for example, `docebo-dev`)
- **Description**: (Optional) description of the integration
- **Owners**: Assign at least one application owner
- **App Logo URI**: (Optional)
- **Authoritative**: Leave unchecked unless this system is the source of truth for identities

Click **Create Application**

### 4. Download docebo_connector.json template

Download the docebo_connector.json file

### 5. Set docebo connectors values

To use this connector, you need to update the `docebo_connector.json` file with your Docebo API details and authentication settings.

The connector is configured to use a **Refresh token **. Update the following fields with values from your Docebo API application:
- `authToken`: Your Docebo auth token
- `refreshToken`: Your Docebo refresh token
- `serviceUri`: The Docebo OAuth token endpoint URL
- `tokenEndpoint`: The Docebo OAuth token endpoint URL
- `clientId`: Your Docebo API client ID
- `clientSecret`: Your Docebo API client secret


### 6. Apply the configuration to PingAIC

Once you’ve updated the JSON file, deploy the connector by making the following API call:

```
curl --location --request PUT 'https://{FQDN}.com/openidm/config/provisioner.openicf/{CONNECTOR_NAME}?waitForCompletion=true' \
--header 'authorization: Bearer <BEARER TOKEN>'\
--header 'content-type: application/json' \
--data '{docebo_connector.json}'
```

- Replace `<FQDN>` with your Ping Advanced Identity Cloud tenant domain  
- Replace `<CONNECTOR_NAME>` with the name you want to assign to this connector  
- Use the contents of your updated `docebo_connector.json` as the request body

### 7. Test the connector

CRUD operations are now enabled for the Docebo connector

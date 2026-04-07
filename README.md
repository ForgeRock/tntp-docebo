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

The SaaS REST connector is configured over REST rather than directly through the connector UI in the connector reference documentation. 

## Files

- `docebo_connector.json` — the connector configuration for the Docebo integration. 

## Connector authentication model

The current JSON is set to use **OAuth**-style token acquisition settings even though `authenticationMethod` is currently set to `TOKEN` in the file. Specifically, the config includes:

- `authenticationMethod`: `TOKEN`
- `authorizationTokenPrefix`: `Bearer`
- `grantType`: `refresh_token`
- `refreshToken`: 
- `tokenEndpoint`: 
- `clientId`: 
- `clientSecret`: 
- `useBasicAuthForOauthTokenNeg`: `true`

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

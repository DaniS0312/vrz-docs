---
id: new_workstation_api 
title: New Workstation
sidebar_label: New Workstation 
sidebar_position: 2
---

---
In this section, we will be looking at how to create a Workstation on Virtual RouteZ via an API call. A Workstation is a
compulsory requirement when using the new event API call, and therefore the workstation involved in an event must be
created prior to logging an event that is linked to it.

**In this section we will look at:**

- [Prerequisites](#prerequisites)
- [API Data](#api-data)
- [Authentication](#authentication)
- [API Parameters](#api-parameters)
- [Responses](#responses)
- [Examples](#examples)

## PREREQUISITES

**Below is what you will need in order to start using API's on Virtual RouteZ:**

- A Virtual RouteZ account for your organisation
- [A Valid API Key](#api-data)
- [The name of the workstation](#api-parameters)
- [A WorkstationUid](#api-parameters)

:::note If you do not have a Virtual RouteZ account for your organisation, you can email Francois@Ideco.co.za to obtain
one.
:::

## API DATA

### POST DATA

This type of data will be Post, which is the HTTP method that is designed to send loads of data to a server from a
specified resource. In this case, we will be sending (Posting) data to the Virtual RouteZ server.

### URL'S TO BE USED

The URL that you will use to post to will be https://web.virtualroutez.app/api/external_workstation/newWorkstation

## AUTHENTICATION

In order to obtain authentication to be able to post to Virtual RouteZ, you will require:

**A Valid API Key:**

To authenticate, add an "api_key" header to your API request that contains an API Key. This API key will be provided to
you by Virtual RouteZ.

***Example Header:***
:::note

POST https://web.virtualroutez.app/api/external_workstation/newWorkstation

Authorization: API Key

:::

**A Body:**
The body for this request will be `new_workstation_json_encoded_object`

## API PARAMETERS

Now that we understand the authentication requirements and URL, let's have a look at the parameters in this API Payload.

| PARAMETER   | DESCRIPTION |TYPE| COMPULSORY|
| ----------- | ----------- |----------- |-----------
| WorkstationName      | This is the name of the workstation being created. For example, if we were harvesting fruit from a farm with various places for various fruits, and example of a WorsktationName would be "Apple Section"       | String      |Yes
| WorkstationUid   | This is the unique identifier for this workstation. This identifier is used for the New Event API call, and link it to the correct organisation via the WorkstationUid       | Array       | Yes
| Description   | This is the description of the workstation. An example of this would be "The Apple Section of the Farm"        | String       | No
| LocationNumber  | This is the location number of the workstation. For example, we could say that 2B could be the section of the farm where our items are grown and procured. The LocationNumber here could be "SEC2B"     | String       | No

## RESPONSES

Each API call will return a response of either successful and unsuccessful. Below are the responses you may receive with
this API call.

### SUCCESSFUL RESPONSES:

If an API call has run successfully, the response below will be given:

```json
{
  "Result": "Success",
  "AuthenticationToken": "123456789987456321",
  "Message": "The workstations have been successfully created."
}
```

### UNSUCCESSFUL RESPONSES:

**Missing Information**
If an API call has not run successfully and a parameter was missing from the payload, the response below will be given:

```json
{
  "Result": "Failed",
  "AuthenticationToken": "123456789987456321",
  "Message": "The following attributes were of the wrong type or blank.",
  "Attributes": [
    "WorkstationName"
  ]
}
```

As you can see in the above example, the WorkstationName was not provided, and as it is a compulsory parameter, resulted
in a failed response.

**Not Authorised:**
If an API key is not valid for this event, the below error will be given:

```json
{
  "Result": "Failed",
  "Message": "Not authorized."
}
```

**Workstation Already Exists:**
In this instance, an error is received as a workstation for this organisation already exists with the WorkstationUid
provided.

```json
{
  "Result": "Failed",
  "AuthenticationToken": "123456789987456321",
  "Message": "The following workstations already exists in the system.",
  "WorkstationUid": [
    "APLFRM"
  ]
}
```

**API Key Is Not Linked to a Valid Organisation**
When the API Key does not link to an organisation that is allowed to generate API Calls for Virtual RouteZ, the
following error will be received:

```json
{
  "Result": "Failed",
  "AuthenticationToken": "12345678987456322",
  "Message": "ApiKey provided is not linked to a valid organisation."
}
```

**Same Payload Requested Less Than A Minute Ago:**
When the exact same payload is run in less than a minute later than it was previously run, the following error will be
received:

```json
{
  "Result": "Failed",
  "AuthenticationToken": "12345678987456322",
  "Message": "Exact same payload requested within a minute ago. Request terminated."
}
```

## EXAMPLES

### Create a Workstation Example

If we were to create a workstation for an apple farm, we could do so with this API call.

**Below is an easy to read example:**
:::note Example

**Name:** Harvesting

**Description:** Harvesting Tree

**UID:** Fruit Tree

**Location Number:** Fruit

:::


**Payload to achieve this:**

```json
{
  "api_key": "8BCUg4TxQKteYR9H5jh7n3czvrWVwiA2lFosLPJumyqa6fID",
  "new_workstation_json_encoded_object": [
    {
      "WorkstationName": "HRVST1",
      "Description": "This is where we grow apples",
      "LocationNumber": "SEC2B",
      "WorkstationUid": "FRM1"
    }
  ]
}
```

### Workstation No Description or Location Number Example

If we wanted to add a workstation, without a Description or a LocationNumber, it is possible to do this by leaving these
parameters out of the API call.
**Below is an easy to read example:**

:::note Example

**Name:** Harvesting

**UID:** Fruit Tree
:::


**Payload to achieve this:**

```json
{
  "api_key": "8BCUg4TxQKteYR9H5jh7n3czvrWVwiA2lFosLPJumyqa6fID",
  "new_workstation_json_encoded_object": [
    {
      "WorkstationName": "MSTN",
      "WorkstationUid": "FRM1"
    }
  ]
}
```

## IMPORTANT NOTES

- If the workstation referenced in the payload does exist, this API call will not work.
- If the workstation referenced in the payload does not exist in the system, a new one will be created.
- If you try to submit the exact same payload within a minute of the last submission, you will get an error: "Exact same
  payload requested within a minute ago. Request terminated.






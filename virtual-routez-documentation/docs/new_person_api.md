---
title: New Person
sidebar_label: New Person
sidebar_position: 1
---

---

In this section, we will be looking at how to create a Person on Virtual RouteZ via an API call. A Person is a
compulsory requirement when using the new event API call, and therefore the person involved in an event must be created
prior to logging an event that is linked to them.

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
- [The first and last name of this person](#api-parameters)
- [An ExternalUid](#api-parameters)

> If you do not have a Virtual RouteZ account for your organisation, you can email Francois@Ideco.co.za to obtain one.

## API DATA

### POST DATA

This type of data will be Post, which is the HTTP method that is designed to send loads of data to a server from a
specified resource. In this case, you will be sending (Posting) data to the Virtual RouteZ server.

### URL'S TO BE USED

The URL that you will use to post to will be https://web.virtualroutez.app/api/external_person/newPerson.

## AUTHENTICATION

In order to obtain authentication to be able to post to Virtual RouteZ, you will require:

**A Valid API Key:**
To authenticate, add an "api_key" header to your API request that contains an API Key. This API key will be provided to
you by Virtual RouteZ.

:::note Example Header:

POST https://web.virtualroutez.app/api/external_person/newPerson

Authorization: API Key

:::

**A Body:**
The body for this request will be `new_person_json_encoded_object`.

## API PARAMETERS

Now that we understand the authentication requirements and URL, let's have a look at the parameters in this API Payload.

| PARAMETER   | DESCRIPTION |TYPE| COMPULSORY|
| ----------- | ----------- |----------- |-----------
| FirstName      | This is the first name of the person being created. For example, if we were creating a person for someone who is working at a workstation performing an operation, "John" could be the FirstName.      | String      |Yes
| LastName   |  This is the first name of the person being created. For example, if we were creating a person for someone who is working at a workstation performing an operation, "Doe" could be the LastName.      | String       | Yes
| IdNumber   | This is the ID number of the person being created.        | String       | No
| EmailAddress  | This is the email address of the person being created. For example, if we were creating a person for someone who is working at a workstation performing an operation, "John@applefarming.com" could be the EmailAddress.     | String       | No
| ExternalUid  | This is the unique identifier of the person being created. For example, if we were creating a person for someone who is working at a workstation performing an operation, "JHNDOE" could be the ExternalUid. | String       | Yes
| OrganisationRole  | This is the role that this person has within the organisation. For example, if we were creating a person for someone who is harvesting apples, "Harvester" could be the OrganisationRole. | String       | No

## RESPONSES

Each API call will return a response of either successful or unsuccessful. Below are the responses you may receive with
this API call.

#### SUCCESSFUL RESPONSES:

If an API call has run successfully, the response below will be given:

```json 
{
    "Result": "Success",
    "AuthenticationToken": "123456789987456321",
    "Message": "The people have been successfully added."
}
```

#### UNSUCCESSFUL RESPONSES:

**Missing Information**
If an API call has not run successfully and a parameter was missing from the payload, the response below will be given:

```json
 {
  "Result": "Failed",
  "AuthenticationToken": "123456789987456321",
  "Message": "The following attributes were of the wrong type or blank.",
  "Attributes": [
    "FirstName"
  ]
}
```

As you can see in the above example, the FirstName was not provided, and as it is a compulsory parameter, resulted in a
failed response.

**Not Authorised:**
If an API key is not valid for this event, the below error will be given:

```json
{
  "Result": "Failed",
  "Message": "Not authorized."
}
```

**Person Already Exists:**
In this instance, an error is received as a person for this organisation already exists with the ExternalUid number
provided.

```json
{
  "Result": "Failed",
  "AuthenticationToken": "333bf3a512752b68112d0360170c1ec4",
  "Message": "The following People already exist in the system.",
  "PersonUid": [
    "JHNDOE"
  ]
}
```

**API Key Is Not Linked To A Valid Organisation**
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

## EXAMPLES #

#### Create a Person Example ###

If we were to create a workstation for an apple farm, we could do so with this API call.

:::note Below is an easy to read example:

**First Name:** John

**Last Name:** Doe

**ExternalUid:** JHNDOE

**Id Number:** 1212556635889

**Email Address:** Johndoe@applefarmers.com

**OrganisationRole:** Harvester

:::


**Payload to achieve this:**

```json
{
  "api_key": "123kjhnmk12345668422",
  "new_person_json_encoded_object": [
      {
        "FirstName": "John",
        "LastName": "Doe",
        "IdNumber": "1212556635889",
        "EmailAddress": "Johndoe@applefarmers.com",
        "ExternalUid": "JHNDOE",
        "OrganisationRole": "Harvester"
      }
  ]
}
```

#### Person No OrganisationRole, EmailAddress or IdNumber Example ###

If we wanted to add a person, without an OrganisationRole, EmailAddress or IdNumber, it is possible to do this by
leaving these parameters out of the API call.

:::note 

**First Name:** John

**Last Name:** Doe

**ExternalUid:** JHNDOE

:::


**Payload to achieve this:**

```json
{
  "api_key": "123kjhnmk12345668422",
  "new_person_json_encoded_object": [
    {
      "FirstName": "John",
      "LastName": "Doe",
      "ExternalUid": "JHNDOE"
    }
  ]
}
```

## IMPORTANT NOTES

- If the person referenced in the payload does exist, this API call will not work.
- If the person referenced in the payload does not exist in the system, a new one will be created.
- If you try to submit the exact same payload within a minute of the last submission, you will get an error: "Exact same
  payload requested within a minute ago. Request terminated.

---






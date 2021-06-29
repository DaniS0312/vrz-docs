---
id: new_event_api
title: New Event 
sidebar_label: New Event
sidebar_position: 3
---

---
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
- [A Valid API Key](#api-parameters)
- [The EventDescription](#api-parameters)
- [The InputItemInfo](#api-parameters)
- [The InputItemName](#api-parameters)
- [The InputItemGtin](#api-parameters)
- [The MetricName](#api-parameters)
- [The MetricUnit](#api-parameters)
- [The MetricValue](#api-parameters)
- [The StartDateTime](#api-parameters)
- [The EndDateTime](#api-parameters)
- [A WorkstationUid](#api-parameters)
- [A PersonUid](#api-parameters)
- [The OutputItemInfo](#api-parameters)
- [The OutputItemName](#api-parameters)
- [The OutputItemGtin](#api-parameters)
- [The OperationUid](#api-parameters)
- [The OperationName](#api-parameters)
- [The OperationDescription](#api-parameters)

:::note

If you do not have a Virtual RouteZ account for your organisation, you can email Francois@Ideco.co.za to obtain one.

:::

## API DATA

### POST DATA

This type of data will be Post, which is the HTTP method that is designed to send loads of data to a server from a
specified resource. In this case, we will be sending (Posting) data to the Virtual Routez server.

### URL'S TO BE USED

The URL that you will use to post to will be https://web.virtualroutez.app/api/external_event/newEvent

## AUTHENTICATION

In order to obtain authentication to be able to post to Virtual Routez, you will require:

**A Valid API Key:**
To authenticate, add a api_key header to your API request that contains an API Key. This API key will be provided to you
by Virtual Routez.

:::note Example Header:

POST https://web.virtualroutez.app/api/external_event/newEvent

Authorization: API Key

:::

**A Body:**
The body for this request will be `new_event_json_encoded_object`.

## API PARAMETERS

Now that we understand the authentication requirements and URL, let's have a look at the parameters in this API Payload.

| PARAMETER   | DESCRIPTION |TYPE| COMPULSORY|
| ----------- | ----------- |----------- |-----------
| EventDescription      | This describes what event took place. A good example could be "Harvesting Bananas"       | String      |Yes
| InputItemInfo   | This starts the array of all of the input items information, such as the name, description, reference, Gtin number, metric name, metric value and metric unit.       | Array       | Yes
| InputItemName   | This is the name of the input item. An example of this would be "Harvested Banana"        | String       | This parameter is compulsory if the input item does not yet exist.
| InputItemGtin  | This is the unique identifier for this item. The API call will only update the item with that Gtin number if the item exists. An Example of this could be "BANANA1"        | String       | Yes
| InputItemReference   | This is the reference number given to this item by your organisation. An example of this could be "BananasFromWorkshop1"        | String       | No
| MetricName   | This is the name of the metric that your item is weighed in. If your item is weighed in kilograms, the metric name would be "Kilogram".        | String       |  This parameter is compulsory if the item does not yet exist. Metrics are applied to both input and output items.
| MetricUnit   | This is the unit of measurement used for the item. An example of this would be if the MetricName is "Gram", the MetricUnit would be "g". Metrics are applied to both input and output items.   | String       | This parameter is compulsory if the item does not yet exist.
| MetricValue   | This parameter refers to the actual amount of weight that this item has. An example of this would be if an item weighed 100 kilograms, the MetricValue would be "100". Metrics are applied to both input and output items.       | Number       | Yes
| StartDateTime   | This is the date and time that your event started. For example, if you were to harvest all fruit on a farm from 09 am on the 16th of June 2021, the StartDateTime would be  "2021-06-16 09:00:00". The time supplied here should be UCT time.      | String       | Yes
| EndDateTime   | This is the date and time that your event ended. For example, if you were to harvest all fruit on a farm until 08:15 am on the 18th of June 2021, the EndDateTime would be  "2021-06-18 08:15:00". The time supplied here should be UCT time.         | String       | Yes
| WorkstationUid   |  This is the unique identifier for the workstation that the event took place at. For example, if an organisation is harvesting various fruits, the workstation for this event could be "BNAFIELD1" The API call will only update the workstation with that Gtin number.       | String       | Yes
| PersonUid   |  This is the unique identifier for the person who did this event. For example, if an organisation is harvesting various fruits, the person within this organisation who did the harvesting will have a unique identifier in the system. For this event, the PersonUID could be "JOHN1" The API call will only update the person with that Gtin number.       | String       | Yes
| OutputItemInfo      | This starts the array of all of the input items information, such as the name, description, reference, Gtin number, metric name, metric value and metric unit.       | Array       | Yes
| OutputItemName      |  This is the name of the output item that was a result of this event happening to the input item. An example of this would be "Sliced Banana"      | String       | Yes
| OutputItemGtin      |  This is the unique identifier for this item. The API call will create an item on Virtual Routez with that Gtin number. An example of this could be "SLICEDBNA1"   | String       | Yes
| OperationName      |This is the name of the operation that your organisation performs. Multiple events can be grouped by operations. An operations main function is to group events in order to see your organisation route. An example of this could be "Procurement of Fruit" | String| Only if the Operation does not yet exist.
| OperationUid      | In order for us to uniquely identify each operation that to your organisation, a unique OperationUid is required. An example of this could be "HARESTBNANA" | String| Yes
| OperationDescription      |This is the description of this operation. An example of this could be "Picking bananas from the banana trees" | String| Only if the Operation does not yet exist.

## RESPONSES

Each API call will return a response of either successful and unsuccessful. Below are the responses you may receive with
this API call.

### SUCCESSFUL RESPONSES:

If an API call has run successfully, the response below will be given:

```json
{
  "Result": "Success"
  "AuthenticationToken": "12345678987456322",
  "Message": "Successfully created new event."
}
```

### UNSUCCESSFUL RESPONSES:

**Not Authorized**
The response below will be given if:

- The API key is not recognized on Virtual Routez.
- The API key has expired.
- The API Key does not contain the relevant operation, or the operation is inactive.

```json
{
  "Result": "Failed",
  "Message": "Not authorized."
}
```

**JSON Structure is Incorrect**
The response below will be given if:

- The JSON string that you are tryng to submit has a mistake that prevents it from being seen as an array. An example of
  this could be a missing or extra comma in the JSON String.

```json
{
  "Result": "Failed",
  "AuthenticationToken": "123456987452133",
  "Message": "The json encoded object is not in the correct format. Please refer to the API endpoint documentation."
}
```

**Basic Validation Failure** :
The response below will be given if:

- There are unrecognized fields in the JSON string.
- Required fields are not present in the JSON string.
- The JSON string is in the incorrect format.
- The input and output item arrays contain error. Thi can send back multiple messages for:
    - There are unrecognized fields in the input or output item arrays.
    - Required fields are not present in the input or output item arrays.
    - The input or output item arrays conatin the incorrect format.

```json
{
  "Result": "Failed",
  "AuthenticationToken": "123456987452133",
  "Message": "Some of the input information provided is invalid.",
  "ValidationProgress": "1/2",
  "ErrorInformationArr": [
    [
      {
        "Message": "Some fields were not in the correct format",
        "IncorrectFormatFields": {
          "StartDateTime": {
            "RequiredFormatType": "datetime",
            "ProvidedValue": "2020-20-10 20:20:20",
            "ValidFormat": "Y-m-d H:i:s",
            "ValidFormatExample": "2020-20-20 20:20:20"
          }
        }
      },
      {
        "Message": "Errors founds in item arrays.",
        "ItemArrayErrors": {
          "InputItemInfo": {
            "UnknownFields": [
              "InputItemGtinn"
            ],
            "RequiredAndNotProvided": [
              "InputItemGtin",
              "MetricValue"
            ]
          }
        }
      }
    ]
  ]
}
```

This response could be given when the date and time format is not provided, when a compulosry parameter is not provided,
such as InputItemGtin, or if a parameter is not recognised. In the above example, you can see that the InputItemGtin
parameter was given as "InputItemGtinn".

**Missing Information:**
The response below will be given if:

- A person or workstation is not found in Virtual Routez.

```json
{
  "Result": "Failed",
  "AuthenticationToken": "12345678987456322",
  "Message": "Some required information you provided does not exist in our system, or is missing when creating an item. Please create them via relevant API.",
  "ValidationProgress": "2/2",
  "RequiredToExistEntityInstances": [
    [
      {
        "Message": "The following entities could not be found.",
        "RequiredToExistEntityInstances": [
          "Workstation",
          "Person"
        ]
      }
    ]
  ]
}
```

- There is missing input item information when creating a new input item.

```json
{
  "Result": "Failed",
  "AuthenticationToken": "12345678987456322",
  "Message": "Some required information you provided does not exist in our system. Please create them via relevant API.",
  "ValidationProgress": "2/2",
  "RequiredToExistEntityInstances": [
    [
      {
        "Message": "The following information is required when creating a new input item.",
        "RequiredToExistEntityInstances": [
          "MetricName",
          "MetricUnit",
          "MetricValue"
        ]
      }
    ]
  ]
}
```

We have the ability to create input items via an API call, however this will require the input item to have an
InputItemName, InputItemGrin, MetricValue, MetricName and MetricUnit. This can also take place when the WorkStationUid
or PersonUid is not recognised.

**Operation Name Not Provided:**
In this instance, the Operation name was not provided in the API call. As this is a compulsory parameter, a failed
response will be received.

```json
{
  "Result": "Failed",
  "AuthenticationToken": "12345678987456322",
  "Message": "When creating a new operation, the operation name is required."
}
```

**API Key Is Not Linked To A Valid Organisation**
When the API Key does not link to an organisation that is allowed to generate API Calls for Virtual Routez, the
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

### Multiple Events Example

There is an existing item with the gtin “TREE”. The first event is harvesting from the tree. There are two outputs:
Fruit and Harvested Tree. From there the second event happens which is transporting the fruit. Another input item exists
called "ORANGE1" and the output item is an offloaded orange. Because the input item already exists, only the input item
Gtin and MetricValue is required.

**Below is a easy to read example:**

:::note Example

**Operation:** Harvesting

**Event 1:** Harvesting Tree

**Input:** Fruit Tree

**MetricValue:** 100

**Output 1:** Fruit

**Output 2:** Harvested Tree

**Operation:** Transportation

**Event 2:** Offloading

**Input:** Orange

**MetricValue:** 25

**Output 1:** Offloaded Orange

:::

**Payload to achieve this:**

```json
{
  "api_key": "123kjhnmk12345668422",
  "new_event_json_encoded_object": [
    {
      "EventDescription": "Harvesting Tree",
      "InputItemInfo": [
        {
          "InputItemGtin": "Fruit Tree",
          "MetricValue": 100
        }
      ],
      "StartDateTime": "2021-01-01 00:00:00",
      "EndDateTime": "2022-01-02 00:00:00",
      "WorkstationUid": "FRMTR1",
      "PersonUid": "JHNDOE",
      "OutputItemInfo": [
        {
          "OutputItemName": "Fruit",
          "OutputItemGtin": "FRTORG1",
          "MetricName": "kilogram",
          "MetricUnit": "kg",
          "MetricValue": 25
        },
        {
          "OutputItemName": "Harvested Tree",
          "OutputItemGtin": "TRHRVSTD",
          "MetricName": "kilogram",
          "MetricUnit": "kg",
          "MetricValue": 100
        }
      ],
      "OperationUid": "HRVSTORG",
      "OperationName": "Harvesting",
      "OperationDescription": "Harvesting Oranges for Transportation"
    },
    {
      "EventDescription": "Offloading",
      "InputItemInfo": [
        {
          "InputItemGtin": "Orange1",
          "MetricValue": 25
        }
      ],
      "StartDateTime": "2020-12-03 00:00:00",
      "EndDateTime": "2022-02-04 00:00:00",
      "WorkstationUid": "FRMTRK1",
      "PersonUid": "JNDOE",
      "OutputItemInfo": [
        {
          "OutputItemName": "Offloaded Orange",
          "OutputItemGtin": "OFFLOADED",
          "MetricName": "kilogram",
          "MetricUnit": "kg",
          "MetricValue": 25
        }
      ],
      "OperationUid": "TRSPRTORNGE",
      "OperationName": "Transportation",
      "OperationDescription": "Transporting Oranges"
    }
  ]
}
```

### Creating an Input Item Example

If we want to create a new input item as well as the output items that were a result of this input item, we will need to
provide all of the parameters that need to exist in order to successfully create an input item.

An example of this would be if there is a fruit tree that was harvested (but not yet added to Virtual Routez) and
allowed for an apple to be an output item, all of the details for each item would need to be referenced, as the fruit
tree is not yet an input item on Virtual Routez.

**Below is a easy to read example:**

:::note Example

**Operation:** Harvesting

**Event 1:** Harvesting Tree

**Input:** Fruit Tree

**Output 1:** Apple
:::

**Payload to achieve this:**

```json
{
  "api_key": "123kjhnmk12345668422",
  "new_person_json_encoded_object": [
    {
      "EventDescription": "Harvesting Tree",
      "InputItemInfo": [
        {
          "InputItemGtin": "FRTREE",
          "InputItemName": "Fruit Tree",
          "MetricName": "kilogram",
          "MetricUnit": "kg",
          "MetricValue": "20"
        }
      ],
      "StartDateTime": "2020-05-20 20:20:20",
      "EndDateTime": "2020-06-20 20:20:20",
      "WorkstationUid": "FARM1",
      "PersonUid": "FARMER1",
      "OutputItemInfo": [
        {
          "OutputItemName": "APPLE",
          "OutputItemGtin": "APP1",
          "MetricName": "kilogram",
          "MetricUnit": "kg",
          "MetricValue": 20
        }
      ],
      "OperationUid": "HARVEST1",
      "OperationName": "Harvesting",
      "OperationDescription": "Harvesting Apples from Trees"
    }
  ]
}
```

### No Input Item Example

Sometimes, we would like to just create output items, and skip adding input items all together. AN example of this would
be not adding the fruit tree to Virtual Routez, and just the fruit. This works the same as creating an input item, but
items can also be added as output items instead of input items. In this case, we would only reference the fruit (output
item) and not the tree (input item)

**Below is a easy to read example:**

:::note Example

**Operation:** Harvesting

**Event 1:** Harvesting Tree

**Input:** ""

**Output 1:** Fruit
:::

**Payload to achieve this:**

```json
{
  "api_key": "123kjhnmk12345668422",
  "new_person_json_encoded_object": [
    {
      "EventDescription": "Harvesting Tree",
      "InputItemInfo": [
      ],
      "StartDateTime": "2021-01-01 00:00:00",
      "EndDateTime": "2022-01-02 00:00:00",
      "WorkstationUid": "MSTN",
      "PersonUid": "JHNDOE",
      "OutputItemInfo": [
        {
          "OutputItemName": "Fruit",
          "OutputItemGtin": "FRT1",
          "MetricName": "kilogram",
          "MetricUnit": "kg",
          "MetricValue": 25
        }
      ],
      "OperationUid": "HARVESTING",
      "OperationName": "Harvesting",
      "OperationDescription": "Harvesting Fruit"
    }
  ]
}
```

### Input Item Exists Example

If we want to create an event for an input item that already exists, only the Gtin number is required for this item. For
example, if we had an apple that was turned into apple juice, if the apple is the input item, we would only need to
reference the Gtin number of the apple.

**Below is a easy to read example:**

:::note Example

**Operation:** Making Juice

**Event 1:** Juicing Apples

**Input:** "APPL"

**Output 1:** Apple Juice

:::

**Payload to achieve this:**

```json
{
  "api_key": "123kjhnmk12345668422",
  "new_event_json_encoded_object": [
    {
      "EventDescription": "Harvesting Tree",
      "InputItemInfo": [
        {
          "InputItemGtin": "Fruit Tree",
          "MetricValue": 100
        }
      ],
      "StartDateTime": "2021-01-01 00:00:00",
      "EndDateTime": "2022-01-02 00:00:00",
      "WorkstationUid": "FRMTR1",
      "PersonUid": "JHNDOE",
      "OutputItemInfo": [
        {
          "OutputItemName": "Fruit",
          "OutputItemGtin": "FRTORG1",
          "MetricName": "kilogram",
          "MetricUnit": "kg",
          "MetricValue": 25
        },
        {
          "OutputItemName": "Harvested Tree",
          "OutputItemGtin": "TRHRVSTD",
          "MetricName": "kilogram",
          "MetricUnit": "kg",
          "MetricValue": 100
        }
      ],
      "OperationUid": "HRVSTORG",
      "OperationName": "Harvesting",
      "OperationDescription": "Harvesting Oranges for Transportation"
    },
    {
      "EventDescription": "Offloading",
      "InputItemInfo": [
        {
          "InputItemGtin": "Orange1",
          "MetricValue": 25
        }
      ],
      "StartDateTime": "2020-12-03 00:00:00",
      "EndDateTime": "2022-02-04 00:00:00",
      "WorkstationUid": "FRMTRK1",
      "PersonUid": "JNDOE",
      "OutputItemInfo": [
        {
          "OutputItemName": "Offloaded Orange",
          "OutputItemGtin": "OFFLOADED",
          "MetricName": "kilogram",
          "MetricUnit": "kg",
          "MetricValue": 25
        }
      ],
      "OperationUid": "TRSPRTORNGE",
      "OperationName": "Transportation",
      "OperationDescription": "Transporting Oranges"
    }
  ]
}
```

### Creating an Operation Example

In order to begin using Virtual Routez, an operation must first be created. This is done by adding the OperationName and
OperationDesription to the OperationUid. Using the same example as above, we can see that the operation in this event
has not yet been created.

```json
{
  "api_key": "123kjhnmk12345668422",
  "new_person_json_encoded_object": [
    {
      "EventDescription": "Harvesting Apples",
      "InputItemInfo": [
        {
          "InputItemGtin": "FRTREE",
          "InputItemName": "Fruit Tree",
          "MetricName": "kilogram",
          "MetricUnit": "kg",
          "MetricValue": "20"
        }
      ],
      "StartDateTime": "2020-05-20 20:20:20",
      "EndDateTime": "2020-06-20 20:20:20",
      "WorkstationUid": "FARM1",
      "PersonUid": "FARMER1",
      "OutputItemInfo": [
        {
          "OutputItemName": "APPLE",
          "OutputItemGtin": "APP1",
          "MetricName": "kilogram",
          "MetricUnit": "kg",
          "MetricValue": 20
        }
      ],
      "OperationUid": "HARVEST1",
      "OperationName": "Harvesting",
      "OperationDescription": "Harvesting Apples from Trees"
    }
  ]
}
```

### Operation Already Exists

If an operation has already been added to Virtual Routez via an API call, we only need to use the OperationUid. As an
example, in the below payload, we can see that the operation does already exist, as only the OperationUid is being used.

```json
{
  "api_key": "123kjhnmk12345668422",
  "new_person_json_encoded_object": [
    {
      "EventDescription": "Harvesting Apples",
      "InputItemInfo": [
        {
          "InputItemGtin": "FRTTREE",
          "InputItemName": "Fruit Tree",
          "MetricName": "kilogram",
          "MetricUnit": "kg",
          "MetricValue": "20"
        }
      ],
      "StartDateTime": "2020-05-20 20:20:20",
      "EndDateTime": "2020-06-20 20:20:20",
      "WorkstationUid": "FRM2",
      "PersonUid": "FRMR2",
      "OutputItemInfo": [
        {
          "OutputItemName": "APPLE",
          "OutputItemGtin": "APP1",
          "MetricName": "kilogram",
          "MetricUnit": "kg",
          "MetricValue": 3
        }
      ],
      "OperationUid": "HRVST1"
    }
  ]
}
```

### Adding an Input Item and Output Item Reference:

If we would like, we are able to add descriptions to the items we create. We can do this for both input and output
items, by adding the InputItemReference or OutputItemReference to the payload.

**Below is a easy to read example:**

:::note Example

**Operation:** Making Juice

**Event 1:** Juicing Apples

**Input:** "APP"

**Input Reference:**"APPRM1

**Output 1:** Apple Juice

**Output Reference:**"APPJCRM1"

:::

**Payload to Achieve This**

```json
{
  "api_key": "123kjhnmk12345668422",
  "new_person_json_encoded_object": [
    {
      "EventDescription": "Juicing Apples",
      "InputItemInfo": [
        {
          "InputItemName": "Apple",
          "InputItemGtin": "APPL01",
          "InputItemReference": "APRM1"
        }
      ],
      "StartDateTime": "2020-05-20 20:20:20",
      "EndDateTime": "2020-06-20 20:20:20",
      "WorkstationUid": "FRM1",
      "PersonUid": "FRMR1",
      "OutputItemInfo": [
        {
          "OutputItemName": "Apple Juice",
          "OutputItemGtin": "APPJCE",
          "OutputItemReference": "APPJCRM1",
          "MetricName": "litres",
          "MetricUnit": "l",
          "MetricValue": 100
        }
      ],
      "OperationUid": "MKJC",
      "OperationName": "Making Apple Juice",
      "OperationDescription": "Turning Apples into Juice"
    }
  ]
}
```

## IMPORTANT NOTES

- If the operation referenced in the payload does exist, the existing operation will be used, and a new operation will
  not be created.
- Date and time format: YYYY-MM-DD HH:MM:SS (2021-04-26 08:10:45)
- If the input item referenced in the payload does exist, the existing item will be used, and a new item will not be
  created.
- Compulsory parameters when creating a new input item are item name, item Gtin and metric value, metric name and metric
  unit.
- The output item will be created on every payload, unless the metric value AND gtin of an item exists.
- If you try to submit the exact same payload within a minute of the last submission, you will get an error: "Exact same
  payload requested within a minute ago. Request terminated.



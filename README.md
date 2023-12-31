<p align="center">
<img src="http://nano.sahaj.ai/27ad2091b1714f583886.png" width="320" height="162" alt="Logo" title="NaN(O) logo">
</p>

Link to original repo: https://github.com/sahaj-nano/nano-digital-marketplace-kavisha4-node
Leaderboard
![image](https://github.com/Kavisha4/Sahaj_Hackathon_APIs/assets/76434647/e0d7dd63-eb08-4587-acb5-4c592379c895)

Workflow completion
![image](https://github.com/Kavisha4/Sahaj_Hackathon_APIs/assets/76434647/8a1bfb4f-cedf-4fc1-84dd-f38269c7b8d9)

# Digital Marketplace - a NaN(O) problem

------

## Problem trying to be addressed

Writing an application for the digital marketplace for women entrepreneurs where they can list their businesses to get a wider audience. Work on the backend side of the platform. Given the short time to market-Need to implement the below 4 functionalities.

1. Create a Business Listing
2. Search for a specific Business Listing based on ID
3. Search Business Listings based on filters
4. Get all listed businesses.
5. Aggregate Business Listings based on given fields for grouping and function to be applied on the group for aggregation. Supported functions are as follows
   1. `COUNT`
   2. `MIN`
   3. `MAX`

Backend functionality is to be written such that requests will be accepted over HTTP and call methods define on store.js file. Goal was to implement these methods as mentioned in the following expectations (Method contract). No databases/libraries can be used to store/maintain data.


------

### Technical details
1. Repository needs to have a `Dockerfile` that starts your HTTP web app 
2.  HTTP app need to expose APIs ([API contract](#api-contract)) on port 8080 
3. No existing databases, libraries and services can be used to store the data
4. Application needs to persist data across restarts
5. Maximum time a single request can take is 5 seconds
6. Data should be persisted in `./digital-marketplace`

------ 

### Score Distribution
1. Implementation of create, search, read all and read by id of business listings - 101
2. Persisting data across application restarts - 200
3. Throughput of the application - 460
4. Implementation of aggregation functionality - 240
-------

## Data to be stored
```
{
    businessListingId: string
    businessName: string,
    ownerName: string,
    category: string,
    city: string,
    establishmentYear: int
}
```

---
## Method contract

>##### create(businessListingRequest)
Creates a new entry in store and returns all the details with businessListingId. For any Business listing, business name will be unique.

* Params
  * businessListingRequest of following type
    ```
    {
       businessName: string,
       ownerName: string,
       category: string,
       city: string,
       establishmentYear: int
    }
    ```
* Returns
  * businessListing of following type
    ```
    {
        businessListingId: string
        businessName: string,
        ownerName: string,
        category: string,
        city: string,
        establishmentYear: int
    }
    ```
  * null if business name already exists  
---

>##### read(id)
Returns the specified business listing.

* Params
  * id of type string
* Returns
  * businessListing of following type if found
    ```
    {
        businessListingId: string
        businessName: string,
        ownerName: string,
        category: string,
        city: string,
        establishmentYear: int
    }
    ```
   * null if listing with given id not found 

---

##### search(searchRequest)
Search business listings with different criteria.

* Params:
  * searchCriteria – SearchCriteria object with following details
    * condition - Value will be either `AND`/`OR`
    * fields - collection of filter criterion
      * fieldName - Value will be one of `ownerName`/`category`/`city` etc
      * eq - optional value hold to check for equality the value stored in the requested `fieldName`
      * neq - ptional value hold to check for non equality the value stored in the requested `fieldName`
      * Either of `eq` or `neq` needs to be supplied - never both.
    * Example:
      ```json
      {
        condition: OR/AND,
        fields: [
            {
                fieldName: string,
                eq: string/number
            },
            {
                fieldName: string,
                neq: string/number
            }
        ]
      }
      ```
* Returns:
  * Collection of BusinessListing matching for given search criteria which is of following type
    ```
    [{
        businessListingId: string
        businessName: string,
        ownerName: string,
        category: string,
        city: string,
        establishmentYear: int
    }.......]
    ```
  * Empty collection if nothing matches


##### readAll()
Return the list of all business listings.

* Params - None
* Returns
  * Collection of businessListing of following type
    ```
    [{
        businessListingId: string
        businessName: string,
        ownerName: string,
        category: string,
        city: string,
        establishmentYear: int
    }.......]
    ```

##### aggregate(aggregateCriteria)
Returns collection of aggregated object for given aggregate criteria.

* Params:
  * aggregateCriteria – AggregateCriteria object with following details
      * groupByFields - List of field names to group by
      * aggregationRequests - collection of aggregation request
          * fieldName - Value will be one of `ownerName`/`category`/`city` etc
          * function - Value will be one of `COUNT`/`MIN`/`MAX`
          * alias - Value will be string and it to be used as key for aggregated value in return object
      * Example:
        ```json
        {
          "groupByFields": ["city","category"],
          "aggregationRequests": [
              {
                  "fieldName": "establishmentYear",
                  "function": "MAX",
                  "alias": "lastEstablishmentYear"
              },
              {
                  "fieldName": "establishmentYear",
                  "function": "MIN",
                  "alias": "firstEstablishmentYear"
              },
              {
                  "fieldName": "businessName",
                  "function": "COUNT",
                  "alias": "totalListedBusinesses"
              }
          ]
        }
        ```
* Returns:
  * Collection of following type
    ```
    [
      {
          "city": "Mumbai",
          "category": "Home Decor",
          "lastEstablishmentYear": 2023,
          "firstEstablishmentYear": 2010,
          "totalListedBusinesses": 4
      },
      {
          "city": "Mumbai",
          "category": "Home backer",
          "lastEstablishmentYear": 2023,
          "firstEstablishmentYear": 2010,
          "totalListedBusinesses": 2
      }
    ]
    ```
  * Empty collection if nothing matches
----
[API Sample Request Response](api-sample.md)
---
## Instructions for coding/committing
* git clone `git repository url` (Skip this step if using github codespaces)
* cd `repository name` (Skip this step if using github codespaces)
* Check for health check [server.js](server.js)
* Goto [store.js](store.js)
* Begin coding
* To test your code locally run following command
  * npm install
  * npm test
* Code needs to be tested via github Execute following commands in the terminal location of your repository
  * git add .
  * git commit -m 'any message regarding your changes'
  * git push
* Wait for build to complete in github actions logs.
* If build is green, you should see the score on the leader board, else check with actions logs.

## Sample Instructions for Codespaces installation
* On your browser after accepting the github invitation,
  * Select "Code" dropdown
  * Select the "Codespaces" tab.
  * Select "Create codespace on main"
* Continue from step 3 of Sample Instructions for coding/commit

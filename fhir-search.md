# Import FHIR search results to pandas dataframe

Import resources from [FHIR Search API](https://www.hl7.org/fhir/search.html) results to [pandas](https://pandas.pydata.org/docs/user_guide/index.html)  dataframe by [fhiry](README.md):

## FHIR search parameters

For filter options you can set by `search_parameters` see [FHIR search common parameters for all resource types](https://www.hl7.org/fhir/search.html#standard) and additional FHIR search parameters for certain resource types like [Patient](https://www.hl7.org/fhir/patient.html#search), [Condition](https://www.hl7.org/fhir/condition.html#search), [Observation](https://www.hl7.org/fhir/observation.html#search), ...

## Example: Import all Observations

Import all resources (since empty search parameters / no filter) of type Observation
```
from fhiry.fhirsearch import Fhirsearch

fs = Fhirsearch()
fs.fhir_base_url = "http://fhir-server:8080/fhir"
df = fs.search(type = "Observation", search_parameters = {})

print(df.info())
```

## Example: Import all conditions with a certain code

Import all condition resources with Snomed (Codesystem `http://snomed.info/sct`) Code `39065001` in the FHIR element Condition.code:

```
from fhiry.fhirsearch import Fhirsearch
fs = Fhirsearch()

fs.fhir_base_url = "http://fhir-server:8080/fhir"

my_fhir_search_parameters = {
    "code": "http://snomed.info/sct|39065001",
}

df = fs.search(type = "Condition", search_parameters = my_fhir_search_parameters)

print(df.info())
```

## Columns
* see [`df.columns`](README.md#columns)


## Decrease RAM usage

If you want to analyze only certain elements, you can decrease RAM usage and network overhead by defining the elements you need for your data analysis by the [FHIR search option `_elements`](https://www.hl7.org/fhir/search.html#elements).

Example:

```
from fhiry.fhirsearch import Fhirsearch
fs = Fhirsearch()

fs.fhir_base_url = "http://fhir-server:8080/fhir"

my_fhir_search_parameters = {
```
... Other FHIR search parameters / filters ...

```

    "_elements": "code,verification-status,recorded-date",
}

df = fs.search(type = "Condition", search_parameters = my_fhir_search_parameters)

print(df.info())
```




## Connection settings

To set connection parameters like authentication, SSL certificates, proxies and so on, set or add standard [Python requests](https://requests.readthedocs.io/en/latest/) keyword arguments to the property `requests_kwargs`.

### Authentication

Authentication is set by [requests parameter `auth`](https://requests.readthedocs.io/en/latest/user/authentication/).

Example using [HTTP Basic Auth](https://requests.readthedocs.io/en/latest/user/authentication/#basic-authentication):

```
from fhiry.fhirsearch import Fhirsearch

fs = Fhirsearch()
fs.fhir_base_url = "http://fhir-server:8080/fhir"

# Set basic auth credentials (https://requests.readthedocs.io/en/latest/user/authentication/#basic-authentication)
fs.requests_kwargs["auth"] = ('myUser', 'myPassword')
```

### Proxy settings

You can set a HTTP(S)-Proxies by [requests parameter `proxies`](https://requests.readthedocs.io/en/latest/user/advanced/#proxies).

Example:

```
fs.requests_kwargs["proxies"] = {
    'http': 'http://10.10.1.10:3128',
    'https': 'http://10.10.1.10:1080',
}
```
# ZenVar JSON
## Overview
This Mongoose OS library allows you to serialize/deserialize [ZenVar variables](https://github.com/zendiy-mgos/zvar) and [dictionaries](https://github.com/zendiy-mgos/zvar-dic) to/from JSON.
## Features
- **Deserialize from JSON** - Deserialize a JSON string into a dynamically allocated variant instance.
- **Serialize to JSON** - Serialize a variant instance into a JSON string.
## Get Started
Include the library into your `mos.yml` file.
```yaml
libs:
  - origin: https://github.com/zendiy-mgos/zvar-json
  # Optional. Include zvar-dic library to enable JSON support for dictionaries
  - origin: https://github.com/zendiy-mgos/zvar-dic
```
**C/C++ sample code**

Deserialize JSON strings into variant instances.
```c
#include "mgos_zvar_json.h"

mgos_zvar_t i = mgos_zvar_json_scanf("234");      // JSON to integer
mgos_zvar_t d = mgos_zvar_json_scanf("378.340");  // JSON to decimal
mgos_zvar_t b = mgos_zvar_json_scanf("true");     // JSON to boolean
mgos_zvar_t s = mgos_zvar_json_scanf("\"AAA\"");  // JSON to string
```
Deserialize a JSON object into a dictionary (this requires the [ZenVar Dictionary library](https://github.com/zendiy-mgos/zvar-dic) to be included in your `mos.yml` file). 
```c
#include "mgos_zvar_dic.h"
#include "mgos_zvar_json.h"

/* {
      "Name": "Mark",
      "Fhater": {
        "Name": "Gregory",
        "Age": 80
      },
      "Age": 46
    }
*/
const char *json = "{\"Name\":\"Mark\",\"Fhater\":{\"Name\":\"Gregory\",\"Age\":80},\"Age\":46}";
mgos_zvar_t dic = mgos_zvar_json_scanf(json);
```
Serialize a variant instance to JSON.
```c
#include "mgos_zvar_json.h"

mgos_zvar_t var = mgos_zvar_new_decimal(122.20);
char *json = json_asprintf("%M", json_printf_zvar, var);
printf("JSON: %s", json); // JSON: 122.200000
free(json);
```
Serialize a dictionary to JSON (this requires the [ZenVar Dictionary library](https://github.com/zendiy-mgos/zvar-dic) to be included in your `mos.yml` file). 
```c
#include "mgos_zvar_dic.h"
#include "mgos_zvar_json.h"

mgos_zvar_t dic = mgos_zvar_new();
mgos_zvar_add_key(dic, "Name", mgos_zvar_new_str("Mark"));
mgos_zvar_add_key(dic, "Weigth", mgos_zvar_new_decimal(102.44));
mgos_zvar_add_key(dic, "Enable", mgos_zvar_new_bool(true));
char *json = json_asprintf("%M", json_printf_zvar, dic);
printf("JSON: %s", json); // JSON: {"Name":"Mark","Weigth":102.440000,"Enable":true}
free(json);
```
## C/C++ API Reference
### mgos_zvar_json_scanf()
```c
mgos_zvar_t mgos_zvar_json_scanf(const char *json);
```
Returns the variant instance deserialized from the provided JSON string, or `NULL` if error. The returned instance must be deallocated using `mgos_zvar_free` (more details [here](https://github.com/zendiy-mgos/zvar#mgos_zvar_free)).

|Parameter||
|--|--|
|json|The string in JSON format to deserialize.|
### json_printf_zvar()
```c
int json_printf_zvar(struct json_out *out, va_list *ap);
```
A helper `%M` callback for [Frozen printing APIs](https://github.com/cesanta/frozen) that prints the provided variant instance. Consumes `mgos_zvar_t var`. Returns number of bytes printed.
```c
// Example
mgos_zvar_t var = mgos_zvar_new_str("Mark");
char *json = json_asprintf("%M", json_printf_zvar, var);
```
## To Do
- Implement serialization/deserialization of variant arrays.
- Implement javascript APIs for [Mongoose OS MJS](https://github.com/mongoose-os-libs/mjs).




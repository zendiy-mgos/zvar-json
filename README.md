# ZenVar
## Overview
This Mongoose OS library adds JSON support to[ZenVar variables](https://github.com/zendiy-mgos/zvar).   
* Boolean (`bool`)
* Integer (`long`)
* Decimal (`double`)
* String (`char *`)
* Dictionary (key/value pair dictionary) - It requires msog_zvar_dic libray.

```c
// `NULL` variable initialization (like void* var = `NULL`)
mgos_zvar_t var = mgos_zvar_new();

// Integer variable initialization (like long var = 101;)
mgos_zvar_t var = mgos_zvar_new_integer(101);

// Boolean variable initialization (like bool var = true;)
mgos_zvar_t var = mgos_zvar_new_bool(true);

// Decimal variable initialization (like double var = 101.99;)
mgos_zvar_t var = mgos_zvar_new_decimal(101.99);

// String variable initialization (like char *var = "Lorem Ipsum";)
mgos_zvar_t var = mgos_zvar_new_str("Lorem Ipsum");
```
## C/C++ API Reference

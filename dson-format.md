# DSON format specification

## Introduction

DSON is a custom binary format used in the Radix infrastructure for the serialization of structured data.

The following document describes the DSON format, the supported basic types and the protocol requirements to serialize data properly.

{% hint style="info" %}
This document specifies version 1.0 of the DSON standard.
{% endhint %}

## Definitions

### DSON types

DSON can represent five primitive types \(numbers, strings, bytes, EUIDs and hashes\) and two structured types \(objects and arrays\).

#### Primitive types

* A **string** is a sequence of zero or more UTF-8 characters.
* A binary **byte buffer** is a sequence of zero or more bytes.
* A **number** is an 8 bytes \(64 bit\) integer, in big-endian format.
* An **EUID** is a variable byte length integer, in big-endian format.
* A **hash** is a 32 bytes binary value.

#### Structured types

* An **object** is an unordered collection of zero or more name/value pairs, where a name is a string and a value is a string, number, bytes, EUID, hash, object, or array.
* An **array** is an ordered sequence of zero or more values, where a value is any of the basic types.

{% hint style="warning" %}
Note: **number** and **EUID** basic types must be serialized in big-endian format.
{% endhint %}

### Type code Table

| DSON type | Type code |
| :--- | :--- |
| NUMBER | 0x02 |
| STRING | 0x03 |
| BYTES | 0x04 |
| OBJECT | 0x05 |
| ARRAY | 0x06 |
| EUID | 0x07 |
| HASH | 0x08 |

## Format encoding

The process to serialize an element using DSON is simple and straight forward. Assuming you have an element **E** of any of the accepted types, you simply have to encode it as:

$$
DSON(E) = Type(E) + Length(E) + Data(E)
$$

* **Type** is the element's serialized type, as defined in the [Type code table](dson-format.md#type-code-table).
* **Length** is the element's serialized data length, as a 32-bit integer in big-endian format.
* **Data** is the element's serialized data. For specific details on each type's data encoding, check the next section.

To summarize, the DSON encoding adds a 5 bytes header before the element's serialized data:

#### Protocol structure

| Type code | + | Data length | + | Data |
| :--- | :--- | :--- | :--- | :--- |
| 1 byte | + | 4 bytes | + | variable |

{% hint style="warning" %}
The **data length** field is a 4 bytes \(32 bit\) integer in big-endian format.
{% endhint %}

You can encode as many elements as needed, by concatenating the results in your output stream:

$$
Output = DSON(E_1) + ... + DSON(E_n)
$$

Next, we will review each DSON type in detail and show how to serialize the data correctly.   
Examples for each type also include a binary dump of the output stream \(in Hex notation\) for further clarification.

### Number

Assume we have a number **N = 4096**. The type length is 8 bytes.

To serialize N, we write to our output stream the type code `(0x02)`, then the data length `(0x00000008)`, and finally we write N as a big-endian integer `(0x0000000000001000)`.

{% code-tabs %}
{% code-tabs-item title="Number example" %}
```text
0000-1060:  ## ## ## ##-## ## 02 00-00 00 08 00-00 00 00 00  ######.. ........
0000-1070:  00 10 00                                         ... 
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### String

Assume we have a string **S = "Radix Devnet"**. The string length is 12 bytes.

To serialize S, we write to our output stream the type code `(0x03)`, then the string length `(0x0000000c)`, and finally we write the UTF-8 encoded string S.

{% code-tabs %}
{% code-tabs-item title="String example" %}
```text
0000-2920:  ## ## ## ##-## ## ## 03-00 00 00 0c-52 61 64 69  #######. ....Radi
0000-2930:  78 20 44 65-76 6e 65 74                          x.Devnet 
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Bytes

Assume we have a byte buffer **B = \[0x03, 0xCA, 0xA2, 0x8A, 0xCC, 0x9B, 0xCD, 0x85, 0x86, 0x9D, 0x38, 0x4D, 0xD9, 0xB7, 0x89, 0x7D, 0x24, 0x0C, 0x1C, 0x01, 0xD1, 0x2B, 0x45, 0x84, 0x54, 0x07, 0x95, 0xE7, 0x8F, 0xEB, 0x4E, 0x51, 0xA9\]**. The byte array length is 33 bytes.

To serialize B, we write to our output stream the type code `(0x04)`, then the byte array length `(0x00000021)`, and finally we write the byte array B.

{% code-tabs %}
{% code-tabs-item title="Byte array example" %}
```text
0000-2790:  ## ## ## ##-## ## 04 00-00 00 21 03-ca a2 8a cc  ######.. ..!.....
0000-27a0:  9b cd 85 86-9d 38 4d d9-b7 89 7d 24-0c 1c 01 d1  .....8M. ..}$....
0000-27b0:  2b 45 84 54-07 95 e7 8f-eb 4e 51 a9              +E.T.... .NQ. 
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Object

Assume we have two numbers, A = 1488326400000 and B = 9223372036854775807, and we define an object **O = {"default": A, "expires": B}**.

To serialize O, first we serialize each of the name/value pairs, and compute the total length: in this case the object length is 42 bytes. Next, we write to our output stream the type code `(0x05)`, the object length `(0x0000002a)` and we write each of the name/value pairs we previously encoded.

To write a name/value pair, we have to write the name string length, the name, and finally the serialized value for it. In our example, for {"default": A} we write "default" length `(0x07)`, then the string, and finally the serialized value of A \(number type, length, and value\).

{% code-tabs %}
{% code-tabs-item title="Object example" %}
```text
0000-1140:  ## ## ## ##-05 00 00 00-2a 07 64 65-66 61 75 6c  ####.... *.defaul
0000-1150:  74 02 00 00-00 08 00 00-01 5a 87 2a-98 00 07 65  t....... .Z.*...e
0000-1160:  78 70 69 72-65 73 02 00-00 00 08 7f-ff ff ff ff  xpires.. ........
0000-1170:  ff ff ff                                         ... 
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Array

Assume we have an EUID E=31655847435213307464496696616, and we define an array **A = \[ E \]**. 

To serialize A, first we serialize each array element and compute the total length: in this case the array length is 17 bytes. Next, we write to our output stream the type code `(0x06)`, the array length `(0x00000011)`, and finally we write the element E that we previously encoded as an EUID basic type.

{% code-tabs %}
{% code-tabs-item title="Array example" %}
```text
0000-2310:  ## ## ## ##-## ## 06 00-00 00 11 07-00 00 00 0c  ######.. ........
0000-2320:  66 49 1a 70-0e 70 12 52-4a 8f cd 28              fI.p.p.R J..( 
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### EUID

Assume we have an EUID **E = 79416**. The EUID length is 3 bytes.

To serialize E, we write to our output stream the type code `(0x07)`, then the EUID length `(0x00000003)`, and finally we write E as a big-endian integer.

{% code-tabs %}
{% code-tabs-item title="EUID example" %}
```text
0000-0f90:  ## ## ## 07-00 00 00 03-01 36 38                 ###..... .68 
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Hash

Assume we have a hash **H = "0000000000000000000000000000000000000000000000000000000000000000"**. The hash length is 32 bytes.

To serialize H, we write to our output stream the type code `(0x08)`, then the hash length `(0x00000020)`, and finally we write the hash data.

{% code-tabs %}
{% code-tabs-item title="Hash example" %}
```text
0000-2750:  ## ## ## ##-## ## ## ##-## ## ## ##-## ## ## 08  ######## #######.
0000-2760:  00 00 00 20-00 00 00 00-00 00 00 00-00 00 00 00  ........ ........
0000-2770:  00 00 00 00-00 00 00 00-00 00 00 00-00 00 00 00  ........ ........
0000-2780:  00 00 00 00                                      .... 
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Examples

In this section we present a complete DSON output stream example.   
You can download an example DSON [binary file](dson-format.md#serialized-output), with the matching [deserialized data](https://pastebin.com/dl/2vL3eXGH).

### Serialized output

The format encoding examples shown above have been taken from the following DSON binary file:

{% file src=".gitbook/assets/example.dson" caption="DSON serialized binary example" %}

{% hint style="info" %}
Tip: you can use this DSON binary file for Unit testing in your development environment.
{% endhint %}

### Deserialized data

The following code is a deserialized example of the [DSON binary file](dson-format.md#serialized-output) using JavaScript object notation:

```javascript
{
  "creator": {
    "serializer": "BASE64",
    "value": "A8qiisybzYWGnThN2beJfSQMHAHRK0WEVAeV54/rTlGp"
  },
  "description": "The Radix development Universe",
  "genesis": [
    {
      "action": "STORE",
      "classification": "commodity",
      "description": "Radix POW",
      "destinations": [
        {
          "serializer": "EUID",
          "value": "31655847435213307464496696616"
        }
      ],
      "icon": {
        "serializer": "BASE64",
        "value": "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"
      },
      "id": {
        "serializer": "EUID",
        "value": "79416"
      },
      "iso": "POW",
      "label": "Proof of Work",
      "maximum_units": 0,
      "owners": [
        {
          "public": {
            "serializer": "BASE64",
            "value": "A8qiisybzYWGnThN2beJfSQMHAHRK0WEVAeV54/rTlGp"
          },
          "serializer": 547221307,
          "version": 100
        }
      ],
      "serializer": 62583504,
      "settings": 4096,
      "signatures": {
        "31655847435213307464496696616": {
          "r": {
            "serializer": "BASE64",
            "value": "e1EntoLaWUuLOC4byiToq5Zqre9CXziQvaasFj/GLTI="
          },
          "s": {
            "serializer": "BASE64",
            "value": "AIciHYosRFxeSEV6AYKUk0oC2Ii/FT4hFosiM5Ka3hkO"
          },
          "serializer": -434788200,
          "version": 100
        }
      },
      "sub_units": 0,
      "timestamps": {
        "default": 1488326400000,
        "expires": 9223372036854775807
      },
      "type": "COMMODITY",
      "version": 100
    },
    {
      "action": "STORE",
      "classification": "currencies",
      "description": "Radix currency asset",
      "destinations": [
        {
          "serializer": "EUID",
          "value": "31655847435213307464496696616"
        }
      ],
      "icon": {
        "serializer": "BASE64",
        "value": "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"
      },
      "id": {
        "serializer": "EUID",
        "value": "80998"
      },
      "iso": "RDX",
      "label": "RADIX",
      "maximum_units": 0,
      "owners": [
        {
          "public": {
            "serializer": "BASE64",
            "value": "A8qiisybzYWGnThN2beJfSQMHAHRK0WEVAeV54/rTlGp"
          },
          "serializer": 547221307,
          "version": 100
        }
      ],
      "scrypt": {
        "serializer": 549088296,
        "version": 100
      },
      "serializer": 62583504,
      "settings": 20483,
      "signatures": {
        "31655847435213307464496696616": {
          "r": {
            "serializer": "BASE64",
            "value": "cqs/kPE1bPyWUsSYfIa4wE00BOUfywCoGq5NR6xL3Ms="
          },
          "s": {
            "serializer": "BASE64",
            "value": "ANr7KQ/RObMLPbhX79bPweV9RS2AhBZkiY1eSSSZG3yq"
          },
          "serializer": -434788200,
          "version": 100
        }
      },
      "sub_units": 100000,
      "timestamps": {
        "default": 1488326400000,
        "expires": 9223372036854775807
      },
      "type": "CURRENCY",
      "version": 100
    },
    {
      "action": "STORE",
      "destinations": [
        {
          "serializer": "EUID",
          "value": "31655847435213307464496696616"
        }
      ],
      "encrypted": {
        "serializer": "BASE64",
        "value": "BQAAAa4HbWVzc2FnZQMAAAAVUmFkaXguLi4uSnVzdCBJbWFnaW5lDHBhcnRpY2lwYW50cwYAAAFNBQAAAIoHYWRkcmVzcwUAAABAB2FkZHJlc3MDAAAABlNZU1RFTQpzZXJpYWxpemVyAgAAAAj/////5mMn1Ad2ZXJzaW9uAgAAAAgAAAAAAAAAZApzZXJpYWxpemVyAgAAAAj/////rhGDEwR0eXBlAwAAAAZTRU5ERVIHdmVyc2lvbgIAAAAIAAAAAAAAAGQFAAAAuQdhZGRyZXNzBQAAAG0HYWRkcmVzcwMAAAAzMThOVVpydzcyM1pROFVzMW14Vk4zMkM1UEcyckR0NEhLTVNhdUxreUtyOFdwSFZHSGl5CnNlcmlhbGl6ZXICAAAACP/////mYyfUB3ZlcnNpb24CAAAACAAAAAAAAABkCnNlcmlhbGl6ZXICAAAACP////+uEYMTBHR5cGUDAAAACFJFQ0VJVkVSB3ZlcnNpb24CAAAACAAAAAAAAABkCnNlcmlhbGl6ZXICAAAACAAAAAAe/f+nB3ZlcnNpb24CAAAACAAAAAAAAABk"
      },
      "operation": "TRANSFER",
      "particles": [
        {
          "asset_id": {
            "serializer": "EUID",
            "value": "80998"
          },
          "destinations": [
            {
              "serializer": "EUID",
              "value": "31655847435213307464496696616"
            }
          ],
          "nonce": 64745110209564,
          "owners": [
            {
              "public": {
                "serializer": "BASE64",
                "value": "A8qiisybzYWGnThN2beJfSQMHAHRK0WEVAeV54/rTlGp"
              },
              "serializer": 547221307,
              "version": 100
            }
          ],
          "quantity": 100000000000,
          "serializer": 1782261127,
          "version": 100
        }
      ],
      "serializer": -760130,
      "signatures": {
        "31655847435213307464496696616": {
          "r": {
            "serializer": "BASE64",
            "value": "DM2NdvlsJBSM8sFQT7O8TIyvRGQX6H/pZOiTh1wIFDM="
          },
          "s": {
            "serializer": "BASE64",
            "value": "Q+JLYVooQ7T+iamn9aqt6eWmcvdBF7HNG+DpE0QIrls="
          },
          "serializer": -434788200,
          "version": 100
        }
      },
      "temporal_proof": {
        "atom_id": {
          "serializer": "EUID",
          "value": "-13067581529069931334871850573"
        },
        "serializer": 1905172290,
        "version": 100,
        "vertices": [
          {
            "clock": 0,
            "commitment": {
              "serializer": "HASH",
              "value": "0000000000000000000000000000000000000000000000000000000000000000"
            },
            "owner": {
              "public": {
                "serializer": "BASE64",
                "value": "A8qiisybzYWGnThN2beJfSQMHAHRK0WEVAeV54/rTlGp"
              },
              "serializer": 547221307,
              "version": 100
            },
            "previous": {
              "serializer": "EUID",
              "value": "0"
            },
            "serializer": -909337786,
            "signature": {
              "r": {
                "serializer": "BASE64",
                "value": "IOVVcu/4lqzPF3h3/kiOjlwP8DH/MMFEhX1PzYY4YIg="
              },
              "s": {
                "serializer": "BASE64",
                "value": "AMRGULP3k7i5523eKI9gmuvElIzhqhbBRDiEBrbOLehr"
              },
              "serializer": -434788200,
              "version": 100
            },
            "timestamps": {
              "default": 1523911346956
            },
            "version": 100
          }
        ]
      },
      "timestamps": {
        "default": 1488326400000
      },
      "version": 100
    }
  ],
  "magic": -1014759422,
  "name": "Radix Devnet",
  "port": 30000,
  "serializer": 492321349,
  "signature.r": {
    "serializer": "BASE64",
    "value": "APUvgyKJKziME+pRTKSXpnHSlN9xJbSGL5EZ04bILny5"
  },
  "signature.s": {
    "serializer": "BASE64",
    "value": "SUCrNHN2qHGw9IW1bCxREHxF+LuhVG4+FtVDmFCnmvs="
  },
  "timestamp": 1488326400000,
  "type": 2,
  "version": 100
}
```




## IVMS101 standard

CODE uses the IVMS101 standard to exchange personal information related to virtual asset transaction. [https://intervasp.org/](https://intervasp.org/)
- A field name of a message is expressed with camelCase whose first character starts with a lowercase. But, 'Originator', 'Beneficiary', 'OriginatorVASP', and 'BeneficiaryVASP' objects corresponding to Entity in ivms101 are expressed with PascalCase.
- The values of all fields are not case-sensitive unless otherwise specified.
- The values of all fields are always expressed with a UTF-8 encoded string. (including boolean, integer, real number, etc.)
- In principle, the values of all fields shall be written in English except when Local Language is permitted.
- You may find more information about the IVMS101 rule for CODE protocol [here](https://codevasp.gitbook.io/code-api-doc-en/api-reference/ivms101).
- Please refer to complete natual person example json in complete-example.json file.
- Please refer to complete legal person example json in complete-example-legal-person.json file.
- Complete json schema is provided in json-schema.json file.
- You may use [https://www.jsonschemavalidator.net/](https://www.jsonschemavalidator.net/) to validate your json format.
  - Select schema: IVMS101 by CODE Protocol

### Initial IVMS101 from an originator VASP
As an originator VASP, you need to send following to beneficiary VSAP. You should know the entityId of beneficiary from CODE, however, you still do not know their VASP information, thus, only send following objects.
```
{
  "Originator": {...},
  "Beneficiary": {...},
  "OriginatingVASP": {...}
}
```

### Response IVMS101 from a beneficiary VASP
When beneficiary VASP response to originator, it should complete the IVMS101 format as following.
```
{
  "Originator": {...},
  "Beneficiary": {...},
  "OriginatingVASP": {...},
  "BeneficiaryVASP": {...}
}
```
You may also include more Beneficiary information in Beneficiary Object such as `customerIdentification`.

### Example of an originating natual person
```
"Originator": {
   "originatorPersons":[
      {
         "naturalPerson":{
            "name":{
               "nameIdentifier":[
                  {
                     "primaryIdentifier":"Barnes",
                     "secondaryIdentifier":"Robert",
                     "nameIdentifierType":"LEGL"
                  }
               ],
               "localNameIdentifier":[
                  {
                     "primaryIdentifier":"로버트 반스",
                     "secondaryIdentifier":"",
                     "nameIdentifierType":"LEGL"
                  }
               ]
            },
            "dateAndPlaceOfBirth":{
               "dateOfBirth":"1990-01-01",
               "placeOfBirth":"LA"
            },
            "customerIdentification":"customernumber in Max 50 Text",
            "countryOfResidence":"US"
         }
      }
   ],
   "accountNumber":[
      "rJChk8e71gxVhyJSr1srzZxWhVisWMMYKZ:tag or memo"
   ]
}
```

### Example of an originating legal person
```
"Originator": {
   "originatorPersons":[
      {
         "legalPerson":{
            "name":{
               "nameIdentifier":[
                  {
                     "legalPersonName":"Coinone Inc.",
                     "legalPersonNameIdentifierType":"LEGL"
                  }
               ]
            },
            "nationalIdentification":{
               "nationalIdentifier":"XXXXXXXXXXXXXXXXXXXX",
               "nationalIdentifierType":"LEIX"
            },
            "customerIdentification":"customernumber in Max 50 Text",
            "countryOfRegistration":"KR"
         }
      },
      {
         "naturalPerson":{
            "name":{
               "nameIdentifier":[
                  {
                     "primaryIdentifier":"Barnes",
                     "secondaryIdentifier":"Robert",
                     "nameIdentifierType":"LEGL"
                  }
               ],
               "localNameIdentifier":[
                  {
                     "primaryIdentifier":"로버트 반스",
                     "secondaryIdentifier":"",
                     "nameIdentifierType":"LEGL"
                  }
               ]
            }
         }
      }
   ],
   "accountNumber":[
      "rJChk8e71gxVhyJSr1srzZxWhVisWMMYKZ:tag or memo"
   ]
}
```

### Example of an originating VASP
```
"OriginatingVASP": {
   "originatingVASP":{
      "legalPerson":{
         "name":{
            "nameIdentifier":[
               {
                  "legalPersonName":"Korbit Inc.",
                  "legalPersonNameIdentifierType":"LEGL"
               }
            ]
         },
         "geographicAddress":[
            {
               "addressType":"GEOG",
               "townName":"Seoul",
               "addressLine": ["100 Teheran-ro 1-gil, Gangnam-gu", "10th floor"],
               "country":"KR"
            }
         ],
         "nationalIdentification":{
            "nationalIdentifier":"EXAMPLE-TAX-ID",
            "nationalIdentifierType":"RAID",
            "registrationAuthority":"RA000657"
         },
         "countryOfRegistration":"KR"
      }
   }
}
```

### Example of a beneficiary natual person
```
"Beneficiary": {
   "beneficiaryPersons":[
      {
         "naturalPerson":{
            "name":{
               "nameIdentifier":[
                  {
                     "primaryIdentifier":"Smith",
                     "secondaryIdentifier":"Alice",
                     "nameIdentifierType":"LEGL"
                  }
               ],
               "localNameIdentifier":[
                  {
                     "primaryIdentifier":"앨리스 스미스",
                     "secondaryIdentifier":"",
                     "nameIdentifierType":"LEGL"
                  }
               ]
            },
            "dateAndPlaceOfBirth":{
               "dateOfBirth":"1990-01-01",
               "placeOfBirth":"LA"
            },
            "customerIdentification":"customernumber in Max 50 Text",
            "countryOfResidence":"US"
         }
      }
   ],
   "accountNumber":[
      "rHcFoo6a9qT5NHiVn1THQRhsEGcxtYCV4d:tag or memo"
   ]
}
```

### Example of a beneficiary legal person
```
"Beneficiary": {
   "beneficiaryPersons":[
      {
         "legalPerson":{
            "name":{
               "nameIdentifier":[
                  {
                     "legalPersonName":"Korbit Inc.",
                     "legalPersonNameIdentifierType":"LEGL"
                  }
               ]
            },
            "nationalIdentification":{
               "nationalIdentifier":"XXXXXXXXXXXXXXXXXXXX",
               "nationalIdentifierType":"LEIX"
            },
            "customerIdentification":"customernumber in Max 50 Text",
            "countryOfRegistration":"KR"
         }
      },
      {
         "naturalPerson":{
            "name":{
               "nameIdentifier":[
                  {
                     "primaryIdentifier":"Smith",
                     "secondaryIdentifier":"Alice",
                     "nameIdentifierType":"LEGL"
                  }
               ],
               "localNameIdentifier":[
                  {
                     "primaryIdentifier":"앨리스 스미스",
                     "secondaryIdentifier":"",
                     "nameIdentifierType":"LEGL"
                  }
               ]
            }
         }
      }
   ],
   "accountNumber":[
      "rHcFoo6a9qT5NHiVn1THQRhsEGcxtYCV4d:tag or memo"
   ]
}
```

### Example of a beneficiary VASP 
```
"BeneficiaryVASP": {
   "beneficiaryVASP":{
      "legalPerson":{
         "name":{
            "nameIdentifier":[
               {
                  "legalPersonName":"Coinone Inc.",
                  "legalPersonNameIdentifierType":"LEGL"
               }
            ]
         },
         "geographicAddress":[
            {
               "addressType":"GEOG", 
               "townName":"Seoul",
               "addressLine": ["100 Teheran-ro 1-gil, Gangnam-gu", "10th floor"],
               "country":"KR"
            }
         ],
         "nationalIdentification":{
            "nationalIdentifier":"6948624434",
            "nationalIdentifierType":"RAID",
            "registrationAuthority":"RA000657"
         },
         "countryOfRegistration":"KR"
      }
   }
}
```

### ENUM List
#### NaturalPersonNameTypeCode
|Code|Name| Description                                                                                                                                        |
|------|---|----------------------------------------------------------------------------------------------------------------------------------------------------|
|ALIA|Alias name| A name other than the legal name by which a natural person is also known.                                                                          |
|BIRT|Name at birth| The name given to a natural person at birth.                                                                                                       |
|MAID|Maiden name| The original name of a natural person who has changed their name after marriage.                                                                   |
|LEGL|Legal name| The name that identifies a natural person for legal, official or administrative purposes.                                                          |
|MISC|Unspecified| A name by which a natural person maybe known but which cannot otherwise be categorized or the category of which the sender is unable to determine. |

#### LegalPersonNameTypeCode
|Code|Name| Description                                                                                                                                        |
|------|---|----------------------------------------------------------------------------------------------------------------------------------------------------|
|LEGL|Legal name| Official name under which an organisation is registered.                                                                          |
|SHRT|Short name| Specifies the short name of the organisation.                                                                                                 |
|TRAD|Trading name| Name used by a business for commercial purposes, although its registered legal name, used for contracts and other formal situations, may be another.                                                               |
 
#### AddressTypeCode
|Code|Name| Description                                                                                                           |
|------|---|-----------------------------------------------------------------------------------------------------------------------|
|HOME|Residential| Address is the home address.                                                                                          |
|BIZZ|Business| Address is the business address.                                                                                      |
|GEOG|Geographic| Address is the unspecified physical(geographical) address suitable for identification of the natural or legal person. |

#### NationalIdentifierTypeCode
|Code| Name                               | Description                                                                                                                                    |
|------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
|ARNU| Alien registration number          | Number assigned by a government agency to identify foreign nationals.                                                                          |
|CCPT| Passport number                    | Number assigned by a passport authority.                                                                                                       |
|RAID| Registration authority identifier  | Identifier of a legal entity as maintained by a registration authority.                                                                        |
|DRLC| Driver license number              | Number assigned to a driver's license.                                                                                                         |
|FIIN| Foreign investment identity number | Number assigned to a foreign investor(other than the alien number).                                                                            |
|TXID| Tax identification number                         | Number assigned by a tax authority to an entity.                                                                                               |
|SOCS| Social security number                         | Number assigned by a social security agency.                                                                                                   |
|IDCD| Identity card number                         | Number assigned by a national authority to an identity card.                                                                                   |
|LEIX| Legal Entity Identifier                          | Legal Entity Identifier (LEI) assigned in accordance with ISO 174421                                                                           |
|MISC| Unspecified                         | A national identifier which may be known but which cannot otherwise be categorized or the category of which the sender is unable to determine. |
* CODE provides K-LEI issuance fee waiver program for alliance members. For more information please visit [https://www.codevasp.com/page-lei](https://www.codevasp.com/page-lei).




## IVMS101 standard

CODE uses the IVMS101 standard to exchange personal information related to virtual transaction. [https://intervasp.org/](https://intervasp.org/)
- A field name of a message is expressed with camelCase whose first character starts with a lowercase. But, 'Originator', 'Beneficiary', 'OriginatorVASP', and 'BeneficiaryVASP' objects corresponding to Entity in ivms101 are expressed with PascalCase.
- The values of all fields are not case-sensitive unless otherwise specified.
- The values of all fields are always expressed with a UTF-8 encoded string. (including boolean, integer, real number, etc.)
- In principle, the values of all fields shall be written in English except when Local Language is permitted.
- You may find more information about the IVMS101 rule for CODE protocol [here](https://codevasp.gitbook.io/code-api-doc-en/api-reference/ivms101).
- Please refer to complete natual person example json in complete-example.json file.
- Please refer to complete legal person example json in complete-example-legal-person.json file.
- Complete json schema is provided in json-schema.json file.
- You can test your IVMS101 format with [https://ivmsvalidator.com/](https://ivmsvalidator.com/).

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
               "streetName":"Example Street",
               "buildingNumber":"123",
               "buildingName":"Example Building",
               "postcode":"00000",
               "townName":"Seoul",
               "countrySubDivision":"N/A",
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
               "streetName":"Example Street",
               "buildingNumber":"456",
               "buildingName":"Example Building",
               "postcode":"00000",
               "townName":"Seoul",
               "countrySubDivision":"N/A",
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

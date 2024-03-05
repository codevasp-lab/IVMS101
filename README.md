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

### Asset Transfer Authorization IVMS101 Request
- ivms101(Required): This is an object defined according to the IVMS101 international standard for each subject involved in the transfer of virtual assets, such as Beneficiary, BeneficiaryVASP, Originator, and OriginatorVASP as per the IVMS101 message standard. The originator's name, asset address, beneficiary's asset address, and OriginatingVASP information in the 'Request for transfer asset authorization' shall be included, and beneficiary name is optional.
  - Originator(Required): Refers to the account holder who allows the Virtual Asset(VA) transfer from that account, the natural or legal person that places the order with the originating VASP to perform the VA transfer.
    - originatorPersons(Required): There are two types of objects, naturalPerson (individual) and legalPerson (corporate), and for legalPerson, both legalPerson (corporation) and naturalPerson (representative) information shall be set. This is an array object, and an element of the array shall define either naturalPerson or legalPerson. For details, please refer to the IVMS101 standard item at the upper section of this manual.
      - naturalPerson(Required): This is an object for setting information on a natural person, and the name information shall be set as required.
        - name(Required):
          - nameIdentifier: Enter the legal name. If a transaction is made between VASPs in Korea, enter in Korean. If a transaction is made with VASPs outside Korea, enter in English. Please refer to the IVMS101 standard item.
            - primaryIdentifier: If you cannot enter or separate the last name, enter the first name and last name together in order.
            - secondaryIdentifier: Enter the first name. If first name and last name cannot be separated, omit them.
            - nameIdentifierType: Fixed as `LEGL`(legal)
          - localNameIdentifier: If a transaction is made with VASPs outside Korea, this is defined to send a local name additionally.
            - primaryIdentifier: Enter the last name in a local name separated by first name and last name. If they cannot be separated, enter the first name and last name together in order.
            - secondaryIdentifier: Enter the first name in a local name separated by first name and last name. If they cannot be separated, omit them.
            - nameIdentifierType: Fixed as `LEGL`(legal)
        - customerIdentification(Optional): This is an identifier (UID or IDX), with which a VASP can identify an originator who transfers the assets
      - legalPerson(Optional): This is an object for setting information on an legal person, and the name object shall be set as required.
        - name(Required):
          - nameIdentifier: Enter the name of a legal person in the registration. If a transaction is made between domestic VASPs, enter it in Korean or in English, and If a transaction is made with overseas VASPs, enter in English.
            - legalPersonName: Legal person name
            - legalPersonNameIdentifierType: Fixed as `LEGL`(legal)
        - customerIdentification(Optional): This is an identifier (UID or IDX), with which a VASP can identify an originator who transfers the assets
    - accountNumber(Required): This is a wallet address which transfers the assets. If tag or memo value is required, separate them with `:` and make one string.
  - Beneficiary(Required): Fill in the information about the individual or legal entity and their representative who will receive the assets. When sending a request, you must include the Beneficiary information, which consists of the name and wallet address. The wallet address information is mandatory, the name information is optional if the tradePrice does not exceed the Travel Rule threshold, but required if it does. The name information is Required when `isExceedingThreshold` is true, and Optional when `isExceedingThreshold` is false. 
  ※ Considering the market volatility and global regulations, we recommend applying the Travel Rule to all transactions. In this case, set the `isExceedingThreshold` as False and enter the beneficiary's name.
    - beneficiaryPersons(Required): The Beneficiary object must include a sub-object called `beneficiaryPersons`. The structure of `beneficiaryPersons` is the same as `originatorPersons`. It can be divided into `naturalPerson` or `legalPerson`. When comparing the name entered with the actual name of the recipient, if the names do not match, the receiving VASP sends a denied response.
      - naturalPerson(Required or Optional): This is an object used to set information about an individual. It is Required when `isExceedingThreshold` is true, and Optional when `isExceedingThreshold` is false.
      - legalPerson(Required or Optional): This is an object used to set information about a legal entity. It is Required when `isExceedingThreshold` is true, and Optional when `isExceedingThreshold` is false.
    - accountNumber(Required): This is a wallet address to which the assets are transferred If tag or memo value is required, separate them with `:` and make one string.
  - OriginatingVASP(Required): Refers to the VASP which initiates the Virtual Asset(VA) transfer and transfers the VA upon receiving the request for a VA transfer on behalf of the originator.
    - originatingVASP(Required):
      - legalPerson(Required): This is the information on the legal person of a VASP to whom you want to transfer an asset
        - name(Required):
          - nameIdentifier: Name information following international notation. (English name)
            - legalPersonName: English legal person name
            - legalPersonNameIdentifierType: Fixed as `LEGL`legal)
        - geographicAddress(Optional): location in the registration documents of a legal person. You shall enter one of the legal person's registration number and address.
          - addressType: Enter `GEOG`.
          - townName: Enter the state/province name.
          - addressLine: Enter the townName sub-address in the array format array of a string.
          - country: This is a two-letter country code determined by ISO-3166-1 alpha-2. Ex) KR, JP, US, etc.
        - nationalIdentification(Optional): This is a legal person identification number which has been certified by the country i.e.a business registration number. You shall enter either the legal person's address or registration number.
          - nationalIdentifier: Business registration number
          - nationalIdentifierType: `RAID`(Registration authority identifier)
          - registrationAuthority: `RA000657` (Korea National Tax Service's RA identification number)
        - countryOfRegistration(Required): country of registration. This is a two-letter country code determined by ISO-3166-1 alpha-2. Ex) KR, JP, US, etc.

### Asset Transfer Authorization IVMS101 Response
- ivms101(Required): This is an object defined according to the IVMS101 international standard for each subject involved in the transfer of virtual assets, such as Beneficiary, BeneficiaryVASP, Originator, and OriginatorVASP as per the IVMS101 message standard. For the Originator, OriginatingVASP information In 'Asset Transfer Authorization Response', copy the data of the request, and enter the Beneficiary and BeneficiaryVASP data.
  - Originator(Required): This is information on the originator (individual) or legal person and representative who wants to transfer the assets, and the request value is copied and used.
  - Beneficiary(Required): information on the originator (individual) or legal person and representative to whom the assets are transferred. You shall enter and send the name and address of an asset in the 'Asset Transfer Authorization Response'.
    - beneficiaryPersons(Required): This shall be included in the Beneficiary object, which is the parent object, and since the structure is the same as `originatorPersons`, please refer to the `originatorPersons` description in the request.
      - naturalPerson(Required): This is an object for setting information on an individual, and the name information shall be set as required.
      - legalPerson(Optional): This is an object for setting information on a legal person, and the name information shall be set as required.
    - accountNumber(Required): This is a wallet address which transfers the assets. If tag or memo value is required, separate them with `:` and make one string.
  - OriginatingVASP(Required): Copy and use the value of the request as the information on the originating VASP to which you want to transfer the asset.
  - BeneficiaryVASP(Required): This is the information on a beneficiary's VASP to which assets are transferred. Since the structure is the same as OriginatingVASP, please refer to the OriginatingVASP description in the request.
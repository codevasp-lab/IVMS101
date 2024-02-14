## IVMS101 standard

CODE 는 가상 거래와 관련한 개인정보를 교환하기 위해 IVMS101 표준을 사용합니다. [https://intervasp.org/](https://intervasp.org/)
- 메시지의 필드 이름은 첫글자를 소문자로 시작하는 camelCase 로 표기합니다. 단, IVMS101 에서 Entity 에 해당하는 'Originator', 'Beneficiary', 'OriginatorVASP', 'BeneficiaryVASP' 객체는 PascalCase 로 표기합니다.
- 모든 필드의 값(Value)는 별도로 명시된 내용이 없으면 대소문자를 구분하지 않습니다.
- 모든 필드의 값(Value)는 항상 UTF-8 인코딩된 문자열로 표현합니다. (boolean 이나 정수, 실수 등 포함)
- 모든 필드의 값(Value)는 한글이 허용되는 경우를 제외하면 영어 표기를 원칙으로 합니다.
- CODE 프로토콜의 IVMS101 규칙은 [여기](https://docs.codevasp.com/api-reference/ivms101)에서 더 자세히 볼 수 있습니다..
- 개인 회원 간 전송의 예제는 complete-example.json 파일을 참고하세요.
- 법인 회원 간 전송의 예제는 complete-example-legal-person.json 파일을 참고하세요.
- 전체 json 스키마는 json-schema.json 파일에 있습니다.
- IVMS101 포맷 검증은 [https://ivmsvalidator.com/](https://ivmsvalidator.com/) 를 이용하셔서 테스트 가능합니다.

### Originator VASP 에서 자산 이전 허가 요청을 할때
Originator VASP로서 다음의 오브젝트를 beneficiary VSAP에게 전송해야합니다. CODE의 VASP LIST API로 부터 이미 entityId를 알고 있으나, VASP의 정보를 정확히 모르기 때문에 아래와 같이 BeneficiaryVASP 객체는 보내지 않습니다.
```
{
  "Originator": {...},
  "Beneficiary": {...},
  "OriginatingVASP": {...}
}
```

### Beneficiary VASP의 응답
Beneficiary VASP 로서 Originator VASP의 응답에 BeneficiaryVASP 객체를 추가하여 응답합니다.
```
{
  "Originator": {...},
  "Beneficiary": {...},
  "OriginatingVASP": {...},
  "BeneficiaryVASP": {...}
}
```
응답 시 `customerIdentification`와 같은 Beneficiary 의 정보를 Beneficiary 객체 내에 추가 기입하는것도 권장합니다.

### Originating natual person 예제
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

### Originating legal person 예제
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

### Originating VASP 예제
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

### Beneficiary natual person 예제
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

### Beneficiary legal person 예제
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

### Beneficiary VASP 예제
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

# IVMS101 standard

CODE 는 가상 자산 거래와 관련한 개인정보를 교환하기 위해 IVMS101 표준을 사용합니다. [https://intervasp.org/](https://intervasp.org/)
- 메시지의 필드 이름은 첫글자를 소문자로 시작하는 camelCase 로 표기합니다. 단, IVMS101 에서 Entity 에 해당하는 `Originator`, `Beneficiary`, `OriginatorVASP`, `BeneficiaryVASP` 객체는 PascalCase 로 표기합니다.
- 모든 필드의 값(Value)는 별도로 명시된 내용이 없으면 대소문자를 구분하지 않습니다.
- 모든 필드의 값(Value)는 항상 UTF-8 인코딩된 문자열로 표현합니다. (boolean 이나 정수, 실수 등 포함)
- 모든 필드의 값(Value)는 한글이 허용되는 경우를 제외하면 영어 표기를 원칙으로 합니다.
- CODE 프로토콜의 IVMS101 규칙은 [여기](https://code-docs-kr.readme.io/reference/ivms101-%ED%91%9C%EC%A4%80)에서 더 자세히 볼 수 있습니다..
- 개인 회원 간 전송의 예제는 complete-example.json 파일을 참고하세요.
- 법인 회원 간 전송의 예제는 complete-example-legal-person.json 파일을 참고하세요.
- 전체 json 스키마는 json-schema.json 파일에 있습니다.
- [https://www.jsonschemavalidator.net/](https://www.jsonschemavalidator.net/) 를 이용해서 json 포맷을 검증 하실 수 있습니다.
  - Select schema: IVMS101 by CODE Protocol 선택하셔서 사용하시면 됩니다.

* * *

## 자산 이전 허가 요청
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

### 자산 이전 허가 요청 IVMS101 Request
- **ivms101**(Required): IVMS101 메시지 표준을 따르는 송금인(`Originator`), 수취인(`Beneficiary`), 송신 VASP(`OriginatorVASP`), 수취VASP(`BeneficiaryVASP`) 등 가상자산 이전에 관여하는 각 주체를 IVMS101 국제 표준에 맞춰서 정의한 객체입니다. '**자산 이전 허가 요청**' 에서는 `Originator` 의 이름과 자산 주소, `Beneficiary` 의 자산 주소, `OriginatingVASP` 정보가 반드시 포함돼야 하고, `Beneficiary` 이름은 선택 사항입니다.
  - **Originator**(Required): 자산을 이전하고자 하는 송금인(개인) 또는 법인 및 대표자에 대한 정보.
    - **originatorPersons**(Required): `naturalPerson`(개인), `legalPerson`(법인) 두 종류의 객체가 있으며, 법인의 경우에는 `legalPerson`(법인)과 `naturalPerson`(대표자) 정보를 모두 설정해야 합니다. 배열 객체이며, 배열의 각 요소(element)는 `naturalPerson` 또는 `legalPerson` 중 하나 만을 정의해야 합니다. 자세한 내용은 [IVMS101 표준](https://code-docs-kr.readme.io/reference/ivms101-%ED%91%9C%EC%A4%80) 항목을 참고해주세요.
      - **naturalPerson**(Required): 개인에 대한 정보를 설정하기 위한 객체로 `name`(이름) 정보를 필수로 설정해야 합니다.
        - **name**(Required):
          - **nameIdentifier**: 법적 성명을 기입합니다. 국내 VASP 끼리 거래하는 경우는 한글로 기입하고, 해외 VASP 와 거래하는 경우 영문으로 기입합니다. [IVMS101 표준](https://code-docs-kr.readme.io/reference/ivms101-%ED%91%9C%EC%A4%80) 항목을 참고해 주세요.
            - **primaryIdentifier**: 성명 중 성을 기입합니다. 분리할 수 없는 경우는 성과 이름을 순서대로 함께 표기합니다.
            - **secondaryIdentifier**: 성명 중 이름을 기입합니다. 성과 이름을 분리할 수 없는 경우는 생략합니다.
            - **nameIdentifierType**: `LEGL`(legal) 로 고정됩니다.
          - **localNameIdentifier**: 해외 VASP 와 거래하는 경우 한글 이름을 추가로 전달하기 위해 정의합니다.
            - **primaryIdentifier**: 한글 표기 성명 중 성을 기입합니다. 분리할 수 없는 경우 성과 이름을 순서대로 함께 표기합니다.
            - **secondaryIdentifier**: 한글 표기 성명 중 이름을 기입합니다. 분리할 수 없는 경우는 생략합니다.
            - **nameIdentifierType**: `LEGL`(legal) 로 고정됩니다.
        - **customerIdentification**(Optional): 자산을 전송하는 송금인을 VASP에서 식별 가능한 식별자 (UID 또는 IDX)
      - **legalPerson**(Optional): 법인에 대한 정보를 설정하기 위한 객체로 `name` 객체를 필수로 설정해야 합니다.
        - **name**(Required):
          - **nameIdentifier**: 법인의 등록 상 명칭을 기입합니다. 국내 VASP 끼리 거래하는 경우는 한글 또는 영문으로 기입하고, 해외 VASP 와 거래하는 경우 영문으로 기입합니다.
            - **legalPersonName**: 법인명을 기입합니다.
            - **legalPersonNameIdentifierType**: `LEGL`(legal) 로 고정됩니다.
        - **customerIdentification**(Optional): 자산을 전송하는 송금인을 VASP에서 식별 가능한 식별자 (UID 또는 IDX)
    - **accountNumber**(Required): 자산을 전송하는 지갑 주소입니다. tag 나 memo 값이 필요한 경우는 `:` 로 구분해서 하나의 문자열로 만듭니다. 검증 방법은 [지갑 주소 검증하기](https://code-docs-kr.readme.io/reference/%EC%A7%80%EA%B0%91-%EC%A3%BC%EC%86%8C-%EA%B2%80%EC%A6%9D%ED%95%98%EA%B8%B0) 페이지를 참조해 주세요.
  - **Beneficiary**(Required): 자산을 수신 받는 수취인(개인) 또는 법인 및 대표자에 대한 정보를 기입합니다. 요청(request)을 보낼 때, `Beneficiary` 정보를 함께 기입하여 보내야 하며, ①이름과 ②지갑 주소로 구성되어 있습니다. 지갑 주소 정보는 필수입니다. 이름 정보는 `tradePrice`가 트래블룰 적용 기준을 초과할 경우, 필수로 설정해야하고, 초과하지 않을 경우 옵션입니다.
  ※ 이름 정보는 `isExceedingThreshold`가 true일 때 Required, `isExceedingThreshold`가 false일 때 Optional입니다.
    - **beneficiaryPersons**(Required): `Beneficiary` 상위 객체에는 반드시 `beneficiaryPersons`라는 하위 객체가 포함되어야 합니다. `beneficiaryPersons`는 `originatorPersons`와 구조가 동일합니다. `beneficiaryPersons` 하위에는 `naturalPerson` 또는 `legalPerson`로 나눌 수 있습니다. 수취 VASP는 입력한 이름과 실제 수취인의 이름을 비교했을 때, 이름이 다를 경우 거절(denied) 응답을 보냅니다.
      - **naturalPerson**(Required or Optional): 개인에 대한 정보를 설정하기 위한 객체이며, `isExceedingThreshold`가 true일 경우 Required, `isExceedingThreshold`가 false일 경우 Optional입니다.
      - **legalPerson**(Required or Optional): 법인에 대한 정보를 설정하기 위한 객체이며, `isExceedingThreshold`가 true일 경우 Required, `isExceedingThreshold`가 false일 경우 Optional입니다.
    - **accountNumber**(Required): 자산을 수신하는 지갑 주소입니다. tag 나 memo 값이 필요한 경우는 `:` 로 구분해서 하나의 문자열로 만듭니다.
  - **OriginatingVASP**(Required): 자산을 전송하려는 송신 VASP 정보입니다.
    - **originatingVASP**(Required):
      - **legalPerson**(Required): 자산을 전송하려는 VASP의 법인 정보입니다.
        - **name**(Required):
          - **nameIdentifier**: 국제 표기법을 따르는 영문 이름 정보입니다.
            - **legalPersonName**: 영문 법인명입니다.
            - **legalPersonNameIdentifierType**: `LEGL`(legal) 로 고정됩니다.
        - **geographicAddress**(Optional): 법인의 등록 서류상 소재지입니다. 법인 주소 또는 법인 등록 번호 중 하나는 반드시 입력해야 합니다.
          - **addressType**: `GEOG` 로 입력합니다.
          - **townName**: 시/도 이름을 입력합니다.
          - **addressLine**: townName 하위 주소를 문자열의 배열 형식으로 입력합니다.
          - **country**: ISO-3166-1 alpha-2 에서 정하는 2글자 국가 코드입니다. 예) `KR`, `JP`, `US` 등
        - **nationalIdentification**(Optional): 국가의 인증을 받은 법인 식별 번호, 즉 사업자등록번호 입니다. 법인 주소 또는 등록 번호 중 하나는 반드시 입력해야 합니다.
          - **nationalIdentifier**: 사업자등록번호
          - **nationalIdentifierType**: `RAID`(Registration authority identifier)
          - **registrationAuthority**: `RA000657` (대한민국 국세청 RA 식별번호)
        - **countryOfRegistration**(Required): 등록 국가. ISO-3166-1 alpha-2 에서 정하는 2글자 국가 코드입니다. 예) `KR`, `JP`, `US` 등

### 자산 이전 허가 요청 IVMS101 Response
- **ivms101**(Required): IVMS101 메시지 표준을 따르는 송금인(`Originator`), 수취인(`Beneficiary`), 송신 VASP(`OriginatorVASP`), 수취 VASP(`BeneficiaryVASP`) 등 가상자산 이전에 관여하는 각 주체를 IVMS101 국제 표준에 맞춰 정의한 객체입니다. 응답(response) 객체에서는 `Originator`, `OriginatingVASP` 정보는 요청(request)의 데이터를 복사하고, `Beneficiary`, `BeneficiaryVASP` 데이터가 추가됩니다.
  - **Originator**(Required): 자산을 전송하고자 하는 송금인(개인) 또는 법인 및 대표자에 대한 정보입니다. 요청의 값을 그대로 복사해서 사용합니다.
  - **Beneficiary**(Required): 자산을 수신하는 수취인(개인) 또는 법인 및 대표자에 대한 정보입니다. 응답(response)에서는 이름과 자산 주소를 기입해서 보내야 합니다.
    - **beneficiaryPersons**(Required): 상위 객체인 `Beneficiary` 객체에 반드시 포함되어야 하며 구조는 `originatorPersons` 와 같으므로 요청의 `originatorPersons` 설명을 참고해 주세요.
      - **naturalPerson**(Required): 개인에 대한 정보를 설정하기 위한 객체로 `name`(이름) 정보를 필수로 설정해야 합니다.
      - **legalPerson**(Optional): 법인에 대한 정보를 설정하기 위한 객체로 `name`(이름) 정보를 필수로 설정해야 합니다.
    - **accountNumber**(Required): 자산을 수신하는 지갑 주소입니다. tag 나 memo 값이 필요한 경우는 `:` 로 구분해서 하나의 문자열로 만듭니다. 검증 방법은 [지갑 주소 검증하기](https://code-docs-kr.readme.io/reference/%EC%A7%80%EA%B0%91-%EC%A3%BC%EC%86%8C-%EA%B2%80%EC%A6%9D%ED%95%98%EA%B8%B0) 페이지를 참조해 주세요.
  - **OriginatingVASP**(Required): 자산을 전송하려는 송신 VASP 정보로 요청의 값을 그대로 복사해서 사용합니다.
  - **BeneficiaryVASP**(Required): 자산을 수신하는 수취 VASP 정보입니다. 구조는 `OriginatingVASP` 와 같으므로 요청의 `OriginatingVASP` 설명을 참고해 주세요.

* * *

## 자산 이전 데이터 요청
자산 이전 데이터 요청은 자산 이전 허가 요청과 반대로, 이미 On-Chain 트랜잭션이 발생하여 익명의 입고가 발생 했을 시, 트래블룰 데이터를 상대방으로부터 얻기 위한 방법입니다. 
### Beneficiary VASP 에서 자산 이전 데이터 요청을 할때
Beneficiary VASP로서 다음의 오브젝트를 Originator VSAP에게 전송해야합니다. TXID로 VASP찾기에서 Originator VASP의 entityId는 이미 알고 있으나 송신자의 정보를 모르기 때문에 아래와 같이 보냅니다.
```
{
  "Beneficiary": {...},
  "BeneficiaryVASP": {...}
}
```

### Originator VASP의 응답
Originator VASP 로서 Beneficiary VASP의 요청에 `Originator` 및 `OriginatorVASP` 객체를 추가하여 응답합니다.
```
{
  "Originator": {...},
  "OriginatingVASP": {...},
  "Beneficiary": {...},
  "BeneficiaryVASP": {...}
}
```

### 자산 이전 데이터 요청 IVMS101 Request
- **ivms101**(Required): IVMS101 메시지 표준을 따르는 송금인(`Originator`), 수취인(`Beneficiary`), 송신 VASP(`OriginatorVASP`), 수취VASP(`BeneficiaryVASP`) 등 가상자산 이전에 관여하는 각 주체를 IVMS101 국제 표준에 맞춰서 정의한 객체입니다. '**자산 이전 데이터 요청**' 에서는 `Beneficiary` 의 이름과 자산 주소, `BeneficiaryVASP` 정보가 반드시 포함돼야 합니다.
  - **Beneficiary**(Required): 자산을 수신 받는 수취인(개인) 또는 법인 및 대표자에 대한 정보를 기입합니다. 요청(request)을 보낼 때, `Beneficiary` 정보를 기입하여 보내야 하며, ①이름과 ②지갑 주소로 구성되어 있습니다. 지갑 주소 정보는 필수입니다. 
    - **beneficiaryPersons**(Required): `naturalPerson`(개인), `legalPerson`(법인) 두 종류의 객체가 있으며, 법인의 경우에는 `legalPerson`(법인)과 `naturalPerson`(대표자) 정보를 모두 설정해야 합니다. 배열 객체이며, 배열의 각 요소(element)는 `naturalPerson` 또는 `legalPerson` 중 하나 만을 정의해야 합니다. 자세한 내용은 [IVMS101 표준](https://code-docs-kr.readme.io/reference/ivms101-%ED%91%9C%EC%A4%80) 항목을 참고해주세요.
      - **naturalPerson**(Required): 개인에 대한 정보를 설정하기 위한 객체로 `name`(이름) 정보를 필수로 설정해야 합니다.
        - **name**(Required):
          - **nameIdentifier**: 법적 성명을 기입합니다. 국내 VASP 끼리 거래하는 경우는 한글로 기입하고, 해외 VASP 와 거래하는 경우 영문으로 기입합니다. [IVMS101 표준](https://code-docs-kr.readme.io/reference/ivms101-%ED%91%9C%EC%A4%80) 항목을 참고해 주세요.
            - **primaryIdentifier**: 성명 중 성을 기입합니다. 분리할 수 없는 경우는 성과 이름을 순서대로 함께 표기합니다.
            - **secondaryIdentifier**: 성명 중 이름을 기입합니다. 성과 이름을 분리할 수 없는 경우는 생략합니다.
            - **nameIdentifierType**: `LEGL`(legal) 로 고정됩니다.
          - **localNameIdentifier**: 해외 VASP 와 거래하는 경우 한글 이름을 추가로 전달하기 위해 정의합니다.
            - **primaryIdentifier**: 한글 표기 성명 중 성을 기입합니다. 분리할 수 없는 경우 성과 이름을 순서대로 함께 표기합니다.
            - **secondaryIdentifier**: 한글 표기 성명 중 이름을 기입합니다. 분리할 수 없는 경우는 생략합니다.
            - **nameIdentifierType**: `LEGL`(legal) 로 고정됩니다.
        - **customerIdentification**(Optional): 자산을 수신 받은 수신인을 VASP에서 식별 가능한 식별자 (UID 또는 IDX)
      - **legalPerson**(Optional): 법인에 대한 정보를 설정하기 위한 객체로 `name` 객체를 필수로 설정해야 합니다.
        - **name**(Required):
          - **nameIdentifier**: 법인의 등록 상 명칭을 기입합니다. 국내 VASP 끼리 거래하는 경우는 한글 또는 영문으로 기입하고, 해외 VASP 와 거래하는 경우 영문으로 기입합니다.
            - **legalPersonName**: 법인명을 기입합니다.
            - **legalPersonNameIdentifierType**: `LEGL`(legal) 로 고정됩니다.
        - **customerIdentification**(Optional): 자산을 수신 받은 수신인을 VASP에서 식별 가능한 식별자 (UID 또는 IDX)
    - **accountNumber**(Required): 자산을 수신 받은 지갑 주소입니다. tag 나 memo 값이 필요한 경우는 `:` 로 구분해서 하나의 문자열로 만듭니다.
  - **BeneficiaryVASP**(Required): 자산을 수신 받은 VASP 정보입니다.
    - **beneficiaryVASP**(Required):
      - **legalPerson**(Required): 자산을 수신 받은 VASP의 법인 정보입니다.
        - **name**(Required):
          - **nameIdentifier**: 국제 표기법을 따르는 영문 이름 정보입니다.
            - **legalPersonName**: 영문 법인명입니다.
            - **legalPersonNameIdentifierType**: `LEGL`(legal) 로 고정됩니다.
        - **geographicAddress**(Optional): 법인의 등록 서류상 소재지입니다. 법인 주소 또는 법인 등록 번호 중 하나는 반드시 입력해야 합니다.
          - **addressType**: `GEOG` 로 입력합니다.
          - **townName**: 시/도 이름을 입력합니다.
          - **addressLine**: townName 하위 주소를 문자열의 배열 형식으로 입력합니다.
          - **country**: ISO-3166-1 alpha-2 에서 정하는 2글자 국가 코드입니다. 예) `KR`, `JP`, `US` 등
        - **nationalIdentification**(Optional): 국가의 인증을 받은 법인 식별 번호, 즉 사업자등록번호 입니다. 법인 주소 또는 등록 번호 중 하나는 반드시 입력해야 합니다.
          - **nationalIdentifier**: 사업자등록번호
          - **nationalIdentifierType**: `RAID`(Registration authority identifier)
          - **registrationAuthority**: `RA000657` (대한민국 국세청 RA 식별번호)
        - **countryOfRegistration**(Required): 등록 국가. ISO-3166-1 alpha-2 에서 정하는 2글자 국가 코드입니다. 예) `KR`, `JP`, `US` 등

### 자산 이전 데이터 요청 IVMS101 Response
- **ivms101**(Required): 응답(response) 객체에서는 `Originator`, `OriginatingVASP` 정보는 TXID로 부터 찾은 데이터로 객체를 생성하고, `Beneficiary`, `BeneficiaryVASP` 데이터는 요청의 객체를 그대로 사용합니다.
  - **Originator**(Required): 송금인(개인) 또는 법인 및 대표자에 대한 정보.
    - **originatorPersons**(Required): `naturalPerson`(개인), `legalPerson`(법인) 두 종류의 객체가 있으며, 법인의 경우에는 `legalPerson`(법인)과 `naturalPerson`(대표자) 정보를 모두 설정해야 합니다. 배열 객체이며, 배열의 각 요소(element)는 `naturalPerson` 또는 `legalPerson` 중 하나 만을 정의해야 합니다. 자세한 내용은 [IVMS101 표준](https://code-docs-kr.readme.io/reference/ivms101-%ED%91%9C%EC%A4%80) 항목을 참고해주세요.
      - **naturalPerson**(Required): 개인에 대한 정보를 설정하기 위한 객체로 `name`(이름) 정보를 필수로 설정해야 합니다.
        - **name**(Required):
          - **nameIdentifier**: 법적 성명을 기입합니다. 국내 VASP 끼리 거래하는 경우는 한글로 기입하고, 해외 VASP 와 거래하는 경우 영문으로 기입합니다. [IVMS101 표준](https://code-docs-kr.readme.io/reference/ivms101-%ED%91%9C%EC%A4%80) 항목을 참고해 주세요.
            - **primaryIdentifier**: 성명 중 성을 기입합니다. 분리할 수 없는 경우는 성과 이름을 순서대로 함께 표기합니다.
            - **secondaryIdentifier**: 성명 중 이름을 기입합니다. 성과 이름을 분리할 수 없는 경우는 생략합니다.
            - **nameIdentifierType**: `LEGL`(legal) 로 고정됩니다.
          - **localNameIdentifier**: 해외 VASP 와 거래하는 경우 한글 이름을 추가로 전달하기 위해 정의합니다.
            - **primaryIdentifier**: 한글 표기 성명 중 성을 기입합니다. 분리할 수 없는 경우 성과 이름을 순서대로 함께 표기합니다.
            - **secondaryIdentifier**: 한글 표기 성명 중 이름을 기입합니다. 분리할 수 없는 경우는 생략합니다.
            - **nameIdentifierType**: `LEGL`(legal) 로 고정됩니다.
        - **customerIdentification**(Optional): 자산을 전송하는 송금인을 VASP에서 식별 가능한 식별자 (UID 또는 IDX)
      - **legalPerson**(Optional): 법인에 대한 정보를 설정하기 위한 객체로 `name` 객체를 필수로 설정해야 합니다.
        - **name**(Required):
          - **nameIdentifier**: 법인의 등록 상 명칭을 기입합니다. 국내 VASP 끼리 거래하는 경우는 한글 또는 영문으로 기입하고, 해외 VASP 와 거래하는 경우 영문으로 기입합니다.
            - **legalPersonName**: 법인명을 기입합니다.
            - **legalPersonNameIdentifierType**: `LEGL`(legal) 로 고정됩니다.
        - **customerIdentification**(Optional): 송금인을 VASP에서 식별 가능한 식별자 (UID 또는 IDX)
    - **accountNumber**(Required): 송금인의 지갑 주소입니다. tag 나 memo 값이 필요한 경우는 `:` 로 구분해서 하나의 문자열로 만듭니다. 
  - **OriginatingVASP**(Required): 송신 VASP 정보입니다.
    - **originatingVASP**(Required):
      - **legalPerson**(Required): 송신 VASP의 법인 정보입니다.
        - **name**(Required):
          - **nameIdentifier**: 국제 표기법을 따르는 영문 이름 정보입니다.
            - **legalPersonName**: 영문 법인명입니다.
            - **legalPersonNameIdentifierType**: `LEGL`(legal) 로 고정됩니다.
        - **geographicAddress**(Optional): 법인의 등록 서류상 소재지입니다. 법인 주소 또는 법인 등록 번호 중 하나는 반드시 입력해야 합니다.
          - **addressType**: `GEOG` 로 입력합니다.
          - **townName**: 시/도 이름을 입력합니다.
          - **addressLine**: townName 하위 주소를 문자열의 배열 형식으로 입력합니다.
          - **country**: ISO-3166-1 alpha-2 에서 정하는 2글자 국가 코드입니다. 예) `KR`, `JP`, `US` 등
        - **nationalIdentification**(Optional): 국가의 인증을 받은 법인 식별 번호, 즉 사업자등록번호 입니다. 법인 주소 또는 등록 번호 중 하나는 반드시 입력해야 합니다.
          - **nationalIdentifier**: 사업자등록번호
          - **nationalIdentifierType**: `RAID`(Registration authority identifier)
          - **registrationAuthority**: `RA000657` (대한민국 국세청 RA 식별번호)
        - **countryOfRegistration**(Required): 등록 국가. ISO-3166-1 alpha-2 에서 정하는 2글자 국가 코드입니다. 예) `KR`, `JP`, `US` 등
  - **Beneficiary**(Required): 자산을 수신 받은 수신자의 정보입니다. 요청의 값을 그대로 복사해서 사용합니다.
  - **BeneficiaryVASP**(Required): 자산을 수신 받은 VASP 정보입니다. 요청의 값을 그대로 복사해서 사용합니다.

* * *
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

* * *

### ENUM 목록
#### NaturalPersonNameTypeCode
| 코드   | 이름            | 설명                                                                                                                                                 |
|------|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| ALIA | Alias name    | 법적 이름 이외에 자연인이 알려진 다른 이름.                                                                        |
| BIRT | Name at birth | 출생 시 자연인에게 부여된 이름.                                                                                                       |
| MAID | Maiden name   | 결혼 후 이름을 변경한 자연인의 본래 이름.                                                               |
| LEGL | Legal name    | 법적, 공식적 또는 행정적 목적으로 자연인을 식별하는 이름.                                                          |
| MISC | Unspecified   | 자연인이 알려져 있을 수 있지만 다른 방식으로 분류할 수 없거나 발신자가 분류를 결정할 수 없는 카테고리의 이름. |

#### LegalPersonNameTypeCode
|코드   | 이름            | 설명                                                                  |
|------|---|---------------------------------------------------------------------|
|LEGL|Legal name| 조직이 등록된 공식 이름.                                                      |
|SHRT|Short name| 조직의 약칭.                                                             |
|TRAD|Trading name| 비즈니스가 상업적 목적으로 사용하는 이름이며, 계약 및 기타 공식적인 상황에 사용되는 등록된 법적 이름은 다를 수 있음. |

#### AddressTypeCode
|코드   | 이름            | 설명                                                                                                        |
|------|---|--------------------------------------------------------------------------------------------------------------------|
|HOME|Residential| 거주지 주소.                                                                                      |
|BIZZ|Business| 사업장 주소.                                                                                   |
|GEOG|Geographic| 자연인 또는 법인을 식별하기에 적합한 구체적으로 명시되지 않은 물리적(지리적) 주소. |

#### NationalIdentifierTypeCode
|코드   | 이름            | 설명                                                                                                                                             |
|------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
|ARNU| Alien registration number          | 외국인을 식별하기 위해 정부 기관에 의해 할당된 번호.                                                                                                                 |
|CCPT| Passport number                    | 여권 발급 기관에 의해 할당된 번호.                                                                                                                           |
|RAID| Registration authority identifier  | 법인의 등록 기관에서 관리하는 법인의 식별자.                                                                                                                      |
|DRLC| Driver license number              | 운전 면허증에 할당된 번호.                                                                                                                                |
|FIIN| Foreign investment identity number | 외국인 투자자에게 할당된 번호(외국인 등록번호와는 다름).                                                                                                               |
|TXID| Tax identification number                         | 세무 당국에 의해 법인 혹은 개인에게 할당된 번호.                                                                                                                   |
|SOCS| Social security number                         | 사회 보장 기관에 의해 할당된 번호.                                                                                                |
|IDCD| Identity card number                         | 국가 기관에 의해 신분증에 할당된 번호.                                                                                |
|LEIX| Legal Entity Identifier                          | ISO 17442에 따라 할당된 법인 식별 코드(Legal Entity Identifier, LEI)                                                                    |
|MISC| Unspecified                         | 발신자가 분류를 결정할 수 없거나 다른 방식으로 분류할 수 없지만 알려져 있을 수 있는 국가 식별자. |
* CODE는 회원사들을 위해 K-LEI 발급 수수료 면제 프로그램을 제공합니다. 자세한 정보는 홈페이지를 방문해 주세요. [https://www.codevasp.com/ko/page-lei](https://www.codevasp.com/ko/page-lei)

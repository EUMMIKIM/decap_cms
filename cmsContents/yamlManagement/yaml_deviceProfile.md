---
contents:
  lang: yaml
  code: >
    openapi: 3.1.0

    info:
      version: null
      title: 디바이스 프로파일
    servers:
      - url: https://ap.api.lge.com
    tags:
      - name: overview
        x-displayName: 개요
        x-traitTag: true
        description: |
          **디바이스 프로파일**은 디바이스의 상태를 기술하거나 디바이스를 제어할 수 있는 속성들을 정의합니다. 디바이스 프로파일은 디바이스 유형별로 제공되며 같은 유형의 기기이라도 제품 또는 모델에 따라 내용에 차이가 있을 수 있습니다. 디바이스 프로파일에 정의된 속성을 이용하여 디바이스의 상태 조회 및 제어와 관련된 기능을 당신의 시스템에 적용하세요.
          본 문서에서는 디바이스 유형별로 아래의 내용을 설명합니다.

            - **디바이스 프로파일 메시지의 스키마와 예제**
              - *디바이스 프로파일 조회 API*를 이용하여 조회한 디바이스 유형별 디바이스 프로파일은 이 스키마를 따릅니다.
              스키마와 함께 기재된 디바이스 프로파일의 메시지 예제를 참고하세요. 
            - **상태 응답 및 제어 요청 메시지의 스키마**
              - *디바이스 상태 조회 API*의 응답 메시지와 *디바이스 제어 API*의 요청 메시지는 디바이스 프로파일을 기반으로 작성됩니다.
              이 스키마를 참고하여 메시지를 구성하고, 예제는 각 API의 정의를 참조하세요.

          디바이스 프로파일 메시지의 스키마는 공통적으로 아래의 항목들을 설명합니다.

            - **notification** : 해당 디바이스에서 지원하는 푸시 메시지의 종류
            - **property** : 속성별 Read/Write 지원 여부와 값의 특성
      - name: refrigerator-profile
        x-displayName: 냉장고
        description: |
          ## 디바이스 프로파일 스키마
          <SchemaDefinition 
            schemaRef="#/components/schemas/refrigerator-profile" 
            exampleRef="#/components/examples/refrigerator-profile-example" />

          ## 요청/응답 스키마
          ### 디바이스 상태 응답
          <SchemaDefinition 
            schemaRef="#/components/schemas/refrigerator-object" 
            showExample={false} showWriteOnly={false} />

          ### 디바이스 제어 요청
          <SchemaDefinition  schemaRef="#/components/schemas/refrigerator-object"  showExample={false} showReadOnly={false} />
      - name: washer-profile
        x-displayName: 세탁기
        description: |
          ## 디바이스 프로파일 스키마
          ## 요청/응답 스키마      
          ### 디바이스 상태 응답
          ### 디바이스 제어 요청
    paths:
      /placeholder:
        get:
          summary: placeholder
          description: placeholder
          operationId: placeholder
          security:
            - basic_auth: []
          responses:
            '200':
              description: placeholder
              content:
                application/json:
                  schema:
                    oneOf:
                      - $ref: '#/components/schemas/refrigerator-profile'
                      - $ref: '#/components/schemas/refrigerator-object'
                  examples:
                    refrigerator-profile-example:
                      $ref: '#/components/examples/refrigerator-profile-example'
            '400':
              description: placeholder
    components:
      securitySchemes:
        basic_auth:
          type: http
          scheme: basic
      schemas:
        refrigerator-profile:
          type: object
          title: Refrigerator
          required:
            - notification
            - property
          properties:
            notification:
              type: object
              minProperties: 1
              properties:
                push:
                  type: array
                  minItems: 1
                  description: |-
                    Push Code | Description
                    -|-
                    FROZEN_IS_COMPLETE | 냉동이 완료되었습니다.
                    DOOR_IS_OPEN | 문이 열렸습니다.
                    TIME_TO_CHANGE_FILTER | 필터 교체 시기입니다.
                    TIME_TO_CHANGE_WATER_FILTER | 정수 필터 교체 시기입니다.
                  items:
                    type: string
                    enum:
                      - TIME_TO_CHANGE_WATER_FILTER
                      - FROZEN_IS_COMPLETE
                      - DOOR_IS_OPEN
                      - TIME_TO_CHANGE_FILTER
            property:
              type: object
              properties:
                doorStatus:
                  type: array
                  description: 냉장고 문 상태
                  items:
                    type: object
                    properties:
                      doorState:
                        type: object
                        description: 냉장고 문 열림 상태
                        properties:
                          mode:
                            type: array
                            items:
                              type: string
                              enum:
                                - r
                          type:
                            type: string
                            enum:
                              - enum
                          value:
                            type: object
                            properties:
                              r:
                                type: array
                                description: |-
                                  Value | Description
                                  -|-
                                  OPEN | 문 열림 (냉장/냉동/컨버터블 중 적어도 1개 이상의 문이 열림)
                                  CLOSE | 문 닫힘 (모든 문이 닫힘)
                                items:
                                  type: string
                                  enum:
                                    - OPEN
                                    - CLOSE
                      locationName:
                        type: string
                        enum:
                          - MAIN
                        description: |-
                          위치 이름
                          Value | Description
                          -|-
                          MAIN | 냉장고 문
                refrigeration:
                  type: object
                  description: 기능
                  properties:
                    expressMode:
                      type: object
                      description: 특급 모드 설정
                      properties:
                        mode:
                          type: array
                          items:
                            type: string
                            enum:
                              - r
                              - w
                        type:
                          type: string
                          enum:
                            - boolean
                        value:
                          type: object
                          properties:
                            r:
                              type: array
                              description: |-
                                Value | Description
                                -|-
                                true | 특급 모드 설정
                                false | 특급 모드 해제
                              items:
                                type: boolean
                                enum:
                                  - true
                                  - false
                            w:
                              type: array
                              description: |-
                                Value | Description
                                -|-
                                true | 특급 모드 설정
                                false | 특급 모드 해제
                              items:
                                type: boolean
                                enum:
                                  - true
                                  - false
                    rapidFreeze:
                      type: object
                      description: 급속 냉동
                      properties:
                        mode:
                          type: array
                          items:
                            type: string
                            enum:
                              - r
                              - w
                        type:
                          type: string
                          enum:
                            - boolean
                        value:
                          type: object
                          properties:
                            r:
                              type: array
                              description: |-
                                Value | Description
                                -|-
                                true | 급속 냉동 설정
                                false | 급속 냉동 해제
                              items:
                                type: boolean
                                enum:
                                  - true
                                  - false
                            w:
                              type: array
                              description: |-
                                Value | Description
                                -|-
                                true | 급속 냉동 설정
                                false | 급속 냉동 해제
                              items:
                                type: boolean
                                enum:
                                  - true
                                  - false
                temperature:
                  type: array
                  description: 온도
                  items:
                    type: object
                    properties:
                      locationName:
                        type: string
                        enum:
                          - FRIDGE
                          - FREEZER
                          - CONVERTIBLE
                        description: |-
                          위치 이름
                          Value | Description
                          -|-
                          FRIDGE | 냉장실
                          FREEZER | 냉동실
                          CONVERTIBLE | 컨버터블
                      targetTemperature:
                        type: object
                        description: 희망온도
                        properties:
                          mode:
                            type: array
                            items:
                              type: string
                              enum:
                                - r
                                - w
                          type:
                            type: string
                            enum:
                              - range
                          value:
                            type: object
                            properties:
                              r:
                                type: object
                                properties:
                                  max:
                                    type: integer
                                    enum:
                                      - 6
                                      - -16
                                    description: |-
                                      locationName | max
                                      -|-
                                      FRIDGE | 6
                                      FREEZER | 16
                                  min:
                                    type: integer
                                    enum:
                                      - 0
                                      - -24
                                    description: |-
                                      locationName | max
                                      -|-
                                      FRIDGE | 0
                                      FREEZER | -24
                                  step:
                                    type: integer
                                    enum:
                                      - 1
                              w:
                                type: object
                                properties:
                                  max:
                                    type: integer
                                    enum:
                                      - 6
                                      - -16
                                    description: |-
                                      locationName | max
                                      -|-
                                      FRIDGE | 6
                                      FREEZER | 16
                                  min:
                                    type: integer
                                    enum:
                                      - 0
                                      - -24
                                    description: |-
                                      locationName | max
                                      -|-
                                      FRIDGE | 0
                                      FREEZER | -24
                                  step:
                                    type: integer
                                    enum:
                                      - 1
                      unit:
                        type: string
                        enum:
                          - C
                          - F
                        description: |-
                          Value | Description
                          -|-
                          C | 섭씨
                          F | 화씨
                waterFilterInfo:
                  type: object
                  description: 워터 필터 상태
                  properties:
                    usedTime:
                      type: object
                      description: 월단위 사용시간 (몇 개월)
                      properties:
                        mode:
                          type: array
                          items:
                            type: string
                            enum:
                              - r
                        type:
                          type: string
                          enum:
                            - number
        refrigerator-object:
          title: Refrigerator
          minProperties: 1
          properties:
            doorStatus:
              type: array
              readOnly: true
              items:
                type: object
                properties:
                  doorState:
                    type: string
                    enum:
                      - OPEN
                      - CLOSE
                  locationName:
                    type: string
                    enum:
                      - MAIN
            refrigeration:
              type: object
              properties:
                expressMode:
                  type: boolean
                  enum:
                    - true
                    - false
                rapidFreeze:
                  type: boolean
                  enum:
                    - true
                    - false
            temperature:
              type: array
              items:
                type: object
                properties:
                  locationName:
                    type: string
                    readOnly: true
                    enum:
                      - FRIDGE
                      - FREEZER
                      - CONVERTIBLE
                    example: FRIDGE
                  targetTemperature:
                    type: integer
                    example: 3
                  unit:
                    type: string
                    readOnly: true
                    enum:
                      - C
                      - F
                    example: C
            waterFilterInfo:
              type: object
              readOnly: true
              properties:
                usedTime:
                  type: number
                  example: 2
      examples:
        refrigerator-profile-example:
          value:
            notification:
              push:
                - TIME_TO_CHANGE_WATER_FILTER
                - FROZEN_IS_COMPLETE
                - DOOR_IS_OPEN
                - TIME_TO_CHANGE_FILTER
            property:
              doorStatus:
                - doorState:
                    mode:
                      - r
                    type: enum
                    value:
                      r:
                        - OPEN
                        - CLOSE
                  locationName: MAIN
              refrigeration:
                expressMode:
                  mode:
                    - r
                    - w
                  type: boolean
                  value:
                    r:
                      - true
                      - false
                    w:
                      - true
                      - false
              temperature:
                - locationName: FRIDGE
                  targetTemperature:
                    mode:
                      - r
                      - w
                    type: range
                    value:
                      r:
                        max: 6
                        min: 0
                        step: 1
                      w:
                        max: 6
                        min: 0
                        step: 1
                  unit: C
                - locationName: FREEZER
                  targetTemperature:
                    mode:
                      - r
                      - w
                    type: range
                    value:
                      r:
                        max: -16
                        min: -24
                        step: 1
                      w:
                        max: -16
                        min: -24
                        step: 1
                  unit: C
              waterFilterInfo:
                usedTime:
                  mode:
                    - r
                  type: number
    x-tagGroups:
      - name: Get Started
        tags:
          - overview
      - name: Category A
        tags:
          - refrigerator-profile
          - washer-profile
---

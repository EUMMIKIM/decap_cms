---
contents:
  lang: yaml
  code: >
    openapi: 3.1.0

    info:
      version: null
      title: 디바이스 프로파일asfasdfadfasfasdf
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
          
          <div class="box box--r-1.25" markdown="block">
              <div class="in-cont" markdown="block">
                <section class="divide mt0" markdown="block">
                  <h3 class="h3 lg">디바이스 상태 조회</h3>

                  <p class="t3">사용자의 디바이스 상태를 조회하는 방법을 설명합니다.</p>

                  <div class="divide-txt" markdown="block">
                    <h4 class="h4 lg">APIs to Be Used</h4>
                    <ul class="bu-dot lg">
                      <li>
                        <Link href="" class="c-heritage link">
                          GET /devices
                        </Link>
                      </li>
                      <li>
                        <Link href="" class="c-heritage link">
                          GET /devices/&#123;device-id&#125;
                        </Link>
                      </li>
                    </ul>
                  </div>

                  <div class="divide-txt">
                    <h4 class="h4 lg">Sequence</h4>
                    <ol class="bu-counter lg">
                      <li>
                        여러분의 서비스는 디바이스 목록 조회 API (<code>GET/device</code>)를 이용하여, LG ThinQ 플랫폼에
                        등록된 사용자의 디바이스 목록을 가져와야 합니다. <br />이 과정은 처음 한 번만 수행하면 되고, 목록을
                        한 번 가져온 후에는 매번 수행할 필요는 없습니다.
                      </li>
                      <li>
                        디바이스 목록에서 상태를 조회할 디바이스의 <code>device-id</code> 값을 확인하고, 이 값을 이용하여
                        디바이스 상태 조회 API (<code>GET/devices/&#123;device - id&#125;</code>)를 호출합니다.
                      </li>
                    </ol>
                    <div class="img-w">
                      <img
                        src="/assets/images/sequence-diagram_1_monitor.png"
                        width={538}
                        height={395}
                        alt="디바이스 상태 조회 시퀀스 다이어그램"
                      />
                    </div>
                  </div>
                </section>

                <section class="divide">
                  <h3 class="h3 lg">디바이스 제어</h3>
                  <p class="t3">사용자의 디바이스를 제어하는 방법을 설명합니다.</p>

                  <div class="divide-txt">
                    <h4 class="h4 lg">APIs to Be Used</h4>
                    <ul class="bu-dot lg">
                      <li>
                        <Link href="" class="c-heritage link">
                          GET /devices
                        </Link>
                      </li>
                      <li>
                        <Link href="" class="c-heritage link">
                          GET /devices/profile/&#123;device-id&#125;
                        </Link>
                      </li>
                      <li>
                        <Link href="" class="c-heritage link">
                          POST /devices/&#123;device-id&#125;
                        </Link>
                      </li>
                    </ul>
                  </div>

                  <div class="divide-txt">
                    <h4 class="h4 lg">Sequence</h4>
                    <ol class="bu-counter lg">
                      <li>
                        여러분의 서비스는 디바이스 목록 조회 API (<code>GET/devices</code>)를 이용하여, LG ThinQ 플랫폼에
                        등록된 사용자의 디바이스 목록을 가져와야 합니다.
                        <br />이 과정은 처음 한 번만 수행하면 되고, 목록을 한 번 가져온 후에는 매번 수행할 필요는 없습니다.
                      </li>
                      <li>
                        디바이스 목록에서 제어 대상 디바이스의&nbsp;<code>device-id</code>&nbsp;값을&nbsp;확인하고, 이 값을
                        이용하여 디바이스 프로파일&nbsp;조회 API (<code>GET/devices/profile/&#123;device - id&#125;</code>
                        )를 호출합니다.
                      </li>
                      <li>
                        API 호출 응답으로 받은 디바이스 프로파일을 바탕으로 해당 디바이스에 대한 제어 명령을 생성합니다.
                        제어 명령은 디바이스 프로파일에서 제어를 원하는 속성을 찾아 name 과 value 쌍으로 표현합니다.
                      </li>
                      <li>
                        <code> device-id </code>와 제어 명령을 이용하여, 디바이스 제어 API (
                        <code>POST/devices/&#123;device - id&#125;</code>)를 호출합니다.
                      </li>
                      <li>API 응답으로 디바이스 제어 결과를 반환 받습니다. </li>
                    </ol>
                    <div class="img-w">
                      <img
                        src="/assets/images/sequence-diagram_2_control.png"
                        width={544}
                        height={585}
                        alt="디바이스 제어 시퀀스 다이어그램"
                      />
                    </div>
                  </div>
                </section>

                <section class="divide">
                  <h3 class="h3 lg">디바이스의 이벤트 구독 </h3>
                  <div class="TED_alertBox">Event API를 사용하기 위해서는&nbsp;LG전자 담당자와 사전 협의가 필요합니다.</div>
                  <p class="t3">
                    디바이스로부터 이벤트 메시지를 구독하는 방법을 설명합니다. 이벤트 메시지는 디바이스의 상태가 변경되는
                    경우 발생하며, 디바이스의 변경된 상태 정보를 담고 있습니다. <br /> 이벤트 메시지에 대한 상세 설명은
                    Event API &gt; 이벤트 메시지 정의를 참고하십시오.
                  </p>

                  <div class="divide-txt">
                    <h4 class="h4 lg">APIs to Be Used</h4>
                    <ul class="bu-dot lg">
                      <li>
                        <Link href="" class="c-heritage link">
                          GET /devices
                        </Link>
                      </li>
                      <li>
                        <Link href="" class="c-heritage link">
                          POST /event/&#123;device-id&#125;
                        </Link>
                      </li>
                    </ul>
                  </div>

                  <div class="divide-txt">
                    <h4 class="h4 lg">Sequence</h4>
                    <ol class="bu-counter lg">
                      <li>
                        여러분의 서비스는 디바이스 목록 조회 API (<code>GET/device</code>)를 이용하여, LG ThinQ 플랫폼에
                        등록된 사용자의 디바이스 목록을 가져와야 합니다. <br /> 이 과정은 처음 한 번만 수행하면 되고, 목록을
                        한 번 가져온 후에는 매번 수행할 필요는 없습니다.
                      </li>
                      <li>
                        디바이스 목록에서 이벤트를 구독할 디바이스에 대한 <code>device-id</code> 값을 확인하고, 이 값을
                        이용해 이벤트 등록 API (<code> POST/event/&#123;device - id&#125;</code>)를 호출합니다.
                      </li>
                      <li>API 응답으로 이벤트 구독 성공/실패에 대한 결과를 반환 받습니다.</li>
                      <li>디바이스의 상태가 변경되었을 때, 이벤트 메시지를 전달 받습니다.</li>
                    </ol>
                    <div class="img-w">
                      <img
                        src="/assets/images/sequence-diagram_3.png"
                        width={753}
                        height={597}
                        alt="디바이스 제어 시퀀스 다이어그램"
                      />
                    </div>
                  </div>
                </section>

                <section class="divide">
                  <h3 class="h3 lg">디바이스의 푸쉬 알림 구독 </h3>
                  <p class="t3">디바이스의 푸쉬 알림을 구독하는 방법을 설명합니다.</p>

                  <div class="divide-txt">
                    <h4 class="h4 lg">APIs to Be Used</h4>
                    <ul class="bu-dot lg">
                      <li>
                        <Link href="" class="c-heritage link">
                          GET /devices
                        </Link>
                      </li>
                      <li>
                        <Link href="" class="c-heritage link">
                          POST /push/&#123;device-id&#125;
                        </Link>
                      </li>
                    </ul>
                  </div>

                  <div class="divide-txt">
                    <h4 class="h4 lg">Sequence</h4>
                    <ol class="bu-counter lg">
                      <li>
                        여러분의 서비스는 디바이스 목록 조회 API (<code>GET/device</code>)를 이용하여, LG ThinQ 플랫폼에
                        등록된 사용자의 디바이스 목록을 가져와야 합니다. 이 과정은 처음 한 번만 수행하면 되고, 목록을 한 번
                        가져온 후에는 매번 수행할 필요는 없습니다.
                      </li>
                      <li>
                        디바이스 목록에서 푸쉬 알림을 받을 디바이스의 <code>device-id</code> 값을 확인하고, 이 값를 이용하여
                        디바이스 알림 구독 API (<code>POST/push/&#123;device - id&#125;</code>)를 호출합니다.
                      </li>
                      <li>API 응답으로 구독 성공/실패에 대한 결과를 반환 받습니다. </li>
                      <li>
                        디바이스로부터 푸쉬 알림이 발생하였을 때, 등록한 callback URL로 푸쉬 메시지를 전달 받습니다.
                        사용자에게 전달해야 하는 메시지가 있다면 적절히 처리합니다.
                      </li>
                    </ol>

                    <div class="TED_noteBox">
                      푸쉬 알림에는 디바이스의 동작 완료 (예: 세탁기-세탁 완료) 혹은 부품 교체 (공기청정기-필터 교체) 등
                      사용자에게 전달해야 하는 메시지가 포함되어 있습니다. 이 메시지는{' '}
                      <Link href="" class="c-heritage link">
                        Common Data Type &gt; Push Type &gt; DEVICE_PUSH
                      </Link>
                      에 해당되며, 각 가전 유형 별로 사용되는 메시지 내용은{' '}
                      <Link href="" class="c-heritage link">
                        Device Profile &gt; Notification (Push Message)
                      </Link>
                      에 정의되어 있습니다.
                    </div>

                    <div class="img-w">
                      <img
                        src="/assets/images/sequence-diagram_4.png"
                        width={753}
                        height={553}
                        alt="디바이스 푸시 알림 시퀀스 다이어그램"
                      />
                    </div>
                  </div>
                </section>
              </div>
            </div>


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

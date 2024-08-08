---
contents:
  lang: yaml
  code: >
    openapi: 3.1.0

    info:
      version: v1.0
      title: Business Connect API
    servers:
      - url: https://ap.api.lge.com
      - url: https://eu.api.lge.com
      - url: https://us.api.lge.com
      - url: https://ap-test.api.lge.com
      - url: https://eu-test.api.lge.com
      - url: https://us-test.api.lge.com
    tags:
      - name: Base URLs
        x-traitTag: true
        description: |
          Lorem **ipsum** dolor sit amet, consectetur adipiscing elit. Integer dui ligula, sodales vel elit a, dignissim congue felis. Duis tempus vulputate erat.  Vestibulum quis feugiat mi. Fusce ut auctor odio. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Sed id nulla orci.  Curabitur luctus tincidunt nibh id pretium. Fusce tempus sem eu arcu viverra tincidunt. Nullam dignissim bibendum lacinia.  Cras vestibulum augue vitae placerat finibus. Duis tristique magna elit, ac placerat lorem viverra in. In non felis non ante porttitor pharetra non ut nisi.  Suspendisse at nisi tincidunt massa suscipit imperdiet eu et felis. Aliquam sodales enim ut congue mollis. Duis tristique nulla.
      - name: API Token
        description: |
          Lorem ipsum dolor sit amet, consectetur adipiscing eit. Integer dui ligula, sodales vel elit a, dignissim congue felis. Duis tempus vulputate erat.  Vestibulum quis feugiat mi. Fusce ut auctor odio. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Sed id nulla orci.  Curabitur luctus tincidunt nibh id pretium. Fusce tempus sem eu arcu viverra tincidunt. Nullam dignissim bibendum lacinia.  Cras vestibulum augue vitae placerat finibus. Duis tristique magna elit, ac placerat lorem viverra in. In non felis non ante porttitor pharetra non ut nisi.  Suspendisse at nisi tincidunt massa suscipit imperdiet eu et felis. Aliquam sodales enim ut congue mollis. Duis tristique nulla.
      - name: LG Device
        x-displayName: Device API
        description: |
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer dui ligula, sodales vel elit a, dignissim congue felis. Duis tempus vulputate erat.  Vestibulum quis feugiat mi. Fusce ut auctor odio. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Sed id nulla orci.  Curabitur luctus tincidunt nibh id pretium. Fusce tempus sem eu arcu viverra tincidunt. Nullam dignissim bibendum lacinia.  Cras vestibulum augue vitae placerat finibus. Duis tristique magna elit, ac placerat lorem viverra in. In non felis non ante porttitor pharetra non ut nisi.  Suspendisse at nisi tincidunt massa suscipit imperdiet eu et felis. Aliquam sodales enim ut congue mollis. Duis vitae nulla.
      - name: LG Device Push
        x-displayName: Event API
        description: |
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer dui ligula, sodales vel elit a, dignissim congue felis. Duis tempus vulputate erat.  Vestibulum quis feugiat mi. Fusce ut auctor odio. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Sed id nulla orci.  Curabitur luctus tincidunt nibh id pretium. Fusce tempus sem eu arcu viverra tincidunt. Nullam dignissim bibendum lacinia.  Cras vestibulum augue vitae placerat finibus. Duis tristique magna elit, ac placerat lorem viverra in. In non felis non ante porttitor pharetra non ut nisi.  Suspendisse at nisi tincidunt massa suscipit imperdiet eu et felis. Aliquam sodales enim ut congue mollis. Duis vitae nulla.
      - name: ThinQ Device
        x-displayName: Device API
        description: |
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer dui ligula, sodales vel elit a, dignissim congue felis. Duis tempus vulputate erat.  Vestibulum quis feugiat mi. Fusce ut auctor odio. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Sed id nulla orci.  Curabitur luctus tincidunt nibh id pretium. Fusce tempus sem eu arcu viverra tincidunt. Nullam dignissim bibendum lacinia.  Cras vestibulum augue vitae placerat finibus. Duis tristique magna elit, ac placerat lorem viverra in. In non felis non ante porttitor pharetra non ut nisi.  Suspendisse at nisi tincidunt massa imperdiet eus et felis. Aliquam sodales enim ut congue mollis. Duis vitae nulla.
      - name: ThinQ Device Push
        x-displayName: Event API
      - name: ThinQ User
        x-displayName: User API
      - name: DR
        x-displayName: DR API
        description: |
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer dui ligula, sodales vel elit a, dignissim congue felis. Duis tempus vulputate erat.  Vestibulum quis feugiat mi. Fusce ut auctor odio. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Sed id nulla orci.  Curabitur luctus tincidunt nibh id pretium. Fusce tempus sem eu arcu viverra tincidunt. Nullam dignissim bibendum lacinia.  Cras vestibulum augue vitae placerat finibus. Dusis tristique magna elit, ac placerat lorem viverra in. In non felis non ante porttitor pharetra non ut nisi.  Suspendisse at nisi tincidunt massa imperdiet eus et felis. Alidquam sodales enim ut congue mollis. Duis vitae nulla.
    paths:
      /token:
        post:
          tags:
            - API Token
          summary: Create a new API Token
          description: |
            This operation creates a new `API Token` which is required for every calls of Business Connect API. This API Token is valid for **24 hours** so that your client have to be ready to call this API if needed. `API Secret` header is required for this operation.
          operationId: createAPIToken
          security:
            - Business_Connect_API_Key: []
          parameters:
            - name: X-Api-Secret
              in: header
              required: true
              schema:
                type: string
              description: |
                `API Secret` is a pair with `API Key`, which was obtained from **My Page** when the paired API Key was initially created.
              example: 5f3c0222-cb51-4ea8-bae9-60879e8760bb
          responses:
            '200':
              description: Successfully processed the request
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/api-token-res'
            '400':
              description: Bad request
            '401':
              description: Unauthorized
      /v1.0/devices:
        get:
          tags:
            - LG Device
          summary: List devices
          description: List all registered devices on your accounts.
          operationId: getLGDevices
          security:
            - Business_Connect_API_Key: []
          parameters:
            - required: true
              $ref: '#/components/parameters/X-Api-Token'
            - required: true
              $ref: '#/components/parameters/X-Message-Id'
            - $ref: '#/components/parameters/X-Country-Code'
          responses:
            '200':
              description: Successfully processed the request
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/device-list-res'
            '400':
              description: Bad request
            '401':
              description: Unauthorized
      /v1.0/devices/profile/{deviceId}:
        get:
          tags:
            - LG Device
          summary: Get Profile of the device
          description: Get Device Profile of the device.
          operationId: getProfileOfLGDevice
          security:
            - Business_Connect_API_Key: []
          parameters:
            - required: true
              $ref: '#/components/parameters/X-Api-Token'
            - required: true
              $ref: '#/components/parameters/X-Message-Id'
            - required: true
              $ref: '#/components/parameters/deviceId'
          responses:
            '200':
              description: Successfully processed the request
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/thinq-device-profile-res'
            '400':
              description: Bad request
            '401':
              description: Unauthorized
      /v1.0/devices/{deviceId}:
        get:
          tags:
            - LG Device
          summary: Get status of the device
          description: Get status of the device.
          operationId: getStatusOfLGDevice
          security:
            - Business_Connect_API_Key: []
          parameters:
            - required: true
              $ref: '#/components/parameters/X-Api-Token'
            - required: true
              $ref: '#/components/parameters/X-Message-Id'
            - required: true
              $ref: '#/components/parameters/deviceId'
          responses:
            '200':
              description: Successfully processed the request
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/thinq-device-status-res'
                  examples:
                    냉장고:
                      $ref: '#/components/examples/refrigerator-object-example-res-1'
            '400':
              description: Bad request
            '401':
              description: Unauthorized
        post:
          tags:
            - LG Device
          summary: Control the device
          description: Control the device.
          operationId: controlLGDevice
          security:
            - Business_Connect_API_Key: []
          parameters:
            - required: true
              $ref: '#/components/parameters/X-Api-Token'
            - required: true
              $ref: '#/components/parameters/X-Message-Id'
            - required: true
              $ref: '#/components/parameters/deviceId'
          requestBody:
            content:
              application/json:
                schema:
                  type: object
                  description: 디바이스 유형별 제어 요청 메시지의 스키마는 [**디바이스 프로파일**](http://www.daum.net) 페이지를 참조해주세요.
                examples:
                  냉장고 - 냉장실 온도를 섭씨 0도로 설정:
                    $ref: '#/components/examples/refrigerator-object-example-req-1'
                  냉장고 - 절전 모드 설정:
                    $ref: '#/components/examples/refrigerator-object-example-req-2'
          responses:
            '200':
              description: Successfully processed the request
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/thinq-device-status-res'
                  examples:
                    냉장고:
                      $ref: '#/components/examples/refrigerator-object-example-res-1'
            '400':
              description: Bad request
            '401':
              description: Unauthorized
      /v1.0/push:
        get:
          tags:
            - LG Device Push
          summary: List IDs of subscribed ThinQ Devices
          description: Get the list of IDs of ThinQ Devices whose push messages has been subscribed
          operationId: listDevicesSubscribed
          security:
            - Business_Connect_API_Key: []
          parameters:
            - required: true
              $ref: '#/components/parameters/X-Api-Token'
            - required: true
              $ref: '#/components/parameters/X-Message-Id'
          responses:
            '200':
              description: Successfully processed the request
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/thinq-device-list-res'
            '400':
              description: Bad request
            '401':
              description: Unauthorized
      /v1.0/push/{deviceId}:
        post:
          tags:
            - LG Device Push
          summary: Subscribe push messages
          description: Subscribe push messages of the ThinQ Device
          operationId: subscribePushMessages
          security:
            - Business_Connect_API_Key: []
          parameters:
            - required: true
              $ref: '#/components/parameters/X-Api-Token'
            - required: true
              $ref: '#/components/parameters/X-Message-Id'
            - required: true
              $ref: '#/components/parameters/deviceId'
          responses:
            '200':
              description: Successfully processed the request
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/device-empty-res'
            '400':
              description: Bad request
            '401':
              description: Unauthorized
        delete:
          tags:
            - LG Device Push
          summary: Unsubscribe push messages
          description: Unsubscribe push messages of the LG Device
          operationId: unsubscribePushMessages
          security:
            - Business_Connect_API_Key: []
          parameters:
            - required: true
              $ref: '#/components/parameters/X-Api-Token'
            - required: true
              $ref: '#/components/parameters/X-Message-Id'
            - required: true
              $ref: '#/components/parameters/deviceId'
          responses:
            '200':
              description: Successfully processed the request
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/device-empty-res'
            '400':
              description: Bad request
            '401':
              description: Unauthorized
      /thinq/v1.0/devices:
        get:
          tags:
            - ThinQ Device
          summary: List devices
          description: List all registered devices on your accounts.
          operationId: getThinQDevices
          security:
            - Business_Connect_API_Key: []
          parameters:
            - required: true
              $ref: '#/components/parameters/X-Api-Token'
            - required: true
              $ref: '#/components/parameters/Authorization'
            - required: true
              $ref: '#/components/parameters/X-Message-Id'
            - required: true
              $ref: '#/components/parameters/X-Country-Code'
          responses:
            '200':
              description: Successfully processed the request
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/device-list-res'
            '400':
              description: Bad request
            '401':
              description: Unauthorized
      /thinq/v1.0/devices/profile/{deviceId}:
        get:
          tags:
            - ThinQ Device
          summary: Get Profile of the device
          description: Get Device Profile of the device.
          operationId: getProfileOfThinQDevice
          security:
            - Business_Connect_API_Key: []
          parameters:
            - required: true
              $ref: '#/components/parameters/X-Api-Token'
            - required: true
              $ref: '#/components/parameters/Authorization'
            - required: true
              $ref: '#/components/parameters/X-Message-Id'
            - required: true
              $ref: '#/components/parameters/X-Country-Code'
            - required: true
              $ref: '#/components/parameters/deviceId'
          responses:
            '200':
              description: Successfully processed the request
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/thinq-device-profile-res'
            '400':
              description: Bad request
            '401':
              description: Unauthorized
      /thinq/v1.0/devices/{deviceId}:
        get:
          tags:
            - ThinQ Device
          summary: Get status of the device
          description: Get status of the device.
          operationId: getStatusOfThinQDevice
          security:
            - Business_Connect_API_Key: []
          parameters:
            - required: true
              $ref: '#/components/parameters/X-Api-Token'
            - required: true
              $ref: '#/components/parameters/Authorization'
            - required: true
              $ref: '#/components/parameters/X-Message-Id'
            - required: true
              $ref: '#/components/parameters/X-Country-Code'
            - required: true
              $ref: '#/components/parameters/deviceId'
          responses:
            '200':
              description: Successfully processed the request
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/thinq-device-status-res'
                examples:
                  냉장고:
                    $ref: ../../../device-schema/refrigerator/refrigerator-object-example-res-1.yaml
            '400':
              description: Bad request
            '401':
              description: Unauthorized
        post:
          tags:
            - ThinQ Device
          summary: Control the device
          description: Control the device.
          operationId: controlThinQDevice
          security:
            - Business_Connect_API_Key: []
          parameters:
            - required: true
              $ref: '#/components/parameters/X-Api-Token'
            - required: true
              $ref: '#/components/parameters/Authorization'
            - required: true
              $ref: '#/components/parameters/X-Message-Id'
            - required: true
              $ref: '#/components/parameters/X-Country-Code'
            - $ref: '#/components/parameters/X-Conditional-Control'
            - required: true
              $ref: '#/components/parameters/deviceId'
          requestBody:
            content:
              application/json:
                schema:
                  type: object
                  description: 디바이스 유형별 제어 요청 메시지의 스키마는 [**디바이스 프로파일**](http://www.daum.net) 페이지를 참조해주세요.
                examples:
                  냉장고 - 냉장실 온도를 섭씨 0도로 설정:
                    $ref: '#/components/examples/refrigerator-object-example-req-1'
                  냉장고 - 절전 모드 설정:
                    $ref: '#/components/examples/refrigerator-object-example-req-2'
          responses:
            '200':
              description: Successfully processed the request
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/thinq-device-status-res'
                  examples:
                    냉장고:
                      $ref: '#/components/examples/refrigerator-object-example-res-1'
            '400':
              description: Bad request
            '401':
              description: Unauthorized
      /thinq/v1.0/push:
        get:
          tags:
            - ThinQ Device Push
          summary: List IDs of subscribed ThinQ Devices
          description: Get the list of IDs of ThinQ Devices whose push messages has been subscribed
          operationId: listThinQDevicesSubscribed
          security:
            - Business_Connect_API_Key: []
          parameters:
            - required: true
              $ref: '#/components/parameters/X-Api-Token'
            - required: true
              $ref: '#/components/parameters/Authorization'
            - required: true
              $ref: '#/components/parameters/X-Message-Id'
            - required: true
              $ref: '#/components/parameters/X-Country-Code'
          responses:
            '200':
              description: Successfully processed the request
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/thinq-device-list-res'
            '400':
              description: Bad request
            '401':
              description: Unauthorized
      /thinq/v1.0/push/{deviceId}:
        post:
          tags:
            - ThinQ Device Push
          summary: Subscribe push messages
          description: Subscribe push messages of the ThinQ Device
          operationId: subscribePushMessagesThinQ
          security:
            - Business_Connect_API_Key: []
          parameters:
            - required: true
              $ref: '#/components/parameters/X-Api-Token'
            - required: true
              $ref: '#/components/parameters/Authorization'
            - required: true
              $ref: '#/components/parameters/X-Message-Id'
            - required: true
              $ref: '#/components/parameters/X-Country-Code'
            - required: true
              $ref: '#/components/parameters/deviceId'
          responses:
            '200':
              description: Successfully processed the request
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/device-empty-res'
            '400':
              description: Bad request
            '401':
              description: Unauthorized
        delete:
          tags:
            - ThinQ Device Push
          summary: Unsubscribe push messages
          description: Unsubscribe push messages of the ThinQ Device
          operationId: unsubscribePushMessagesThinQ
          security:
            - Business_Connect_API_Key: []
          parameters:
            - required: true
              $ref: '#/components/parameters/X-Api-Token'
            - required: true
              $ref: '#/components/parameters/Authorization'
            - required: true
              $ref: '#/components/parameters/X-Message-Id'
            - required: true
              $ref: '#/components/parameters/X-Country-Code'
            - required: true
              $ref: '#/components/parameters/deviceId'
          responses:
            '200':
              description: Successfully processed the request
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/device-empty-res'
            '400':
              description: Bad request
            '401':
              description: Unauthorized
      /thinq/v1.0/users:
        get:
          tags:
            - ThinQ User
          summary: Get User Number
          description: Get User Number of the user
          operationId: getUserNumber
          security:
            - Business_Connect_API_Key: []
          parameters:
            - required: true
              $ref: '#/components/parameters/X-Api-Token'
            - required: true
              $ref: '#/components/parameters/Authorization'
            - required: true
              $ref: '#/components/parameters/X-Message-Id'
            - required: true
              $ref: '#/components/parameters/X-Country-Code'
          responses:
            '200':
              description: Successfully processed the request
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/device-empty-res'
            '400':
              description: Bad request
            '401':
              description: Unauthorized
      /thinq/v1.0/user/service:
        delete:
          tags:
            - ThinQ User
          summary: Disconnect the user
          description: Deactivate the user and Disconnect the user from your service
          operationId: disconnectService
          security:
            - Business_Connect_API_Key: []
          parameters:
            - required: true
              $ref: '#/components/parameters/X-Api-Token'
            - required: true
              $ref: '#/components/parameters/Authorization'
            - required: true
              $ref: '#/components/parameters/X-Message-Id'
            - required: true
              $ref: '#/components/parameters/X-Country-Code'
          responses:
            '200':
              description: Successfully processed the request
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/device-empty-res'
            '400':
              description: Bad request
            '401':
              description: Unauthorized
      /dr/v1.0/groups:
        post:
          tags:
            - DR
            - DR Group
          summary: Create & Update a Group
          description: A POST request to create a new group
          parameters:
            - in: header
              name: X-Api-Key
              description: API Key provided to the client in advance
              required: true
              schema:
                type: string
                example: x-api-key
            - in: header
              name: X-Api-Token
              description: API Token obtained by the client within last 24 hours
              required: true
              schema:
                type: string
                example: x-api-token
            - in: header
              name: X-Message-Id
              description: an ID generated by the client for request tracking  (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
                example: x-message-id
            - in: header
              name: X-Country-Code
              description: country code
              required: true
              schema:
                type: string
                example: us
          requestBody:
            description: Group Infomation
            content:
              application/json:
                schema:
                  type: object
                  properties:
                    groupId:
                      type: string
                      example: k-apt-1168011000
                      description: Group ID
                    groupName:
                      type: string
                      example: 압구정 현대 1차 아파트
                      description: Group name
                    partnerId:
                      type: string
                      example: kepco-herit-lge
                      description: 그룹을 생성한 전력사/통합사 ID
                    areaCode:
                      type: string
                      example: '1168011000'
                      description: |-
                        - 법정동 코드(국토교통부)
                        - 10자리번호
                    buildingCode:
                      type: string
                      example: '1168011000103690001004767'
                      description: |-
                        - 도로명주소건물관리번호(공간정보 오픈플랫폼)
                        - 25자리번호
                        - 법정동코드(10) + 산여부(1) + 지번본번(4) + 지번부번(4) + 시스템번호(6)
                    comment:
                      type: string
                      example: 압구정 현대 1차 아파트
                      description: description of the resource
                  required:
                    - groupId
                    - partnerId
          responses:
            '200':
              description: '`2000`: Successful operation'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
              content:
                application/json:
                  schema:
                    type: object
                    properties:
                      code:
                        type: integer
                        example: 2000
                      data:
                        type: object
                        properties:
                          groupId:
                            type: string
                            example: k-apt-1168011000
                            description: Group ID
            '400':
              description: |-
                `4000`: Bad Request<br>
                `4101`: Missing or Invalid Parameter(s) in Request Header<br>
                `4102`: Missing or Invalid Parameter(s) in Request Body<br>
                `4103`: Missing or Invalid Token<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '401':
              description: '`4201`: Invalid or Expired Token<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '403':
              description: '`4202`: Unauthorized request<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '404':
              description: |-
                `4301`: Resource Not Found: User<br>
                `4302`: Resource Not Found: Event<br>
                `4303`: Resource Not Found: Event Target<br>
                `4304`: Resource Not Found: Device<br>
                `4305`: Resource Not Found: Zip File task<br>
                `4306`: Resource Not Found: Zip File<br>
                `4307`: Resource Not Found: Partner<br>
                `4308`: Resource Not Found: Group<br>
                `4313`: Resource Not Found: Partner User<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '405':
              description: Not Allowed
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '409':
              description: |-
                `4309`: Resource Already Exists: User<br>
                `4310`: Resource Already Exists: Event<br>
                `4311`: Resource Already Exists: Event Target<br>
                `4312`: Resource Already Exists: Partner User<br>
                `4314`: Resource Already Exists: Device<br>
                `4315`: Resource Already Exists: Group<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '500':
              description: '`5000`: Internal Server Error<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
      /dr/v1.0/groups/{groupId}:
        get:
          tags:
            - DR
            - DR Group
          summary: Get a Single Group
          description: A GET request to check a group of an aggregator
          parameters:
            - in: header
              name: X-Api-Key
              description: API Key provided to the client in advance
              required: true
              schema:
                type: string
                example: x-api-key
            - in: header
              name: X-Api-Token
              description: API Token obtained by the client within last 24 hours
              required: true
              schema:
                type: string
                example: x-api-token
            - in: header
              name: X-Message-Id
              description: an ID generated by the client for request tracking  (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
                example: x-message-id
            - in: header
              name: X-Country-Code
              description: country code
              required: true
              schema:
                type: string
                example: us
            - in: path
              name: groupId
              description: a specific group ID. It could be group of users or group of end devices.
              required: true
              schema:
                type: string
                example: k-apt-1168011000
          responses:
            '200':
              description: '`2000`: Successful operation'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
              content:
                application/json:
                  schema:
                    type: object
                    properties:
                      code:
                        type: integer
                        example: 2000
                      data:
                        type: object
                        description: contains information of the group
                        properties:
                          groupId:
                            type: string
                            example: k-apt-1168011000
                            description: uniquely identifiable group ID which is registered to LG DR service.
                          groupName:
                            type: string
                            example: 압구정 현대 1차 아파트
                            description: Group name
                          partnerId:
                            type: string
                            example: kepco-herit-lge
                            description: an ID of an aggregator/utility that created the group
                          areaCode:
                            type: string
                            example: '1168011000'
                            description: |-
                              - 법정동 코드(국토교통부)
                              - 10자리번호
                          buildingCode:
                            type: string
                            example: '1168011000103690001004767'
                            description: |-
                              - 도로명주소건물관리번호(공간정보 오픈플랫폼)
                              - 25자리번호
                              - 법정동코드(10) + 산여부(1) + 지번본번(4) + 지번부번(4) + 시스템번호(6)
                          comment:
                            type: string
                            example: 압구정 현대 1차 아파트
                            description: description of the resource
            '400':
              description: |-
                `4000`: Bad Request<br>
                `4101`: Missing or Invalid Parameter(s) in Request Header<br>
                `4102`: Missing or Invalid Parameter(s) in Request Body<br>
                `4103`: Missing or Invalid Token<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '401':
              description: '`4201`: Invalid or Expired Token<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '403':
              description: '`4202`: Unauthorized request<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '404':
              description: |-
                `4301`: Resource Not Found: User<br>
                `4302`: Resource Not Found: Event<br>
                `4303`: Resource Not Found: Event Target<br>
                `4304`: Resource Not Found: Device<br>
                `4305`: Resource Not Found: Zip File task<br>
                `4306`: Resource Not Found: Zip File<br>
                `4307`: Resource Not Found: Partner<br>
                `4308`: Resource Not Found: Group<br>
                `4313`: Resource Not Found: Partner User<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '405':
              description: Not Allowed<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '409':
              description: |-
                `4309`: Resource Already Exists: User<br>
                `4310`: Resource Already Exists: Event<br>
                `4311`: Resource Already Exists: Event Target<br>
                `4312`: Resource Already Exists: Partner User<br>
                `4314`: Resource Already Exists: Device<br>
                `4315`: Resource Already Exists: Group<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '500':
              description: '`5000`: Internal Server Error'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
      /dr/v1.0/partners/{partnerId}/groups:
        get:
          tags:
            - DR
            - DR Group
          summary: Get Groups
          description: A GET request to check groups of aggregator
          parameters:
            - in: header
              name: X-Api-Key
              description: API Key provided to the client in advance
              required: true
              schema:
                type: string
                example: x-api-key
            - in: header
              name: X-Api-Token
              description: API Token obtained by the client within last 24 hours
              required: true
              schema:
                type: string
                example: x-api-token
            - in: header
              name: X-Message-Id
              description: an ID generated by the client for request tracking  (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
                example: x-message-id
            - in: header
              name: X-Country-Code
              description: country code
              required: true
              schema:
                type: string
                example: us
            - in: path
              name: partnerId
              description: an ID of an aggregator/utility that created the group
              required: true
              schema:
                type: string
                example: partnerId
          responses:
            '200':
              description: '`2000`: Successful operation'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
              content:
                application/json:
                  schema:
                    type: object
                    properties:
                      code:
                        type: integer
                        example: 2000
                      data:
                        type: array
                        description: contains list of group
                        items:
                          properties:
                            groupId:
                              type: string
                              example: k-apt-1168011000
                              description: uniquely identifiable group ID which is registered to LG DR service.
                            groupName:
                              type: string
                              example: 압구정 현대 1차 아파트
                              description: Group name
                            partnerId:
                              type: string
                              example: kepco-herit-lge
                              description: an ID of an aggregator/utility that created the group
                            areaCode:
                              type: string
                              example: '1168011000'
                              description: |-
                                - 법정동 코드(국토교통부)
                                - 10자리번호
                            buildingCode:
                              type: string
                              example: '1168011000103690001004767'
                              description: |-
                                - 도로명주소건물관리번호(공간정보 오픈플랫폼)
                                - 25자리번호
                                - 법정동코드(10) + 산여부(1) + 지번본번(4) + 지번부번(4) + 시스템번호(6)
                            comment:
                              type: string
                              example: 압구정 현대 1차 아파트
                              description: description of the resource
                  examples:
                    200 OK:
                      value:
                        code: 2000
                        data:
                          - groupId: k-apt-1168011000
                            groupName: 압구정 현대 1차 아파트
                            partnerId: kepco-herit-lge
                            areaCode: '1168011000'
                            buildingCode: '1168011000103690001004767'
                            comment: 압구정 현대 1차 아파트
                          - groupId: k-apt-1165010700
                            groupName: 반포2동 아크로리버파크
                            partnerId: kepco-herit-lge
                            areaCode: '1165010700'
                            buildingCode: '1165010700100020001017796'
                            comment: 반포2동 아크로리버파크
            '400':
              description: |-
                `4000`: Bad Request<br>
                `4101`: Missing or Invalid Parameter(s) in Request Header<br>
                `4102`: Missing or Invalid Parameter(s) in Request Body<br>
                `4103`: Missing or Invalid Token<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '401':
              description: '`4201`: Invalid or Expired Token<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '403':
              description: '`4202`: Unauthorized request<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '404':
              description: |-
                `4301`: Resource Not Found: User<br>
                `4302`: Resource Not Found: Event<br>
                `4303`: Resource Not Found: Event Target<br>
                `4304`: Resource Not Found: Device<br>
                `4305`: Resource Not Found: Zip File task<br>
                `4306`: Resource Not Found: Zip File<br>
                `4307`: Resource Not Found: Partner<br>
                `4308`: Resource Not Found: Group<br>
                `4313`: Resource Not Found: Partner User<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '405':
              description: Not Allowed
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '409':
              description: |-
                `4309`: Resource Already Exists: User<br>
                `4310`: Resource Already Exists: Event<br>
                `4311`: Resource Already Exists: Event Target<br>
                `4312`: Resource Already Exists: Partner User<br>
                `4314`: Resource Already Exists: Device<br>
                `4315`: Resource Already Exists: Group<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '500':
              description: '`5000`: Internal Server Error'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
      /dr/v1.0/users:
        post:
          tags:
            - DR
            - DR User
          summary: Create User (ThinQ app 공통)
          description: A POST request to create or update a user
          parameters:
            - in: header
              name: X-Api-Key
              description: API Key provided to the client in advance
              required: true
              schema:
                type: string
                example: x-api-key
            - in: header
              name: X-Api-Token
              description: API Token obtained by the client within last 24 hours
              required: true
              schema:
                type: string
                example: x-api-token
            - in: header
              name: X-Message-Id
              description: an ID generated by the client for request tracking  (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
                example: x-message-id
            - in: header
              name: X-Country-Code
              description: country code
              required: true
              schema:
                type: string
                example: us
          requestBody:
            description: User Infomation
            content:
              application/json:
                schema:
                  type: object
                  properties:
                    userNo:
                      type: string
                      example: US1234
                      description: User Number issued by LG EMP service. You can use Get Profile API on EMP service to get user number of the target user. user key 값. 중복 불가
                    mail:
                      type: string
                      example: saldkjb@glkd.com
                      description: |-
                        Uniquely identifiable userNo which should not contain personal information nor provide clue to trace the target user.
                        User id registered to LG EMP service. You can use Get Profile API on EMP service to get user id of the target user.
                    groupId:
                      type: string
                      example: lgeDR-CE-TV
                      description: Uniquely identifiable group ID which is registered to LG DR service.
                    accessToken:
                      type: string
                      example: abcd123456
                      description: access_token issued by LG EMP service
                    refreshToken:
                      type: string
                      example: qaz2wsx345
                      description: refresh_token issued by LG EMP service
                    partner:
                      type: object
                      description: Partner information
                      properties:
                        partnerId:
                          type: string
                          example: abcded
                          description: Partner ID
                        programId:
                          type: string
                          example: abc123
                          description: DR Program Id (OC - mandatory)
                        programName:
                          type: string
                          example: OC DR Program
                          description: DR Program name (OC - mandatory)
                        partnerUserId:
                          type: string
                          example: abc123@lge.com
                          description: Partner system User ID
                        authCodeExt:
                          type: string
                          example: dlksjdfkjqjl
                          description: 3rd party OAuth 2.0 연동을 위한 Auth code
                      required:
                        - partnerId
                        - authCodeExt
                    insertTs:
                      type: number
                      example: 1234566000
                      description: timestamp when this request is made (10 digits). If empty, default timestamp will be added automatically.
                    updateTs:
                      type: number
                      example: 1234566000
                      description: timestamp when this request is updated (10 digits). If empty, default timestamp will be added automatically.
                  required:
                    - userNo
                    - accessToken
                    - refreshToken
                    - partner
          responses:
            '200':
              description: '`2000`: Successful operation'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
              content:
                application/json:
                  schema:
                    type: object
                    properties:
                      code:
                        type: integer
                        example: 2000
                      data:
                        type: object
                        properties:
                          userNo:
                            type: string
                            example: user1234
                            description: encrypted User No.
            '400':
              description: |-
                `4000`: Bad Request<br>
                `4101`: Missing or Invalid Parameter(s) in Request Header<br>
                `4102`: Missing or Invalid Parameter(s) in Request Body<br>
                `4103`: Missing or Invalid Token<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '401':
              description: '`4201`: Invalid or Expired Token<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '403':
              description: '`4202`: Unauthorized request<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '404':
              description: |-
                `4301`: Resource Not Found: User<br>
                `4302`: Resource Not Found: Event<br>
                `4303`: Resource Not Found: Event Target<br>
                `4304`: Resource Not Found: Device<br>
                `4305`: Resource Not Found: Zip File task<br>
                `4306`: Resource Not Found: Zip File<br>
                `4307`: Resource Not Found: Partner<br>
                `4308`: Resource Not Found: Group<br>
                `4313`: Resource Not Found: Partner User<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '405':
              description: Not Allowed<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '409':
              description: |-
                `4309`: Resource Already Exists: User<br>
                `4310`: Resource Already Exists: Event<br>
                `4311`: Resource Already Exists: Event Target<br>
                `4312`: Resource Already Exists: Partner User<br>
                `4314`: Resource Already Exists: Device<br>
                `4315`: Resource Already Exists: Group<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '500':
              description: '`5000`: Internal Server Error<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
      /dr/v1.0/users/{userNo}:
        delete:
          tags:
            - DR
            - DR User
          summary: Delete a User (ThinQ app 공통)
          description: A DELETE request to delete a user
          parameters:
            - in: header
              name: X-Api-Key
              description: API Key provided to the client in advance
              required: true
              schema:
                type: string
                example: x-api-key
            - in: header
              name: X-Api-Token
              description: API Token obtained by the client within last 24 hours
              required: true
              schema:
                type: string
                example: x-api-token
            - in: header
              name: X-Message-Id
              description: an ID generated by the client for request tracking  (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
                example: x-message-id
            - in: header
              name: X-Country-Code
              description: country code
              required: true
              schema:
                type: string
                example: us
            - in: path
              name: userNo
              description: EMP User No.
              required: true
              schema:
                type: string
                example: user1234
          responses:
            '200':
              description: '`2000`: Successful operation'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
              content:
                application/json:
                  schema:
                    type: object
                    properties:
                      code:
                        type: integer
                        example: 2000
                      data:
                        type: object
                        properties:
                          userNo:
                            type: string
                            example: user1234
                            description: deleted User No.
            '400':
              description: |-
                `4000`: Bad Request<br>
                `4101`: Missing or Invalid Parameter(s) in Request Header<br>
                `4102`: Missing or Invalid Parameter(s) in Request Body<br>
                `4103`: Missing or Invalid Token<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '401':
              description: '`4201`: Invalid or Expired Token<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '403':
              description: '`4202`: Unauthorized request<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '404':
              description: |-
                `4301`: Resource Not Found: User<br>
                `4302`: Resource Not Found: Event<br>
                `4303`: Resource Not Found: Event Target<br>
                `4304`: Resource Not Found: Device<br>
                `4305`: Resource Not Found: Zip File task<br>
                `4306`: Resource Not Found: Zip File<br>
                `4307`: Resource Not Found: Partner<br>
                `4308`: Resource Not Found: Group<br>
                `4313`: Resource Not Found: Partner User<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '405':
              description: Not Allowed<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '409':
              description: |-
                `4309`: Resource Already Exists: User<br>
                `4310`: Resource Already Exists: Event<br>
                `4311`: Resource Already Exists: Event Target<br>
                `4312`: Resource Already Exists: Partner User<br>
                `4314`: Resource Already Exists: Device<br>
                `4315`: Resource Already Exists: Group<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '500':
              description: '`5000`: Internal Server Error<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
        get:
          tags:
            - User
          summary: Get a User
          description: A GET request to get user information
          parameters:
            - in: header
              name: X-Api-Key
              description: API Key provided to the client in advance
              required: true
              schema:
                type: string
                example: x-api-key
            - in: header
              name: X-Api-Token
              description: API Token obtained by the client within last 24 hours
              required: true
              schema:
                type: string
                example: x-api-token
            - in: header
              name: X-Message-Id
              description: an ID generated by the client for request tracking  (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
                example: x-message-id
            - in: header
              name: X-Country-Code
              description: country code
              required: true
              schema:
                type: string
                example: us
            - in: path
              name: userNo
              description: EMP User No.
              required: true
              schema:
                type: string
                example: user1234
          responses:
            '200':
              description: '`2000`: Successful operation'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
              content:
                application/json:
                  schema:
                    type: object
                    properties:
                      code:
                        type: integer
                        example: 2000
                      data:
                        type: object
                        properties:
                          userNo:
                            type: string
                            example: user1234
                            description: EMP User No.
                          drHomeId:
                            type: string
                            example: home-01
                            description: ID of the ThinQ Home that the user has selected to be linked to DR Service
                          partnerId:
                            type: string
                            example: ohmconnect-01
                            description: the Partner ID (ID of the DR Service Provider that the user is enrolled to)
                          groupId:
                            type: string
                            example: group-01
                            description: the ID of the group the user is grouped to.
            '400':
              description: |-
                `4000`: Bad Request<br>
                `4101`: Missing or Invalid Parameter(s) in Request Header<br>
                `4102`: Missing or Invalid Parameter(s) in Request Body<br>
                `4103`: Missing or Invalid Token<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '401':
              description: '`4201`: Invalid or Expired Token<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '403':
              description: '`4202`: Unauthorized request<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '404':
              description: |-
                `4301`: Resource Not Found: User<br>
                `4302`: Resource Not Found: Event<br>
                `4303`: Resource Not Found: Event Target<br>
                `4304`: Resource Not Found: Device<br>
                `4305`: Resource Not Found: Zip File task<br>
                `4306`: Resource Not Found: Zip File<br>
                `4307`: Resource Not Found: Partner<br>
                `4308`: Resource Not Found: Group<br>
                `4313`: Resource Not Found: Partner User<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '405':
              description: Not Allowed<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '409':
              description: |-
                `4309`: Resource Already Exists: User<br>
                `4310`: Resource Already Exists: Event<br>
                `4311`: Resource Already Exists: Event Target<br>
                `4312`: Resource Already Exists: Partner User<br>
                `4314`: Resource Already Exists: Device<br>
                `4315`: Resource Already Exists: Group<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '500':
              description: '`5000`: Internal Server Error<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
      /dr/v1.0/users/{userNo}/devices:
        get:
          tags:
            - DR
            - DR Devices
          summary: Get Devices (User Based, ThinQ app 공통)
          description: A Get request to get device list of a user
          parameters:
            - in: header
              name: X-Api-Key
              description: API Key provided to the client in advance
              required: true
              schema:
                type: string
                example: x-api-key
            - in: header
              name: X-Api-Token
              description: API Token obtained by the client within last 24 hours
              required: true
              schema:
                type: string
                example: x-api-token
            - in: header
              name: X-Message-Id
              description: an ID generated by the client for request tracking  (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
                example: x-message-id
            - in: header
              name: X-Country-Code
              description: country code
              required: true
              schema:
                type: string
                example: us
            - in: path
              name: userNo
              description: EMP User No.
              required: true
              schema:
                type: string
                example: KRXXXXXY
          responses:
            '200':
              description: '`2000`: Successful operation'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
              content:
                application/json:
                  schema:
                    type: object
                    properties:
                      code:
                        type: integer
                        example: 2000
                      data:
                        type: array
                        items:
                          properties:
                            deviceId:
                              type: string
                              example: 111e0f0a-8d50-41f8-9120-94a8728c57e4
                              description: Device ThinQ ID
                            macAddress:
                              type: string
                              example: abcd1234
                              description: MAC address of the device (encrypted)
                            groupId:
                              type: string
                              example: k-apt-1168011000
                              description: Group ID
                            userNo:
                              type: string
                              example: KRXXXXXY
                              description: EMP User No.
                            homeId:
                              type: string
                              example: '1'
                              description: ThinQ Home ID (ThinQ Home ID the devie is registered to)
                            drParticipate:
                              type: boolean
                              example: true
                              description: Auto-DR Selected Device or Not (true/false)
                            opt:
                              type: string
                              example: IN
                              description: Currently in DR Event Mode or not ("IN"/"OUT")
                            deviceType:
                              type: string
                              example: DEVICE_AIR_CONDITIONER
                              description: Device type
                            modelName:
                              type: string
                              example: PAC_910604_US
                              description: Model Name of the device
                            alias:
                              type: string
                              example: air conditioner
                              description: alias of the device
                  examples:
                    200 OK:
                      value:
                        code: 2000
                        data:
                          - deviceId: 111e0f0a-8d50-41f8-9120-94a8728c57e4
                            macAddress: abcd1234
                            groupId: k-apt-1168011000
                            userNo: KRXXXXXY
                            homeId: '1'
                            drParticipate: true
                            opt: IN
                            deviceType: DEVIE_AIR_CONDITIONER
                            modelName: PAC_910604_US
                            alias: air conditioner
                          - deviceId: aaaaaaaa-1234-11d3-80ae-044eaf8f70cc
                            macAddress: bcdef2345
                            groupId: k-apt-1165010700
                            userNo: KRXXXXXY
                            homeId: '2'
                            drParticipate: true
                            opt: OUT
                            deviceType: DEVIE_AIR_CONDITIONER
                            modelName: PAC_910604_US
                            alias: air conditioner
            '400':
              description: |-
                `4000`: Bad Request<br>
                `4101`: Missing or Invalid Parameter(s) in Request Header<br>
                `4102`: Missing or Invalid Parameter(s) in Request Body<br>
                `4103`: Missing or Invalid Token<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '401':
              description: '`4201`: Invalid or Expired Token<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '403':
              description: '`4202`: Unauthorized request<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '404':
              description: |-
                `4301`: Resource Not Found: User<br>
                `4302`: Resource Not Found: Event<br>
                `4303`: Resource Not Found: Event Target<br>
                `4304`: Resource Not Found: Device<br>
                `4305`: Resource Not Found: Zip File task<br>
                `4306`: Resource Not Found: Zip File<br>
                `4307`: Resource Not Found: Partner<br>
                `4308`: Resource Not Found: Group<br>
                `4313`: Resource Not Found: Partner User<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '405':
              description: Not Allowed<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '409':
              description: |-
                `4309`: Resource Already Exists: User<br>
                `4310`: Resource Already Exists: Event<br>
                `4311`: Resource Already Exists: Event Target<br>
                `4312`: Resource Already Exists: Partner User<br>
                `4314`: Resource Already Exists: Device<br>
                `4315`: Resource Already Exists: Group<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '500':
              description: '`5000`: Internal Server Error<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
      /dr/v1.0/users/{userNo}/devices/{deviceId}:
        get:
          tags:
            - DR
            - DR Devices
          summary: Get a Single Device (User based)
          description: A GET request to get a single device on user based
          parameters:
            - in: header
              name: X-Api-Key
              description: API Key provided to the client in advance
              required: true
              schema:
                type: string
                example: x-api-key
            - in: header
              name: X-Api-Token
              description: API Token obtained by the client within last 24 hours
              required: true
              schema:
                type: string
                example: x-api-token
            - in: header
              name: X-Message-Id
              description: an ID generated by the client for request tracking  (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
                example: x-message-id
            - in: header
              name: X-Country-Code
              description: country code
              required: true
              schema:
                type: string
                example: us
            - in: path
              name: userNo
              description: EMP User No.
              required: true
              schema:
                type: string
                example: KRXXXXXY
            - in: path
              name: deviceId
              description: Device ThinQ ID
              required: true
              schema:
                type: string
                example: 111e0f0a-8d50-41f8-9120-94a8728c57e4
          responses:
            '200':
              description: '`2000`: Successful operation'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
              content:
                application/json:
                  schema:
                    type: object
                    properties:
                      code:
                        type: integer
                        example: 2000
                      data:
                        type: object
                        properties:
                          deviceId:
                            type: string
                            example: 111e0f0a-8d50-41f8-9120-94a8728c57e4
                            description: Device ThinQ ID
                          macAddress:
                            type: string
                            example: abcd1234
                            description: MAC address of the device (encrypted)
                          groupId:
                            type: string
                            example: k-apt-1168011000
                            description: Group ID
                          userNo:
                            type: string
                            example: KRXXXXXY
                            description: EMP User No.
                          homeId:
                            type: string
                            example: '1'
                            description: ThinQ Home ID (ThinQ Home ID the devie is registered to)
                          drParticipate:
                            type: boolean
                            example: true
                            description: Auto-DR Selected Device or Not (true/false)
                          opt:
                            type: string
                            example: IN
                            description: Currently in DR Event Mode or not ("IN"/"OUT")
                          deviceType:
                            type: string
                            example: DEVICE_AIR_CONDITIONER
                            description: Device type
                          modelName:
                            type: string
                            example: PAC_910604_US
                            description: Model Name of the device
                          alias:
                            type: string
                            example: air conditioner
                            description: alias of the device
            '400':
              description: |-
                `4000`: Bad Request<br>
                `4101`: Missing or Invalid Parameter(s) in Request Header<br>
                `4102`: Missing or Invalid Parameter(s) in Request Body<br>
                `4103`: Missing or Invalid Token<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '401':
              description: '`4201`: Invalid or Expired Token<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '403':
              description: '`4202`: Unauthorized request<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '404':
              description: |-
                `4301`: Resource Not Found: User<br>
                `4302`: Resource Not Found: Event<br>
                `4303`: Resource Not Found: Event Target<br>
                `4304`: Resource Not Found: Device<br>
                `4305`: Resource Not Found: Zip File task<br>
                `4306`: Resource Not Found: Zip File<br>
                `4307`: Resource Not Found: Partner<br>
                `4308`: Resource Not Found: Group<br>
                `4313`: Resource Not Found: Partner User<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '405':
              description: Not Allowed<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '409':
              description: |-
                `4309`: Resource Already Exists: User<br>
                `4310`: Resource Already Exists: Event<br>
                `4311`: Resource Already Exists: Event Target<br>
                `4312`: Resource Already Exists: Partner User<br>
                `4314`: Resource Already Exists: Device<br>
                `4315`: Resource Already Exists: Group<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '500':
              description: '`5000`: Internal Server Error<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
      /dr/v1.0/events:
        post:
          tags:
            - DR
            - DR Event
          summary: Create & update DR Event
          description: A POST request to create a new DR request.
          parameters:
            - in: header
              name: X-Api-Key
              description: API Key provided to the client in advance
              required: true
              schema:
                type: string
                example: x-api-key
            - in: header
              name: X-Api-Token
              description: API Token obtained by the client within last 24 hours
              required: true
              schema:
                type: string
                example: x-api-token
            - in: header
              name: X-Message-Id
              description: an ID generated by the client for request tracking  (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
                example: x-message-id
            - in: header
              name: X-Country-Code
              description: country code
              required: true
              schema:
                type: string
                example: us
          requestBody:
            description: Event information
            content:
              application/json:
                schema:
                  type: object
                  properties:
                    eventId:
                      type: string
                      example: 2023-04-25-dr-01
                      description: ID of an event
                    eventName:
                      type: string
                      example: 2023-04-25-dr-01
                      description: Event Name
                    partnerId:
                      type: string
                      example: herit-01
                      description: Partner ID
                    startTs:
                      type: number
                      example: 1682423399890
                      description: milliseconds
                    endTs:
                      type: number
                      example: 1682426999000
                      description: milliseconds
                    targets:
                      type: array
                      items:
                        properties:
                          targetType:
                            type: string
                            example: GROUP
                            description: type of targets (GROUP, USER, DEVICE)
                          targetIds:
                            type: array
                            items:
                              type: string
                              example: k-apt-1168011000
                            description: list of ids (of GROUP, USER or DEVICE)
                        required:
                          - targetType
                          - targetIds
                    signals:
                      type: array
                      items:
                        properties:
                          signalId:
                            type: string
                            example: 2023-04-25-dr-01-signal-01
                            description: Signal ID
                          deviceType:
                            type: string
                            example: DEVICE_AIR_CONDITIONER
                            description: Device Types of the target for the Signal
                          modelNames:
                            type: array
                            items:
                              type: string
                              example: RAC_056905_WW
                            description: Model Names of the target for the Signal
                          signalType:
                            type: string
                            example: CONTROL_RESTORE
                            description: signal Types (CONTROL, CONTROL_RESTORE)
                          signalData:
                            type: object
                            properties:
                              temperature:
                                type: object
                                properties:
                                  min:
                                    type: number
                                    example: 26
                                  max:
                                    type: number
                                    example: 32
                                  unit:
                                    type: string
                                    example: C
                            description: |-
                              Specific control values for the Signal
                              ex) { temerpature: { min: 26, max: 32, unit: "C } }
                          constrolType:
                            type: string
                            example: Temperature
                            description: Control Method (Temperature, TwoSetTemperature, ToU, Empty)
                          restoreType:
                            type: string
                            example: Temperature
                            description: Restore Method (Temperature, TwoSetTemperature, Empty)
                  required:
                    - eventId
                    - eventName
                    - partnerId
                    - startTs
                    - endTs
                    - targets
                examples:
                  Create & Update Event:
                    value:
                      eventId: 2023-04-25-dr-01
                      eventName: 2023-04-25-dr-01
                      partnerId: herit-01
                      startTs: 1682423399890
                      endTs: 1682426999000
                      targets:
                        - targetType: GROUP
                          targetIds:
                            - k-apt-1168011000
                            - k-apt-1165010700
                        - targetType: USER
                          targetIds:
                            - USXXXXXX
                            - KRXXXXXX
                      signals:
                        - signalId: 2023-04-25-dr-01-signal-01
                          deviceType: DEVICE_AIR_CONDITIONER
                          modelNames:
                            - RAC_056905_WW
                          signalType: CONTROL_RESTORE
                          signalData:
                            temperature:
                              min: 26
                              max: 32
                              unit: C
                          controlType: Temperature
                          restoreType: Temperature
                        - signalId: 2023-04-25-dr-01-signal-02
                          deviceType: DEVICE_TV
                          modelNames: []
                          signalType: CONTROL
                          signalData: {}
                          controlType: Empty
                          restoreType: Empty
          responses:
            '200':
              description: '`2000`: Successful operation'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
              content:
                application/json:
                  schema:
                    type: object
                    properties:
                      code:
                        type: integer
                        example: 2000
                      data:
                        type: object
                        properties:
                          eventId:
                            type: string
                            example: 2023-04-25-dr-01-signal-01
                            description: event ID of DR
            '400':
              description: |-
                `4000`: Bad Request<br>
                `4101`: Missing or Invalid Parameter(s) in Request Header<br>
                `4102`: Missing or Invalid Parameter(s) in Request Body<br>
                `4103`: Missing or Invalid Token<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '401':
              description: '`4201`: Invalid or Expired Token<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '403':
              description: '`4202`: Unauthorized request<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '404':
              description: |-
                `4301`: Resource Not Found: User<br>
                `4302`: Resource Not Found: Event<br>
                `4303`: Resource Not Found: Event Target<br>
                `4304`: Resource Not Found: Device<br>
                `4305`: Resource Not Found: Zip File task<br>
                `4306`: Resource Not Found: Zip File<br>
                `4307`: Resource Not Found: Partner<br>
                `4308`: Resource Not Found: Group<br>
                `4313`: Resource Not Found: Partner User<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '405':
              description: Not Allowed<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '409':
              description: |-
                `4309`: Resource Already Exists: User<br>
                `4310`: Resource Already Exists: Event<br>
                `4311`: Resource Already Exists: Event Target<br>
                `4312`: Resource Already Exists: Partner User<br>
                `4314`: Resource Already Exists: Device<br>
                `4315`: Resource Already Exists: Group<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '500':
              description: '`5000`: Internal Server Error<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
      /dr/v1.0/events/{eventId}/targets:
        post:
          tags:
            - DR
            - DR Event Target
          summary: Create Event Target (POST)
          description: A POST request to create a DR Event Target
          parameters:
            - in: header
              name: X-Api-Key
              description: API Key provided to the client in advance
              required: true
              schema:
                type: string
                example: x-api-key
            - in: header
              name: X-Api-Token
              description: API Token obtained by the client within last 24 hours
              required: true
              schema:
                type: string
                example: x-api-token
            - in: header
              name: X-Message-Id
              description: an ID generated by the client for request tracking  (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
                example: x-message-id
            - in: header
              name: X-Country-Code
              description: country code
              required: true
              schema:
                type: string
                example: us
            - in: path
              name: eventId
              description: Event ID
              required: true
              schema:
                type: string
                example: event-01
          requestBody:
            description: Event information
            content:
              application/json:
                schema:
                  type: object
                  properties:
                    type:
                      type: string
                      example: USER
                      description: |-
                        type of targets
                        - GROUP
                        - USER
                        - DEVICE
                    id:
                      type: string
                      example: KRXXXX
                      description: |-
                        ids of each targets
                        - when type is USER: userNo
                        - when type is GROUP: groupId
                        - when type is DEVICE: deviceId
                    opt:
                      type: string
                      example: OUT
                      description: |-
                        Opt in / out of an event
                        - IN: participate in the DR Event (default)
                        - OUT: Opt out of an Event
                  required:
                    - type
                    - id
                    - opt
          responses:
            '200':
              description: '`2000`: Successful operation'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
              content:
                application/json:
                  schema:
                    type: object
                    properties:
                      code:
                        type: integer
                        example: 2000
                      data:
                        $ref: '#/components/schemas/dr-event-target-opt-res'
            '400':
              description: |-
                `4000`: Bad Request<br>
                `4101`: Missing or Invalid Parameter(s) in Request Header<br>
                `4102`: Missing or Invalid Parameter(s) in Request Body<br>
                `4103`: Missing or Invalid Token<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '401':
              description: '`4201`: Invalid or Expired Token<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '403':
              description: '`4202`: Unauthorized request<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '404':
              description: |-
                `4301`: Resource Not Found: User<br>
                `4302`: Resource Not Found: Event<br>
                `4303`: Resource Not Found: Event Target<br>
                `4304`: Resource Not Found: Device<br>
                `4305`: Resource Not Found: Zip File task<br>
                `4306`: Resource Not Found: Zip File<br>
                `4307`: Resource Not Found: Partner<br>
                `4308`: Resource Not Found: Group<br>
                `4313`: Resource Not Found: Partner User<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '405':
              description: Not Allowed<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '409':
              description: |-
                `4309`: Resource Already Exists: User<br>
                `4310`: Resource Already Exists: Event<br>
                `4311`: Resource Already Exists: Event Target<br>
                `4312`: Resource Already Exists: Partner User<br>
                `4314`: Resource Already Exists: Device<br>
                `4315`: Resource Already Exists: Group<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '500':
              description: '`5000`: Internal Server Error<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
      /dr/v1.0/events/{eventId}/targets/{targetId}:
        post:
          tags:
            - DR
            - DR Event Target
          summary: Update Event Target (POST)
          description: A POST request to update a DR Event Target
          parameters:
            - in: header
              name: X-Api-Key
              description: API Key provided to the client in advance
              required: true
              schema:
                type: string
                example: x-api-key
            - in: header
              name: X-Api-Token
              description: API Token obtained by the client within last 24 hours
              required: true
              schema:
                type: string
                example: x-api-token
            - in: header
              name: X-Message-Id
              description: an ID generated by the client for request tracking  (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
                example: x-message-id
            - in: header
              name: X-Country-Code
              description: country code
              required: true
              schema:
                type: string
                example: us
            - in: path
              name: eventId
              description: Event ID
              required: true
              schema:
                type: string
                example: event-01
            - in: path
              name: targetId
              description: Target ID
              required: true
              schema:
                type: string
                example: KRXXXX
          requestBody:
            description: Event information
            content:
              application/json:
                schema:
                  type: object
                  properties:
                    opt:
                      type: string
                      example: OUT
                      description: |-
                        Opt in / out of an event
                        - IN: participate in the DR Event (default)
                        - OUT: Opt out of an Event
          responses:
            '200':
              description: '`2000`: Successful operation'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
              content:
                application/json:
                  schema:
                    type: object
                    properties:
                      code:
                        type: integer
                        example: 2000
                      data:
                        $ref: '#/components/schemas/dr-event-target-opt-res'
            '400':
              description: |-
                `4000`: Bad Request<br>
                `4101`: Missing or Invalid Parameter(s) in Request Header<br>
                `4102`: Missing or Invalid Parameter(s) in Request Body<br>
                `4103`: Missing or Invalid Token<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '401':
              description: '`4201`: Invalid or Expired Token<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '403':
              description: '`4202`: Unauthorized request<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '404':
              description: |-
                `4301`: Resource Not Found: User<br>
                `4302`: Resource Not Found: Event<br>
                `4303`: Resource Not Found: Event Target<br>
                `4304`: Resource Not Found: Device<br>
                `4305`: Resource Not Found: Zip File task<br>
                `4306`: Resource Not Found: Zip File<br>
                `4307`: Resource Not Found: Partner<br>
                `4308`: Resource Not Found: Group<br>
                `4313`: Resource Not Found: Partner User<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '405':
              description: Not Allowed<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '409':
              description: |-
                `4309`: Resource Already Exists: User<br>
                `4310`: Resource Already Exists: Event<br>
                `4311`: Resource Already Exists: Event Target<br>
                `4312`: Resource Already Exists: Partner User<br>
                `4314`: Resource Already Exists: Device<br>
                `4315`: Resource Already Exists: Group<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '500':
              description: '`5000`: Internal Server Error<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
      /dr/v1.0/events/{eventId}/targets/batch:
        post:
          tags:
            - DR
            - DR Event Target
          summary: Create & Update Event Target in a Batch
          description: A POST request to update Targets for a DR Event
          parameters:
            - in: header
              name: X-Api-Key
              description: API Key provided to the client in advance
              required: true
              schema:
                type: string
                example: x-api-key
            - in: header
              name: X-Api-Token
              description: API Token obtained by the client within last 24 hours
              required: true
              schema:
                type: string
                example: x-api-token
            - in: header
              name: X-Message-Id
              description: an ID generated by the client for request tracking  (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
                example: x-message-id
            - in: header
              name: X-Country-Code
              description: country code
              required: true
              schema:
                type: string
                example: us
            - in: path
              name: eventId
              description: Event ID
              required: true
              schema:
                type: string
                example: event-01
          requestBody:
            description: DR Event를 위한 Target 업데이트 정보
            content:
              application/json:
                schema:
                  type: array
                  items:
                    properties:
                      action:
                        type: string
                        example: create
                        description: |-
                          Action to be performed for each targets (path)
                          - create
                          - replace(or update)
                      path:
                        type: string
                        example: /
                        description: |-
                          ID of each target
                          - create: "/"
                          - update: "/{targetId}"
                          - Ex) creating: "/"
                          - Ex) updating: "/23lk2jl1k32j1"
                      value:
                        type: object
                        example:
                          type: DEVICE
                          id: +2YjaqUmSr3ke2nmdssxAG5n0KDMwrJrvi8bTbzbdHelEEflqNkAx0WWmTIgyCQf
                          opt: IN
                        description: |-
                          Opt in / out of an event
                          - create: {id, type, opt}
                          - update: {opt}
                          - Ex) creating: { type: "DEVICE", id: "23lk2jl1k32j1", opt: "IN"}
                          - Ex) updating: { opt: "IN"}
                    required:
                      - action
                      - path
                      - value
                examples:
                  create:
                    value:
                      - action: create
                        path: /
                        value:
                          type: DEVICE
                          id: +2YjaqUmSr3ke2nmdssxAG5n0KDMwrJrvi8bTbzbdHelEEflqNkAx0WWmTIgyCQf
                          opt: IN
                  update:
                    value:
                      - action: update
                        path: /KRXXX
                        value:
                          opt: OUT
                  create/update:
                    value:
                      - action: create
                        path: /
                        value:
                          type: DEVICE
                          id: +2YjaqUmSr3ke2nmdssxAG5n0KDMwrJrvi8bTbzbdHelEEflqNkAx0WWmTIgyCQf
                          opt: IN
                      - action: update
                        path: /KRXXX
                        value:
                          opt: OUT
                      - action: update
                        path: /KRXXXY
                        value:
                          opt: IN
          responses:
            '200':
              description: '`2000`: Successful operation'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
              content:
                application/json:
                  schema:
                    type: object
                    properties:
                      code:
                        type: integer
                        example: 2000
                      data:
                        type: object
                        properties:
                          action:
                            type: string
                            example: create
                          path:
                            type: string
                            example: /
                          value:
                            type: object
                            example:
                              type: DEVICE
                              id: +2YjaqUmSr3ke2nmdssxAG5n0KDMwrJrvi8bTbzbdHelEEflqNkAx0WWmTIgyCQf
                              opt: IN
                          status:
                            type: string
                            example: success
                  examples:
                    create:
                      value:
                        code: 2000
                        data:
                          - action: create
                            path: /
                            value:
                              type: DEVICE
                              id: +2YjaqUmSr3ke2nmdssxAG5n0KDMwrJrvi8bTbzbdHelEEflqNkAx0WWmTIgyCQf
                              opt: IN
                            status: success
                    update:
                      value:
                        code: 2000
                        data:
                          - action: update
                            path: /KRXXXX
                            value:
                              opt: OUT
                            status: success
                    create/update:
                      value:
                        code: 2000
                        data:
                          - action: create
                            path: /
                            value:
                              type: DEVICE
                              id: +2YjaqUmSr3ke2nmdssxAG5n0KDMwrJrvi8bTbzbdHelEEflqNkAx0WWmTIgyCQf
                              opt: IN
                            status: success
                          - action: update
                            path: /KRXXXX
                            value:
                              opt: OUT
                            status: success
                          - action: update
                            path: /KRXXXY
                            value:
                              opt: IN
                            status: success
            '400':
              description: |-
                `4000`: Bad Request<br>
                `4101`: Missing or Invalid Parameter(s) in Request Header<br>
                `4102`: Missing or Invalid Parameter(s) in Request Body<br>
                `4103`: Missing or Invalid Token<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '401':
              description: '`4201`: Invalid or Expired Token<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '403':
              description: '`4202`: Unauthorized request<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '404':
              description: |-
                `4301`: Resource Not Found: User<br>
                `4302`: Resource Not Found: Event<br>
                `4303`: Resource Not Found: Event Target<br>
                `4304`: Resource Not Found: Device<br>
                `4305`: Resource Not Found: Zip File task<br>
                `4306`: Resource Not Found: Zip File<br>
                `4307`: Resource Not Found: Partner<br>
                `4308`: Resource Not Found: Group<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '405':
              description: Not Allowed<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '409':
              description: |-
                `4309`: Resource Already Exists: User<br>
                `4310`: Resource Already Exists: Event<br>
                `4311`: Resource Already Exists: Event Target<br>
                `4312`: Resource Already Exists: Device<br>
                `4313`: Resource Already Exists: Group<br>
                `4314`: Resource Already Exists: authCodeExt(Already connected 3rd party user)<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '500':
              description: '`5000`: Internal Server Error'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
      /dr/v1.0/data-zip/files:
        post:
          tags:
            - DR
            - DR Data Delivery
          summary: Create a Data Zip File
          description: A POST Request for a creation of a zip file for DR Status data of devices during a DR Event
          parameters:
            - in: header
              name: X-Api-Key
              description: API Key provided to the client in advance
              required: true
              schema:
                type: string
                example: x-api-key
            - in: header
              name: X-Api-Token
              description: API Token obtained by the client within last 24 hours
              required: true
              schema:
                type: string
                example: x-api-token
            - in: header
              name: X-Message-Id
              description: an ID generated by the client for request tracking  (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
                example: x-message-id
            - in: header
              name: X-Country-Code
              description: country code
              required: true
              schema:
                type: string
                example: us
          requestBody:
            description: DR Event & Target Information for zip file creation
            content:
              application/json:
                schema:
                  type: object
                  properties:
                    eventId:
                      type: string
                      example: 2023-04-26-01-dr-event-01
                      description: event Id
                    targets:
                      type: array
                      items:
                        properties:
                          targetId:
                            type: string
                            example: USXXXX
                            description: ID of a target
                          targetType:
                            type: string
                            example: USER
                            description: |-
                              Target Type
                              - USER
                              - DEVICE
                              - GROUP
                  required:
                    - eventId
                examples:
                  Create a Data Zip File:
                    value:
                      eventId: 2023-04-26-01-dr-event-01
                      targets:
                        - targetType: USER
                          targetId: USXXXX
                        - targetType: GROUP
                          targetId: k-apt-1168011000
          responses:
            '200':
              description: '`2000`: Successful operation'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
              content:
                application/json:
                  schema:
                    type: object
                    properties:
                      code:
                        type: integer
                        example: 2000
                      data:
                        type: object
                        properties:
                          fileName:
                            type: string
                            example: data-zip-2023-04-26-01-dr-event-01-1548892800123.zip
                            description: name of the zie file
                  examples:
                    Create a Data Zip File:
                      value:
                        code: 2000
                        data:
                          fileName: data-zip-2023-04-26-01-dr-event-01-1548892800123.zip
            '400':
              description: |-
                `4000`: Bad Request<br>
                `4101`: Missing or Invalid Parameter(s) in Request Header<br>
                `4102`: Missing or Invalid Parameter(s) in Request Body<br>
                `4103`: Missing or Invalid Token<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '401':
              description: '`4201`: Invalid or Expired Token<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '403':
              description: '`4202`: Unauthorized request<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '404':
              description: |-
                `4301`: Resource Not Found: User<br>
                `4302`: Resource Not Found: Event<br>
                `4303`: Resource Not Found: Event Target<br>
                `4304`: Resource Not Found: Device<br>
                `4305`: Resource Not Found: Zip File task<br>
                `4306`: Resource Not Found: Zip File<br>
                `4307`: Resource Not Found: Partner<br>
                `4308`: Resource Not Found: Group<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '405':
              description: Not Allowed<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '409':
              description: |-
                `4309`: Resource Already Exists: User<br>
                `4310`: Resource Already Exists: Event<br>
                `4311`: Resource Already Exists: Event Target<br>
                `4312`: Resource Already Exists: Device<br>
                `4313`: Resource Already Exists: Group<br>
                `4314`: Resource Already Exists: authCodeExt(Already connected 3rd party user)<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '500':
              description: '`5000`: Internal Server Error'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
      /dr/v1.0/data-zip/files/{filename}:
        get:
          tags:
            - DR
            - DR Data Delivery
          summary: Download a Data Zip File
          description: A GET Request to get download link for zip file
          parameters:
            - in: header
              name: X-Api-Key
              description: API Key provided to the client in advance
              required: true
              schema:
                type: string
                example: x-api-key
            - in: header
              name: X-Api-Token
              description: API Token obtained by the client within last 24 hours
              required: true
              schema:
                type: string
                example: x-api-token
            - in: header
              name: X-Message-Id
              description: an ID generated by the client for request tracking  (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
                example: x-message-id
            - in: header
              name: X-Country-Code
              description: country code
              required: true
              schema:
                type: string
                example: us
            - in: path
              name: filename
              description: Name of the zip file (value received as a response from Create a Data zip API)
              required: true
              schema:
                type: string
                example: data-zip-2023-04-26-01-dr-event-01-1548892800123.zip
          responses:
            '200':
              description: '`2000`: Successful operation'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
              content:
                application/json:
                  schema:
                    type: object
                    properties:
                      code:
                        type: integer
                        example: 2000
                      data:
                        type: object
                        properties:
                          presignedUrl:
                            type: string
                            example: https://test-dr.s3.{region}.amazonaws.com/data-zip-2023-04-26-01-dr-event-01-1548892800123?response-content-disposition=inline&X-Amz-Security-Token=IQoJ......
                            description: zip file download url (temporary url)
            '400':
              description: |-
                `4000`: Bad Request<br>
                `4101`: Missing or Invalid Parameter(s) in Request Header<br>
                `4102`: Missing or Invalid Parameter(s) in Request Body<br>
                `4103`: Missing or Invalid Token<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '401':
              description: '`4201`: Invalid or Expired Token<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '403':
              description: '`4202`: Unauthorized request<br>'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '404':
              description: |-
                `4301`: Resource Not Found: User<br>
                `4302`: Resource Not Found: Event<br>
                `4303`: Resource Not Found: Event Target<br>
                `4304`: Resource Not Found: Device<br>
                `4305`: Resource Not Found: Zip File task<br>
                `4306`: Resource Not Found: Zip File<br>
                `4307`: Resource Not Found: Partner<br>
                `4308`: Resource Not Found: Group<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '405':
              description: Not Allowed<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '409':
              description: |-
                `4309`: Resource Already Exists: User<br>
                `4310`: Resource Already Exists: Event<br>
                `4311`: Resource Already Exists: Event Target<br>
                `4312`: Resource Already Exists: Device<br>
                `4313`: Resource Already Exists: Group<br>
                `4314`: Resource Already Exists: authCodeExt(Already connected 3rd party user)<br>
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
            '500':
              description: '`5000`: Internal Server Error'
              headers:
                X-Request-Id:
                  schema:
                    type: string
                  description: an ID generated by API service for request tracking
                X-Response-Code:
                  schema:
                    type: string
                  description: a response code of API service against request
                X-Message-Id:
                  schema:
                    type: string
                  description: The value of the X-Message-Id header sent by the client during API calls
    components:
      securitySchemes:
        Business_Connect_API_Key:
          type: apiKey
          description: |
            `API Key` obtained from **My Page**.
          in: header
          name: X-Api-Key
      schemas:
        api-token-res:
          type: object
          description: Response Object that includes API Token.
          properties:
            access_token:
              type: string
              description: API Token which is represented as JWT(header.payload.signature).
              example: eyJhbGciOiJIUzI1NiIsImtpZCI6InNpbTIifQ.eyJleHAiOjE3MDQzNDE3MDIsImlhdCI6MTcwNDI1NTMwMiwiaXNzIjoiTEcgQnVzaW5lc3MgQ29ubmVjdCIsInJvbGVzIjpbImdldEJlY29uVXNlcnMiLCJnZXREclVzZXJzIiwicG9zdFRva2VuIl0sInN1YiI6IjRkMmM2MWUxLTM0YzQtZTk2Yy05NDU2LTE1YmQ5ODNjNTAxOSJ9.plRjXmZRoXkOy_U95VXGzX-ouJyCrorEmMO8OzrEvF8
        device-base-res:
          type: object
          description: Response Object for any device
          properties:
            messageId:
              type: string
              description: The `X-Message-Id` value included in the header is returned so that it can be checked from the outside upon request.
              example: 2ADaRijIk8CvaSHVPeEWNw
            timestamp:
              type: string
              description: Refers to time when the request is received. Follows the ISO 8601 format.
              example: '2024-10-01T06:23:20.866279'
        lg-device-list-item:
          type: object
          title: LG Device
          properties:
            deviceId:
              type: string
              description: An ID that can identify the device
              example: eb8ce6a99e63beb7e2074409bc244f3fd6c534e40ca270b6895371f12b398660
            deviceInfo:
              type: object
              properties:
                deviceType:
                  type: string
                  description: |
                    Appliance type of the device https://thinq.developer.lge.com/en/cloud/docs/thinq-connect/api-reference/common-data-type//#3
                  example: DEVICE_WASHTOWER_WASHER
                alias:
                  type: string
                  description: An alias of the device
                  example: My New WashTower Washer
                modelName:
                  type: string
                  description: A model name of the device, appeared when the device is one of **ThinQ Devices**
                  example: FAKPK21021
                reportable:
                  type: boolean
                  description: Whether or not your service is subscribing to events which occur when the device status changes, appeared when the device is one of **ThinQ Devices**
                  example: false
                groupId:
                  type: string
                  description: |
                    Group ID, appeared when the `deviceType` is `DEVICE_WASHTOWER_WASHER` or `DEVICE_WASHTOWER_DRYER`
                  example: '171807013576723372'
                parentId:
                  type: string
                  description: Device ID of the parent of this device, appeared when `deviceType` is `IDU`
                  example: fe12ed5bca00acc0ed68ec9f632342d0822a929f377b76cbe700649a11053f23
              required:
                - deviceType
          required:
            - deviceId
            - deviceInfo
        device-list-res:
          description: Object for LG Devices list
          allOf:
            - $ref: '#/components/schemas/device-base-res'
            - type: object
              properties:
                response:
                  type: array
                  items:
                    allOf:
                      - $ref: '#/components/schemas/lg-device-list-item'
        thinq-device-profile-res:
          description: Object for the Device Profile of a ThinQ Device
          allOf:
            - $ref: '#/components/schemas/device-base-res'
            - type: object
              properties:
                response:
                  type: object
                  description: 디바이스 유형별 디바이스 프로파일 메시지의 스키마는 [**디바이스 프로파일**](http://www.naver.com) 페이지를 참조해주세요.
                  example: <<디바이스 프로파일 페이지에서 참조>>
        thinq-device-status-res:
          description: Object for status of ThinQ Devices
          allOf:
            - $ref: '#/components/schemas/device-base-res'
            - type: object
              properties:
                response:
                  type: object
                  description: 디바이스 유형별 디바이스 프로파일 메시지의 스키마는 [**디바이스 프로파일**](http://www.naver.com) 페이지를 참조해주세요.
        thinq-device-list-item:
          type: object
          title: ThinQ Device
          properties:
            deviceId:
              type: string
              description: An ID that can identify the device
              example: eb8ce6a99e63beb7e2074409bc244f3fd6c534e40ca270b6895371f12b398660
            deviceInfo:
              type: object
              properties:
                deviceType:
                  type: string
                  description: |
                    Appliance type of the device https://thinq.developer.lge.com/en/cloud/docs/thinq-connect/api-reference/common-data-type//#3
                  example: DEVICE_WASHTOWER_WASHER
                alias:
                  type: string
                  description: An alias of the device
                  example: My New WashTower Washer
                modelName:
                  type: string
                  description: A model name of the device, appeared when the device is one of **ThinQ Devices**
                  example: FAKPK21021
                reportable:
                  type: boolean
                  description: Whether or not your service is subscribing to events which occur when the device status changes, appeared when the device is one of **ThinQ Devices**
                  example: false
                groupId:
                  type: string
                  description: |
                    Group ID, appeared when the `deviceType` is `DEVICE_WASHTOWER_WASHER` or `DEVICE_WASHTOWER_DRYER`
                  example: '171807013576723372'
              required:
                - deviceType
                - alias
                - modelName
                - reportable
          required:
            - deviceId
            - deviceInfo
        thinq-device-list-res:
          description: Object for ThinQ Devices list
          allOf:
            - $ref: '#/components/schemas/device-base-res'
            - type: object
              properties:
                response:
                  type: array
                  items:
                    allOf:
                      - $ref: '#/components/schemas/thinq-device-list-item'
        device-empty-res:
          description: Object for Empty Response
          allOf:
            - $ref: '#/components/schemas/device-base-res'
            - type: object
              properties:
                response:
                  type: object
        dr-event-target-opt-res:
          type: object
          properties:
            type:
              type: string
              example: USER
              description: |-
                type of targets
                - GROUP
                - USER
                - DEVICE
            id:
              type: string
              example: KRXXXX
              description: |-
                ids of each targets
                - when type is USER: userNo
                - when type is GROUP: groupId
                - when type is DEVICE: deviceId
            opt:
              type: string
              example: OUT
              description: |-
                Opt in / out of an event
                - IN: participate in the DR Event (default)
                - OUT: Opt out of an Event
      parameters:
        X-Api-Token:
          name: X-Api-Token
          in: header
          schema:
            type: string
          description: |
            `API Token` obtained from **[Create a new API Token](/#tag/API-Token/operation/createAPIToken)** API.
          example: eyJleHAiOjE3MDQzNDE3MDIsImlhdCI6MTcwNDI1NTMwMiwiaXNzIjoiTEcgQnVzaW5lc3MgQ29ubmVjdCIsInJvbGVzIjpbImdldEJlY29uVXNlcnMiLCJnZXREclVzZXJzIiwicG9zdFRva2VuIl0sInN1YiI6IjRkMmM2MWUxLTM0YzQtZTk2Yy05NDU2LTE1YmQ5ODNjNTAxOSJ9
        X-Message-Id:
          name: X-Message-Id
          in: header
          schema:
            type: string
          description: |
            A value for tracking the information requested by LG ThinQ Platform that is used to track the flow of a specific API and to find the cause of an error. You must create and enter a new unique value for each API call. How to create: 
              Must be created with the url-safe-base64-no-padding (UUID Version 4) method.
              Up to 22 characters
          example: 2ADaRijIk8CvaSHVPeEWNw
        X-Country-Code:
          name: X-Country-Code
          in: header
          schema:
            type: string
          description: |
            You can specify the target country of your service. Refer to Common Data Type > Contry Code for the available country code. https://thinq.developer.lge.com/en/cloud/docs/thinq-connect/api-reference/common-data-type/#1
          example: KR
        deviceId:
          name: deviceId
          in: path
          schema:
            type: string
          description: an ID that can identify the device
          example: eb8ce6a99e63beb7e2074409bc244f3fd6c534e40ca270b6895371f12b398660
        Authorization:
          name: Authorization
          in: header
          schema:
            type: string
          description: |
            `Bearer` OAuth Token issued by **LGE Members Platform** for a specific ThinQ user. 
          example: Bearer 5a9a713f51a95c53d781addd1af0dfa4f6e1e7420a8bff3c5198308dac571aa9845832b8d29bbe1f04deec2d35229c6d
        X-Conditional-Control:
          name: X-Conditional-Control
          in: header
          schema:
            type: string
          description: |
            if `true`,
              The request first retrieves the device status and executes the control command only in a controllable status.
            if `false`,
              The request executes the control command without checking for the device status.
            default is `true`
          example: true
      examples:
        refrigerator-object-example-res-1:
          value:
            messageId: 2ADaRijIk8CvaSHVPeEWNw
            timestamp: '2024-10-01T06:23:20.866279'
            response:
              temperature:
                - targetTemperature: 5
                  locationName: FRIDGE
                  unit: ''
                - targetTemperature: -23
                  locationName: FREEZER
                  unit: ''
              refrigeration:
                expressMode: true
              powerSave:
                powerSaveEnabled: true
        refrigerator-object-example-req-1:
          description: 냉장고 - 냉장실 온도를 섭씨 0도로 설정
          value:
            temperature:
              - targetTemperature: 0
                locationName: FRIDGE
                unit: C
        refrigerator-object-example-req-2:
          description: 냉장고 - 절전 모드 설정
          value:
            powerSave:
              powerSaveEnabled: true
    x-tagGroups:
      - name: Get started
        tags:
          - Base URLs
      - name: Authentication
        tags:
          - API Token
      - name: For Enterprise
        tags:
          - LG Device
          - LG Device Push
      - name: For EndUser
        tags:
          - ThinQ Device
          - ThinQ Device Push
          - ThinQ User
      - name: Solution
        tags:
          - DR
---

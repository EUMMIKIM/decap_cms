---
contents:
  lang: yaml
  code: >
    openapi: 3.0.3

    info:
      title: BUSINESS CONNECT
      description: |-
        BUSINESS CONNECT 연동을 위한 RESTful APIs

        HTTP Status Code   |   X-Response-Code Header   |   Description
        -------------------|----------------------------|-------------------
        200 | 0000 | OK
        400 | 2003 | Invalid facility type
        401 | 1000 | Not Authorized
        403 | 1001 | Access deny site
        &nbsp; | 1002 | Invalid role
        404 | 3000 | Not found resource
        405 | 2007 | Method Not Allowed
        412 | 2002 | Type is not valid
        &nbsp; | 2004 | Missing request parameter
        &nbsp; | 2008 | Invalid facility fields
        &nbsp; | 2009 | Invalid datetime
        500 | 3001 | Internal Server Error
      contact:
        email: bizdev@lge.com
      version: 3.1.0-SNAPSHOT
    servers:
      - url: https://ap-dv.api.lge.com/becon/v1.0
        description: KIC / DEV
      - url: https://ap-qa.api.lge.com/becon/v1.0
        description: KIC / QA
      - url: https://ap.api.lge.com/becon/v1.0
        description: KIC / PRD
    tags:
      - name: Overview
        description: Business Connect의 개략적인 소개입니다.
      - name: Call Sequence
        description: Image 영역입니다.
      - name: Auth
        description: Token 발급
      - name: Facility
        description: 설비 관련 API
      - name: Gateway
        description: 게이트웨이 관련 API
      - name: Site
        description: 현장 관련 API
    paths:
      /overview:
        tags:
        - Overview
      /token:
        post:
          tags:
          - Auth
          description: |-
            Create an authentication token to use an API that requires authentication.
            The token is valid for 24 hours.
          operationId: createToken
          parameters:
          - name: X-Api-Key
            in: header
            required: true
            schema:
              type: string
            description: 클라이언트에게 사전에 주어진 API Key
          - name: X-Api-Secret
            in: header
            required: true
            schema:
              type: string
            description: 클라이언트에게 사전에 주어진 API Secret
          responses:
            '200':
              description: OK
              content:
                "*/*":
                  schema:
                    "$ref": "#/components/schemas/TokenResponse"
            '400':
              description: Bad Request
            '401':
              description: Unauthorized
            '500':
              description: Internal Server Error
      /sites:
        get:
          tags:
            - Site
          summary: 현장 목록 조회
          description: 해당 서비스에서 접근 가능한 현장의 목록을 조회합니다.
          operationId: getSiteListUsingGET
          parameters:
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespSiteList'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": [
                            {
                              "country": "string",
                              "latitude": "string",
                              "longitude": "string",
                              "register_datetime": 0,
                              "site_addr": "string",
                              "site_code": "string",
                              "site_name": "string"
                            }
                          ]
                        }
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
      /sites/{site-code}:
        get:
          tags:
            - Site
          summary: 현장 정보 조회
          description: 서비스에서 접근 가능한 현장 중에서 특정 현장의 정보를 조회합니다.
          operationId: getSiteInfoUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespSiteInfo'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "country": "string",
                            "latitude": "string",
                            "longitude": "string",
                            "register_datetime": 0,
                            "site_addr": "string",
                            "site_code": "string",
                            "site_name": "string"
                          }
                        }
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code): String[10])<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
      /sites/{site-code}/gateways:
        get:
          tags:
            - Gateway
          summary: GW 리스트 조회
          description: 현장의 모든 게이트웨이(gateway)의 리스트를 조회합니다.
          operationId: getGwListUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespGatewayList'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": [
                            {
                              "gw_id": "string",
                              "gw_ip": "string",
                              "gw_name": "string",
                              "gw_type": "string",
                              "mac": "string",
                              "register_datetime": 0,
                              "version": "string"
                            }
                          ]
                        }
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10])<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}:
        get:
          tags:
            - Gateway
          summary: GW 정보 조회
          description: 현장에 있는 특정 게이트웨이(gateway) 정보를 조회합니다.
          operationId: getGwInfoUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespGatewayInfo'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "gw_id": "string",
                            "gw_ip": "string",
                            "gw_name": "string",
                            "gw_type": "string",
                            "mac": "string",
                            "register_datetime": 0,
                            "version": "string"
                          }
                        }
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/state:
        get:
          tags:
            - Gateway
          summary: GW 상태 조회
          description: 현장에 있는 특정 게이트웨이(gateway)의 상태 값을 조회합니다.
          operationId: getGwStateUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespGatewayState'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "capacity_oper": "string",
                            "cycle_select": "string",
                            "cycle_status": "string",
                            "demand_conn": "string",
                            "demand_status": "string",
                            "emergency_stop_status": "string",
                            "energy_capacity": "string",
                            "energy_capacity_set_rate": "string",
                            "energy_end_time": "string",
                            "energy_operation": "string",
                            "energy_operation_mode": "string",
                            "energy_operation_set_rate": "string",
                            "energy_operation_sleep_time": "string",
                            "energy_oversettemp": "string",
                            "energy_set_time": "string",
                            "energy_start_time": "string",
                            "energy_timebase": "string",
                            "energy_timebase_continue_time": "string",
                            "energy_timebase_no_working_time_end": "string",
                            "energy_timebase_no_working_time_start": "string",
                            "energy_weather": "string",
                            "log_status": "string",
                            "operation_oper": "string",
                            "operation_rate": "string",
                            "pdi_status": "string",
                            "sch_status": "string",
                            "total_load_capa": "string"
                          }
                        }
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities:
        get:
          tags:
            - Facility
          summary: 공조 설비 리스트 조회
          description: 특정 현장의 모든 공조 설비 리스트를 조회합니다.
          operationId: getFacilitiesUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityList'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": [
                            {
                              "facility_code": "string",
                              "facility_id": "string",
                              "facility_name": "string",
                              "facility_type": "string",
                              "num_idu_unit": 0,
                              "num_odu_unit": 0,
                              "parent_code": "string",
                              "parent_id": "string",
                              "parent_name": "string"
                            }
                          ]
                        }
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/state:
        get:
          tags:
            - Facility
          summary: 공조 설비 상태 조회
          description: 특정 GW(게이트웨이) 하위에 있는 모든 공조 설비의 상태값을 조회합니다.
          operationId: getFacilitiesStateUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility_types
              in: query
              description: 공조 설비 유형
              required: false
              style: form
              explode: true
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilitiesState'
                  examples:
                    example-0:
                      value: |- 
                        {
                          "code": "string",
                          "msg": "string",
                          "result": [
                            {
                              "facility_id": "string",
                              "facility_type": "ODU"
                            },
                            {
                              "facility_parent_id": "string",
                              "facility_id": "string",
                              "facility_type": "UNIT"
                            },
                            {
                              "facility_id": "string",
                              "facility_type": "VENT"
                            },
                            {
                              "facility_id": "string",
                              "facility_type": "AWHP"
                            },
                            {
                              "facility_id": "string",
                              "facility_type": "AIRCARE"
                            },
                            {
                              "facility_id": "string",
                              "facility_type": "CHILLER"
                            },
                            {
                              "facility_type": "IDU",
                              "facility_id": "string"
                            },
                            {
                              "facility_type": "AHU",
                              "facility_id": "string"
                            },
                            {
                              "facility_id": "string",
                              "facility_parent_id": "string",
                              "facility_type": "IDU"
                            }
                          ]
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/{facility-type}/{facility-id}:
        get:
          tags:
            - Facility
          summary: 현장의 공조 설비 정보 조회
          description: >-
            현장의 공조 설비 정보를 조회합니다. 본 API로 정보를 조회할 수 있는 공조 설비 타입은 AHU, AIRCARE, AWHP,
            CHILLER, ODU, VENT입니다.
          operationId: getFacilityInfoUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-type
              in: path
              description: 공조 설비 유형
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityInfo'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "facility_code": "string",
                            "facility_id": "string",
                            "facility_name": "string",
                            "facility_type": "string",
                            "num_idu_unit": 0,
                            "num_odu_unit": 0,
                            "parent_code": "string",
                            "parent_id": "string",
                            "parent_name": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2004`: Missing request parameter (파라미터 입력 오류)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/{facility-type}/{facility-id}/cycle:
        get:
          tags:
            - Facility
          summary: AHU/CHILLER 사이클 데이터 조회
          description: AHU(공기조화기)/CHILLER(칠러) 타입 공조 설비의 사이클 데이터를 조회합니다.
          operationId: getFacilityCycleUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-type
              in: path
              description: 공조 설비 유형
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: dateStart
              in: query
              description: 조회할 시작 날짜 (unix time(sec))
              required: true
              style: form
              schema:
                type: string
            - name: dateEnd
              in: query
              description: 조회할 종료 날짜 (unix time(sec))
              required: false
              style: form
              schema:
                type: string
            - name: fields
              in: query
              description: 조회 항목
              required: false
              style: form
              explode: true
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
               `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityCycle'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "sensor_data_count": 0,
                            "sensor_data_end": 0,
                            "sensor_data_list": [
                              {
                                "additionalProp1": "string",
                                "additionalProp2": "string",
                                "additionalProp3": "string"
                              }
                            ],
                            "sensor_data_start": 0,
                            "sensor_id": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2004`: Missing request parameter (파라미터 입력 오류)<br><br>
                `2008`: Invalid facility fields <br><br>
                `2009`: Invalid datetime (시간 포맷 오류 ex: unixtime)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/ahu/{facility-id}/history/ctrl:
        get:
          tags:
            - Facility
          summary: AHU 제어 이력 조회
          description: AHU(공기조화기)에 대한 제어 이력을 조회합니다.
          operationId: getFacilityCtrlHistoryAhuUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: yyyyMMdd
              in: query
              description: 조회할 날짜 (yyyyMMdd)
              required: true
              style: form
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityCtrlHistAhu'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "account": "string",
                            "affect": "string",
                            "auto_vent": "string",
                            "cool_ea_damper": "string",
                            "cool_mix_damper": "string",
                            "cool_oa_damper": "string",
                            "datetime": 0,
                            "fan_ea_damper": "string",
                            "fan_mix_damper": "string",
                            "fan_oa_damper": "string",
                            "heat_ea_damper": "string",
                            "heat_mix_damper": "string",
                            "heat_oa_damper": "string",
                            "humidity": "string",
                            "lock": "string",
                            "mode": "string",
                            "oper": "string",
                            "sethumidity": "string",
                            "settemp": "string",
                            "smoke_detection": "string",
                            "ui_cmd": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2004`: Missing request parameter (파라미터 입력 오류) <br><br>
                `2009`: Invalid datetime (시간 포맷 오류 ex: yyyyMMdd)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/ahu/{facility-id}/state:
        get:
          tags:
            - Facility
          summary: AHU 상태 조회
          description: AHU(공기조화기)의 상태 값을 조회합니다.
          operationId: getFacilityStateAhuUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: fields
              in: query
              description: 조회 항목
              required: false
              style: form
              explode: true
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityStateAhu'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "autovent": "string",
                            "basetemp": "string",
                            "co2": "string",
                            "co2value": "string",
                            "cooleadamper": "string",
                            "coolmixdamper": "string",
                            "cooloadamper": "string",
                            "eadamper": "string",
                            "err": "string",
                            "faneadamper": "string",
                            "fanmixdamper": "string",
                            "fanoadamper": "string",
                            "fanspeed": "string",
                            "filter": "string",
                            "heateadamper": "string",
                            "heater": "string",
                            "heatmixdamper": "string",
                            "heatoadamper": "string",
                            "humidifier": "string",
                            "humidity": "string",
                            "lock": "string",
                            "mixdamper": "string",
                            "mixinghumidity": "string",
                            "mixingtemp": "string",
                            "mode": "string",
                            "oadamper": "string",
                            "oper": "string",
                            "outerhumidity": "string",
                            "outertemp": "string",
                            "set_co2": "string",
                            "sethumidity": "string",
                            "settemp": "string",
                            "smoke": "string",
                            "supplyfan": "string",
                            "supplyfan1": "string",
                            "supplyfan2": "string",
                            "supplyfan3": "string",
                            "supplyfan4": "string",
                            "supplyfan5": "string",
                            "supplyfan6": "string",
                            "supplyfannum": "string",
                            "supplyhumidity": "string",
                            "supplytemp": "string",
                            "ventfan": "string",
                            "ventfan1": "string",
                            "ventfan2": "string",
                            "ventfan3": "string",
                            "ventfan4": "string",
                            "ventfan5": "string",
                            "ventfan6": "string",
                            "ventfannum": "string",
                            "venthumidity": "string",
                            "venttemp": "string",
                            "vocvalue": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2008`: Invalid facility fields<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/aircare/{facility-id}/history/ctrl:
        get:
          tags:
            - Facility
          summary: AIRCARE 제어 이력 조회
          description: AIRCARE(상업용 공기청정기)에 대한 제어 이력을 조회합니다.
          operationId: getFacilityCtrlHistoryAircareUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: yyyyMMdd
              in: query
              description: 조회할 날짜 (yyyyMMdd)
              required: true
              style: form
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityCtrlHistAircare'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "account": "string",
                            "affect": "string",
                            "air_sensor": "string",
                            "datetime": 0,
                            "filter_reset": "string",
                            "id_group": "string",
                            "mode": "string",
                            "oper": "string",
                            "operlock": "string",
                            "scope": "string",
                            "ui_cmd": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2004`: Missing request parameter (파라미터 입력 오류) <br><br>
                `2009`: Invalid datetime (시간 포맷 오류 ex: yyyyMMdd)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)<br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/aircare/{facility-id}/state:
        get:
          tags:
            - Facility
          summary: AIRCARE 상태 조회
          description: AIRCARE(상업용 공기청정기)의 상태 값을 조회합니다.
          operationId: getFacilityStateAircareUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: fields
              in: query
              description: 조회 항목
              required: false
              style: form
              explode: true
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityStateAircare'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "air_defense": "string",
                            "air_sensor": "string",
                            "cleanliness": "string",
                            "err": "string",
                            "fanspeed": "string",
                            "filter": "string",
                            "filter_remain": "string",
                            "filter_reset": "string",
                            "fine_dust": "string",
                            "fine_dust_status": "string",
                            "mode": "string",
                            "oper": "string",
                            "operlock": "string",
                            "smell_status": "string",
                            "superultrafine_dust": "string",
                            "superultrafine_dust_status": "string",
                            "ultrafine_dust": "string",
                            "ultrafine_dust_status": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2008`: Invalid facility fields<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/awhp/{facility-id}/history/ctrl:
        get:
          tags:
            - Facility
          summary: AWHP 제어 이력 조회
          description: AWHP(공기열원식 히트펌프)에 대한 제어 이력을 조회합니다.
          operationId: getFacilityCtrlHistoryAwhpUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: yyyyMMdd
              in: query
              description: 조회할 날짜 (yyyyMMdd)
              required: true
              style: form
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityCtrlHistAwhp'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "account": "string",
                            "affect": "string",
                            "awsettemp": "string",
                            "datetime": 0,
                            "hotwater": "string",
                            "id_group": "string",
                            "lock": "string",
                            "mode": "string",
                            "oper": "string",
                            "scope": "string",
                            "ui_cmd": "string",
                            "waterouttempflag": "string",
                            "watertanksettemp": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2004`: Missing request parameter (파라미터 입력 오류) <br><br>
                `2009`: Invalid datetime (시간 포맷 오류 ex: yyyyMMdd)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/awhp/{facility-id}/state:
        get:
          tags:
            - Facility
          summary: AWHP 상태 조회
          description: AWHP(공기열원식 히트펌프)의 상태 값을 조회합니다.
          operationId: getFacilityStateAwhpUsingGET_1
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: fields
              in: query
              description: 조회 항목
              required: false
              style: form
              explode: true
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityStateAwhp'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "control_type": "string",
                            "droomtempflag": "string",
                            "droomtempflag_2": "string",
                            "emergency_oper": "string",
                            "err": "string",
                            "hotwater": "string",
                            "lock": "string",
                            "lownoisemode_oper": "string",
                            "mode": "string",
                            "oper": "string",
                            "pipein_temp": "string",
                            "pipeout_temp": "string",
                            "quick_dh_wheat_oper": "string",
                            "roomtemp": "string",
                            "settemp": "string",
                            "settemp_hotwater": "string",
                            "sun_temp": "string",
                            "tank_temp": "string",
                            "type": "string",
                            "waterouttempflag": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2008`: Invalid facility fields<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/chiller/{facility-id}/history/ctrl:
        get:
          tags:
            - Facility
          summary: CHILLER 제어 이력 조회
          description: CHILLER(칠러)에 대한 제어 이력을 조회합니다.
          operationId: getFacilityCtrlHistoryChillerUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: yyyyMMdd
              in: query
              description: 조회할 날짜 (yyyyMMdd)
              required: true
              style: form
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityCtrlHistChiller'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "account": "string",
                            "affect": "string",
                            "alarm_release": "string",
                            "datetime": 0,
                            "mode": "string",
                            "motor_tot_running_current_a": "string",
                            "oper": "string",
                            "set_cool_water_temp": "string",
                            "set_demand_limit": "string",
                            "set_hot_water_temp": "string",
                            "setdemandlimit": "string",
                            "setmotorlimit": "string",
                            "ui_cmd": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2004`: Missing request parameter (파라미터 입력 오류) <br><br>
                `2009`: Invalid datetime (시간 포맷 오류 ex: yyyyMMdd)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/chiller/{facility-id}/state:
        get:
          tags:
            - Facility
          summary: CHILLER 상태 조회
          description: CHILLER(칠러)의 상태 값을 조회합니다.
          operationId: getFacilityStateChillerUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: fields
              in: query
              description: 조회 항목
              required: false
              style: form
              explode: true
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityStateChiller'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "chilled_water_entering_temp": "string",
                            "chilled_water_leaving_temp": "string",
                            "controlvalvelimit": "string",
                            "detail_type": "string",
                            "err": "string",
                            "mode": "string",
                            "oper": "string",
                            "setcoolwatertemp": "string",
                            "setdemandlimit": "string",
                            "sethotwatertemp": "string",
                            "seticewatertemp": "string",
                            "tot_running_current_a": "string",
                            "type": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br> 
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2008`: Invalid facility fields<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/odu/{facility-id}/history/ctrl:
        get:
          tags:
            - Facility
          summary: ODU 제어 이력 조회
          description: ODU(에어컨 실외기)에 대한 제어 이력을 조회합니다.
          operationId: getFacilityCtrlHistoryOduUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: odu-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: yyyyMMdd
              in: query
              description: 조회할 날짜 (yyyyMMdd)
              required: true
              style: form
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityCtrlHistOdu'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "ability_enhance_mode": "string",
                            "ability_enhance_step": "string",
                            "account": "string",
                            "additional_defrost": "string",
                            "affect": "string",
                            "datetime": 0,
                            "max_rpm": "string",
                            "night_low_noise": "string",
                            "night_low_noise_end": "string",
                            "night_low_noise_start": "string",
                            "remote_4way_off": "string",
                            "remote_defrost": "string",
                            "remote_fdd_start": "string",
                            "remote_oil_return": "string",
                            "remote_refrigerant_noise_Step": "string",
                            "remote_reset": "string",
                            "remote_slc_Step": "string",
                            "snow_removal": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"         
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2004`: Missing request parameter (파라미터 입력 오류) <br><br>
                `2009`: Invalid datetime (시간 포맷 오류 ex: yyyyMMdd)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/odu/{facility-id}/state:
        get:
          tags:
            - Facility
          summary: ODU 상태 조회
          description: ODU(에어컨 실외기)의 상태 값을 조회합니다.
          operationId: getFacilityStateOduUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: odu-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: fields
              in: query
              description: 조회 항목
              required: false
              style: form
              explode: true
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityStateOdu'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "ability_enhance": "string",
                            "ability_enhance_enable": "string",
                            "ability_enhance_step": "string",
                            "add_defrost": "string",
                            "add_defrost_enable": "string",
                            "air_temp": "string",
                            "capacity": "string",
                            "err": "string",
                            "fan1_micom_version": "string",
                            "fan2_micom_version": "string",
                            "fan_rpm_enable": "string",
                            "fan_rpm_step": "string",
                            "inv1_comp_micom_version": "string",
                            "inv2_comp_micom_version": "string",
                            "micom_ver": "string",
                            "night_low_noise": "string",
                            "night_low_noise_enable": "string",
                            "opermode": "string",
                            "refrigerant": "string",
                            "remote_4way_off": "string",
                            "remote_4way_off_enable": "string",
                            "remote_cal_refrigerant": "string",
                            "remote_defrost": "string",
                            "remote_defrost_enable": "string",
                            "remote_fdd_start": "string",
                            "remote_fdd_start_enable": "string",
                            "remote_oil_return": "string",
                            "remote_oil_return_enable": "string",
                            "remote_refrigerant_noise": "string",
                            "remote_refrigerant_noise_enable": "string",
                            "remote_refrigerant_noise_step": "string",
                            "remote_reset": "string",
                            "remote_reset_enable": "string",
                            "remote_slc": "string",
                            "remote_slc_enable": "string",
                            "remote_slc_step": "string",
                            "remote_total_refrigerant": "string",
                            "remove_snow": "string",
                            "remove_snow_enable": "string",
                            "slave_unit": "string",
                            "sync_micom_version": "string",
                            "target_high_pressure": "string",
                            "target_low_pressure": "string",
                            "target_sub_cool": "string",
                            "type": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2008`: Invalid facility fields<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/odu/{odu-id}/awhp/{facility-id}/history/ctrl:
        get:
          tags:
            - Facility
          summary: ODU-AWHP 제어 이력 조회
          description: 특정 ODU(실외기) 하위에 있는 AWHP(공기열원식 히트펌프)에 대한 제어 이력을 조회합니다.
          operationId: getFacilityCtrlHistoryOduAwhpUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: odu-id
              in: path
              description: ODU 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: yyyyMMdd
              in: query
              description: 조회할 날짜 (yyyyMMdd)
              required: true
              style: form
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityCtrlHistAwhp'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "account": "string",
                            "affect": "string",
                            "awsettemp": "string",
                            "datetime": 0,
                            "hotwater": "string",
                            "id_group": "string",
                            "lock": "string",
                            "mode": "string",
                            "oper": "string",
                            "scope": "string",
                            "ui_cmd": "string",
                            "waterouttempflag": "string",
                            "watertanksettemp": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2004`: Missing request parameter (파라미터 입력 오류) <br><br>
                `2009`: Invalid datetime (시간 포맷 오류 ex: yyyyMMdd)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/odu/{odu-id}/awhp/{facility-id}/state:
        get:
          tags:
            - Facility
          summary: ODU-AWHP 상태 조회
          description: 특정 ODU(실외기) 하위에 있는 AWHP(공기열원식 히트펌프)의 상태 값을 조회합니다.
          operationId: getFacilityStateAwhpUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: odu-id
              in: path
              description: ODU 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: fields
              in: query
              description: 조회 항목
              required: false
              style: form
              explode: true
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityStateAwhp'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "control_type": "string",
                            "droomtempflag": "string",
                            "droomtempflag_2": "string",
                            "emergency_oper": "string",
                            "err": "string",
                            "hotwater": "string",
                            "lock": "string",
                            "lownoisemode_oper": "string",
                            "mode": "string",
                            "oper": "string",
                            "pipein_temp": "string",
                            "pipeout_temp": "string",
                            "quick_dh_wheat_oper": "string",
                            "roomtemp": "string",
                            "settemp": "string",
                            "settemp_hotwater": "string",
                            "sun_temp": "string",
                            "tank_temp": "string",
                            "type": "string",
                            "waterouttempflag": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2008`: Invalid facility fields<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/odu/{odu-id}/idu/{facility-id}/history/ctrl:
        get:
          tags:
            - Facility
          summary: IDU 제어 이력 조회
          description: 특정 ODU(에어컨 실외기) 하위에 있는 IDU(에어컨 실내기)에 대한 제어 이력을 조회합니다.
          operationId: getFacilityCtrlHistoryIduUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: odu-id
              in: path
              description: ODU 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: yyyyMMdd
              in: query
              description: 조회할 날짜 (yyyyMMdd)
              required: true
              style: form
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityCtrlHistIdu'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "account": "string",
                            "aco_lower": "string",
                            "aco_oper": "string",
                            "aco_upper": "string",
                            "affect": "string",
                            "air_clean": "string",
                            "air_clean_by_oneself": "string",
                            "comfort": "string",
                            "comfort_step": "string",
                            "cool_lower_lim": "string",
                            "cool_settemp": "string",
                            "cool_upper_lim": "string",
                            "datetime": 0,
                            "energy_control": "string",
                            "fan": "string",
                            "fanlock": "string",
                            "heat_lower_lim": "string",
                            "heat_settemp": "string",
                            "heat_upper_lim": "string",
                            "heater_mode": "string",
                            "heater_oper": "string",
                            "id_group": "string",
                            "lock": "string",
                            "mode": "string",
                            "modelock": "string",
                            "occupied": "string",
                            "oper": "string",
                            "outdoor_heater": "string",
                            "outdoor_heatpump": "string",
                            "outdoor_temp1": "string",
                            "outdoor_temp2": "string",
                            "plasma": "string",
                            "setback_lower": "string",
                            "setback_oper": "string",
                            "setback_upper": "string",
                            "settemp": "string",
                            "settemp_llim": "string",
                            "settemp_lock": "string",
                            "settemp_ulim": "string",
                            "swing": "string",
                            "ui_cmd": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2004`: Missing request parameter (파라미터 입력 오류) <br><br>
                `2009`: Invalid datetime (시간 포맷 오류 ex: yyyyMMdd)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/odu/{odu-id}/idu/{facility-id}/state:
        get:
          tags:
            - Facility
          summary: IDU 상태 조회
          description: 특정 ODU(에어컨 실외기) 하위에 있는 IDU(에어컨 실내기)의 상태 값을 조회합니다.
          operationId: getFacilityStateIduUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: odu-id
              in: path
              description: ODU 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: fields
              in: query
              description: 조회 항목
              required: false
              style: form
              explode: true
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityStateIdu'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "aco_lower": "string",
                            "aco_oper": "string",
                            "aco_upper": "string",
                            "air_clean": "string",
                            "air_clean_avail": "string",
                            "air_clean_by_oneself": "string",
                            "air_clean_plasma_avail": "string",
                            "capacity": "string",
                            "cmd7_avail": "string",
                            "comfort": "string",
                            "comfort_avail": "string",
                            "comfort_step": "string",
                            "cool_lower_lim": "string",
                            "cool_settemp": "string",
                            "cool_upper_lim": "string",
                            "dustsensor_avail": "string",
                            "energy_control": "string",
                            "err": "string",
                            "fan": "string",
                            "fanlock": "string",
                            "filter": "string",
                            "fine_dust": "string",
                            "fine_dust_status": "string",
                            "heat_lower_lim": "string",
                            "heat_settemp": "string",
                            "heat_upper_lim": "string",
                            "heater_oper": "string",
                            "humidity": "string",
                            "icon": "string",
                            "lev": "string",
                            "lock": "string",
                            "mode": "string",
                            "modelock": "string",
                            "occupied": "string",
                            "oper": "string",
                            "outdoor_avail": "string",
                            "outdoor_heater": "string",
                            "outdoor_heatpump": "string",
                            "outdoor_temp1": "string",
                            "outdoor_temp2": "string",
                            "plasma": "string",
                            "plasma_avail": "string",
                            "plasma_failure": "string",
                            "roomtemp": "string",
                            "setback_lower": "string",
                            "setback_oper": "string",
                            "setback_upper": "string",
                            "settemp": "string",
                            "settemp_llim": "string",
                            "settemp_ulim": "string",
                            "settemplock": "string",
                            "superultrafine_dust": "string",
                            "superultrafine_dust_status": "string",
                            "swing": "string",
                            "twoset_automode": "string",
                            "type": "string",
                            "ultrafine_dust": "string",
                            "ultrafine_dust_status": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2008`: Invalid facility fields <br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/odu/{odu-id}/unit/{facility-id}/state:
        get:
          tags:
            - Facility
          summary: UNIT 상태 조회
          description: 특정 ODU(실외기) 하위에 있는 UNIT의 상태 값을 조회합니다.
          operationId: getFacilityStateUnitUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: odu-id
              in: path
              description: ODU 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: fields
              in: query
              description: 조회 항목
              required: false
              style: form
              explode: true
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityStateUnit'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "fan1_micom_ver": "string",
                            "fan2_micom_ver": "string",
                            "id_unit": "string",
                            "inv1_micom_ver": "string",
                            "inv2_micom_ver": "string",
                            "micom_sync_ver": "string",
                            "micom_ver": "string",
                            "onboarding_status": "string",
                            "onboarding_time": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2008`: Invalid facility fields<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/odu/{odu-id}/{facility-type}/{facility-id}:
        get:
          tags:
            - Facility
          summary: ODU 하위 공조 설비 정보 조회
          description: >-
            특정 ODU 하위에 있는 공조 설비의 정보를 조회합니다. 본 API로 공조 설비의 정보를 조회할 경우, 공조 설비는 AWHP,
            IDU, UNIT 타입이어야 합니다.
          operationId: getODUFacilityInfoUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: odu-id
              in: path
              description: ODU 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-type
              in: path
              description: 공조 설비 유형
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityInfo'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "facility_code": "string",
                            "facility_id": "string",
                            "facility_name": "string",
                            "facility_type": "string",
                            "num_idu_unit": 0,
                            "num_odu_unit": 0,
                            "parent_code": "string",
                            "parent_id": "string",
                            "parent_name": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2008`: Invalid facility fields<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/odu/{odu-id}/{facility-type}/{facility-id}/cycle:
        get:
          tags:
            - Facility
          summary: IDU/UNIT 사이클 데이터 조회
          description: 특정 ODU(실외기) 하위에 있는 IDU/UNIT 타입 공조 설비의 사이클 데이터를 조회합니다.
          operationId: getFacilityOduCycleUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: odu-id
              in: path
              description: ODU 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-type
              in: path
              description: 공조 설비 유형
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: dateStart
              in: query
              description: 조회할 시작 날짜 (unix time(sec))
              required: true
              style: form
              schema:
                type: string
            - name: dateEnd
              in: query
              description: 조회할 종료 날짜 (unix time(sec))
              required: false
              style: form
              schema:
                type: string
            - name: fields
              in: query
              description: 조회 항목
              required: false
              style: form
              explode: true
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityCycle'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "sensor_data_count": 0,
                            "sensor_data_end": 0,
                            "sensor_data_list": [
                              {
                                "additionalProp1": "string",
                                "additionalProp2": "string",
                                "additionalProp3": "string"
                              }
                            ],
                            "sensor_data_start": 0,
                            "sensor_id": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2004`: Missing request parameter (파라미터 입력 오류)<br><br>
                `2008`: Invalid facility fields <br><br>
                `2009`: Invalid datetime (시간 포맷 오류 ex: unix time)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/vent/{facility-id}/history/ctrl:
        get:
          tags:
            - Facility
          summary: VENT 제어 이력 조회
          description: VENT(환기장치)에 대한 제어 이력을 조회합니다.
          operationId: getFacilityCtrlHistoryVentUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: yyyyMMdd
              in: query
              description: 조회할 날짜 (yyyyMMdd)
              required: true
              style: form
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityCtrlHistVent'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "ac_mode": "string",
                            "ac_oper": "string",
                            "account": "string",
                            "affect": "string",
                            "datetime": 0,
                            "fan": "string",
                            "fanlock": "string",
                            "filter": "string",
                            "function": "string",
                            "heater": "string",
                            "humidity": "string",
                            "id_group": "string",
                            "lock": "string",
                            "mode": "string",
                            "modelock": "string",
                            "ntfc_oper": "string",
                            "ntfclock": "string",
                            "oper": "string",
                            "plasma": "string",
                            "set_temp": "string",
                            "settemp_llim": "string",
                            "settemp_ulim": "string",
                            "settemplock": "string",
                            "ui_cmd": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2004`: Missing request parameter (파라미터 입력 오류) <br><br>
                `2009`: Invalid datetime (시간 포맷 오류 ex: yyyyMMdd)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

      /sites/{site-code}/gateways/{gw-id}/facilities/vent/{facility-id}/state:
        get:
          tags:
            - Facility
          summary: VENT 상태 조회
          description: VENT(환기장치)의 상태 값을 조회합니다.
          operationId: getFacilityStateVentUsingGET
          parameters:
            - name: site-code
              in: path
              description: 현장 코드
              required: true
              style: simple
              schema:
                type: string
            - name: gw-id
              in: path
              description: 게이트웨이 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: facility-id
              in: path
              description: 공조 설비 아이디
              required: true
              style: simple
              schema:
                type: string
            - name: fields
              in: query
              description: 조회 항목
              required: false
              style: form
              explode: true
              schema:
                type: string
            - name: X-Api-Key
              in: header
              description: 클라이언트에게 사전에 주어진 API Key
              required: true
              schema:
                type: string
            - name: X-Api-Token
              in: header
              description: 클라이언트가 24시간 이내 획득한 API Token
              required: true
              schema:
                type: string
            - name: X-Message-Id
              in: header
              description: 요청의 추적을 위하여 클라이언트가 생성한 ID (UUID Version 4, url-safe-base64-no-padding, length:22)
              required: false
              schema:
                type: string
          responses:
            '200':
              description: |-
                `0000`: Success (성공)
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/RespFacilityStateVent'
                  examples:
                    example-0:
                      value: |-
                        {
                          "code": "string",
                          "msg": "string",
                          "result": {
                            "ac_mode": "string",
                            "ac_oper": "string",
                            "co2": "string",
                            "co2value": "string",
                            "err": "string",
                            "fan": "string",
                            "fanlock": "string",
                            "filter": "string",
                            "function": "string",
                            "heater": "string",
                            "humidify": "string",
                            "lock": "string",
                            "master": "string",
                            "mode": "string",
                            "modelock": "string",
                            "ntfc_avail": "string",
                            "ntfc_oper": "string",
                            "ntfclock": "string",
                            "oper": "string",
                            "plasma": "string",
                            "settemp": "string",
                            "settemp_llim": "string",
                            "settemp_ulim": "string",
                            "settemplock": "string",
                            "type": "string",
                            "vent_cmd4_avail": "string"
                          }
                        }
            '400':
              description: |-
                `2003`: Invalid facility type (유효하지 않은 공조 설비 타입)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '401':
              description: |-
                `1000`: Not authorized (인증 실패)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '403':
              description: |-
                `1001`: Access deny site (접근 불가 현장)<br><br>
                `1002`: Invalid role (권한 없음)<br><br>
              headers:
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '404':
              description: |-
                `3000`: Not found resource (리소스 데이터가 없음)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '412':
              description: |-
                `2002`: Type is invalid (site-code: String[10], gw-id: String[8])<br><br>
                `2008`: Invalid facility fields
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"
            '500':
              description: |-
                `3001`: Internal server error (내부 서버 오류)<br><br>
              headers:
                X-Message-Id:
                  "$ref": "#/components/headers/X-Message-Id"
                X-Request-Id:
                  "$ref": "#/components/headers/X-Request-Id"
                X-Response-Code:
                  "$ref": "#/components/headers/X-Response-Code"

    components:
      headers:
        X-Message-Id:
          schema:
            type: string
          description: 클라이언트가 API 호출 시 전송했던 X-Message-Id 헤더 값
        X-Request-Id:
          schema:
            type: string
          description: API 서비스가 요청 추적을 위해 생성한 ID
        X-Response-Code:
          schema:
            type: string
          description: 요청에 대한 API 서비스의 응답 코드
      schemas:
        TokenResponse:
          type: object
          properties:
            access_token:
              type: string
              default: jwt_token(header.payload.signature)
        CompletableFuture«ResponseEntity«Resp«Array«RespCtrlHistoryResult»»»»:
          title: CompletableFuture«ResponseEntity«Resp«Array«RespCtrlHistoryResult»»»»
          type: object
          properties:
            cancelled:
              type: boolean
            completedExceptionally:
              type: boolean
            done:
              type: boolean
            numberOfDependents:
              type: integer
              format: int32
        CompletableFuture«ResponseEntity«Resp«Array«RespFacilityResult»»»»:
          title: CompletableFuture«ResponseEntity«Resp«Array«RespFacilityResult»»»»
          type: object
          properties:
            cancelled:
              type: boolean
            completedExceptionally:
              type: boolean
            done:
              type: boolean
            numberOfDependents:
              type: integer
              format: int32
        CompletableFuture«ResponseEntity«Resp«Array«RespGatewayResult»»»»:
          title: CompletableFuture«ResponseEntity«Resp«Array«RespGatewayResult»»»»
          type: object
          properties:
            cancelled:
              type: boolean
            completedExceptionally:
              type: boolean
            done:
              type: boolean
            numberOfDependents:
              type: integer
              format: int32
        CompletableFuture«ResponseEntity«Resp«Array«RespSiteResult»»»»:
          title: CompletableFuture«ResponseEntity«Resp«Array«RespSiteResult»»»»
          type: object
          properties:
            cancelled:
              type: boolean
            completedExceptionally:
              type: boolean
            done:
              type: boolean
            numberOfDependents:
              type: integer
              format: int32
        CompletableFuture«ResponseEntity«Resp«List«Map«string,object»»»»»:
          title: CompletableFuture«ResponseEntity«Resp«List«Map«string,object»»»»»
          type: object
          properties:
            cancelled:
              type: boolean
            completedExceptionally:
              type: boolean
            done:
              type: boolean
            numberOfDependents:
              type: integer
              format: int32
        CompletableFuture«ResponseEntity«Resp«RespFacilityResult»»»:
          title: CompletableFuture«ResponseEntity«Resp«RespFacilityResult»»»
          type: object
          properties:
            cancelled:
              type: boolean
            completedExceptionally:
              type: boolean
            done:
              type: boolean
            numberOfDependents:
              type: integer
              format: int32
        CompletableFuture«ResponseEntity«Resp«RespFacilityStateResult»»»:
          title: CompletableFuture«ResponseEntity«Resp«RespFacilityStateResult»»»
          type: object
          properties:
            cancelled:
              type: boolean
            completedExceptionally:
              type: boolean
            done:
              type: boolean
            numberOfDependents:
              type: integer
              format: int32
        CompletableFuture«ResponseEntity«Resp«RespGatewayResult»»»:
          title: CompletableFuture«ResponseEntity«Resp«RespGatewayResult»»»
          type: object
          properties:
            cancelled:
              type: boolean
            completedExceptionally:
              type: boolean
            done:
              type: boolean
            numberOfDependents:
              type: integer
              format: int32
        CompletableFuture«ResponseEntity«Resp«RespGatewayStateResult»»»:
          title: CompletableFuture«ResponseEntity«Resp«RespGatewayStateResult»»»
          type: object
          properties:
            cancelled:
              type: boolean
            completedExceptionally:
              type: boolean
            done:
              type: boolean
            numberOfDependents:
              type: integer
              format: int32
        CompletableFuture«ResponseEntity«Resp«RespSiteResult»»»:
          title: CompletableFuture«ResponseEntity«Resp«RespSiteResult»»»
          type: object
          properties:
            cancelled:
              type: boolean
            completedExceptionally:
              type: boolean
            done:
              type: boolean
            numberOfDependents:
              type: integer
              format: int32
        CompletableFuture«ResponseEntity«Resp«string»»»:
          title: CompletableFuture«ResponseEntity«Resp«string»»»
          type: object
          properties:
            cancelled:
              type: boolean
            completedExceptionally:
              type: boolean
            done:
              type: boolean
            numberOfDependents:
              type: integer
              format: int32
        DeviceRegisterInfo:
          title: DeviceRegisterInfo
          type: object
          properties:
            deviceId:
              type: string
            deviceType:
              type: string
            mbrNo:
              type: string
        JsonNode:
          title: JsonNode
          type: object
          properties:
            array:
              type: boolean
            bigDecimal:
              type: boolean
            bigInteger:
              type: boolean
            binary:
              type: boolean
            boolean:
              type: boolean
            containerNode:
              type: boolean
            double:
              type: boolean
            empty:
              type: boolean
            float:
              type: boolean
            floatingPointNumber:
              type: boolean
            int:
              type: boolean
            integralNumber:
              type: boolean
            long:
              type: boolean
            missingNode:
              type: boolean
            nodeType:
              type: string
              enum:
                - ARRAY
                - BINARY
                - BOOLEAN
                - MISSING
                - 'NULL'
                - NUMBER
                - OBJECT
                - POJO
                - STRING
            'null':
              type: boolean
            number:
              type: boolean
            object:
              type: boolean
            pojo:
              type: boolean
            short:
              type: boolean
            textual:
              type: boolean
            valueNode:
              type: boolean
        Mono«ResponseEntity«Resp«string»»»:
          title: Mono«ResponseEntity«Resp«string»»»
          type: object
        Mono«Resp«RespCycleResult»»:
          title: Mono«Resp«RespCycleResult»»
          type: object
        ReqCycleCtrl:
          title: ReqCycleCtrl
          type: object
          properties:
            oper:
              type: string
              description: |-
                사이클 제어 타입
                * 용량제어(ABILITYENHANCE)
                * 제설모드제어(REMOVESNOW)
                * 제상모드제어(ADDDEFROST)
                * 수동제상운전(REMOTE_DEFROST)
                * 수동오일회수(REMOTE_OIL_RETURN)
                * 실외기 재시동(REMOTE_RESET)
                * 사방변 OFF제어(REMOTE_4WAY_OFF)
                * 능동절전운전(REMOTE_SLC)
                * 실외기 냉매 소음절감 모드제어(REMOTE_REFRIGERANT_NOISE)
                * 실외기 팬 최대 RPM제한 설정(MAXRPM)
            value:
              type: string
              description: |-
                사이클 제어 목표값
                * ABILITYENHANCE(0~10)
                * REMOVESNOW(ON/OFF)
                * ADDDEFROST(ON/OFF)
                * REMOTE_DEFROST(ON)
                * REMOTE_OIL_RETURN(ON)
                * REMOTE_RESET(ON)
                * REMOTE_4WAY_OFF(ON)
                * REMOTE_SLC(0~4)
                * REMOTE_REFRIGERANT_NOISE(0(OFF),1,2)
                * MAXRPM(0~7)
        RespAhuCtrlHistoryResult:
          title: RespAhuCtrlHistoryResult
          type: object
          properties:
            account:
              type: string
              description: 제어계정
            affect:
              type: string
              description: 제어주체
            auto_vent:
              type: string
              description: 자동환기
            cool_ea_damper:
              type: string
              description: 냉방ea댐퍼개도
            cool_mix_damper:
              type: string
              description: 냉방mix댐퍼개도
            cool_oa_damper:
              type: string
              description: 냉방oa댐퍼개도
            datetime:
              type: integer
              description: 제어시간
              format: int64
            fan_ea_damper:
              type: string
              description: 송풍ea댐퍼개도
            fan_mix_damper:
              type: string
              description: 송풍mix댐퍼개도
            fan_oa_damper:
              type: string
              description: 송풍oa댐퍼개도
            heat_ea_damper:
              type: string
              description: 난방ea댐퍼개도
            heat_mix_damper:
              type: string
              description: 난방mix댐퍼개도
            heat_oa_damper:
              type: string
              description: 난방oa댐퍼개도
            humidity:
              type: string
              description: 가습
            lock:
              type: string
              description: 잠금
            mode:
              type: string
              description: 운전모드
            oper:
              type: string
              description: 동작
            scope:
              type: string
              description: 제어범위
            sethumidity:
              type: string
              description: 목표습도
            settemp:
              type: string
              description: 설정온도
            smoke_detection:
              type: string
              description: 화재감지
            ui_cmd:
              type: string
        RespAhuStateResult:
          title: RespAhuStateResult
          type: object
          properties:
            autovent:
              type: string
              description: 자동환기
            basetemp:
              type: string
              description: 실내온도 설정기준
            co2:
              type: string
              description: CO2농도(설비 버전에 따라 co2 또는 co2value)
            co2value:
              type: string
              description: CO2농도(설비 버전에 따라 co2 또는 co2value)
            cooleadamper:
              type: string
              description: 냉방ea댐퍼개도
            coolmixdamper:
              type: string
              description: 냉방mix댐퍼개도
            cooloadamper:
              type: string
              description: 냉방oa댐퍼개도
            eadamper:
              type: string
              description: ea댐퍼개도
            err:
              type: string
              description: 에러 상태
            faneadamper:
              type: string
              description: 송풍ea댐퍼개도
            fanmixdamper:
              type: string
              description: 송풍mix댐퍼개도
            fanoadamper:
              type: string
              description: 송풍oa댐퍼개도
            fanspeed:
              type: string
              description: 풍량상태
            filter:
              type: string
              description: 필터청소
            heateadamper:
              type: string
              description: 난방ea댐퍼개도
            heater:
              type: string
              description: 히터 운전 상태
            heatmixdamper:
              type: string
              description: 난방mix댐퍼개도
            heatoadamper:
              type: string
              description: 난방oa댐퍼개도
            humidifier:
              type: string
              description: 가습기 운전 상태
            humidity:
              type: string
              description: 가습
            lock:
              type: string
              description: 잠금
            mixdamper:
              type: string
              description: mix댐퍼개도
            mixinghumidity:
              type: string
              description: 혼합기습도
            mixingtemp:
              type: string
              description: 혼합기온도
            mode:
              type: string
              description: 운전모드
            oadamper:
              type: string
              description: oa댐퍼개도
            oper:
              type: string
              description: 동작
            outerhumidity:
              type: string
              description: 외기습도
            outertemp:
              type: string
              description: 외기온도
            set_co2:
              type: string
            sethumidity:
              type: string
              description: 목표습도
            settemp:
              type: string
              description: 실내설정온도
            smoke:
              type: string
              description: 화재감지
            supplyfan:
              type: string
              description: 급기 팬 운전 상태
            supplyfan1:
              type: string
              description: 1번 급기 팬 운전상태
            supplyfan2:
              type: string
              description: 2번 급기 팬 운전상태
            supplyfan3:
              type: string
              description: 3번 급기 팬 운전상태
            supplyfan4:
              type: string
              description: 4번 급기 팬 운전상태
            supplyfan5:
              type: string
              description: 5번 급기 팬 운전상태
            supplyfan6:
              type: string
              description: 6번 급기 팬 운전상태
            supplyfannum:
              type: string
              description: 급기 팬 개수
            supplyhumidity:
              type: string
              description: 급기습도
            supplytemp:
              type: string
              description: 급기온도
            ventfan:
              type: string
              description: 환기 팬 운전 상태
            ventfan1:
              type: string
              description: 1번 환기 팬 운전상태
            ventfan2:
              type: string
              description: 2번 환기 팬 운전상태
            ventfan3:
              type: string
              description: 3번 환기 팬 운전상태
            ventfan4:
              type: string
              description: 4번 환기 팬 운전상태
            ventfan5:
              type: string
              description: 5번 환기 팬 운전상태
            ventfan6:
              type: string
              description: 6번 환기 팬 운전상태
            ventfannum:
              type: string
              description: 환기 팬 개수
            venthumidity:
              type: string
              description: 환기습도(실내습도)
            venttemp:
              type: string
              description: 환기온도(실내온도)
        RespAirCareCtrlHistoryResult:
          title: RespAirCareCtrlHistoryResult
          type: object
          properties:
            account:
              type: string
              description: 제어계정
            affect:
              type: string
              description: 제어주체
            air_sensor:
              type: string
              description: 공기질 센서
            datetime:
              type: integer
              description: 제어시간
              format: int64
            filter_reset:
              type: string
              description: 필터 알림 리셋
            id_group:
              type: string
              description: 관리그룹 ID
            mode:
              type: string
              description: 운전모드
            oper:
              type: string
              description: 동작
            operlock:
              type: string
              description: 운전잠금
            scope:
              type: string
              description: 제어범위
            ui_cmd:
              type: string
        RespAircareStateResult:
          title: RespAircareStateResult
          type: object
          properties:
            air_sensor:
              type: string
              description: 공기질 센서
            cleanliness:
              type: string
              description: 종합청정도
            err:
              type: string
              description: 에러 상태
            fanspeed:
              type: string
              description: 청정세기
            filter:
              type: string
              description: 필터 알람
            filter_remain:
              type: string
              description: 필터 잔여량
            filter_reset:
              type: string
              description: 필터 알림 리셋
            fine_dust:
              type: string
              description: 미세먼지농도
            fine_dust_status:
              type: string
              description: 미세먼지상태
            mode:
              type: string
              description: 운전모드
            oper:
              type: string
              description: 동작
            operlock:
              type: string
              description: 운전잠금
            smell_status:
              type: string
              description: 냄새 상태
            superultrafine_dust:
              type: string
              description: 극초미세먼지농도
            superultrafine_dust_status:
              type: string
              description: 극초미세먼지상태
            ultrafine_dust:
              type: string
              description: 초미세먼지농도
            ultrafine_dust_status:
              type: string
              description: 초미세먼지상태
        RespAwhpCtrlHistoryResult:
          title: RespAwhpCtrlHistoryResult
          type: object
          properties:
            account:
              type: string
              description: 제어계정
            affect:
              type: string
              description: 제어주체
            awsettemp:
              type: string
              description: 공기/출수 설정온도
            datetime:
              type: integer
              description: 제어시간
              format: int64
            emergency_oper:
              type: string
              description: 비상운전(ON, OFF)
            hotwater:
              type: string
              description: 온수운전
            id_group:
              type: string
              description: 관리그룹 ID
            lock:
              type: string
              description: 잠금
            lownoisemode_oper:
              type: string
              description: 무소음 모드(ON, OFF)
            mode:
              type: string
              description: 운전모드
            oper:
              type: string
              description: 동작
            quickDHWheat_oper:
              type: string
              description: 파워급탕(ON, OFF)
            scope:
              type: string
              description: 제어범위
            waterouttempflag:
              type: string
              description: 온도표시설정
            watertanksettemp:
              type: string
              description: 급탕 희망온도
        RespAwhpStateResult:
          title: RespAwhpStateResult
          type: object
          properties:
            control_type:
              type: string
              description: |-
                제어 방식
                - watertank_control: 온수 제어
                - waterinout_control: 입출수 제어
                - dual_control: 겸용
              example: watertank_control, waterinout_control, dual_control
            droomtempflag:
              type: string
              description: |-
                현재온도 표시기준1
                - waterouttemp: 출수
                - waterintemp: 입수
                - roomtemp: 공기
                - watertanktemp: 온수
                - none: 사용안함
              example: waterouttemp, waterintemp, roomtemp, watertanktemp, none
            droomtempflag_2:
              type: string
              description: |-
                현재온도 표시기준2
                - waterouttemp: 출수
                - waterintemp: 입수
                - roomtemp: 공기
                - watertanktemp: 온수
                - none: 사용안함
              example: waterouttemp, waterintemp, roomtemp, watertanktemp, none
            emergency_oper:
              type: string
            err:
              type: string
              description: 에러 상태
            hotwater:
              type: string
              description: 온수운전
            lock:
              type: string
              description: 잠금
            lownoisemode_oper:
              type: string
              description: Silent mode 운전 여부
            mode:
              type: string
              description: 운전모드
            oper:
              type: string
              description: 동작
            pipein_temp:
              type: string
            pipeout_temp:
              type: string
            quick_dh_wheat_oper:
              type: string
            roomtemp:
              type: string
              description: 현재온도
            settemp:
              type: string
              description: 공기/출수 설정온도
            settemp_hotwater:
              type: string
              description: 급탕 희망온도
            sun_temp:
              type: string
              description: 태양열원 온도
            tank_temp:
              type: string
              description: 급탕 탱크 온도
            type:
              type: string
              description: AWHP 타입
            waterouttempflag:
              type: string
              description: 온도표시설정
        RespChillerCtrlHistoryResult:
          title: RespChillerCtrlHistoryResult
          type: object
          properties:
            account:
              type: string
              description: 제어계정
            affect:
              type: string
              description: 제어주체
            alarm_release:
              type: string
              description: 알람해제
            datetime:
              type: integer
              description: 제어시간
              format: int64
            mode:
              type: string
              description: 운전모드
            motor_tot_running_current_a:
              type: string
              description: 모터전류제한(J_TURBO)
            oper:
              type: string
              description: 동작
            scope:
              type: string
              description: 제어범위
            set_cool_water_temp:
              type: string
              description: 냉방설정온도
            set_demand_limit:
              type: string
              description: 디맨드제한율(J_TURBO)
            set_hot_water_temp:
              type: string
              description: 난방설정온도
            setdemandlimit:
              type: string
              description: 디맨드제한율
            setmotorlimit:
              type: string
              description: 모터전류제한
            ui_cmd:
              type: string
        RespChillerStateResult:
          title: RespChillerStateResult
          type: object
          properties:
            chilled_water_entering_temp:
              type: string
              description: 칠러입구온도
            chilled_water_leaving_temp:
              type: string
              description: 칠러출구온도
            controlvalvelimit:
              type: string
              description: 흡수식 칠어의 콘트롤 밸브 상한값
            detail_type:
              type: string
              description: 칠러 상세 타입
            error_code:
              type: string
              description: 에러 상태
            mode:
              type: string
              description: 운전모드
            oper:
              type: string
              description: 동작
            setcoolwatertemp:
              type: string
              description: 냉방설정온도
            setdemandlimit:
              type: string
              description: 디맨드제한율(J_TURBO)
            sethotwatertemp:
              type: string
              description: 난방설정온도
            seticewatertemp:
              type: string
              description: 제빙설정온도
            tot_running_current_a:
              type: string
              description: 모터전류제한(J_TURBO)
            type:
              type: string
              description: 칠러 타입
        RespCtrlHistoryResult:
          title: RespCtrlHistoryResult
          type: object
          properties:
            account:
              type: string
              description: 제어계정
            affect:
              type: string
              description: 제어주체
            datetime:
              type: integer
              description: 제어시간
              format: int64
            mode:
              type: string
              description: 운전모드
            oper:
              type: string
              description: 동작
            scope:
              type: string
              description: 제어범위
        RespCycleResult:
          title: RespCycleResult
          type: object
          properties:
            sensor_data_count:
              type: integer
              description: 조회 결과 데이터 개수
              format: int32
            sensor_data_end:
              type: integer
              description: 조회 결과 데이터의 끝 시간
              format: int64
            sensor_data_list:
              type: array
              description: 조회 결과 목록
              items:
                type: object
                additionalProperties:
                  type: string
            sensor_data_start:
              type: integer
              description: 조회 결과 데이터의 시작 시간
              format: int64
            sensor_id:
              type: string
              description: 개별 설비의 추상화된 식별자
        RespFacilitiesState:
          title: RespFacilitiesState
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              type: array
              description: 응답 결과
              items:
                $ref: '#/components/schemas/RespFacilityStateResult'
        RespFacilityCtrlHistAhu:
          title: RespFacilityCtrlHistAhu
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespAhuCtrlHistoryResult'
        RespFacilityCtrlHistAircare:
          title: RespFacilityCtrlHistAircare
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespAirCareCtrlHistoryResult'
        RespFacilityCtrlHistAwhp:
          title: RespFacilityCtrlHistAwhp
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespAwhpCtrlHistoryResult'
        RespFacilityCtrlHistChiller:
          title: RespFacilityCtrlHistChiller
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespChillerCtrlHistoryResult'
        RespFacilityCtrlHistIdu:
          title: RespFacilityCtrlHistIdu
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespIduCtrlHistoryResult'
        RespFacilityCtrlHistOdu:
          title: RespFacilityCtrlHistOdu
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespOduCtrlHistoryResult'
        RespFacilityCtrlHistVent:
          title: RespFacilityCtrlHistVent
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespVentCtrlHistoryResult'
        RespFacilityCycle:
          title: RespFacilityCycle
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespCycleResult'
        RespFacilityInfo:
          title: RespFacilityInfo
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespFacilityResult'
        RespFacilityList:
          title: RespFacilityList
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              type: array
              description: 응답 결과
              items:
                $ref: '#/components/schemas/RespFacilityResult'
        RespFacilityResult:
          title: RespFacilityResult
          type: object
          properties:
            facility_code:
              type: string
              description: 공조 설비 코드
            facility_id:
              type: string
              description: 공조 설비 아이디
            facility_name:
              type: string
              description: 공조 설비 이름
            facility_parent_code:
              type: string
              description: IDU 또는 UNIT 타입일 경우 상위의 ODU 설비를 지정
            facility_parent_id:
              type: string
              description: IDU 또는 UNIT 타입일 경우 상위의 ODU 설비를 지정
            facility_parent_name:
              type: string
              description: IDU 또는 UNIT 타입일 경우 상위의 ODU 설비를 지정
            facility_type:
              type: string
              description: 공조 설비 타입 구분(IDU,ODU,VENT,AWHP,AHU,CHLLER,AIRCARE)
            num_awhp_unit:
              type: integer
              description: ODU 타입일 경우 사용, ODU 하위에 설치된 AWHP 설비의 개수
              format: int32
            num_idu_unit:
              type: integer
              description: ODU 타입일 경우 사용, ODU 하위에 설치된 IDU 설비의 개수
              format: int32
            num_odu_unit:
              type: integer
              description: ODU 타입일 경우 사용, ODU 를 구성하고 있는 UNIT의 개수
              format: int32
        RespFacilityStateAhu:
          title: RespFacilityStateAhu
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespAhuStateResult'
        RespFacilityStateAircare:
          title: RespFacilityStateAircare
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespAircareStateResult'
        RespFacilityStateAwhp:
          title: RespFacilityStateAwhp
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespAwhpStateResult'
        RespFacilityStateChiller:
          title: RespFacilityStateChiller
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespChillerStateResult'
        RespFacilityStateIdu:
          title: RespFacilityStateIdu
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespIduStateResult'
        RespFacilityStateOdu:
          title: RespFacilityStateOdu
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespOduStateResult'
        RespFacilityStateResult:
          title: RespFacilityStateResult
          type: object
          properties:
            mode:
              type: string
              description: 운전모드
            oper:
              type: string
              description: 동작
        RespFacilityStateUnit:
          title: RespFacilityStateUnit
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespUnitStateResult'
        RespFacilityStateVent:
          title: RespFacilityStateVent
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespVentStateResult'
        RespGatewayInfo:
          title: RespGatewayInfo
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespGatewayResult'
        RespGatewayList:
          title: RespGatewayList
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              type: array
              description: 응답 결과
              items:
                $ref: '#/components/schemas/RespGatewayResult'
        RespGatewayResult:
          title: RespGatewayResult
          type: object
          properties:
            gw_id:
              type: string
              description: 게이트웨이 아이디
            gw_ip:
              type: string
              description: 게이트웨이 아이피
            gw_name:
              type: string
              description: 게이트웨이 이름
            gw_type:
              type: string
              description: 게이트웨이 타입
            mac:
              type: string
              description: 게이트웨에 MAC주소
            register_datetime:
              type: integer
              description: 게이트웨이 등록일
              format: int64
            version:
              type: string
              description: 게이트웨이 버전
        RespGatewayState:
          title: RespGatewayState
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespGatewayStateResult'
        RespGatewayStateResult:
          title: RespGatewayStateResult
          type: object
          properties:
            capacity_oper:
              type: string
              description: 용량제어 상태
            cycle_select:
              type: string
              description: 사이클 전송 유무
            cycle_status:
              type: string
              description: 사이클 모니터링 활성화 상태
            demand_conn:
              type: string
              description: 디맨드 연결 상태
            demand_status:
              type: string
              description: 디맨드 모드 설정. off일 경우 피크 모드
            emergency_stop_status:
              type: string
              description: 에너지 제어 관련 항목
            energy_capacity:
              type: string
              description: 에너지 명령(용량) 상태
            energy_capacity_set_rate:
              type: string
              description: 에너지 명령 용량률 상태
            energy_end_time:
              type: string
              description: 에너지제어 적용 종료 시간
            energy_operation:
              type: string
              description: 에너지 명령(운전률) 상태
            energy_operation_mode:
              type: string
              description: 에너지제어 모드(ODU, IDU)
            energy_operation_set_rate:
              type: string
              description: 에너지 명령 운전률 상태
            energy_operation_sleep_time:
              type: string
              description: 운전률 동작 주기
            energy_oversettemp:
              type: string
            energy_set_time:
              type: string
              description: 에너지제어 적용 상태
            energy_start_time:
              type: string
              description: 에너지제어 적용 시작 시간
            energy_timebase:
              type: string
              description: 시간기반 에너지 제어 상태
            energy_timebase_continue_time:
              type: string
            energy_timebase_no_working_time_end:
              type: string
              description: 비근무 종료 시간
            energy_timebase_no_working_time_start:
              type: string
              description: 비근무 시작 시간
            energy_weather:
              type: string
              description: on1(일반,일반사무건물) 또는 on2(절전,공기관) 또는 off
            log_status:
              type: string
              description: 에러 이력 활성화 상태
            operation_oper:
              type: string
              description: 실내기 운전 상태
            operation_rate:
              type: string
              description: 전체 현재 운전률
            pdi_status:
              type: string
              description: 전력량 활성화 상태
            sch_status:
              type: string
              description: 스케쥴 활성화 상태
            total_load_capa:
              type: string
              description: 부하용량
        RespIduCtrlHistoryResult:
          title: RespIduCtrlHistoryResult
          type: object
          properties:
            account:
              type: string
              description: 제어계정
            aco_lower:
              type: string
              description: 자동전환난방
            aco_oper:
              type: string
              description: 자동전환
            aco_upper:
              type: string
              description: 자동전환냉방
            affect:
              type: string
              description: 제어주체
            air_clean:
              type: string
              description: 공기청정설정 상태
            air_clean_by_oneself:
              type: string
              description: 공기청정설정. plasma와 조합하여 사용
            comfort:
              type: string
              description: 쾌적절전
            comfort_step:
              type: string
              description: 쾌적절전단계
            cool_lower_lim:
              type: string
              description: 냉방하한온도
            cool_settemp:
              type: string
              description: 냉방설정온도
            cool_upper_lim:
              type: string
              description: 냉방상한온도
            datetime:
              type: integer
              description: 제어시간
              format: int64
            energy_control:
              type: string
              description: 에너지 제어 여부(ON, OFF)
            fan:
              type: string
              description: 설정풍량
            fanlock:
              type: string
              description: 팬 잠금
            heat_lower_lim:
              type: string
              description: 난방하한온도
            heat_settemp:
              type: string
              description: 난방설정온도
            heat_upper_lim:
              type: string
              description: 난방상한온도
            heater_mode:
              type: string
              description: 히터조합난방 자동/수동
            heater_oper:
              type: string
              description: 히터조합난방 기준실외온도 설정/해제
            id_group:
              type: string
              description: 관리그룹 ID
            lock:
              type: string
              description: 잠금
            mode:
              type: string
              description: 운전모드
            modelock:
              type: string
              description: 모드 잠금
            occupied:
              type: string
              description: 재실/부재
            oper:
              type: string
              description: 동작
            outdoor_heater:
              type: string
              description: heater_mode = OFF일 때, 난방기 설정/해제
            outdoor_heatpump:
              type: string
              description: heater_mode = ON(자동) 일 때, 에어컨 설정/해제
            outdoor_temp1:
              type: string
              description: heater_mode = ON(자동) 일 때, 설정온도
            outdoor_temp2:
              type: string
              description: heater_mode = ON(자동) 일 때, 설정온도
            plasma:
              type: string
              description: 공기청정설정. air_clean_by_oneself와 조합하여 사용
            scope:
              type: string
              description: 제어범위
            setback_lower:
              type: string
              description: 온도제한냉방
            setback_oper:
              type: string
              description: 온도제한
            setback_upper:
              type: string
              description: 온도제한난방
            settemp:
              type: string
              description: 설정온도
            settemp_llim:
              type: string
              description: 설정온도 하한
            settemp_lock:
              type: string
              description: 설정온도 잠금
            settemp_ulim:
              type: string
              description: 설정온도 상한
            swing:
              type: string
              description: 풍향자동
            ui_cmd:
              type: string
        RespIduStateResult:
          title: RespIduStateResult
          type: object
          properties:
            aco_lower:
              type: string
              description: 자동전환난방
            aco_oper:
              type: string
              description: 자동전환
            aco_upper:
              type: string
              description: 자동전환냉방
            air_clean:
              type: string
              description: 공기청정 운전상태
            air_clean_avail:
              type: string
              description: 공기청정 지원상태
            air_clean_by_oneself:
              type: string
              description: 공기청정단독 운전상태
            air_clean_plasma_avail:
              type: string
              description: 공기청정 운전상태
            capacity:
              type: string
              description: 실내기 용량
            cmd7_avail:
              type: string
              description: CMD7 지원상태
            comfort:
              type: string
              description: 쾌적절전상태
            comfort_avail:
              type: string
              description: 쾌적절전 지원상태
            comfort_step:
              type: string
              description: 쾌적절전단계
            cool_lower_lim:
              type: string
              description: 냉방 하한 온도
            cool_settemp:
              type: string
              description: 냉방설정온도
            cool_upper_lim:
              type: string
              description: 냉방 상한 온도
            dustsensor_avail:
              type: string
              description: 공기청정단독 지원상태(2.0)
            energy_control:
              type: string
              description: 부하제어상태
            err:
              type: string
              description: 에러 상태
            fan:
              type: string
              description: 풍량
            fanlock:
              type: string
              description: 풍량잠금
            filter:
              type: string
              description: 필터 교체 알람
            fine_dust:
              type: string
              description: 미세먼지농도
            fine_dust_status:
              type: string
              description: 미세먼지상태
            heat_lower_lim:
              type: string
              description: 난방 하한 온도
            heat_settemp:
              type: string
              description: 난방설정온도
            heat_upper_lim:
              type: string
              description: 난방 상한 온도
            heater_oper:
              type: string
              description: 히터조합난방 기준실외온도 설정/해제
            humidity:
              type: string
              description: 습도
            icon:
              type: string
              description: 실내기 아이콘
            lev:
              type: string
              description: 실내기 LEV
            lock:
              type: string
              description: 잠금
            mode:
              type: string
              description: 운전모드
            modelock:
              type: string
              description: 운전모드잠금
            occupied:
              type: string
              description: 재실/부재
            oper:
              type: string
              description: 동작
            outdoor_avail:
              type: string
              description: 히터조합난방기준 실외온도 기능 유무
            outdoor_heater:
              type: string
              description: 히터조합난방 heater_mode = OFF일 때, 난방기 설정/해제
            outdoor_heatpump:
              type: string
              description: 히터조합난방 heater_mode = ON(자동) 일 때, 에어컨 설정/해제
            outdoor_temp1:
              type: string
              description: |-
                실외온도 히터제어 관련
                (실내기 중 히터 제어 기능이 추가된 모델의 경우 온도값 2개를 추가로 설정 가능)
            outdoor_temp2:
              type: string
              description: |-
                실외온도 히터제어 관련
                (실내기 중 히터 제어 기능이 추가된 모델의 경우 온도값 2개를 추가로 설정 가능)
            plasma:
              type: string
              description: 공기청정 운전상태
            plasma_avail:
              type: string
            plasma_failure:
              type: string
            roomtemp:
              type: string
              description: 실내온도
            setback_lower:
              type: string
              description: 온도제한냉방
            setback_oper:
              type: string
              description: 온도제한
            setback_upper:
              type: string
              description: 온도제한난방
            settemp:
              type: string
              description: 설정온도
            settemp_llim:
              type: string
              description: 설정온도 하한
            settemp_ulim:
              type: string
              description: 설정온도 상한
            settemplock:
              type: string
              description: 온도잠금
            superultrafine_dust:
              type: string
              description: 극초미세먼지농도
            superultrafine_dust_status:
              type: string
              description: 극초미세먼지상태
            swing:
              type: string
              description: 풍향 자동
            twoset_automode:
              type: string
              description: 2set 지원여부(리모컨포함)
            type:
              type: string
              description: 실내기 타입
            ultrafine_dust:
              type: string
              description: 초미세먼지농도
            ultrafine_dust_status:
              type: string
              description: 초미세먼지상태
        RespOduCtrlHistoryResult:
          title: RespOduCtrlHistoryResult
          type: object
          properties:
            ability_enhance_mode:
              type: string
              description: 용량제어
            ability_enhance_step:
              type: string
              description: 용량제어 단계
            account:
              type: string
              description: 제어계정
            additional_defrost:
              type: string
              description: 제상모드
            affect:
              type: string
              description: 제어주체
            datetime:
              type: integer
              description: 제어시간
              format: int64
            max_rpm:
              type: string
              description: 실외기 팬 최대 RPM제한 설정
            mode:
              type: string
              description: 운전모드
            night_low_noise:
              type: string
              description: '-'
            night_low_noise_end:
              type: string
              description: '-'
            night_low_noise_start:
              type: string
              description: '-'
            oper:
              type: string
              description: 동작
            remote_4way_off:
              type: string
              description: 사방병 OFF제어
            remote_defrost:
              type: string
              description: 수동제상운전
            remote_fdd_start:
              type: string
              description: '-'
            remote_oil_return:
              type: string
              description: 수동오일회수
            remote_refrigerant_noise_Step:
              type: string
              description: 실외기 냉매 소음절감 모드 단계
            remote_reset:
              type: string
              description: 실외기 재시작(SUPER5)
            remote_slc_Step:
              type: string
              description: 능동절전운전 단계
            scope:
              type: string
              description: 제어범위
            snow_removal:
              type: string
              description: 제설모드
        RespOduStateResult:
          title: RespOduStateResult
          type: object
          properties:
            ability_enhance:
              type: string
              description: 용량제어 상태
            ability_enhance_enable:
              type: string
              description: 용량제어 설정 상태
            ability_enhance_step:
              type: string
              description: 용량제어 단계
            add_defrost:
              type: string
              description: 제상모드 상태
            add_defrost_enable:
              type: string
              description: 제상모드 설정 상태
            air_temp:
              type: string
              description: 공기온도 AD값
            capacity:
              type: string
              description: 실외기 용량
            error:
              type: string
              description: 에러 상태
            fan1_micom_version:
              type: string
              description: 각 보드 SW 버전 정보 (micom 보드 2개 설치 모델의 경우)
            fan2_micom_version:
              type: string
              description: 각 보드 SW 버전 정보 (micom 보드 2개 설치 모델의 경우)
            fan_rpm_enable:
              type: string
              description: 실외기 팬 최대 RPM제한 설정 상태
            fan_rpm_step:
              type: string
              description: 실외기 팬 RPM 설정값
            inv1_comp_micom_version:
              type: string
              description: 각 보드 SW 버전 정보 (micom 보드 2개 설치 모델의 경우)
            inv2_comp_micom_version:
              type: string
              description: 각 보드 SW 버전 정보 (micom 보드 2개 설치 모델의 경우)
            micom_ver:
              type: string
              description: 실외기 버전
            mode:
              type: string
              description: 운전모드
            night_low_noise:
              type: string
              description: 실외기저소음운전
            night_low_noise_enable:
              type: string
              description: 실외기저소음운전 지원 여부
            oper:
              type: string
              description: 동작
            opermode:
              type: string
              description: 실외기 운전 모드
            refrigerant:
              type: string
              description: 냉매 종류
            remote_4way_off:
              type: string
              description: 사방변 OFF제어 상태
            remote_4way_off_enable:
              type: string
              description: 사방변 OFF제어 설정 상태
            remote_cal_refrigerant:
              type: string
              description: 계산된 냉매량
            remote_defrost:
              type: string
              description: 수동제상운전 상태
            remote_defrost_enable:
              type: string
              description: 수동제상운전 설정 상태
            remote_fdd_start:
              type: string
              description: 시운전
            remote_fdd_start_enable:
              type: string
              description: 시운전 지원 여부
            remote_oil_return:
              type: string
              description: 수동오일회수 상태
            remote_oil_return_enable:
              type: string
              description: 수동오일회수 설정 상태
            remote_refrigerant_noise:
              type: string
              description: 실외기 냉매 소음절감 모드 상태
            remote_refrigerant_noise_enable:
              type: string
              description: 실외기 냉매 소음절감 모드 설정 상태
            remote_refrigerant_noise_step:
              type: string
              description: 실외기 냉매 소음절감 모드 단계
            remote_reset:
              type: string
              description: 실외기 재시작 상태
            remote_reset_enable:
              type: string
              description: 실외기 재시작 설정 상태
            remote_slc:
              type: string
              description: 능동절전운전 상태
            remote_slc_enable:
              type: string
              description: 능동절전운전 설정 상태
            remote_slc_step:
              type: string
              description: 능동절전운전 단계
            remote_total_refrigerant:
              type: string
              description: 총 냉매량
            remove_snow:
              type: string
              description: 제설모드 상태
            remove_snow_enable:
              type: string
              description: 제설모드 설정 상태
            slave_unit:
              type: string
              description: UNIT 수
            sync_micom_version:
              type: string
              description: 실외기 싱크 버전
            target_high_pressure:
              type: string
              description: 목표고압
            target_low_pressure:
              type: string
              description: 목표저압
            target_sub_cool:
              type: string
              description: 목표과냉도
            type:
              type: string
              description: 실외기 타입
        RespSiteInfo:
          title: RespSiteInfo
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespSiteResult'
        RespSiteList:
          title: RespSiteList
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              type: array
              description: 응답 결과
              items:
                $ref: '#/components/schemas/RespSiteResult'
        RespSiteResult:
          title: RespSiteResult
          type: object
          properties:
            country:
              type: string
              description: 현장 리전
            latitude:
              type: string
              description: 현장 좌표(위도)
            longitude:
              type: string
              description: 현장 좌표(경도)
            register_datetime:
              type: integer
              description: 현장 등록시간
              format: int64
            site_addr:
              type: string
              description: 현장 주소
            site_code:
              type: string
              description: 현장 코드
            site_name:
              type: string
              description: 현장 이름
        RespString:
          title: RespString
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              type: string
              description: 응답 결과
        RespUnitStateResult:
          title: RespUnitStateResult
          type: object
          properties:
            fan1_micom_ver:
              type: string
              description: 각 보드 SW 버전 정보 (micom 보드 2개 설치 모델의 경우)
            fan2_micom_ver:
              type: string
              description: 각 보드 SW 버전 정보 (micom 보드 2개 설치 모델의 경우)
            id_unit:
              type: string
              description: 유닛 아이디
            inv1_micom_ver:
              type: string
              description: 각 보드 SW 버전 정보 (micom 보드 2개 설치 모델의 경우)
            inv2_micom_ver:
              type: string
              description: 각 보드 SW 버전 정보 (micom 보드 2개 설치 모델의 경우)
            micom_sync_ver:
              type: string
              description: MICOM 싱크 버전
            micom_ver:
              type: string
              description: MICOM 버전
            mode:
              type: string
              description: 운전모드
            onboarding_status:
              type: string
              description: 온보딩 상태
            onboarding_time:
              type: string
              description: 온보딩 시간
            oper:
              type: string
              description: 동작
        RespVentCtrlHistoryResult:
          title: RespVentCtrlHistoryResult
          type: object
          properties:
            ac_mode:
              type: string
              description: 에어컨운전모드
            ac_oper:
              type: string
              description: 에어컨운전상태
            account:
              type: string
              description: 제어계정
            affect:
              type: string
              description: 제어주체
            datetime:
              type: integer
              description: 제어시간
              format: int64
            fan:
              type: string
              description: 설정풍량
            fanlock:
              type: string
              description: 팬 잠금
            filter:
              type: string
              description: 필터
            function:
              type: string
              description: 부가기능
            heater:
              type: string
              description: 히터
            humidity:
              type: string
              description: 가습
            id_group:
              type: string
              description: 관리그룹 ID
            lock:
              type: string
              description: 잠금
            mode:
              type: string
              description: 운전모드
            modelock:
              type: string
              description: 모드 잠금
            ntfc_oper:
              type: string
              description: 환기 NTFC 운전
            ntfclock:
              type: string
              description: 환기 NTFC 잠금
            oper:
              type: string
              description: 동작
            plasma:
              type: string
              description: 플라스마
            scope:
              type: string
              description: 제어범위
            set_temp:
              type: string
              description: 설정온도
            settemp_llim:
              type: string
              description: 설정온도하한
            settemp_ulim:
              type: string
              description: 설정온도상한
            settemplock:
              type: string
              description: 온도 잠금
            ui_cmd:
              type: string
        RespVentStateResult:
          title: RespVentStateResult
          type: object
          properties:
            ac_mode:
              type: string
              description: 에어컨운전모드
            ac_oper:
              type: string
              description: 에어컨운전상태
            co2:
              type: string
              description: CO2 농도(2.0 설비)
            co2value:
              type: string
              description: CO2 농도(3.0 설비)
            err:
              type: string
              description: 에러 상태
            fan:
              type: string
              description: 풍량
            fanlock:
              type: string
              description: 팬 잠금
            filter:
              type: string
              description: 필터
            function:
              type: string
              description: 부가기능
            heater:
              type: string
              description: 히터
            humidify:
              type: string
              description: 가습
            lock:
              type: string
              description: 잠금
            master:
              type: string
              description: 마스터 여부
            mode:
              type: string
              description: 운전모드
            modelock:
              type: string
              description: 모드 잠금
            ntfc_avail:
              type: string
              description: 환기 NTFC 지원 상태
            ntfc_oper:
              type: string
              description: 환기 NTFC 운전 상태
            ntfclock:
              type: string
              description: 환기 NTFC 잠금
            oper:
              type: string
              description: 동작
            settemp:
              type: string
              description: 설정온도
            settemp_llim:
              type: string
              description: 설정온도하한
            settemp_ulim:
              type: string
              description: 설정온도상한
            settemplock:
              type: string
              description: 온도 잠금
            type:
              type: string
              description: 타입
            vent_cmd4_avail:
              type: string
              description: 환기 CMD4 지원 상태
        Resp«Array«RespCtrlHistoryResult»»:
          title: Resp«Array«RespCtrlHistoryResult»»
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              type: array
              description: 응답 결과
              items:
                $ref: '#/components/schemas/RespCtrlHistoryResult'
        Resp«Array«RespFacilityResult»»:
          title: Resp«Array«RespFacilityResult»»
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              type: array
              description: 응답 결과
              items:
                $ref: '#/components/schemas/RespFacilityResult'
        Resp«Array«RespGatewayResult»»:
          title: Resp«Array«RespGatewayResult»»
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              type: array
              description: 응답 결과
              items:
                $ref: '#/components/schemas/RespGatewayResult'
        Resp«Array«RespSiteResult»»:
          title: Resp«Array«RespSiteResult»»
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              type: array
              description: 응답 결과
              items:
                $ref: '#/components/schemas/RespSiteResult'
        Resp«List«Map«string,object»»»:
          title: Resp«List«Map«string,object»»»
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              type: array
              description: 응답 결과
              items:
                type: object
                additionalProperties:
                  type: object
        Resp«RespCycleResult»:
          title: Resp«RespCycleResult»
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespCycleResult'
        Resp«RespFacilityResult»:
          title: Resp«RespFacilityResult»
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespFacilityResult'
        Resp«RespFacilityStateResult»:
          title: Resp«RespFacilityStateResult»
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespFacilityStateResult'
        Resp«RespGatewayResult»:
          title: Resp«RespGatewayResult»
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespGatewayResult'
        Resp«RespGatewayStateResult»:
          title: Resp«RespGatewayStateResult»
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespGatewayStateResult'
        Resp«RespSiteResult»:
          title: Resp«RespSiteResult»
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              description: 응답 결과
              $ref: '#/components/schemas/RespSiteResult'
        Resp«string»:
          title: Resp«string»
          type: object
          properties:
            code:
              type: string
              description: 상태 코드
            msg:
              type: string
              description: 상태 메시지
            result:
              type: string
              description: 응답 결과
---

---
contents:
  lang: yaml
  code: >
    openapi: 3.0.1

    info:
      title: OpenAPI definition
      version: v0
    servers:

    - url: http://localhost:8080
      description: Generated server url
    paths:
      /realms/{domain}/{serverType}/protocol/openid-connect/token/introspect:
        post:
          tags:
          - route-global-controller
          summary: NTH.004
          operationId: keycloakIntrospect
          parameters:
          - name: domain
            in: path
            required: true
            schema:
              type: string
          - name: serverType
            in: path
            required: true
            schema:
              type: string
          - name: Authorization
            in: header
          requestBody:
            content:
              application/x-www-form-urlencoded:
                schema:
                  $ref: '#/components/schemas/RouteIntrospectRequest'
          responses:
            "200":
              description: OK
              content:
                '*/*':
                  schema:
                    type: string
      /realms/{domain}/{serverType}/protocol/lge-openid-connect/token/introspect:
        post:
          tags:
          - route-global-controller-lge
          summary: NTH.004.1
          operationId: keycloakIntrospect_1
          parameters:
          - name: domain
            in: path
            required: true
            schema:
              type: string
          - name: serverType
            in: path
            required: true
            schema:
              type: string
          - name: Authorization
            in: header
          requestBody:
            content:
              application/x-www-form-urlencoded:
                schema:
                  $ref: '#/components/schemas/RouteIntrospectRequest'
          responses:
            "200":
              description: OK
              content:
                '*/*':
                  schema:
                    type: string
      /realms/{domain}/{serverType}/protocol/openid-connect/userinfo:
        get:
          tags:
          - route-global-controller
          summary: NTH.006
          operationId: keycloakUserInfo
          parameters:
          - name: domain
            in: path
            required: true
            schema:
              type: string
          - name: serverType
            in: path
            required: true
            schema:
              type: string
          - name: Authorization
            in: header
          responses:
            "200":
              description: OK
              content:
                '*/*':
                  schema:
                    type: string
      /realms/{domain}/{serverType}/protocol/lge-openid-connect/userinfo:
        get:
          tags:
          - route-global-controller-lge
          summary: NTH.006.1
          operationId: keycloakUserInfo_1
          parameters:
          - name: domain
            in: path
            required: true
            schema:
              type: string
          - name: serverType
            in: path
            required: true
            schema:
              type: string
          - name: Authorization
            in: header
          responses:
            "200":
              description: OK
              content:
                '*/*':
                  schema:
                    type: string
      /lokr/health:
        get:
          tags:
          - health-controller
          operationId: health
          responses:
            "200":
              description: OK
    components:
      schemas:
        RouteIntrospectRequest:
          type: object
          properties:
            token:
              type: string
---

# AsyncApi

[AsyncAPI](https://www.asyncapi.com/blog/understanding-asyncapis) — это открытый стандарт и спецификация, предназначенные для
описания событийно-ориентированных архитектур и асинхронных коммуникаций между сервисами. Он позволяет формализовать и
документировать взаимодействия в системах, основанных на обмене сообщениями, таких как микросервисы, IoT-устройства и
распределённые приложения.

Пример контракта:
```yaml
asyncapi: 2.0.0
info:
  title: Account Service
  version: "1.0.0"
  description: |
    Manages user accounts in the system.
  license:
    name: Apache 2.0
    url: https://www.apache.org/licenses/LICENSE-2.0

servers:
  production:
    url: mqtt://test.mosquitto.org
    protocol: mqtt
    description: Test MQTT broker

channels:
  user/signedup:
    subscribe:
      operationId: emitUserSignUpEvent
      message:
        $ref: "#/components/messages/UserSignedUp"

components:
  messages:
    UserSignedUp:
      name: userSignedUp
      title: User signed up event
      summary: Inform about a new user registration in the system
      contentType: application/json
      payload:
        $ref: "#/components/schemas/userSignedUpPayload"

  schemas:
    userSignedUpPayload:
      type: object
      properties:
        firstName:
          type: string
          description: "foo"
        lastName:
          type: string
          description: "bar"
        email:
          type: string
          format: email
          description: "baz"
        createdAt:
          type: string
          format: date-time
```

Код DocHub:
```code-frame
/docs/dochub.presentations.asyncapi.example
```

Результат:

![AsyncAPI контракт](@document/dochub.presentations.asyncapi.example)



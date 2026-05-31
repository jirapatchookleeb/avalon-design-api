- Socket Message

```json
{
  "stage": "LOBBY",
  "type": "JOIN_ROOM",
  "message": {
    "user_name": "donald548"
  }
}
```

- Socket Broadcast

```json
    "data":{
        "userId":"ad93e256-f1e4-489a-b3d9-27f53f6cf5b6",
        "userName":"donald548" //true,false
    }
```

```mermaid
sequenceDiagram
    participant Client
    participant Server-Socket
    participant DB
    participant Socket-Context

    Client->>Server-Socket:Connect by room_id

    alt room_id not exist
        Server-Socket-->>Client:Connection Failed
    end

    Server-Socket->>DB:CREATE user

    alt Create data failed
        DB-->>Server-Socket: Failed to create data
        Server-Socket-->>Client: Connection Failed
    else Create succeed
        DB-->>Server-Socket: Created Succeed
    end

    Server-Socket->>Socket-Context: Save user_id

    Server-Socket->>DB:CREATE room_user
    alt Create data failed
        DB-->>Server-Socket: Failed to create data
        Server-Socket-->>Client: Connection Failed
    else Create succeed
        DB-->>Server-Socket: Created Succeed
    end

    Note over Client,Server-Socket: Broadcast updated room player

    Server-Socket-->>Client:Join room succeed
```

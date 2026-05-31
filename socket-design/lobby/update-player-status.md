- Socket Message

```json
{
  "stage": "LOBBY",
  "type": "UPDATE_STATUS",
  "message": {
    "status": true // true,false
  }
}
```

- Socket Broadcast

```json
{
  "data": {
    "userId": "ad93e256-f1e4-489a-b3d9-27f53f6cf5b6",
    "status": true //true , false
  }
}
```

```mermaid
sequenceDiagram
    participant Client
    participant Server-Socket
    participant DB

    Client->>Server-Socket:connect by room_id

    alt room_id not exist
        Server-Socket-->>Client:Connection Failed
    end

    Server-Socket->>DB:UPDATE room_user(req.message.status)

    alt Update data failed
        DB-->>Server-Socket: Failed to update data
        Server-Socket-->>Client: Connection Failed
    else Create succeed
        DB-->>Server-Socket: Updated Succeed
    end

    Note over Client,Server-Socket: Broadcast updated room player(player status)

    Server-Socket-->>Client:Send socket broadcast
```

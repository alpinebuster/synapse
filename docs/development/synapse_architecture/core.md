# Core

## Network Request Processing Flow in Synapse
┌────────────────────────────────────────────────────────┐
│ 1. Client Sends HTTP Request                           │
│ POST /rooms/{roomId}/send/m.room.message               │
│ Body: {"body": "Hello World"}                          │
└───────────────┬────────────────────────────────────────┘
                ↓
┌────────────────────────────────────────────────────────┐
│ 2. Twisted Web Server Receives Request                 │
│ (synapse/app/homeserver.py -> _listener_http)          │
│ - Listens on HTTP ports                                │
│ - Routes requests to the corresponding Resource        │
└───────────────┬────────────────────────────────────────┘
                ↓
┌────────────────────────────────────────────────────────┐
│ 3. REST Resource Routing (synapse/rest/__init__.py)    │
│ ClientRestResource                                     │
│ - Registers all REST API endpoints                     │
│ - Maps URLs to specific Servlets                       │
└───────────────┬────────────────────────────────────────┘
                ↓
┌────────────────────────────────────────────────────────┐
│ 4. Specific REST Servlet                               │
│ (synapse/rest/client/rooms.py)                         │
│ class RoomsSendEventRestServlet:                       │
│   async def on_POST(self, request)                     │
│ - Parameter validation                                 │
│ - Calls Handler                                        │
└───────────────┬────────────────────────────────────────┘
                ↓
┌────────────────────────────────────────────────────────┐
│ 5. Business Logic Handler                              │
│ (synapse/handlers/message.py)                          │
│ class EventCreationHandler:                            │
│   async def create_and_send_event()                    │
│ - Permission checks                                    │
│ - Event construction                                   │
│ - Calls Controller                                     │
└───────────────┬────────────────────────────────────────┘
                ↓
┌────────────────────────────────────────────────────────┐
│ 6. Data Control Layer (Controller)                     │
│ (synapse/storage/controllers/persist_events.py)        │
│ class PersistEventsController:                         │
│   async def persist(event)                             │
│ - Coordinates multiple data operations                 │
│ - Transaction management                               │
│ - Calls multiple DAOs                                  │
└───────────────┬────────────────────────────────────────┘
                ↓
┌────────────────────────────────────────────────────────┐
│ 7. Data Access Layer (DAO)                             │
│ (synapse/storage/databases/main/)                      │
│ - EventsStore.insert_event()                           │
│ - RoomMemberStore.update_member()                      │
│ - StateStore.update_state()                            │
│ - StreamStore.add_to_stream()                          │
│ Executes concrete SQL queries                          │
└───────────────┬────────────────────────────────────────┘
                ↓
┌────────────────────────────────────────────────────────┐
│ 8. Database Connection Layer                           │
│ (synapse/storage/database.py)                          │
│ - Manages connection pool                              │
│ - Begins/commits/rolls back transactions               │
│ - Executes SQL                                         │
└───────────────┬────────────────────────────────────────┘
                ↓
┌────────────────────────────────────────────────────────┐
│ 9. Database Engine (Driver)                            │
│ (synapse/storage/engines/)                             │
│ - PostgreSQL or SQLite adapter                         │
│ - Converts SQL into database-specific dialect          │
└───────────────┬────────────────────────────────────────┘
                ↓
         🗄️ Real Database (PostgreSQL/SQLite)
                ↓
┌────────────────────────────────────────────────────────┐
│ 10. Return Response                                    │
│ Result propagates back layer by layer                  │
│ Handler → Servlet → REST Resource → HTTP Response      │
└───────────────┬────────────────────────────────────────┘
                ↓
         📱 Client Receives Response

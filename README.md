# 📞 How to Create a Teams Communication App

This guide outlines the steps required to create a communication app using Microsoft Teams via Microsoft Graph API. All actions require authentication using OAuth 2.0, and the obtained token must be included in the request headers (`Authorization: Bearer <token>`).

----------

## 🔐 1. Authenticate Using OAuth

Use OAuth 2.0 to authenticate users and acquire an access token. This token must be used in all subsequent API requests to Microsoft Graph.

----------

## 👥 2. Sync User List

To add users to a group chat, you need their Microsoft Graph `userId`. Use the following API to retrieve the list of users:

```
GET https://graph.microsoft.com/v1.0/users

```

Include the `Authorization` header with the bearer token in your request.

----------

## 💬 3. Create a Group Chat

Once you have the `userId`s, create a group chat by adding the relevant users:

```
POST https://graph.microsoft.com/v1.0/chats
Content-Type: application/json
Authorization: Bearer <token>

```

### Request Body:

```json
{
  "chatType": "group",
  "topic": "Group chat title",
  "members": [
    {
      "@odata.type": "#microsoft.graph.aadUserConversationMember",
      "roles": ["owner"],
      "user@odata.bind": "https://graph.microsoft.com/v1.0/users('USER_ID_1')"
    },
    {
      "@odata.type": "#microsoft.graph.aadUserConversationMember",
      "roles": ["owner"],
      "user@odata.bind": "https://graph.microsoft.com/v1.0/users('USER_ID_2')"
    }
  ]
}

```

Replace `USER_ID_1` and `USER_ID_2` with actual user IDs retrieved earlier.

----------

## 🆔 4. Get the Chat ID

The response from the `POST /chats` request will include a `chat-id`. This ID is required for sending messages and retrieving chat history.

----------

## 📩 5. Send a Message or Notification

Use the `chat-id` to send messages to the group chat:

```
POST https://graph.microsoft.com/v1.0/chats/{chat-id}/messages
Content-Type: application/json
Authorization: Bearer <token>

```

### Request Body:

```json
{
  "body": {
    "content": "Hello World"
  }
}

```

Replace `{chat-id}` with the actual chat ID.

----------

## 🕓 6. Retrieve Chat History

To get the conversation history (on demand or when a ticket is closed), use:

```
GET https://graph.microsoft.com/v1.0/chats/{chat-id}/messages
Authorization: Bearer <token>

```

This will return the list of messages exchanged in the chat.


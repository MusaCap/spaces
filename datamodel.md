# 🧩 Spaces Platform – Data Model

This data model supports a modular social media platform where users interact through multiple, optionally integrated "Spaces" (e.g., Work, Friends, Family).

---

## 🧱 Entities and Relationships

### 1. User

Represents an individual on the platform.

| Field           | Type      | Description                      |
|------------------|-----------|----------------------------------|
| user_id          | UUID      | Primary key                      |
| email            | String    | Unique email                     |
| password_hash    | String    | Hashed password                  |
| global_name      | String    | Display name                     |
| profile_pic      | URL       | Global profile image             |
| joined_at        | Timestamp | Registration date                |

**Relationships**:
- One User → Many UserProfiles (1:n)
- One User → Many Spaces via SpaceMembership (n:n)

---

### 2. UserProfile

Space-specific user identity.

| Field        | Type    | Description                                 |
|---------------|---------|---------------------------------------------|
| profile_id    | UUID    | Primary key                                 |
| user_id       | UUID    | FK to User                                  |
| space_id      | UUID    | FK to Space                                 |
| nickname      | String  | Display name within the Space               |
| avatar_url    | URL     | Avatar for this Space                       |
| bio           | Text    | Short intro                                 |
| visibility    | Enum    | (Public, Members-only, Private)             |

---

### 3. Space

A container for a context-based group.

| Field        | Type      | Description                                 |
|---------------|-----------|---------------------------------------------|
| space_id      | UUID      | Primary key                                 |
| name          | String    | Space name                                  |
| type          | Enum      | (Work, Friends, Family, Custom)             |
| owner_id      | UUID      | FK to User (creator)                        |
| is_private     | Boolean   | If true, invite-only                        |
| created_at     | Timestamp | Creation date                               |

**Relationships**:
- One Space → Many UserProfiles
- One Space → Many Posts, Messages, Chats

---

### 4. SpaceMembership

Joins users and Spaces.

| Field          | Type      | Description                          |
|-----------------|-----------|--------------------------------------|
| membership_id   | UUID      | Primary key                          |
| user_id         | UUID      | FK to User                           |
| space_id        | UUID      | FK to Space                          |
| role            | Enum      | (Admin, Moderator, Member, Viewer)   |
| status          | Enum      | (Active, Invited, Banned)            |
| joined_at       | Timestamp | Date joined                          |

---

### 5. Post

Content posted inside a Space.

| Field         | Type      | Description                                  |
|----------------|-----------|----------------------------------------------|
| post_id        | UUID      | Primary key                                  |
| space_id       | UUID      | FK to Space                                  |
| author_id      | UUID      | FK to User                                   |
| content        | Text      | Post text                                    |
| media_url      | URL       | (Optional) media attachment                  |
| visibility     | Enum      | (This Space, Linked Spaces, Global)          |
| created_at     | Timestamp | Posted time                                  |

---

### 6. Message

Chat message in a ChatGroup.

| Field         | Type      | Description                        |
|----------------|-----------|------------------------------------|
| message_id     | UUID      | Primary key                        |
| space_id       | UUID      | FK to Space                        |
| sender_id      | UUID      | FK to User                         |
| chat_id        | UUID      | FK to ChatGroup                    |
| content        | Text      | Message text                       |
| timestamp      | Timestamp | Sent time                          |

---

### 7. ChatGroup

Group or 1:1 conversation.

| Field         | Type      | Description                          |
|----------------|-----------|--------------------------------------|
| chat_id        | UUID      | Primary key                          |
| space_id       | UUID      | FK to Space                          |
| name           | String    | Chat group name (optional)           |
| is_private     | Boolean   | Visibility restriction               |

---

### 8. SpaceLinkage

Cross-space sharing logic.

| Field           | Type      | Description                                |
|------------------|-----------|--------------------------------------------|
| link_id          | UUID      | Primary key                                |
| from_space_id    | UUID      | FK to Space                                |
| to_space_id      | UUID      | FK to Space                                |
| integration_type | Enum      | (Read-only, Share-to, Full Sync)           |
| enabled_by       | UUID      | FK to User who enabled it                  |
| created_at       | Timestamp | Link creation time                         |

---

### 9. Notification

Tracks aler

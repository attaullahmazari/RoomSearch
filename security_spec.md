# RoomSearch Security Specification

## Data Invariants
1. A **User** profile can only be created by the authenticated user with the matching UID.
2. A **Hostel** can only be created by a user with the 'owner' role.
3. A **Hostel** update (except for verification) can only be performed by its owner.
4. Only an **Admin** can verify a hostel (set `isVerified: true`).
5. A **Booking** can only be created by a 'student'.
6. A **Booking** status update ('confirmed'/'cancelled') can be done by the owner of the hostel or the student who booked it (with restrictions).
7. **Chats** and **Messages** are only accessible to participants of that chat.
8. **PII** (emails) must be protected.

## The Dirty Dozen Payloads (Rejection Tests)
1. **Identity Spoofing**: Attempt to create a user profile for UID 'A' while authenticated as UID 'B'.
2. **Role Escalation**: Attempt to set `role: 'admin'` during registration.
3. **Ghost verified**: Attempt to create a hostel with `isVerified: true` as an owner.
4. **Price Poisoning**: Attempt to set `price: -100` or a non-numeric value.
5. **Unauthorized Update**: Attempt to edit a hostel listing owned by another user.
6. **Shadow Booking**: Attempt to book as an 'owner' role.
7. **Orphaned Message**: Attempt to send a message to a chat the user is not a participant of.
8. **Metadata Injection**: Attempt to change `createdAt` on an existing document.
9. **Chat Scraping**: Attempt to list messages of a chat the user is not in.
10. **ID Poisoning**: Attempt to use a 2MB string as a document ID.
11. **Verification Bypass**: Attempt to verify own hostel by spoofing admin doc existence (harder to do, but rules should check real admin collection).
12. **PII Leak**: Attempt to read another user's email via list query without proper filters.

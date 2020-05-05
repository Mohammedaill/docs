---
section: libraries
toc: true
description: User Management with Auth0.Swift
topics:
  - libraries
  - swift
  - users
contentType: how-to
useCase: enable-mobile-auth
---
# User Management with Auth0.Swift

The Management API provides functionality that allows you to link and unlink separate user accounts from different providers, tying them to a single profile (see [User Account Linking](/users/concepts/overview-user-account-linking) for details). It also allows you to update user metadata.

## Link users

Linking user accounts will allow a user to authenticate from any of their accounts and no matter which one they use, still pull up the same profile upon login. Auth0 treats all of these accounts as separate profiles by default, so if you wish a user's accounts to be linked, this is the way to go.

The `link` method accepts two parameters, the primary user id and the secondary user token (the token obtained after login with this identity). The user id in question is the unique identifier for this user account. If the id is in the format `facebook|1234567890`, the id required is the portion after the delimiting pipe.

```swift
Auth0
   .users(token: "user-scoped access token")
   .link(userId, withOtherUserToken: "another user token")
   .start { result in
      switch result {
      case .success(let userInfo):
        print("User: \(userInfo)")
      case .failure(let error):
        print(error)
      }
   }
```

## Unlink users

Unlinking users is a similar process to the linking of users. The `unlink` method takes three parameters, though: the secondary user id, and the secondary provider (the provider of the secondary user), and the primary user id.
The parameters read, essentially: "Unlink this **secondary user** (with this **provider**) from this **primary user**".

```swift
Auth0
   .users(token: "user-scoped access token")
   .unlink(identityId: identifier, provider: provider, fromUserId:userId)
   .start { result in
      switch result {
      case .success(let userInfo):
        print("User: \(userInfo)")
      case .failure(let error):
        print(error)
      }
   }
```

::: note
Note that when accounts are linked, the secondary account's metadata is not merged with the primary account's metadata. Similarly, when unlinking two accounts, the secondary account does not retain the primary account's metadata when it becomes separate again.
:::

## Retrieve user metadata

```swift
Auth0
    .users(token: "user-scoped access token")
    .get(userId, fields: ["user_metadata"], include: true)
    .start { result in
        switch result {
        case .success(let userInfo):
            print("User: \(userInfo)")
        case .failure(let error):
            print(error)
        }
    }
```

## Update user metadata

When updating user metadata, you will create a `userMetadata` object, and then call the `patch` method, passing it the user id and the `userMetadata` object. The values in this object will overwrite existing values with the same key, or add new ones for those that don't yet exist in the user metadata.

```swift
Auth0
    .users(token: "user-scoped access token")
    .patch("user identifier", userMetadata: ["first_name": "John", "last_name": "Doe"])
    .start { result in
        switch result {
        case .success(let userInfo):
          print("User: \(userInfo)")
        case .failure(let error):
          print(error)
        }
    }
```

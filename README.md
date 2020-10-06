# ISteamFriends-EFriendRelationship
### Steam ISteamFriends API EFriendRelationship enum demystified

I put this repo up for anyone, who like me, tries to figure out what the values of the [EFriendRelationship](https://partner.steamgames.com/doc/api/ISteamFriends#EFriendRelationship) enum actually mean.

After getting utterly confused with the names and comments, I spend an hour testing this out with a friend. We unfriended, requested, ignored, blocked etc each other until we pinned down what each enum value actually means.

`**tl;dr: "Blocked" is "Ignored". "Ignored" is "Blocked". "IgnoredFriend" is complicated.**`


### Language clarification

I will use `LOCAL USER` and `REMOTE USER` here.
* `LOCAL USER` is the user of the Steam account your app is currently running on.
* `REMOTE USER` is the user identified by the steam id you pass to `GetFriendRelationship()`.


### enum value definitions

`k_EFriendRelationshipNone (0)`
* What it says. There is no relationship between the LOCAL USER and the REMOTE USER.

`k_EFriendRelationshipBlocked (1)` **IGNORE THE NAME, NO ONE HAS BEEN BLOCKED!**
* This state just means that the LOCAL USER has JUST NOW ignored a friend request from the REMOTE USER. This state is temporary and not stored anywhere.

`k_EFriendRelationshipRequestRecipient (2)`
* The REMOTE USER has requested to be friends with the LOCAL USER.

`k_EFriendRelationshipFriend (3)`
* What it says. The LOCAL USER and the REMOTE USER are friends on Steam.

`k_EFriendRelationshipRequestInitiator (4)`
* The LOCAL USER has requested to be friends with the REMOTE USER.

`k_EFriendRelationshipIgnored (5)` **IGNORE THE NAME, THIS MEANS BLOCKED!**
* The LOCAL USER has explicitly BLOCKED the REMOTE USER from comments/chat/etc. This state is permanent and stored.

`k_EFriendRelationshipIgnoredFriend (6)`
* This is a combination of `k_EFriendRelationshipIgnored` and `k_EFriendRelationshipFriend`. The LOCAL USER and the REMOTE USER are friends on Steam. AND the LOCAL USER has explicitly BLOCKED the REMOTE USER, but without unfriending them. (In the Steam friends menu this is called "Block All Communication".)

`k_EFriendRelationshipSuggested_DEPRECATED (7)`
* Deprecated -- Unused.

`k_EFriendRelationshipMax (8)`
* The total number of friend relationships used for looping and verification.


### Important to know about rejecting ("ignoring") friend requests:

Please note that when a user ignores a friend request, the requesting user knows that immediately, because their state changes from "RequestInitiator" to "None" immediately! So this should actually not be called "ignore", it should be called "reject".

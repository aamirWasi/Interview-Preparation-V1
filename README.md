# Interview-Preparation-V1
Q1: What is the main difference between the HTTP PUT and PATCH methods?

Answer:
1. PUT is used to update a resource by replacing the entire resource with a new version. It usually requires sending the full resource in the request.
2. PATCH is used to partially update a resource. It allows you to send only the fields that need to be updated, rather than the entire resource.

1. PUT Method:
Idempotency rule: PUT is idempotent. Making the same PUT request multiple times will always result in the same state on the server.
Explanation:

PUT is used to update or replace a resource on the server. The key point is that it either creates or fully updates the resource in a way that every time you send the same data, the result will be the same.
Example: Let's say you're updating a user's profile information with PUT:

    PUT /users/123
    {
      "name": "John Doe",
      "email": "john@example.com"
    }

The first time you send this request, it updates or creates the user profile for user 123 with the given name and email.
If you send the exact same request again, nothing will change. The server will see that the resource is already in that state (the name is still "John Doe" and the email is still "john@example.com"), so it won’t do anything different.
Result: No matter how many times you repeat the request, the final state will be the same.
2. POST Method:
Idempotency rule: POST is NOT idempotent. Making the same POST request multiple times may result in multiple resources being created or multiple actions being taken.
Explanation:

POST is used to create a new resource or perform an action. When you send a POST request, it typically creates a new resource, and sending the same request again may create a duplicate or trigger an additional action.
Example: Let's say you're submitting a form to create a new user:

    POST /users
    {
      "name": "John Doe",
      "email": "john@example.com"
    }
The first time you send this request, a new user with the name "John Doe" and the email "john@example.com" is created.
If you send the same request again, it will create another user with the same data, so now you’ll have two users with the same information (if the system allows duplicates).
Result: Repeating the request leads to different outcomes—each POST creates a new resource.

3. PATCH Method:
Idempotency rule: PATCH is idempotent if the same request is applied multiple times. However, the change depends on how you use it.
Explanation:

PATCH is used to partially update a resource. It modifies only the specified fields. Sending the same PATCH request multiple times should lead to the same result as sending it once (if the request doesn’t conflict with the resource's current state).
Example: Let's say you're updating just the email of a user:
    
    PATCH /users/123
    {
      "email": "newemail@example.com"
    }

The first time you send this request, it updates only the email field of user 123 to "newemail@example.com".

If you send the same request again, the server will still update the email to "newemail@example.com", but since it’s already the same, the state won’t change.

Result: If the request is structured properly, sending the same PATCH request multiple times won’t change anything after the first one. The result is idempotent because the resource is left in the same state.
PATCH: Generally Idempotent. Repeating the same request leads to the same outcome, assuming the state doesn’t conflict.

Q2: When would you choose to use PUT instead of PATCH?

Answer:
1. Use PUT when you want to completely replace the resource. If you don’t send the full resource with a PUT request, any fields you omit might be reset or lost.
For example, if you have an object with 5 properties and want to change 1 of them, using PUT means you should send all 5 properties. Any missing properties might be deleted.
2. Use PATCH when you only need to update part of a resource and want to avoid sending the entire object.
For example, if you want to update just one field in a large object, PATCH allows you to send a small request with only the modified field(s), which can be more efficient.

Q3: Can you use PUT to create a resource?

Answer:
Yes, PUT can also be used for creation if the resource does not exist yet. If you send a PUT request to a URI that does not exist, it can create a new resource at that URI.
PATCH is typically not used for creating new resources, as it is primarily for modifications.

Q4: In which scenarios could using PATCH introduce complexity compared to PUT?

Answer:
Partial updates can introduce complexity in terms of validation and conflict resolution. With PATCH, the server needs to handle merging the changes into the existing resource, which can lead to issues like conflicting changes or data inconsistency.
Also, using PATCH requires more attention to how changes are applied, especially in concurrent environments where multiple users may update the resource at the same time.

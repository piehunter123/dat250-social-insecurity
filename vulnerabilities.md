Injection vulnerabilities:
- HTML inject: no validation on uploads containing malicious html. Can make classic img embedded GET requests, XSS attacks.
- JS inject: no validation on uploads containing script tags. Are run by default.

File upload validation:
- No file size limit.
- No file type filter.

No rate limiting on login, uploading.
- Credentials can be brute forced.
- DoS is simple. Can upload (large) files repeatedly.

Sensitivie data exposure/Broken Access Control:
- Can view profile details in the profile and friends tab without authentication, ie: [/profile/user](http://localhost:5000/profile/jonmhm)

Indexing:
- Usernames can easily be checked by fetching [/stream/<username>](http://localhost:5000/stream/jonmhm)
- Uploaded media can be indexed: [Link to image](http://127.0.0.1:5000/uploads/eqnr%20stock%20in%203%20space.gif)

Backend insight:
- Going to a non-existing users stream returns a LOT of useful information about the database:
```SQL
WHERE p.u_id IN (SELECT u_id FROM Friends WHERE f_id = {user["id"]})
OR p.u_id IN (SELECT f_id FROM Friends WHERE u_id = {user["id"]})
OR p.u_id = {user["id"]}
```

Insufficient logging:
- User actions are not logged, no guard against indexing/vulnerability probing. No way to identify a breach.

Plain text database:
- Sensitive data is readily available in plain text.
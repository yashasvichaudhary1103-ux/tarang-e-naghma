\# Tarang-e-Naghma — Day 2

\# APIs: Complete Learning Notes

\### From Zero to Interview-Ready



\## 1. What Are We Learning?



Tarang-e-Naghma is a mobile music application. Our mobile app will display songs, play music, manage playlists and support offline downloads.



But where will the song information come from? How will the app register a user? How will it retrieve playlists saved on the server?



For many of these operations, our mobile app needs to communicate with a backend server.



\*\*This is where APIs come in.\*\*



By the end of this chapter, you should understand:

\- What an API is and why we need it.

\- How a frontend communicates with a backend.

\- What requests, responses, endpoints and JSON mean.

\- What HTTP and REST APIs are.

\- The difference between GET, POST, PUT, PATCH and DELETE.

\- Common HTTP status codes.

\- How APIs will work in Tarang-e-Naghma.

\- Frequently asked interview questions with answers.



\---



\# PART 1 — UNDERSTANDING THE BASICS



\## 2. What Is an API?



API stands for \*\*Application Programming Interface\*\*.



An API is a defined interface through which one software component can request information or functionality from another software component.



In simple words:



\*\*An API allows different software systems to communicate according to agreed rules.\*\*



For example, a mobile application can request a list of songs from a backend API. The backend processes the request and returns the information the app needs.



An API does not automatically store all the data, design the interface or play the music. It provides a defined way to access functionality or exchange information.



\## 3. Real-Life Example: A Restaurant



Imagine you visit a restaurant.



1\. You are the customer.

2\. You tell the waiter what you want.

3\. The waiter communicates your order to the kitchen.

4\. The kitchen prepares the food.

5\. The waiter brings your order to you.



Think of this as an analogy for software communication.



| Restaurant | Software application |

|---|---|

| Customer | Frontend / client |

| Waiter taking the order | API interaction |

| Kitchen | Backend processing |

| Kitchen's ingredients and records | An imperfect analogy for backend data sources |

| Food delivered to customer | Response returned to the client |



Important: The waiter is only an analogy for the API. In a real application, the API is an interface and set of rules, not a human-like program that independently does all the work.



\### Why can't the frontend simply do everything?



The frontend can handle many tasks locally, but some operations need trusted server-side processing and shared data.



For example:

\- User authentication.

\- Retrieving server-side song metadata.

\- Saving playlists to a user's account.

\- Synchronizing likes across devices.

\- Checking whether a user is authorized to perform an operation.



The backend can handle these operations, and APIs provide the interface through which the mobile app accesses them.



\---



\# PART 2 — HOW AN API WORKS



\## 4. Frontend, Backend, API and Database



These are related, but they are not the same thing.



\### Frontend



The frontend is the part of the application that the user interacts with.



In Tarang-e-Naghma, our frontend will use React Native and Expo.



Its responsibilities include:

\- Displaying songs and artists.

\- Showing the music player.

\- Handling button taps.

\- Displaying playlists and liked songs.

\- Showing loading indicators and error messages.



\### Backend



The backend handles server-side application logic.



We plan to use Java and Spring Boot.



Its responsibilities include:

\- Processing API requests.

\- Authenticating users.

\- Applying business rules.

\- Retrieving or updating database records.

\- Managing playlists and favourites.

\- Providing authorized access to track metadata and audio streams.



\### API



The API defines how the frontend can communicate with the backend.



For example, the frontend may request a list of tracks through a GET endpoint.



\### Database



The database stores persistent information.



We plan to use PostgreSQL for data such as:

\- Users.

\- Track metadata.

\- Playlists.

\- Playlist-track relationships.

\- Liked songs.

\- Listening history.



The database is accessed by the backend for normal server-side operations. Our mobile app should not directly connect to PostgreSQL using database credentials.



\## 5. The Complete Request-Response Cycle



Suppose a user opens the Home screen.



Here is what may happen:



1\. The React Native app needs the available songs.

2\. The app sends an HTTP request to a Spring Boot API.

3\. Spring Boot receives the request.

4\. The backend checks what operation is being requested and whether the user is authorized, if necessary.

5\. The backend retrieves the required data from PostgreSQL.

6\. The backend creates an HTTP response, often containing JSON.

7\. The mobile app receives the response.

8\. React Native displays the song information on the screen.



This is called the \*\*request-response model\*\*.



\### Example architecture



React Native app

&#x20;      |

&#x20;      | HTTP request

&#x20;      v

Spring Boot REST API

&#x20;      |

&#x20;      | Database query

&#x20;      v

PostgreSQL

&#x20;      |

&#x20;      | Query result

&#x20;      v

Spring Boot REST API

&#x20;      |

&#x20;      | HTTP response

&#x20;      v

React Native app



The backend does not necessarily query the database for every request. Some information may be cached or processed in other ways. This is our basic conceptual flow.



\---



\# PART 3 — REQUESTS, RESPONSES AND ENDPOINTS



\## 6. What Is a Request?



A request is a message sent by a client to ask a server to perform an operation or return information.



For example:



"Give me the available tracks."



A typical HTTP request can contain:

\- HTTP method.

\- URL or endpoint.

\- Headers.

\- Optional body.



Not every request contains a body.



\## 7. What Is a Response?



A response is the server's reply to a request.



It commonly contains:

\- HTTP status code.

\- Headers.

\- Optional response body.



For example, the backend may return a list of tracks and the status code 200, meaning the request succeeded.



If something goes wrong, the response may contain an appropriate error status and an error message.



\## 8. What Is an Endpoint?



An endpoint is a particular API address used to access an operation.



For example:



`GET /api/tracks`



This is an illustrative endpoint we may implement in Tarang-e-Naghma.



It means the client is requesting the list of tracks.



Another example:



`GET /api/tracks/101`



This could retrieve information about track 101.



An endpoint generally consists of a path used with an HTTP method. The complete request also includes the server's base URL.



For example:



`http://localhost:8080/api/tracks`



Here:

\- `http://localhost:8080` is the base address in a local development setup.

\- `/api/tracks` is the path.

\- `GET` is the HTTP method.



The port and address may change when we configure and deploy the backend.



\---



\# PART 4 — WHAT IS HTTP?



\## 9. HTTP Explained



HTTP stands for \*\*Hypertext Transfer Protocol\*\*.



It is a protocol used for communication between clients and servers.



A protocol is a set of agreed rules about how communication takes place.



In our project, HTTP will define how the mobile app sends requests to the Spring Boot backend and how the backend returns responses.



HTTPS is HTTP protected by TLS encryption. We should use HTTPS for production API communication to protect data while it travels over the network.



\### Example



The mobile app requests:



`GET /api/tracks`



The server responds with:

\- A status code.

\- Headers.

\- Track data, often in JSON format.



HTTP defines the request-response format. It does not dictate how our Java business logic or PostgreSQL database must be implemented.



\---



\# PART 5 — WHAT IS JSON?



\## 10. JSON Explained



JSON stands for \*\*JavaScript Object Notation\*\*.



It is a widely used text format for exchanging structured data between systems.



Example response:



```json

{

&#x20; "id": 101,

&#x20; "title": "Example Song",

&#x20; "artist": "Example Artist",

&#x20; "duration": 240

}

```



This object contains:

\- `id`: track identifier.

\- `title`: song title.

\- `artist`: artist name.

\- `duration`: duration in seconds.



These values are illustrative sample data.



\### JSON rules to remember



1\. Object keys are written in double quotes.

2\. Strings use double quotes.

3\. Numbers do not need quotation marks.

4\. Key-value pairs are separated by commas.

5\. Objects use curly braces.

6\. Arrays use square brackets.

7\. Standard JSON does not support comments.



Example array:



```json

\[

&#x20; {

&#x20;   "id": 101,

&#x20;   "title": "Example Song"

&#x20; },

&#x20; {

&#x20;   "id": 102,

&#x20;   "title": "Another Song"

&#x20; }

]

```



This represents a list of two tracks.



\### Is JSON the same as an API?



No.



\- API: The interface and rules used to access functionality or exchange information.

\- HTTP: A protocol commonly used to send API requests and responses.

\- JSON: A data format often used for request and response bodies.



An API can use other data formats and communication protocols too.



\---



\# PART 6 — REST APIs



\## 11. What Is REST?



REST stands for \*\*Representational State Transfer\*\*.



It is an architectural style for designing networked applications.



A RESTful API commonly exposes resources through URLs and uses HTTP methods to operate on those resources.



In Tarang-e-Naghma, resources may include:

\- Tracks.

\- Users.

\- Playlists.

\- Liked songs.



Example endpoints:



`GET /api/tracks`



Retrieve tracks.



`GET /api/playlists`



Retrieve playlists accessible to the authenticated user.



`POST /api/playlists`



Create a playlist.



`DELETE /api/playlists/25`



Delete playlist 25, subject to authorization and business rules.



These are planned examples, not yet implemented endpoints.



\### What makes REST useful?



\- It provides a predictable way to organize API resources.

\- It uses familiar HTTP methods.

\- It separates the client from the server implementation.

\- It allows different clients, such as Android apps and web apps, to use the same backend API.



REST has additional architectural constraints, including stateless communication. A well-designed REST API should follow the relevant constraints rather than merely using URLs and HTTP methods.



\### What does stateless mean?



In a stateless request model, the server does not rely on hidden conversational session state from a previous request to understand the current request.



For example, an authenticated request can include a bearer access token. The server uses that token to identify and authorize the request.



The server may still store persistent user data in PostgreSQL. Stateless communication does not mean the application has no database or no stored information.



\---



\# PART 7 — HTTP METHODS



\## 12. GET



GET is used to retrieve a resource or information.



Example:



`GET /api/tracks`



Meaning: Retrieve the list of tracks.



GET requests should not be designed to change application state as a side effect.



In our app:

\- Fetch available tracks.

\- Retrieve playlist details.

\- Retrieve the current user's liked songs.



\## 13. POST



POST is commonly used to submit data or request an operation that creates a resource.



Example:



`POST /api/playlists`



A request body could contain:



```json

{

&#x20; "name": "My Favourites"

}

```



The backend validates the input and may create a playlist in PostgreSQL.



POST is not limited to creating resources. It can also be used for other operations, depending on the API design.



\## 14. PUT



PUT is used to create or replace the state of a resource at a known target URI, depending on the API design.



Example:



`PUT /api/playlists/25`



The client may submit the complete editable representation of playlist 25.



In a conventional replacement design, fields omitted from the representation may not be preserved. The exact behaviour depends on the API contract.



\## 15. PATCH



PATCH is used to apply a partial modification to a resource.



Example:



`PATCH /api/playlists/25`



Request body:



```json

{

&#x20; "name": "My Chill Songs"

}

```



This could change only the playlist name without requiring the client to submit every playlist field.



\## 16. DELETE



DELETE requests removal of a resource or its association, according to the API's semantics.



Example:



`DELETE /api/playlists/25`



This requests deletion of playlist 25.



The backend must verify that the user has permission to delete that playlist.



\### Quick comparison



| Method | Common purpose | Tarang-e-Naghma example |

|---|---|---|

| GET | Retrieve | Get available tracks |

| POST | Submit/create | Create a playlist |

| PUT | Replace resource state | Replace playlist details |

| PATCH | Partially update | Rename a playlist |

| DELETE | Remove | Delete a playlist |



Important: HTTP methods have defined semantics. They are not simply interchangeable labels for database operations.



\---



\# PART 8 — HTTP STATUS CODES



\## 17. Why Do Status Codes Matter?



A status code tells the client the general outcome of an HTTP request.



\### Successful responses



\*\*200 OK\*\*



The request succeeded.



Example: The backend successfully returned a track list.



\*\*201 Created\*\*



A resource was successfully created.



Example: A new playlist was created.



\*\*204 No Content\*\*



The request succeeded and no response body is being returned.



Example: A playlist was successfully deleted.



\### Client errors



\*\*400 Bad Request\*\*



The request is malformed or fails validation, depending on the API contract.



Example: A playlist name is missing or invalid.



\*\*401 Unauthorized\*\*



The request lacks valid authentication credentials.



Example: An access token is missing, expired or invalid.



\*\*403 Forbidden\*\*



The server understands the request but refuses to authorize it.



Example: A user tries to modify another user's private playlist.



\*\*404 Not Found\*\*



The requested resource or endpoint could not be found.



Example: A requested track ID does not exist.



\### Server errors



\*\*500 Internal Server Error\*\*



The server encountered an unexpected condition.



Example: An unhandled backend exception occurred.



\### Important distinction: 401 vs 403



\- 401: Valid authentication is missing or has failed.

\- 403: The request is understood, but permission is denied.



The exact error handling should be consistent with the API's authentication and authorization design.



\---



\# PART 9 — API SECURITY



\## 18. Why Should APIs Be Secure?



An API may expose personal data and operations that should only be available to authorized users.



For Tarang-e-Naghma, users should not be able to modify each other's private playlists simply by changing a playlist ID.



Important security measures include:



\### Authentication



Authentication answers: "Who are you?"



We plan to support user registration and login.



\### Authorization



Authorization answers: "What are you allowed to do?"



For example, a user may edit their own playlist but not another user's private playlist.



\### Password hashing



Passwords should not be stored as plain text.



We plan to use BCrypt for password hashing in the backend.



\### JWT



JWT stands for JSON Web Token.



A signed JWT can carry claims that a server verifies when processing an authenticated request.



A mobile client may send an access token in an HTTP header:



`Authorization: Bearer <access\_token>`



The backend must validate the token and enforce access permissions. A signed JWT is not automatically encrypted, and sensitive information should not be placed in readable token claims.



\### HTTPS



HTTPS protects data in transit between the client and server.



\### Input validation



The backend must validate client-supplied data rather than trusting it.



For example, a playlist name should satisfy the application's rules, and a requested resource must belong to or be accessible by the relevant user.



\### Important rule



Never embed database passwords, signing secrets or other server-only credentials in the mobile app.



\---



\# PART 10 — HOW APIS WILL WORK IN TARANG-E-NAGHMA



\## 19. Feature: Fetching Songs



A planned endpoint:



`GET /api/tracks`



Possible flow:



1\. User opens the Home screen.

2\. The app requests the available tracks.

3\. Spring Boot handles the request.

4\. The backend retrieves the required metadata from PostgreSQL.

5\. The backend returns a JSON response.

6\. The app renders the track cards.



\## 20. Feature: Creating a Playlist



A planned endpoint:



`POST /api/playlists`



Possible flow:



1\. User enters a playlist name.

2\. The app sends the playlist data.

3\. Spring Boot verifies authentication and validates the request.

4\. The backend checks the user's permissions and business rules.

5\. The backend saves the playlist in PostgreSQL.

6\. The backend returns a response confirming the created resource.



The client should handle both success and failure responses.



\## 21. Feature: Liking a Song



We might design an endpoint such as:



`PUT /api/me/liked-tracks/101`



This could represent adding track 101 to the authenticated user's liked songs.



The exact method and endpoint are design decisions we will make while implementing the backend. A DELETE endpoint could remove the like.



The backend should determine the user from the authenticated identity, rather than trusting an arbitrary user ID supplied by the client.



\## 22. Feature: Streaming Audio



Audio streaming is different from retrieving ordinary JSON metadata.



A streaming endpoint might be designed as:



`GET /api/tracks/101/audio`



The backend may stream audio bytes from an authorized source.



For efficient seeking, the server may support HTTP byte-range requests and return `206 Partial Content` when a valid range is served.



The mobile audio player manages playback, seeking and buffering.



The backend supplies the stream; it does not directly control whether the phone remains locked or which playback controls appear on the lock screen.



\## 23. Feature: Offline Playback



When a user downloads a song:



1\. The app obtains the authorized audio data.

2\. The audio file is saved to persistent private device storage.

3\. Local metadata is recorded, including the track ID and local file path.

4\. The app displays the track in the Downloads screen.

5\. When offline, the audio player reads the local file instead of requesting the audio stream again.



An API is not required every time a downloaded song is played.



However, the app may need the backend to synchronize playlists, account information or other server-side data when connectivity returns.



\---



\# PART 11 — COMMON MISCONCEPTIONS



\## 24. API vs Backend



An API is an interface through which functionality is exposed. The backend is the server-side application that implements the processing and may expose APIs.



\## 25. API vs Database



An API is not a database. An API may allow a client to retrieve data that the backend reads from a database.



\## 26. HTTP vs JSON



HTTP is a communication protocol. JSON is a data format. An HTTP response can contain JSON, but HTTP and JSON are not the same thing.



\## 27. REST vs HTTP



HTTP is a protocol. REST is an architectural style. RESTful APIs commonly use HTTP, but REST and HTTP are not synonyms.



\## 28. API vs Endpoint



An API can contain many endpoints. An endpoint represents a particular accessible path and operation within the API.



\## 29. API vs SDK



An API defines an interface. An SDK, or Software Development Kit, is a collection of tools and code that helps developers use a platform or service. An SDK may provide a convenient client for calling an API.



\## 30. Does Every Button Need an API?



No.



A button that opens a local screen or pauses a locally playing song may not need a backend API request.



A button that saves a playlist to the user's account will generally need backend communication when synchronization with the server is required.



\---



\# PART 12 — INTERVIEW QUESTIONS WITH ANSWERS



\## Beginner Level



\### Q1. What is an API?



\*\*Answer:\*\* API stands for Application Programming Interface. It defines how software components communicate and access functionality or exchange information.



\### Q2. Why does Tarang-e-Naghma need APIs?



\*\*Answer:\*\* APIs allow the React Native app to communicate with our Spring Boot backend. The app can retrieve track information, authenticate users, create playlists and synchronize liked songs through these interfaces.



\### Q3. What is an API endpoint?



\*\*Answer:\*\* An endpoint is a particular API address used to access an operation or resource. For example, `GET /api/tracks` could retrieve the available tracks.



\### Q4. What is JSON?



\*\*Answer:\*\* JSON is a text format for representing structured data. It is commonly used to exchange data between a frontend and backend.



\### Q5. What is HTTP?



\*\*Answer:\*\* HTTP is a protocol that defines how clients and servers exchange requests and responses.



\### Q6. What is the difference between frontend and backend?



\*\*Answer:\*\* The frontend handles the user interface and user interactions. The backend processes server-side business logic, authentication and data operations.



\### Q7. What is a REST API?



\*\*Answer:\*\* A REST API follows the principles of the REST architectural style. It commonly exposes resources through URLs and uses HTTP methods to retrieve or modify those resources.



\### Q8. What is the difference between an API and a database?



\*\*Answer:\*\* A database stores data, whereas an API provides a defined interface through which clients or other systems can access functionality or information.



\## Intermediate Level



\### Q9. What is the difference between GET and POST?



\*\*Answer:\*\* GET is used to retrieve information. POST submits data or requests an operation, commonly to create a resource.



\### Q10. What is the difference between PUT and PATCH?



\*\*Answer:\*\* PUT commonly replaces a resource's state at a known URI, whereas PATCH applies partial modifications.



\### Q11. What happens when an API request is sent?



\*\*Answer:\*\* The client sends a request to an endpoint. The server processes it, performs any necessary validation and data operations, and returns an HTTP response containing a status code and, when appropriate, a response body.



\### Q12. What is the difference between 401 and 403?



\*\*Answer:\*\* 401 indicates that valid authentication credentials are missing or have failed. 403 indicates that the server refuses to authorize the requested operation.



\### Q13. Why do we use status codes?



\*\*Answer:\*\* Status codes communicate the outcome of a request so that the client can distinguish successful operations from client errors and server errors.



\### Q14. What does stateless mean in REST?



\*\*Answer:\*\* Each request should contain the information necessary for the server to understand and process it without relying on hidden conversational state from previous requests. Persistent data can still be stored in a database.



\### Q15. Can a mobile app directly connect to PostgreSQL?



\*\*Answer:\*\* It should not connect directly in a typical production architecture. The backend provides a controlled interface, protects database credentials and enforces authentication, authorization and business rules.



\## Project-Based Interview Questions



\### Q16. Explain the flow when a user opens the Home screen.



\*\*Answer:\*\* The React Native app sends a request to the Spring Boot API. The backend retrieves the required track metadata from PostgreSQL and returns a response. The app then displays the received tracks.



\### Q17. How will you secure playlist APIs?



\*\*Answer:\*\* We will authenticate users, validate access tokens, enforce authorization checks and verify that the requested playlist belongs to the user or is otherwise accessible to them.



\### Q18. Does playing an offline song require an API call?



\*\*Answer:\*\* Not necessarily. If the audio file and its local metadata are available, the app can play the file directly from device storage without requesting the audio from the backend.



\### Q19. Does the backend control background playback?



\*\*Answer:\*\* No, not directly. The backend may provide the audio stream, but background playback depends on the mobile audio player, operating-system capabilities and the app's configuration.



\### Q20. How would you handle a failed API request?



\*\*Answer:\*\* We would handle unsuccessful HTTP responses and network failures, display a meaningful error or retry option, and preserve locally available content where appropriate.



\### Q21. Why use Spring Boot instead of putting all logic in React Native?



\*\*Answer:\*\* Spring Boot allows us to centralize server-side business logic, authentication, authorization and database access. It also lets multiple clients use the same backend without duplicating these responsibilities.



\### Q22. What happens if the server returns a 500 status code?



\*\*Answer:\*\* It indicates an internal server error. The mobile app should handle the failure gracefully, avoid displaying sensitive internal details, and offer an appropriate recovery option. We would investigate the server logs to identify the underlying cause.



\## Advanced Follow-Up Questions



\### Q23. Is every HTTP API a REST API?



\*\*Answer:\*\* No. HTTP APIs can follow different designs. A RESTful API follows the REST architectural constraints, not merely the use of HTTP.



\### Q24. Is a JWT always encrypted?



\*\*Answer:\*\* No. A typical signed JWT provides integrity and authenticity, but its claims are often readable. Encryption is a separate property.



\### Q25. Is GET always safe to retry?



\*\*Answer:\*\* GET is defined as a safe HTTP method, so clients can generally retry it without intending to change application state. However, network failures and application-specific side effects still require careful engineering.



\### Q26. What is the difference between authentication and authorization?



\*\*Answer:\*\* Authentication verifies identity. Authorization determines what an authenticated identity is allowed to do.



\### Q27. What if two devices update a playlist?



\*\*Answer:\*\* The backend needs a clear consistency strategy. Depending on the requirements, it could use version checks, optimistic locking or other concurrency controls to prevent unintended overwrites.



\### Q28. How can we avoid repeatedly fetching the same song metadata?



\*\*Answer:\*\* We can consider caching, conditional requests or local persistence, depending on the freshness requirements. We must also invalidate or refresh cached data when it becomes outdated.



\---



\# PART 13 — PRACTICE EXERCISES



\## Exercise 1: Identify the method



Choose an appropriate HTTP method for each operation.



1\. Retrieve available tracks.

2\. Create a playlist.

3\. Rename a playlist using a partial update.

4\. Delete a playlist.

5\. Retrieve the details of a specific track.



\## Exercise 2: Understand a response



Consider this response:



```json

{

&#x20; "id": 101,

&#x20; "title": "Example Song",

&#x20; "duration": 240

}

```



Answer:

1\. What is the track ID?

2\. What is the title?

3\. What does the duration represent if our API defines it in seconds?

4\. Is this JSON an API endpoint or a data representation?



\## Exercise 3: Debug the architecture



A developer places PostgreSQL credentials directly inside the mobile application and connects the app to the database.



Explain why this is unsafe and how our planned Spring Boot backend avoids the problem.



\## Exercise 4: Design an endpoint



Propose an endpoint to retrieve the currently authenticated user's liked songs.



Write:

\- HTTP method.

\- Endpoint path.

\- What the backend should return.

\- What authentication or authorization checks are needed.



\---



\# PART 14 — QUICK REVISION SHEET



| Term | One-line meaning |

|---|---|

| API | Defined interface for software communication |

| HTTP | Protocol for client-server communication |

| HTTPS | HTTP protected by TLS |

| JSON | Text format for structured data |

| Endpoint | Address and operation exposed by an API |

| Request | Message sent by the client |

| Response | Reply returned by the server |

| REST | Architectural style for networked applications |

| GET | Retrieve a resource |

| POST | Submit data or request an operation |

| PUT | Replace resource state |

| PATCH | Partially modify a resource |

| DELETE | Request resource deletion |

| 200 | Request succeeded |

| 201 | Resource created |

| 400 | Bad or invalid request |

| 401 | Authentication failed or is missing |

| 403 | Operation is forbidden |

| 404 | Resource or endpoint not found |

| 500 | Internal server error |

| Authentication | Verify identity |

| Authorization | Check permissions |

| JWT | Token format often used to carry signed claims |

| Stateless | Each request carries the information needed to process it without hidden conversational state |



\## Final takeaway



In Tarang-e-Naghma, React Native will provide the mobile interface, Spring Boot will implement our backend APIs and business logic, and PostgreSQL will store persistent server-side data.



The mobile app will use these APIs when it needs server-side information or operations. For offline playback, the app can instead use a downloaded local audio file and local metadata.



\*\*This is the foundation we need before implementing our first Spring Boot API.\*\*


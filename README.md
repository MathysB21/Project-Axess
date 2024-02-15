# Project-Axess
Physical access control application and services. Imagine Microsoft Authenticator for physical doors. This app with its services allows full control of physical access, user control and access logging.

## About
This project was built for my third year Software Engineering degree. The idea was to build an access control application using multiple technologies to create a full-stack application with a mobile application attached.

The system has multiple components:
- API
    The API facilitates the communication between the SQL DB, the mobile app and the WebSocket
- WebSocket
    The WebSocket listens for updates, especially when the door unlock was successful and then demonstrates these door unlocks visually through the WebSocket client (The webpage displaying the doors).
- SQL Database
    The SQL DB stores the user details, request details, access logs, admin details and details about the building areas.
- Web-based Admin Dashboard
    This dashboard allows Admins to log in and see the active requests, users, access logs and the building areas.
- Mobile Application
    The mobile app is to be installed on all the users' smartphones. This will allow them to authenticate themselves through biometrics and then a username and password. The user will need to create an account and the request will have to be approved by an admin and then the user can open doors inside the app after signing in.

For this project the API, WebSocket, SQL Server and Admin Dashboard need to run on the same work station.

## Requirements
- Node
- ExpressJS
- PHP installed
- XAMPP
- MySQL
- WebSocket

## How to run for system admins
1. Open XAMPP after installation, run Apache and MySQL
2. Make sure the DB is setup correctly on the work station. You can do this through PHPMyAdmin which you can access through XAMPP or via MySQL Workbench. You can find the SQL Table Scripts inside the Admin Dashboard folder.
3. Make sure to put the Admin Dashboard inside the xampp/htdocs folder in the work station's C drive. Test that it's working by going to the following URL. Note that you will need to create some Admin users so they can sign in to this Admin Dashboard.
    > localhost/Admin%20Dashboard
4. Run the API by making the API folder the current directory, then run the following command. To test that it's working, go to the URL below the command.
    > npm run dev
    > localhost:8080/
5. Run the WebSocket by opening a command prompt in the WebSocket directory and running the following command:
    > node server.js
6. Open another command prompt in the WebSocket folder and then run the following:
    > node app.js
7. Inside the WebSocket's src folder, open the index.html to open the Door Monitoring page.
8. Use ThunderClient (inside VS Code) to test the API and see the doors open and close. You can test each of the POST and GET functions found inside the API folder's app.js file.
9. Load the Mobile Application into Android Studio and either load a virtual device or load the app onto an Android device to see the application come to life.

## How to use for system users
1. Get the application from the system administrator and install it on your mobile phone.
2. Open the app and register. Note that you need to allow the app to use biometrics
3. You will see a Pending Approval screen. You will see this screen until a system admin approves your account request on the Admin Dashboard.
4. Once approved, you will need to sign into the app again and verify your identity using your phone's biometrics. 
5. You will be relayed to a Main Screen where you can open any door that your account is allowed to open.
6. If your door request was successful, the corresponding door will open on the Door Monitoring page.

## More about the system
### Admin Dashboard
On the admin dashboard you will be met with a left panel and a main panel on the right side of the screen. In the left panel you will be able to choose between multiple screens: Requests, Users, Areas, Access Logs and then a Sign Out button. 

Inside the Requests screen you will find all active user account requests. You will have the ability to approve a request which will then store the user credentials and allow them to access their requested areas. Inside the Users screen you can see all the active users currently registered on the system and their details, which include which areas they are allowed to access. You can also remove users should the need ever arise. The Areas screen just shows the areas that were configured on the system, nothing too special. The Access Logs screen allows the Admin users to have a look at which users had accessed which areas and at what date and time.

### API
Inside the API you will find multiple GET and POST functions.
- GET - / - This simply tests if the API is currently working and will return a Success string
- GET - /getBuildingAreas - This function returns all the building areas configured
- POST - /createAccountRequest - This function allows the mobile applications to create account requests which the admins will need to approve
- POST - /login - This function allows the users to login and will allow the user to access the main screen in the app where they can open doors
- POST - /openDoor - This is the most important function and can only be run if the requesting account is logged in. Here the user sends their details and the requested door and if this request is successful, the corresponding door will open on the Door Monitoring page

### WebSocket
This is a simple WebSocket that listens for success messages from the openDoor function inside the API. 
Should it receive a success message, it will open the corresponding door on the web page and provide a visual reference and confirmation that the door opened.

### Mobile Application
The mobile application authenticates the user using their stored biometrics on their device, thereby minimising risk and management of sensitive biometrics data by us. When the user opens the app for the first time they will be met by a registration screen where they need to create an account and then select the areas they want to have access to. After sending the request they will be sent to a Pending approval screen. When the System admin approves the request the user will need to sign in again and then they will have access to the main screen where they can choose which doors they want to open.
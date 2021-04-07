# Google Classroom notifier Discord bot

A simple Discord bot that sends message to your discord channel and notifies you whenever there's an update in any of the Google Classroom you're in.

## Requirements

- Node.js v12 or later
- A G Suite for Education account with Google Classroom enabled
- A Discord bot token
- [A channel ID for the notifications to be sent to](https://support.discord.com/hc/en-us/articles/206346498-Where-can-I-find-my-User-Server-Message-ID-)
- A domain name or ngrok for the initial setup (for the callback URI)
- OAuth2 credentials for Google's API with the following scopes:

```
https://www.googleapis.com/auth/classroom.courses.readonly
https://www.googleapis.com/auth/classroom.announcements.readonly
```

If you want to get notified about classwork too, you also need to grant this
scope:

```
https://www.googleapis.com/auth/classroom.coursework.me.readonly

```

If you want the bot to attach the files that comes with the announcement then you have to grant this scope as well.

```
https://www.googleapis.com/auth/drive.readonly

```

## Getting Started

### 1. Clone the repo

```
git clone https://github.com/Bla-nk/Discord_GoogleClassroom.git
cd Discord_GoogleClassroom

```

### 2. Install the dependencies

```
npm install

```

### 3. Create and modify the config file

```
cp config.example.json config.json
vim config.json

```
Here you will need to add all the necessary details such as bot token, classroom IDs/links, Google API OAuth2 credentials etc. Everything is explained at the end of this README  
### 4. Build the app and start it to authorize your app

```
npm run build
npm run start

```

On the first run, you will be prompted to open a URL to authorize your
application. Open that URL in your browser, log in with the account that has an
active G Suite for Education account with access to Google Classroom, and
authorize the application.

Once you're redirected to the Express server, copy the code that shows up in
the JSON response and paste it into the terminal. The rest of the configuration
should be automatic.

Once the configuration is successful, the Express server will shut down and the
bot will continue to run. On the next startup, you will not have to perform this
step and the Express server will not start either.

### 5. Invite your bot to your server

Open the following link in your browser:

```
https://discord.com/oauth2/authorize?client_id=APP_ID&scope=bot

```

Here the "APP_ID" is to be replaced by the App ID of your bot.

## Configuration

### `server.port`

The port number on which the Express server will start in case the app was not
authorized yet.

### `bot.token`

The token of your Discord bot.

### `bot.channel`

The ID of the channel that the notifications will be sent to.

### `bot.checkInterval`

Specifies how often (minutes) the bot will check your classroom for news. The
bot will not send a message to your Discord channel if there aren't any new
posts.

### `bot.pingEveryone`

If set to `true`, the bot will ping @everyone when sending a notification.

### `bot.timezone`

An IANA-compliant timezone string. Defaults to `UTC`. Example: `Europe/Berlin`.

### `google.*`

Properties that are required for authenticating with your Google app.

#### `clientId`, `authURI`, `tokenURI`, `clientSecret`

These are details that you will receive in the form of a JSON file when creating
the app in the Google Developer Console.

#### `redirectURI`

The URI to which Google's authorization service will redirect the client (you)
after you've authorized the app. This must end in `/authorize`. You can use a
custom domain for this or [ngrok](https://ngrok.com/).

#### `scopes`

Usually you don't want to change these values. Those two are necessary for the
bot to function correctly.

`classroom.courses.readonly` is required for mapping the course enrollment codes
to course objects. `classroom.announcements.readonly` is obviously required for
listing and checking new posts.

`classroom.coursework.me.readonly` is an optional scope that is required only if
you want to be notified about classwork too, not just announcements.

`drive.readonly` is an optional scope that is used for fetching the attached
Google Drive files and sending them to your Discord channel. If you don't need
this feature, just don't provide this scope.

#### `enrollmentCodes`

An array of codes that you used when you joined the Google Classroom course.
This is optional if `linkIDs` is specified.

#### `linkIDs`

An array of link IDs (last component of the classroom's URL). For example, the
link ID of https://classroom.google.com/u/0/c/MTA7AfC10BO0AVEt is
`MTA7AfC10BO0AVEt`. This is useful if you have access to the classroom but you
don't know what the enrollment code is. The parameter is optional if you've
specified `enrollmentCodes` instead.

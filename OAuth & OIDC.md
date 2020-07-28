### OAuth (Open Authentication) & OIDC (Open ID Connect)

Authentication is a process to verify an identity
Authorisation is a process to verfiy what someone is allow to do

#### OAuth App Scenario

- End user (Resource Owner) delegates access to an app, say Yelp (Client Application), giving it permission on the user's behalf to do something
- Yelp then go to obtain access (token with limited scope) from Google (Authorisation Server), 
- then it uses this token to access Google Contact API (Resource Server) to read all your contacts.

To unfold the scenario in more details:

- End user first click the `Connect with Google` button on Yelp website to kick start the OAuth process
- The user will be sent to authorisation server (Google) to authorise (with `Redirect URL: yelp.com/callback`, `Response type: code` and `Scope: profile contacts`)
- User grants profile contacts reading access to Yelp
- User is redirected back to Yelp with a one-time authorisation code
- Yelp uses the authorisation code to exchange it for an access token with authorisation server in back channel (not via Browser)
- Yelp finally talks to resource server (Google Contact API) with the access token and retrieve the Google contacts

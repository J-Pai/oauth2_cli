# Google OAuth2 Token CLI Utility
Commandline utility for generating Google OAuth2 tokens for use in other
commandline tools.

From a GCP project's console ([credentials page](https://console.cloud.google.com/apis/credentials)),
create an OAuth2 Client ID and download the client secret JSON to a secure location.

## Installation
From inside the repository directory:

```bash
sudo apt install python3 python3-dev python3-pip python3-venv

mkdir -p cmake/build
cd cmake/build
cmake ../..
make
sudo make install

oauth2_cli /path/to/client/secret/secret.json $FLASK_SECRET
```

Specify `$FLASK_SECRET` if you want to automatically request the token. If
`$FLASK_SECRET` is not specified, `oauth2_cli` will require you to input a code
that is generated after the Google login page.

## Google OAuth2 Management

Get token information:

```
https://www.googleapis.com/oauth2/v3/tokeninfo?access_token=ACCESS_TOKEN
```

Get user profile information:

```
https://www.googleapis.com/oauth2/v3/userinfo?access_token=ACCESS_TOKEN
```

Revoke token:

```
curl -d -X -POST --header "Content-type:application/x-www-form-urlencoded" \
        https://oauth2.googleapis.com/revoke?token={token}
```

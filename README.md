# Netlify-cms-github-oauth-provider

***External authentication providers were enabled in netlify-cms version 0.4.3. Check your web console to see your netlify-cms version.***

[netlify-cms](https://www.netlifycms.org/) has its own github OAuth client. This implementation was created by reverse engineering the results of that client, so it's not necessary to reimplement client part of [netlify-cms](https://www.netlifycms.org/).

Github and Github Enterprise are currently supported, but as this is a general Oauth client, feel free to submit a PR to add other git hosting providers.

Other implementations in: [Go lang](https://github.com/igk1972/netlify-cms-oauth-provider-go).

## 1) Install Locally

**Install Repo Locally**

```
git clone https://github.com/vencax/netlify-cms-github-oauth-provider
cd netlify-cms-github-oauth-provider
npm install
```

**Create Oauth App**
Information is available on the [Github Developer Documentation](https://developer.github.com/apps/building-integrations/setting-up-and-registering-oauth-apps/registering-oauth-apps/). Fill out the fields however you like, except for **authorization callback URL**. This is where Github will send your callback after a user has authenticated, and should be `https://your.server.com/callback` for use with this repo.

## 2) Config

### Auth Provider Config

Configuration is done with environment variables, which can be supplied as command line arguments, added in your app  hosting interface, or loaded from a .env ([dotenv](https://github.com/motdotla/dotenv)) file.

**Example .env file:**

```
NODE_ENV=production
OAUTH_CLIENT_ID=f432a9casdff1e4b79c57
OAUTH_CLIENT_SECRET=pampadympapampadympapampadympa
REDIRECT_URL=https://your.server.com/callback
GIT_HOSTNAME=https://github.website.com
OAUTH_TOKEN_PATH=/login/oauth/access_token
OAUTH_AUTHORIZE_PATH=/login/oauth/authorize
OAUTH_PROVIDER=github
```

**Client ID & Client Secret:**
After registering your Oauth app, you will be able to get your client id and client secret on the next page.

**Redirect URL (optional):**
Include this if you  need your callback to be different from what is supplied in your Oauth app configuration.

**Git Hostname (Optional):**
This is only necessary for use with Github Enterprise, or a different Oauth provider like GitLab.

**Token and Authorize Paths (Optional):**
These may need to be set to the correct paths when using a different Oauth provider (e.g. GitLab).

**Provider (Optional):**
If you are using an OAuth provider other than GitHub or GitHub Enterprise, you will need to set this to provider name (e.g. `gitlab`, `github`, `bitbucket`) so that the CMS can recgonize it. This is normally the same as the backend name in your CMS config.

### CMS Config
You also need to add `base_url` to the backend section of your netlify-cms's config file. `base_url` is the live URL of this repo with no trailing slashes.

```
backend:
  name: github
  repo: user/repo   # Path to your Github repository
  branch: master    # Branch to update
  base_url: https://your.server.com # Path to ext auth provider
  api_root: https://github.website.com # (optional) Git server, should match the GIT_HOSTNAME above.
```

## 3) Push

Basic instructions for pushing to heroku are available in the [original blog post](http://www.vxk.cz/tips/2017/05/18/netlify-cms/).

# Ember backend

Tiny Express proxy that holds your Anthropic API key server-side and forwards
chat requests from the Ember frontend to the Anthropic API.

## Deploy on Railway (via GitHub)

1. Push this folder to a new GitHub repo (e.g. `ember-backend`).
2. In Railway: New Project -> Deploy from GitHub repo -> select the repo.
3. In the Railway project -> Variables tab, add:
   - `ANTHROPIC_API_KEY` = your key (starts with `sk-ant-...`)
4. Railway will detect Node automatically and run `npm install` then `npm start`.
5. Once deployed, Railway gives you a public URL, e.g.
   `https://ember-backend-production.up.railway.app`
6. Test it: visit that URL in a browser, you should see "Ember backend is running."
7. Test the endpoint:
   ```
   curl -X POST https://YOUR-RAILWAY-URL/api/chat \
     -H "Content-Type: application/json" \
     -d '{"messages":[{"role":"user","content":"hi"}]}'
   ```

## Point the frontend at it

In `ai-chat.html`, set `API_BASE_URL` near the top of the `<script>` block to
your Railway URL, e.g.:

```js
const API_BASE_URL = "https://ember-backend-production.up.railway.app";
```

Then re-upload `ai-chat.html` to amb1.website.

## Lock down CORS (recommended once live)

In `server.js`, replace:
```js
app.use(cors());
```
with:
```js
app.use(cors({ origin: "https://amb1.website" }));
```
This stops other sites from using your backend (and your API key/budget).

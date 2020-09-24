# Asset building made easy


Deploying your `css` and `js` files can be  an annoying task:

- Should I build my assets locally and push changes to the repo?
- Should I keep my `/public/css` and `/public/js` empty and add these folders to my `.gitignore`, and build my assets as a build step on deployment (or within a CI setup?
- What if I would like to serve my css and javascript assets from a CDN?

Not to mention you would have to deal with:
- Merge confilcts if you're working in a team
- Versioning and minifying the assets based on your `ENV` (eg. local/staging/production)

I think we can do something better :smile:

What if we could just run a simple commmand

```bash
assets:build
```

That would trigger a service to build your css/js assets.  
Push them to a `S3` cloud service and be done with it. 

```html
<link rel="stylesheet" href="https://assets.mydomain.com/latest/styles.css">
<script src="https://assets.mydomain.com/latest/main.js"
```

- The service should take care of cache busting.
- You should be able to pull in a specific version  
assets.mydomain.com/**version**/main.js -> /v1.0/main.js
- Or a specific commit hash  
assets.mydomain.com/**hash**/main.js -> /afb05cf1d/main.js

Other options would be:
- Deliver the assets to version control with a `PR` or `auto commit`
- Downloading compiled assets from `S3`
- Rsync

<hr>

### To make this happen the service should...

- Connect with a github account / API token
- Clone the repo
- Install node.js dependencies (or re-use them by detecting changes between builds)
- Build the assets
- Push them to an `S3` filesystem
- Send notifications / webhook when the build process has completed (or failed)
- Additionally setup `CNAME` record to point `assets.mydomain.com` to where the files are hosted. `S3` / `Cloudflare` caching?

<hr>

### Create a Laravel package to...

Build the assets:

```bash
assets:build --sha --tag --env
```

Download the assets if not using a CDN or working locally:

```bash
assets:download --sha --tag --env
```

Upload assets if you would like to build the assets locally or manually:
```bash
assets:upload --sha --tag --env
```

Retrieve a manifest file:

```bash
asssets:manifest --sha --tag --env
```

Delete assets:

```
assets:delete --sha --tag --env
```

For an easy setup:

```
assets:configure --usernmame --password --token
```



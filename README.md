# Asset building made easy


Deploying your `css` and `js` files can be  an annoying task:

- Should I build my assets locally and push changes to the repo?
- Should I add my `/public/css` and `/public/js` folders to `.gitignore`, and build my assets on deployment?
- What if I would like to serve my css and javascript assets from a CDN?
- What if I am rolling back to a previous version in case something went wrong?

Not to mention you would have to deal with:
- Versioning
- Minifying
- Dealing with different environments (eg. local/staging/production)
- Merge confilcts if you're working in a team

I think we can do something better :smile:

What if we could just run a simple commmand

```bash
assets:build
```

That would trigger a service to build your css/js assets.  
Push them to a `S3` cloud service and be done with it. 

```html
<link rel="stylesheet" href="https://assets.mydomain.com/latest/styles.css">
<script src="https://assets.mydomain.com/latest/main.js">
```

- The service should take care of cache busting.
- You should be able to pull in a specific version  
`assets.mydomain.com/`**version**`/main.js` -> /v1.0/main.js
- Or a specific commit hash  
`assets.mydomain.com/`**hash**`/main.js` -> /afb05cf1d/main.js
- Or from a specific branch  
`assets.mydomain.com/`**staging**`/`**hash**`/main.js`

Other options would be:
- Deliver the assets to version control with a `PR` or `auto commit`
- Downloading compiled assets from `S3`
- Rsync

### Benefits

- No more asset builing on production servers
- Serve your static js/css files from a CDN without the hassle
- Assets are available to you based on a commit hash and/or version tag
- Easily pull down the latests assets while developing or sharing in a team

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

> This could be a Laravel optimized package or any other console application which can provide the above functionality for non Laravel apps

<hr>

### Things to think about

- [Subresource Integrity](https://www.w3.org/TR/SRI/): <script integrity="sha384...">
- [Cross-origin resource sharing](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)

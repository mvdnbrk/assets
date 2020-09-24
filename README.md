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

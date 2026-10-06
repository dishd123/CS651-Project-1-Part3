# CS651 Project 1 Part 3: StudyBoard on Amazon S3

StudyBoard is the website of our group's startup idea: a computer vision and machine learning product that turns photos of class whiteboards and notes into readable notes, flashcards and a study tutor. This repository holds the full site source and serves it as an Amazon S3 static website, with no server.

| Item | Value |
| --- | --- |
| Live site | http://part3.studyboard.tech.s3-website-us-east-1.amazonaws.com/ |
| Wiki | https://github.com/dishd123/CS651-Project-1-Part3/wiki |
| YouTube video | VIDEO-LINK-PENDING |
| Setup and deploy steps | [S3Deployment/README.md](S3Deployment/README.md) |

The site is always up: S3 does not need a Learner Lab session. It is plain HTTP, because S3 website endpoints do not support HTTPS. Type `http://` in front of the address, and if Chrome says the connection is not secure, choose Continue to site.

## Group

CS651 Web Systems, CSU East Bay, Fall 2026: Disha Deshmukh, Huda Joad, Venkatesh Katta, Ndeye Traore.

## What is where

| Path | What it holds |
| --- | --- |
| `S3Deployment/` | `bucket-policy.json`, the public-read policy on the bucket, and a README with every setup and deploy step |
| `index.html`, `about.html`, `contact.html` | The Home, About and Contact pages |
| `app/`, `login/` | The App page and the Sign In page. Each HTML file is an empty shell that loads a React entry point from `src/` |
| `src/` | The React source: the App page (`src/app/`), the Sign In page (`src/login/`) and the shared menu `SiteNav.jsx` |
| `css/site.css`, `js/site.js` | The site's styles, and the shared menu, footer and contact form script for the plain HTML pages |
| `public/images/` | The photos used on the pages |

## Run it locally

```
npm install
npm run dev
```

Then open the address it prints. To deploy, see [S3Deployment/README.md](S3Deployment/README.md).

AI use: Claude (Anthropic), ChatGPT (OpenAI) and GitHub Copilot helped plan the deployment steps, work through code and commands, check AWS prices and limits, and draft and edit the documentation. We ran every command, made every console change and took every screenshot ourselves.

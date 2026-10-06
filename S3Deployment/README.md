# S3Deployment: host the StudyBoard site on Amazon S3

This folder records how the StudyBoard site is hosted as an Amazon S3 static website. The wiki page S3 Bucket Setup shows every step with screenshots.

| File | What it is |
| --- | --- |
| `bucket-policy.json` | The bucket policy: anyone may read (`s3:GetObject`) every object in `part3.studyboard.tech`, and nothing else |
| `README.md` | This file |

The live site: http://part3.studyboard.tech.s3-website-us-east-1.amazonaws.com/

All commands run from the repository root, with the AWS CLI using the Learner Lab credentials from AWS Details in `~/.aws/credentials`. Every command names the region, because the Learner Lab credentials do not set one.

## 1. Build the site

```
npm install
npm run build
find dist -name .DS_Store -delete
```

`npm run build` runs Vite, which writes the finished site into `dist/`. The `find` line removes hidden macOS files, so they never reach the public bucket.

## 2. Create the bucket

Console: S3, General purpose buckets, Create bucket. Region us-east-1, bucket type General purpose, Global namespace, name `part3.studyboard.tech`. Leave everything else at its default, including Block all public access on. Or with the CLI:

```
aws s3 mb s3://part3.studyboard.tech --region us-east-1
```

## 3. Upload the site

Check first with a dry run. Every `delete:` line must be a file under `assets/`:

```
aws s3 sync dist/ s3://part3.studyboard.tech/ --delete --region us-east-1 --dryrun
```

Then upload, and list what is in the bucket:

```
aws s3 sync dist/ s3://part3.studyboard.tech/ --delete --region us-east-1
aws s3 ls s3://part3.studyboard.tech/ --recursive --summarize --human-readable
```

## 4. Turn on static website hosting

Console: the bucket, Properties, Static website hosting, Edit. Enable, Host a static website, Index document `index.html`, Error document `index.html`, Save changes. Or with the CLI:

```
aws s3 website s3://part3.studyboard.tech/ --index-document index.html --error-document index.html
```

## 5. Make the site public

Turn off Block Public Access for the bucket. Console: Permissions, Block public access (bucket settings), Edit, clear Block all public access, Save changes, type `confirm`. Or with the CLI:

```
aws s3api put-public-access-block --bucket part3.studyboard.tech --region us-east-1 --public-access-block-configuration "BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false"
```

Add the bucket policy. Console: Permissions, Bucket policy, Edit, paste `bucket-policy.json`, Save changes. Or with the CLI:

```
aws s3api put-bucket-policy --bucket part3.studyboard.tech --region us-east-1 --policy file://S3Deployment/bucket-policy.json
```

## 6. Check

```
aws s3api get-bucket-policy-status --bucket part3.studyboard.tech --region us-east-1
curl -I http://part3.studyboard.tech.s3-website-us-east-1.amazonaws.com/
```

The first must print `"IsPublic": true`. The second must print `HTTP/1.1 200 OK` and `Server: AmazonS3`.

## Update the site

Build and sync again. Only new and changed files are uploaded, and old built files are deleted:

```
npm run build
find dist -name .DS_Store -delete
aws s3 sync dist/ s3://part3.studyboard.tech/ --delete --region us-east-1
```

Reload the page with Cmd+Shift+R, so the browser does not show its saved copy.

## Remove the site

This deletes every file and then the bucket:

```
aws s3 rb s3://part3.studyboard.tech --force --region us-east-1
```

AI use: Claude (Anthropic), ChatGPT (OpenAI) and GitHub Copilot helped with the commands and with drafting and editing these instructions. We ran every step on our bucket ourselves.

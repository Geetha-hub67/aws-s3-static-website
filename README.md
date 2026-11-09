# AWS S3 Static Website

## What I built
I hosted a static website on Amazon S3. I created a general purpose bucket in the Sydney region, uploaded an `index.html` page, and turned on static website hosting. The first version returned a 403 Forbidden error, so I diagnosed and fixed it.

![Website screenshot](site-screenshot.png)

## Problem: 403 Forbidden
- **Symptom:** Opening the S3 website endpoint showed `403 Forbidden`.
- **Diagnosis:** The file was in the bucket and website hosting was on, so the file existed. A 403 means the server answered but refused, so I checked permissions. Block public access was on, and no policy allowed visitors to read the files.
- **Fix:** I turned off Block public access for this bucket and added a read-only bucket policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadForWebsite",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::geetha-fixit-site-2026/*"
    }
  ]
}
```

- **Result:** The website loaded in the browser.

## What I learned
- A **403** means the server answered but refused, so I check permissions. A **404** means the file is missing.
- Turning off Block public access only removes the lock. A bucket policy is what grants access.
- `s3:GetObject` allows reading files only, not uploading or deleting.
- Public buckets should hold only files meant for everyone, and I turn the block back on when I'm done.

## Clean-up
I deleted the objects and the bucket after the lab so nothing was left public.

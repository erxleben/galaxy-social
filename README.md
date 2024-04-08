# galaxy Social

To create a post, follow these steps:

1. Create a pull request in the "toots" folder.
2. Inside the pull request, create a new file with the extension ".toot".
3. Use the following template for the post content:

```
---
social_media: bluesky, mastodon
images: "1.jpg", "2.png"
alt_texts: "1", "2"
mentions: bluesky[a], mastodon[b]
hashtags: bluesky[a, b], mastodon[a, b]
---
Text
```

# Add a new social media

To create a new plugin you have to add a python file with the function of `create_post` and add it to main.py

# Secrets

Usernames, Passwords, and tokens have to be saved in [github secrets](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions#creating-secrets-for-a-repository) then add to `publish_content.yml` and use as a environ object.
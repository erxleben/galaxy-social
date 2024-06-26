# Galaxy Social

## How to Create a Post

To create a post on Galaxy Social, follow these steps:

1. **Create a Branch:** Begin by [creating a branch](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository#creating-a-branch) in this repository or [your forked repository](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo#forking-a-repository).

2. **Add a New File:** Within the "posts" folder (or sub-folders), create a new file with the extension ".md" and [commit it to your branch](https://docs.github.com/en/repositories/working-with-files/managing-files/creating-new-files).

3. **Use the Post Template:** Utilize the following template for the post content:

```yaml
---
media:
  - bluesky
  - mastodon
  - matrix
  - slack

images:
  - url: https://example.com/a.jpg
    alt_text: A
  - url: https://example.org/b.png
    alt_text: B

mentions:
  bluesky:
    - a.bsky.social
  mastodon:
    - a
  matrix:
    - a:matrix.org

hashtags:
  bluesky:
    - a
    - b
  mastodon:
    - c
    - d
---
Your text content goes here.

```
**Notes on the Template:**
- Everything between the two `---` are metatags and should be in YAML format.

- "media" (Required): Ensure the media name is implemented inside the `plugins.yml`.

- "images" (Optional): Include links to desired images. The alt_text is optional but recommended.

- "mentions" and "hashtags" (Optional): Follow the specified format as shown in the template.

- "Your text content goes here." (Required): This is the content that will be posted to social media platforms. When the character limit is reached on a social media, it will be divided into several posts as a thread.

4. **Create a Pull Request:** Once your post is ready, [create a pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request?tool=webui#creating-the-pull-request) to the main branch from another branch or [from your fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork)

5. **Preview and Review:** After each pull request, the "Create Preview" GitHub action will run. It will preview the content as a comment to the pull request and highlight any errors that need to be fixed before merging.

6. **Publish Your Content:** Upon merging the pull request, the "Publish Content" GitHub action will run. The results will be added to `processed_files.json` in the processed_files branch.

By following these steps, you can effectively create and publish posts on Galaxy Social.


## Add a New Social Media Platform

Expanding the capabilities of Galaxy Social by adding a new social media platform is a straightforward process. Follow these steps to integrate a new platform:

1. **Create a Plugin File**: Begin by adding a Python file to the `lib/plugins` folder. This file should contain a class with a function named `create_post(content, mentions, hashtags, images, alt_texts)`. This function will handle the process of sending announcements to the desired social media platform.

2. **Update plugins.yml**: Next, update the `plugins.yml` file to include the new social media platform. Follow this template:

```yaml
  - name: name_of_the_media 
    class: file_name.class_name
    enabled: true
    config:
      token: $TOKEN_SAVED_IN_PUBLISH_CONTENT
      room_id: "room_id"
```
Ensure to replace `name_of_the_media` with the name of the new platform, and `file_name.class_name` with the appropriate file and class name for the plugin.
The `name` is then used in the `media` tag in the post file (posts/*.md) to determine the social media.

3. **Configuration**: In the `config` section, specify any required variables for initializing the plugin class. This may include authentication tokens, room IDs, or other platform-specific parameters. Any configuration that needs to be securely passed with GitHub secrets should be prefixed with `$` in order to be easily identifiable within the workflow YAML file.

4. **GitHub Secrets**: Add any tokens or variables required for the plugin to GitHub secrets. This ensures sensitive information is securely stored. Refer to [GitHub secrets documentation](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions#creating-secrets-for-a-repository) for guidance on creating secrets.

5. **Enable the Plugin**: Simply set `enabled: true` to enable the new social media platform. This ensures that it will be implemented when creating posts.

6. **Update publish_content.yml**: Finally, update the `publish_content.yml` file to include an environment variable referencing the token saved in GitHub secrets. Use the following template: (Don't put the prefixed `$` in here)

```yaml
...
env:
  TOKEN_SAVED_IN_PUBLISH_CONTENT: ${{ secrets.TOKEN_SAVED_IN_GITHUB_SECRETS }}
...
```

Replace `TOKEN_SAVED_IN_GITHUB_SECRETS` with the name of the secret containing the token for the new social media platform.

By following these steps, you can seamlessly integrate a new social media platform into Galaxy Social, expanding its reach and functionality.

### Duplicate a Social Media Platform with Different Token

If you need to use the same social media platform with different authentication tokens, you can duplicate the entry in the `plugins.yml` file. Follow these steps:

1. **Duplicate Entry**: Copy the entry for the social media platform in the `plugins.yml` file and paste it below the original entry.

2. **Update Name and Tokens**: Change the name of the duplicated entry to reflect the new configuration, and replace the token with the new authentication token.

3. **Configuration**: Adjust any other configuration parameters as needed for the duplicated entry.

4. **Use Name in Post**: Remember that the name you specify in the `plugins.yml` file must also be used within the `media` tag when creating a post. Ensure consistency to link the post with the correct social media platform.

By following these steps, you can effectively duplicate a social media platform with a different token for specific use cases or configurations.

## Run locally
You can execute this repository on your machine by running `lib/galaxy_social.py` with the argument `--files Files ...` or `--folder FOLDER` to process files, or add `--preview` to preview the file as markdown. Also there is `--json-out processed_files.json` that could be change where to save the json results output.

Remember to add the env variable that needed for each social media seperatly.

## Social media implemented
- Bluesky
- Mastodon
- Matrix: hashtags and alt_text for image are not working!
- Slack: mentions and hashtags are not working!
- Linkedin: not implemented but the first draft of python file is in the plugins.
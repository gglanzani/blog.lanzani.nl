---
title: Migrate Bitbucket repositories to GitHub
tags: ["LLM", "automation", "github", "code"]
author: Giovanni Lanzani
date: 2025-12-29T00:00:00Z
url: /2025/migrate-bitbucket-repositories-to-github
---

Just before Christmas, I got an email from Bitbucket saying that inactive workspaces will be deleted.

Almost 20 years ago, I created a Bitbucket workspace to host my university stuff (mostly for my research thesis and various papers).

And while I have the final PDFs, I did not want all that LaTeX to go to waste, so I decided to migrate everything to GitHub (which, since 20 years ago, added private repositories to their free offering).

Creating these scripts is a use case where LLMs really shine—although I had to doctor it a bit, as the way it concocted to create repositories on GitHub didn't work.

The final script is in [this](https://gist.github.com/gglanzani/d3d3830b0c90826eda120560ab9d024a) gist, and to use it you need the GitHub CLI utility `gh` installed (see the [instructions](https://github.com/cli/cli?tab=readme-ov-file#installation) if you don't have it) and ready to go.

Once that is done, it's a matter of writing the names of your repositories in a file called, f.e., `repos.txt` and then calling the script:

```sh
BB_WORKSPACE=your_bitbucket_workspace \
GH_OWNER=your_github_username \
./migrate-bitbucket.sh repos.txt
```

After a while, depending on the number of repositories, you should see

```sh
echo "🎉 All migrations complete."
```

Notes:

- Repositories are created private by default.
- The script assumes repositories with clashing names do not exist on GitHub.
- You need to have access to [Bitbucket](https://support.atlassian.com/bitbucket-cloud/docs/configure-ssh-and-two-step-verification/) and [GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) through SSH.

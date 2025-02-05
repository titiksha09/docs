---
title: Managing pull requests for dependency updates
intro: 'You manage pull requests raised by {% data variables.product.prodname_dependabot %} in much the same way as other pull requests, but there are some extra options.'
redirect_from:
  - /github/administering-a-repository/managing-pull-requests-for-dependency-updates
  - /code-security/supply-chain-security/managing-pull-requests-for-dependency-updates
  - /code-security/supply-chain-security/keeping-your-dependencies-updated-automatically/managing-pull-requests-for-dependency-updates
versions:
  fpt: '*'
  ghec: '*'
  ghes: '> 3.2'
type: how_to
topics:
  - Repositories
  - Version updates
  - Security updates
  - Pull requests
  - Dependencies
  - Vulnerabilities
shortTitle: Manage Dependabot PRs
---

{% data reusables.dependabot.beta-security-and-version-updates %}
{% data reusables.dependabot.enterprise-enable-dependabot %}

## About {% data variables.product.prodname_dependabot %} pull requests

{% data reusables.dependabot.pull-request-introduction %}

When {% data variables.product.prodname_dependabot %} raises a pull request, you're notified by your chosen method for the repository. Each pull request contains detailed information about the proposed change, taken from the package manager. These pull requests follow the normal checks and tests defined in your repository. 
{% ifversion fpt or ghec %}In addition, where enough information is available, you'll see a compatibility score. This may also help you decide whether or not to merge the change. For information about this score, see "[AUTOTITLE](/code-security/dependabot/dependabot-security-updates/about-dependabot-security-updates)."{% endif %}

If you have many dependencies to manage, you may want to customize the configuration for each package manager so that pull requests have specific reviewers, assignees, and labels. For more information, see "[AUTOTITLE](/code-security/dependabot/dependabot-version-updates/customizing-dependency-updates)."

{% ifversion dependabot-updates-paused %}

{% note %}

**Note**: If you don't interact with {% data variables.product.prodname_dependabot %} pull requests for a repository during a 90-day time period, {% data variables.product.prodname_dependabot %} considers your repository as inactive, and will automatically pause {% data variables.product.prodname_dependabot_updates %}. For more information about inactivity criteria, see "[About {% data variables.product.prodname_dependabot_version_updates %}](/code-security/dependabot/dependabot-version-updates/about-dependabot-version-updates#about-automatic deactivation-of-dependabot-updates)" and "[About {% data variables.product.prodname_dependabot_security_updates %}]((/code-security/dependabot/dependabot-security-updates/about-dependabot-security-updates#about-automatic deactivation-of-dependabot-updates)."

{% endnote %}

{% endif %}

## Viewing {% data variables.product.prodname_dependabot %} pull requests

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-pr %}
1. Any pull requests for security or version updates are easy to identify.
    - The author is {% ifversion fpt or ghec %}[dependabot](https://github.com/dependabot){% else %}dependabot{% endif %}, the bot account used by {% data variables.product.prodname_dependabot %}.
    - By default, they have the `dependencies` label.

## Changing the rebase strategy for {% data variables.product.prodname_dependabot %} pull requests

By default, {% data variables.product.prodname_dependabot %} automatically rebases pull requests to resolve any conflicts. If you'd prefer to handle merge conflicts manually, you can disable this using the `rebase-strategy` option. For details, see "[AUTOTITLE](/code-security/dependabot/dependabot-version-updates/configuration-options-for-the-dependabot.yml-file#rebase-strategy)."

## Allowing {% data variables.product.prodname_dependabot %} to rebase and force push over extra commits

By default, {% data variables.product.prodname_dependabot %} will stop rebasing a pull request once extra commits have been pushed to it. To allow {% data variables.product.prodname_dependabot %} to force push over commits added to its branches, include any of the following strings: `[dependabot skip]` , `[skip dependabot]`, `[dependabot-skip]`, or `[skip-dependabot]`, in either lower or uppercase, to the commit message.

## Managing {% data variables.product.prodname_dependabot %} pull requests with comment commands

{% data variables.product.prodname_dependabot %} responds to simple commands in comments. Each pull request contains details of the commands you can use to process the pull request (for example: to merge, squash, reopen, close, or rebase the pull request) under the "{% data variables.product.prodname_dependabot %} commands and options" section. The aim is to make it as easy as possible for you to triage these automatically generated pull requests.

You can use any of the following commands on a {% data variables.product.prodname_dependabot %} pull request.

- `@dependabot cancel merge` cancels a previously requested merge.
- `@dependabot close` closes the pull request and prevents {% data variables.product.prodname_dependabot %} from recreating that pull request. You can achieve the same result by closing the pull request manually.
- `@dependabot ignore this dependency` closes the pull request and prevents {% data variables.product.prodname_dependabot %} from creating any more pull requests for this dependency (unless you reopen the pull request or upgrade to the suggested version of the dependency yourself).
- `@dependabot ignore this major version` closes the pull request and prevents {% data variables.product.prodname_dependabot %} from creating any more pull requests for this major version (unless you reopen the pull request or upgrade to this major version yourself).
- `@dependabot ignore this minor version` closes the pull request and prevents {% data variables.product.prodname_dependabot %} from creating any more pull requests for this minor version (unless you reopen the pull request or upgrade to this minor version yourself).
- `@dependabot merge` merges the pull request once your CI tests have passed.
- `@dependabot rebase` rebases the pull request.
- `@dependabot recreate` recreates the pull request, overwriting any edits that have been made to the pull request.
- `@dependabot reopen` reopens the pull request if the pull request is closed.
- `@dependabot squash and merge` squashes and merges the pull request once your CI tests have passed.

{% data variables.product.prodname_dependabot %} will react with a "thumbs up" emoji to acknowledge the command, and may respond with a comment on the pull request. While {% data variables.product.prodname_dependabot %} usually responds quickly, some commands may take several minutes to complete if {% data variables.product.prodname_dependabot %} is busy processing other updates or commands.

If you run any of the commands for ignoring dependencies or versions, {% data variables.product.prodname_dependabot %} stores the preferences for the repository centrally. While this is a quick solution, for repositories with more than one contributor it is better to explicitly define the dependencies and versions to ignore in the configuration file. This makes it easy for all contributors to see why a particular dependency isn't being updated automatically. For more information, see "[AUTOTITLE](/code-security/dependabot/dependabot-version-updates/configuration-options-for-the-dependabot.yml-file#ignore)."

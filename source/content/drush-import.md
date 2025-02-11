---
title: Importing Drush Site Archives with Terminus
description: Import a Drupal Drush site archive using Terminus, the Pantheon CLI tool.
cms: "Drupal"
categories: [get-started]
tags: [migrate, terminus, drush]
---

One of the easiest ways to move an existing Drupal site to Pantheon is to import a [Drush archive file](https://drushcommands.com/drush-8x/core/archive-dump/) using our [Terminus command-line interface](/terminus). This automates the packaging of the existing installation, improving the chances of success.

Follow the steps below carefully to ensure that you import your Drupal site correctly.

## Before You Begin

- Create a Pantheon account with at least one free Dev site slot open. 

   - A [Pantheon account is free](https://dashboard.pantheon.io/register), and if you need an extra Dev site to try this out, reach out to your Account Manager and they can grant you one.

- If you have a non-Composer managed Drupal 7 site, verify that you are using Drush 8. Note that Composer-managed sites are not supported.

- If you have a Composer-managed Drupal 9 site, verify that you are using Drush 11. Note that only Composer-managed sites are supported.

   <Alert title="Note"  type="info" >

   If you are migrating a Drupal 7 or 8 site and want to upgrade to a Drupal 9 site, use one of the following guides instead:

      - Your site is Composer-managed: [Migrate a Composer Managed Drupal 9 Site from Another Platform](/guides/drupal-9-unhosted-composer)

      - Your site is not Composer-managed: [Migrate a Drupal 9 Site from Another Platform](/guides/drupal-9-unhosted)

   </Alert>

- Verify that you have Drush access on your existing Drupal site.

## Generate a Drush Archive

The first thing you'll need to do is to generate a Drush archive of your existing site. 

1. Navigate to site root.

1. Run the command below if you have Drush access to the site direct via the shell:

   ```bash{promptUser: user}
   drush archive-dump --destination=drush-archive.tar.gz
   ```

   - This creates a file called `drush-archive.tar.gz` that's available via the public internet. If you have the file locally, you can put it on Dropbox, S3, or any number of other places. The important thing is that you have a Drush archive that can be downloaded via a URL.

## Install Terminus

Install [Terminus 3](/terminus/terminus-3-0).

## Import Your Archive

<TabList>

<Tab title="Drupal 7 non-Composer" id="d7" active={true}>

1. Authenticate into Pantheon with Terminus:

   ```bash{promptUser: user}
   terminus auth:login --email=<email> --machine-token=<machine_token>
   ```

   You're now ready to perform command-line operations with Pantheon! For instance, you can run `terminus site:list` to get a list of your existing sites.

1. Start an import:

   ```bash{promptUser: user}
   terminus site:import <site> <url>
   ```

    <Alert title="Note" type="info">
    Before starting an import make sure you have an existing site on your account.
    </Alert>

  At that point the script will poll as the site containers are spun up and the archive is imported. You can wait for that to complete, or cancel out and check back in your Dashboard.


</Tab>

<Tab title="Drupal 9 Composer-managed" id="d9">

1. Install the [Terminus Conversion Tools](https://github.com/pantheon-systems/terminus-conversion-tools-plugin#installation) plugin. 

1. Run the command below to create the site on Pantheon. Change the `site-machine-name` to a unique name that you would like to use for your site.

```bash{promptUser: user}
 terminus conversion:import-site site-machine-name /path/to/archive.tar.gz --site-label="Site Label"
```

</Tab>

</TabList>

## Automate Imports

Every aspect of the Terminus process is designed to support automation. You can script imports to run several concurrently (or in serial).

Terminus is a rapidly evolving project, so stay tuned. Let us know what you would like to see, and forks and pull requests are always welcome!

## More Resources

- [Manually Migrate Sites to Pantheon](/migrate-manual)
- [Terminus Manual](/terminus)
- [Drupal Drush Command-Line Utility](/drush)

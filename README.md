# Mayhem for Code: Example CI Integration

[![Mayhem for Code](https://drive.google.com/uc?export=view&id=1JXEbfCDMMwwnDaOgs5-XlPWQwZR93fv4)](http://mayhem.forallsecure.com/)

A GitHub Action walk through for using Mayhem for Code to check for reliability, performance, and security issues in your application binary (packaged as a containerized [Docker](https://docs.docker.com/get-started/overview/) image) as a part of a CI pipeline.

Want to try it out? Visit [mCode GitHub Action](https://github.com/ForAllSecure/mcode-action/) to get set up!

## Example GitHub Actions Integration

Lighttpd version `1.4.15` has already been known to have vulnerabilities, which were fixed in subsequent updates such as `1.4.52`. We'll use Mayhem in a CI pipeline to build these targets and prove that vulnerabilities were found and actually fixed in the later update, and ultimately showcase how you can integrate Mayhem in your own code too.

We have two branches in this repository: `main` and `fixed`

The `main` branch contains a [Dockerfile](https://github.com/ForAllSecure/mcode-action-examples/blob/main/Dockerfile) with build instructions for setting up a containerized `lighttpd 1.4.15` application. The corresponding [Mayhemfile](https://github.com/ForAllSecure/mcode-action-examples/blob/main/Mayhemfile) contains the configuration options for the resulting CI pipeline Mayhem run. When executing a new workflow/pipeline using the mCode GitHub Action, the `lighttpd 1.4.15` will be built as a Docker image, pushed to the GitHub Container Registry, and ingested by Mayhem to fuzz the containerized target.

The `fixed` branch contains both a [Dockerfile](https://github.com/ForAllSecure/mcode-action-examples/blob/fixed/Dockerfile) and corresponding [Mayhemfile](https://github.com/ForAllSecure/mcode-action-examples/blob/fixed/Mayhemfile) as well, representing the containerized `lighttpd 1.4.52` application and it's Mayhem configuration options, respectively. Naturally, a developer may create a pull request from the `fixed` branch to the `main` branch to update (and fix) the existing `lighttpd 1.4.15` application on `main.` The mCode GitHub Action will automatically execute to initiate the CI pipeline and will use the existing test cases generated from the `lighttpd 1.4.15` Mayhem run to perform regression testing on the new `lighttpd 1.4.52` target, thereby proving previous crashing test cases have been fixed via the update.

## Getting Started

1. Fork this repository and create a Mayhem account to copy and paste your account token to GitHub Secrets for your repository:

    a. Navigate to [mayhem.forallsecure.com](https://mayhem.forallsecure.com/) to register an account.

    b. Click your profile drop-down and go to *Settings* > *API Tokens* to access your account API token.

    c. Copy and paste your Mayhem token to your [GitHub Secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-an-organization).

2. On the `main` branch, navigate to your GitHub repository `Actions` tab and execute a CI pipeline for the `main` branch. This will build and push the `lighttpd 1.4.15` image to the GitHub Container Registry and use Mayhem to fuzz the resulting Docker image. Results can be found in the `Security` tab or on the Mayhem server itself with more details about the specific run.

    > **Note:** You may be required to set your package visibility settings to `Public` to give Mayhem permissions to ingest your Docker image from the GitHub Container Registry. Click on your package in the right-hand pane of your GitHub repository and go to *Package Settings*. Then, scroll down to *Package Visibility* and set the package to `Public`.

3. Now, switch to the `fixed` branch. Create a pull request and set the PR to merge to `main` (for your forked repo). The mCode GitHub Action will automatically begin building and pushing the new `lighttpd 1.4.52` image to the GitHub Container Registry and use Mayhem to fuzz the resulting Docker image against the previous crashing test cases for `lighttpd 1.4.15` via regression testing. Results can then be found in the PR or on the Mayhem server itself with more details about the specific run. In addition, issues in the `Security` tab will also display defects that have been fixed as a result of the update.

Congrats! You just integrated Mayhem in a CI pipeline for the `lighttpd` target application! Extrapolating from this, you should be able to incorporate the same steps to integrate Mayhem into your own CI pipeline for your custom code.

## About Us

ForAllSecure was founded with the mission to make the world’s critical software safe. The company has been applying its patented technology from over a decade of CMU research to solving the difficult challenge of making software safer. ForAllSecure has partnered with Fortune 1000 companies in aerospace, automotive and high-tech industries, as well as the US Department of Defense to integrate Mayhem into software development cycles for continuous security. Profitable and revenue-funded, the company is scaling rapidly.

* [https://forallsecure.com/](https://forallsecure.com/)
* [https://forallsecure.com/mayhem-for-code](https://forallsecure.com/mayhem-for-code)
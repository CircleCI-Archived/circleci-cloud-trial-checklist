# CircleCI Trial Checklist
This checklist is a guide to making the most of your CircleCI trial. Managing DevOps tools can be complex, so we want to help you understand the full potential of CircleCI so that you can make an informed decision.

## Phase 1: Prerequisites
This phase is all about getting up-and-running with CircleCI. It is likely that you have already done some or all of these steps, but we want to include them to make sure that you have the best possible starting point for your trial.


### &#9744; Set up a CircleCI account (skip if you already have an account)
Head to [https://circleci.com/signup](https://circleci.com/signup). Click on the “Sign Up” button for whichever VCS provider you would like to use with CircleCI. Sign in with you VCS login and authorize CircleCI to access your repositories.


### &#9744; Remove trial setup account (skip if you've already removed the trial setup account)
If an account was created to set up your CircleCI trial, and you haven't removed it yet, do so now.


### &#9744; Install the CircleCI CLI (skip if you have already installed the CLI)
The CircleCI CLI allows you to run CircleCI actions locally, such as validating your config file, running local builds, and operating on Orbs. If you're using Homebrew on MacOS, run the following command:
```bash
$ brew install circleci
```
 Visit [this documentation](https://circleci.com/docs/2.0/local-cli/#section=configuration) for instructions on other installation methods and on using the CircleCI CLI.


### &#9744; First green build
If you have yet to run a build on CircleCI, the introduction walkthrough is one of the best ways to get acquainted with our system and to get your first green build. To get started, head over to the [Introduction documentation](https://circleci.com/docs/2.0/getting-started/#section=getting-started) and follow the instructions.


### &#9744; Read the rest of the "Getting Started" docs
Once you've gotten your first green build, continue to go through the ["Getting Started" section](https://circleci.com/docs/2.0/hello-world/#section=getting-started) of the CircleCI documentation. This information will give you a good starting point for adapting CircleCI to meet the unique needs of your projects.


## Phase 2: Adapting your configuration to your project
Now that you understand the basics of CircleCI, it's time to start customizing your configuration to build the optimal CI framework for your project. In this section we will give you short overviews of the features you can use and provide links to the documentation that will help you incorporate them into your `config.yml` file.

### &#9744; Add a project to CircleCI
You've probably added a new project to CircleCI by now, but here is a quick refresher as you start to experiment with CircleCI on your projects.

:page_facing_up: [__Full _Projects and Builds_ documentation__](https://circleci.com/docs/2.0/project-build/#section=getting-started)

1. On the left navigation bar, click “Add Projects”. The list automatically populates with the repositories in your VCS organization. You can search for projects using the “Filter projects…” search bar. If you are setting up a macOS project, click on the “macOS” tab.
2. If you aren’t seeing the correct repositories, make sure you are operating in the correct organization. You can switch between your organizations using the drop down in the top left corner.
3. Click “Set Up Project” next to the project you want to add to CircleCI.
4. On the next screen select the operating system and language for the project. And then copy the sample configuration code.
5. In a new browser tab, log in to your VCS, and navigate to the repository that you are setting up.
6. In the root directory of the repository, create a new directory named `.circleci`
7. Inside the `.circleci` folder, create a new file named `config.yml`
8. Paste the sample configuration code into the `config.yml` file and commit it to your master branch
9. Return to CircleCI and click “Start Building”
10. You should be taken to your first build, and you can see the logs from the execution environment


### &#9744; Read the configuration docs
The configuration section of our documentation will walk you through some of CircleCI's more advanced features that will help you begin to build out an advanced CI framework. Here's where you should start:

1. [Getting started with CircleCI config](https://circleci.com/docs/2.0/config-intro/#section=configuration)

   An overview of how CircleCI config and execution works. Introduction to commands, workflow orchestration, and executors.

2. [Configuration reference](https://circleci.com/docs/2.0/configuration-reference/#section=configuration)

   This is the source of truth for CircleCI config. If you're having a tough time writing configuration for a specific feature, this is a good place to start troubleshooting.

3. [Executors Configuration](https://circleci.com/docs/2.0/executor-intro/#section=configuration)
4. [Orbs Configuration](https://circleci.com/docs/2.0/orb-intro/#section=configuration)
5. [Example configurations](https://circleci.com/docs/2.0/examples-intro/#section=configuration)

   If you're looking for some inspiration, these example `config.yml` files could help you out.

These are just a few docs to help you get started. We suggest you review the rest of our documentation to gain the best possible understanding of how CircleCI can adapt to your specific use case.


### &#9744; Update your `config.yml` file

Once you've read through our documentation, spend some time updating your configuration to optimize it for your project. Here are a few questions to ask yourself when doing so:

1. Does your workflow optimize for maximum parallelism? The more jobs you can run asynchronously, the quicker your workflow will finish. Here is an [example config](https://circleci.com/docs/2.0/sample-config/#sample-configuration-with-parallel-jobs) with parallel jobs.
2. Are you running any jobs that are building very slowly? You can declare a more powerful [`resource_class`](https://circleci.com/docs/2.0/configuration-reference/#resource_class) such as `medium+` or `large` for these jobs to make them run faster. Larger resource classes have more vCPUs and RAM. You can also declare a `resource_class: small` for lightweight jobs like code health checks to save credits. There is no secret sauce here for choosing the right resource class, but experimenting can help you optimize your workflows over time.
3. Do you have commands, jobs, or executors that are repeated in your config? For any of these, you should create reusable components using top level keys:

```YAML
commands:
  reusable-command-1:
    # Create a reusable command here. A reusable command can be called as a step in a job.

jobs:
  reusable-job-1:
    # Create a reusable job here. A reusable job can be used in a workflow.

executors:
  reusable-executor-1:
    # Create a reusable executor here. Reusable executors can be called as the executor for any job.
```

4. Could any of your commands, jobs, or executors be reused in other projects? If so, you may want to [create an orb](#creating-orbs) that defines them.


### &#9744; Rerun a failed job with SSH for debugging

If one of your jobs fails, one of the best ways to debug it is to SSH into its container as it is running. You can do this by rerunning any failed job with SSH.

To rerun a job with SSH, open the job in the CircleCI UI, then, in the upper right corner of the page, click the arrow next to the _"Rerun workflow"_ button and select _"Rerun job with SSH"_.

<img src="/img/rerun-ssh.png" width="250px" />

From your CLI on your machine, SSH into the build using the information provided in the CircleCI UI. Happy debugging!


### &#9744; Optimize your caching strategy

One of the best ways to speed up your CircleCI builds is to use good caching practices. You can learn about CircleCI's caching capabilities in our [caching documentation](https://circleci.com/docs/2.0/caching/). We also have documentation dedicated to techniques for [optimizing your caching strategy](https://circleci.com/docs/2.0/optimizations/).

If you're using `docker build` in your workflow, you can also use [Docker Layer Caching](https://circleci.com/docs/2.0/docker-layer-caching/) to decrease Docker image build times.


### &#9744; Explore Orbs

[Orbs](https://circleci.com/orbs) are reusable, sharable packages of CircleCI configuration. Orbs allow you to import pre-build commands, jobs, and executors into your configuration file, so that you don't spend time reinventing the wheel.


#### Using existing orbs

Our ecosystem is already full of existing orbs built by our community. Have a look through the [Orbs Registry](https://circleci.com/orbs/registry) to see if any existing orbs could simplify your configuration. If want to learn more about using existing orbs, [read our documentation](https://circleci.com/docs/2.0/using-orbs/#section=configuration).


#### Creating orbs

If you have a use case for an orb that hasn't been created yet, it's easy to build it yourself so that you can use it across all of your projects. [Read our documentation](https://circleci.com/docs/2.0/creating-orbs/#section=configuration) on creating orbs to get started.

**Note** - At this time _all_ orbs are public, meaning that anyone can see their source or use them in a project. Make sure to follow best practices and parameterize any secrets your orb may use so that they are not published in your orb's source code.

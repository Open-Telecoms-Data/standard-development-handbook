# Set up your development environment

The OFDS development environment contains everything you need to edit, test and build the OFDS schema, codelists and documentation.

The recommended approach to setting up your development environment is to [use GitHub Codespaces](#use-github-codespaces), a pre-configured and hosted development environment. By using a hosted development environment, you avoid any problems that might arise from setting up a development environment on your local machine. However, if you are familiar with managing Python virtual environments and dependencies, you can [set up a development environment on your local machine](#set-up-a-local-development-environment).

## Use GitHub Codespaces

A codespace is a development environment that's hosted in the cloud. You can connect to a codespace from your browser or from Visual Studio Code.

```{note}
All GitHub personal accounts include a quota of free compute time and storage for GitHub Codespaces, after which usage is billable. It is expected that using the OFDS development environment is unlikely to exceed the free quota. However, it is good practice to [stop a codespace](https://docs.github.com/en/codespaces/developing-in-a-codespace/stopping-and-starting-a-codespace#stopping-a-codespace) when it is not in use and [delete a codespace](https://docs.github.com/en/codespaces/developing-in-a-codespace/deleting-a-codespace) when it is no longer needed. For more information on quotas and costs, see [GitHub Codespaces billing](https://docs.github.com/en/billing/concepts/product-billing/github-codespaces).
```

Follow the steps below to set up the OFDS development environment in a codespace.

1. Open the [OFDS GitHub repository](https://github.com/Open-Telecoms-Data/open-fibre-data-standard)
2. Click the  green **<> Code** button and select **Codespaces**
3. Click **Create codespace on \<branch-name\>**

A web-based version of Visual Studio Code will open in a new browser tab. For an introduction to Visual Studio Code, see [Get started with Visual Studio Code](https://code.visualstudio.com/docs/getstarted/getting-started). Key actions you'll need to perform when working on OFDS include:

* Navigating the standard repository and opening files using the [**explorer view**](https://code.visualstudio.com/docs/getstarted/userinterface#_explorer-view)
* Making changes to files using the [**editor**](https://code.visualstudio.com/docs/editing/codebasics)
* Running tests and building documentation using the [**terminal**](https://code.visualstudio.com/docs/terminal/basics)
* Committing and pushing changes using the [**source control interface**](https://code.visualstudio.com/docs/sourcecontrol/overview#_source-control-interface) panel

## Set up a local development environment

The recommended approach is to [use GitHub Codespaces](#use-github-codespaces) for your development environment. However, if you prefer to set up a development environment on your local machine, follow the steps below:

### Clone the repository

```bash
git clone git@github.com:Open-Telecoms-Data/open-fibre-data-standard.git
cd open-fibre-data-standard
```

Subsequent instructions assume that your current working directory is `open-fibre-data-standard`, unless otherwise stated.

### Update submodules

```bash
git submodule update --init --recursive --remote 
```

### Create and activate a Python virtual environment

The following instructions assume you have [Python 3.12](https://www.python.org/downloads/) or newer installed on your machine.

You can use your preferred method of managing Python virtual environments, e.g. to use `venv`, which is included in the standard Python installation:

1. Create a virtual environment called `.ve`.
    a. Linux/MacOS users

      ```bash
      python3 -m venv .ve
      ```

    a. Windows users

      ```bash
      py -m venv .ve
      ```

1. Activate the virtual environment. You must run this command for each new terminal session.
    a. Linux/MacOS users

      ```bash
      source .ve/bin/activate
      ```

    b. Windows users

      ```bash
      .\.ve\Scripts\activate
      ```

### Install requirements

```bash
pip install --upgrade pip setuptools
pip install -r requirements.txt
```
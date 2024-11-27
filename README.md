# Predictive AI Starter

This template runs a basic Predictive AI deployment workflow in DataRobot. It is a good starter for making a new recipe. We recommend modifying this README to include information about the recipe you are creating.
You should include a **summary** of your recipe as well as some **examples** of pipeline changes.

> [!WARNING]
> Application Templates are intended to be starting points that provide guidance on how to develop, serve, and maintain AI applications.
> They require a developer or data scientist to adapt, and modify them to business requirements before being put into production.

## Table of Contents
1. [Setup](#setup)
2. [Architecture Overview](#architecture-overview)
3. [Why Build AI Apps with DataRobot App Templates?](#why-build-ai-apps-with-datarobot-app-templates)
4. [Make Changes](#make-changes)
   - [Change the Data and How the Model is Trained](#change-the-data-and-how-the-model-is-trained)
   - [Change the Frontend](#change-the-front-end)
   - [Change the Language in the Frontend](#change-the-language-in-the-frontend)
5. [Share Results](#share-results)
6. [Delete All Resources](#delete-all-provisioned-resources)
7. [Setup for Advanced Users](#setup-for-advanced-users)
8. [Data Privacy](#data-privacy)

## Setup

> [!IMPORTANT]  
> If you are running in DataRobot Codespaces, `pulumi` is already configured and the repo automatically cloned;
> please skip to **Step 3**.

1. If `pulumi` is not already installed, install the CLI following instructions [here](https://www.pulumi.com/docs/iac/download-install/).
   After installing for the first time, restart your terminal and run:

   ```sh
   pulumi login --local  # omit --local to use Pulumi Cloud (requires separate account)
   ```

2. Clone the template repository.

   ```sh
   git clone https://github.com/datarobot/recipe-template.git
   cd recipe-template
   ```

3. Rename the file `.env.template` to `.env` in the root directory of the repo and populate your credentials.

   ```sh
   DATAROBOT_API_TOKEN=...
   DATAROBOT_ENDPOINT=...  # e.g. https://app.datarobot.com/api/v2
   MAIN_APP_LOCALE=... # e.g. en_US
   PULUMI_CONFIG_PASSPHRASE=...  # required, choose an alphanumeric passphrase to be used for encrypting pulumi config
   ```
   Use the following resources to locate the required credentials:
   - **DataRobot API Token**: Refer to the *Create a DataRobot API Key* section of the [DataRobot API Quickstart docs](https://docs.datarobot.com/en/docs/api/api-quickstart/index.html#create-a-datarobot-api-key).
   - **DataRobot Endpoint**: Refer to the *Retrieve the API Endpoint* section of the same [DataRobot API Quickstart docs](https://docs.datarobot.com/en/docs/api/api-quickstart/index.html#retrieve-the-api-endpoint).

4. In a terminal run:

   ```sh
   python quickstart.py YOUR_PROJECT_NAME  # Windows users may have to use `py` instead of `python`
   ```

Advanced users desiring control over virtual environment creation, dependency installation, environment variable setup
and `pulumi` invocation see [here](#setup-for-advanced-users).

## Architecture Overview

![Guarded RAG Architecture](https://s3.amazonaws.com/datarobot_public/drx/recipe_gifs/recipe-template.svg)

App Templates contain three families of complementary logic. For this template you can [opt-in](#make-changes) to fully 
custom AI logic and a fully custom frontend or utilize DR's off the shelf offerings:
- **AI Logic**: needed to service AI requests, generate predictions, and manage predictive models.
  ```
  notebooks/  # Model training logic
  ```
- **App Logic**: needed for user consumption; whether via a hosted frontend or integrating into an external consumption layer
  ```
  frontend/  # Streamlit frontend
  ```
- **Operational Logic**: needed to turn it all on
  ```
  __main__.py  # Pulumi program for configuring DR to serve and monitor AI & App logic
  infra/  # Settings for resources and assets created in DR
  ```

## Why build AI Apps with DataRobot App Templates?

App Templates transform your AI projects from notebooks to production-ready applications. Too often, getting models into production means rewriting code, juggling credentials, and coordinating with multiple tools & teams just to make simple changes. DataRobot's composable AI apps framework eliminates these bottlenecks, letting you spend more time experimenting with your ML and app logic and less time wrestling with plumbing and deployment.

- Start Building in Minutes: Deploy complete AI applications instantly, then customize AI logic or frontend independently - no architectural rewrites needed.
- Keep Working Your Way: Data scientists keep working in notebooks, developers in IDEs, and configs stay isolated - update any piece without breaking others.
- Iterate With Confidence: Make changes locally and deploy with confidence - spend less time writing and troubleshooting plumbing, more time improving your app.

Each template provides an end-to-end AI architecture, from raw inputs to deployed application, while remaining highly customizable for specific business requirements.

## Make changes

### Change the data and how the model is trained

1. Edit the following notebook:
   - `notebooks/train_model.ipynb`: Handles training data ingest and preparation and model training settings.

   The last cell of the notebook is required, as it writes outputs needed for the rest of the pipeline.
2. Run the revised notebook.
3. Run `pulumi up` to update your stack with these changes.

   ```sh
   source set_env.sh  # On windows use `set_env.bat`
   pulumi up
   ```

### Change the front-end

1. Ensure you have already run `pulumi up` at least once (to provision the time series deployment).
2. Streamlit assets are in `frontend/` and can be edited. After provisioning the stack
   at least once, you can also test the frontend locally using `streamlit run app.py` from the
   `frontend/` directory (don't forget to initialize your environment using `source set_env.sh`).
3. Run `pulumi up` again to update your stack with the changes.

   ```sh
   source set_env.sh  # On windows use `set_env.bat`
   pulumi up
   ```

#### Change the language in the frontend

Optionally, you can set the application locale in `meta_template/i18n.py`, e.g. `APP_LOCALE = LanguageCode.JA`. Supported locales are Japanese and English, with English set as the default.

## Share results

1. Log into the DataRobot application.
2. Navigate to **Registry > Applications**.
3. Navigate to the application you want to share, open the actions menu, and select **Share** from the dropdown.

## Delete all provisioned resources

```sh
pulumi down
```

Then run the jupyter notebook `notebooks/delete_non_pulumi_assets.ipynb`

## Setup for advanced users

For manual control over the setup process adapt the following steps for MacOS/Linux to your environent:

```sh
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
source set_env.sh
pulumi stack init YOUR_PROJECT_NAME
pulumi up 
```

e.g. for Windows/conda/cmd.exe this would be:

```sh
conda create --prefix .venv pip
conda activate .\.venv
pip install -r requirements.txt
set_env.bat
pulumi stack init YOUR_PROJECT_NAME
pulumi up 
```

For projects that will be maintained, DataRobot recommends forking the repo so upstream fixes and improvements can be merged in the future.

## Data Privacy
Your data privacy is important to us. Data handling is governed by the DataRobot [Privacy Policy](https://www.datarobot.com/privacy/), please review before using your own data with DataRobot.
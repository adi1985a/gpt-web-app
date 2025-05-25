# ☁️🤖 Altostra GPT WebApp Template: OpenAI & Serverless 🚀
_An Altostra template for deploying a serverless web application on AWS that allows users to send prompts to OpenAI GPT models via a simple web UI._

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) <!-- Assuming MIT if not specified -->
[![Altostra](https://img.shields.io/badge/Platform-Altostra-blueviolet.svg)]() <!-- Generic Altostra badge -->
[![AWS](https://img.shields.io/badge/Cloud-AWS-232F3E.svg?logo=amazon-aws)](https://aws.amazon.com/)
[![OpenAI](https://img.shields.io/badge/AI%20Model-OpenAI%20GPT-412991.svg?logo=openai)](https://openai.com/)
[![Python](https://img.shields.io/badge/Language-Python%20(Lambda)-3776AB.svg?logo=python&logoColor=white)]() <!-- Assuming Lambdas are Python -->
[![Node.js](https://img.shields.io/badge/Language-Node.js%20(Lambda)-339933.svg?logo=node.js)]() <!-- Or Node.js, adjust as needed -->

## 📋 Table of Contents
1.  [Overview](#-overview)
2.  [Template Content & Architecture](#-template-content--architecture)
    *   [AWS Infrastructure Components](#aws-infrastructure-components)
    *   [Application Flow](#application-flow)
3.  [Customization Options](#-customization-options)
    *   [Changing the GPT Model](#changing-the-gpt-model)
    *   [Changing the SSM Parameter Name for API Key](#changing-the-ssm-parameter-name-for-api-key)
4.  [Setup & Deployment with Altostra](#️-setup--deployment-with-altostra)
5.  [File Structure (Conceptual)](#-file-structure-conceptual)
6.  [Contributing](#-contributing)
7.  [License](#-license)
8.  [Contact](#-contact)

## 📄 Overview

This **Altostra template** provides the foundation for creating and deploying a serverless web application that enables users to interact with preselected OpenAI GPT textual models. The application features a simple web UI for submitting prompts and receiving responses. The entire infrastructure is managed and provisioned on **Amazon Web Services (AWS)** through Altostra's tooling.

## 🏗️ Template Content & Architecture

This Altostra template orchestrates the following AWS services and application logic:

### AWS Infrastructure Components:
*   🌍 **Amazon CloudFront**: Distributes the static website content globally with low latency and high transfer speeds, ensuring a fast user experience.
*   🪣 **Amazon S3 Bucket**: Stores the static website content (HTML, CSS, JavaScript files for the UI).
*   🔗 **Amazon API Gateway (REST API)**: Creates an HTTP API to serve as the backend endpoint for the web application.
*   ⚙️ **AWS Lambda Functions (3)**: Handle requests to the REST API:
    1.  **Prompt Handler Lambda**: Receives user prompts from the API Gateway, sends them to the configured OpenAI GPT model, and returns the model's response.
    2.  **API Key Storage Lambda**: Securely stores a user-provided OpenAI API key in **AWS Systems Manager (SSM) Parameter Store** as a `SecureString`.
    3.  **API Key Check Lambda**: Verifies the existence of the OpenAI API key in the SSM Parameter Store.

### 🔁 Application Flow:

1.  The website, served by CloudFront, first checks if an OpenAI API key is already configured and stored in SSM Parameter Store (via the API Key Check Lambda).
2.  **If the API key does not exist**: The web application prompts the user to enter their OpenAI API key. This key is then sent through API Gateway to the API Key Storage Lambda, which saves it securely in SSM Parameter Store.
3.  **Once the API key exists**: The web application allows the user to:
    *   Enter a prompt in the web UI.
    *   Send the prompt via API Gateway to the Prompt Handler Lambda.
    *   The Prompt Handler Lambda then communicates with the selected OpenAI GPT model to get a response.
    *   The response is returned to the web UI and displayed to the user.

## 🔧 Customization Options

Two primary environment variables can be configured within the Altostra project to alter the application's behavior:

1.  **GPT Model (`GPT_MODEL`)**:
    *   Determines which OpenAI textual model is used for generating responses.
    *   Defaults to `text-davinci-003`.
2.  **SSM Parameter Name for API Key (`GPT_API_KEY_PARAM`)**:
    *   The name of the parameter in AWS Systems Manager (SSM) Parameter Store where the OpenAI API key is stored.
    *   Defaults to `gpt-api-key`.

To change either of these options, you need to edit the project using **Visual Studio Code with the Altostra extension installed**. After making changes, create a new version and deploy it.

**Example deployment command (after changes and versioning):**
```bash
alto deploy my-stack --push --env production
```
## 🧠 Changing the GPT Model Used

_(Assumes your Altostra stack is named `my-stack`)_

1. Review available OpenAI textual models.
2. Copy the ID of the model you wish to use.
3. Open your project in **VSCode** with the **Altostra extension**.
4. In the **Altostra Cloud Designer**, locate the Lambda function named `prompt`.
5. Find the `GPT_MODEL` environment variable.
6. Paste the copied model ID as the value for `GPT_MODEL`.
7. Click **Save**.
8. Create a new project version using the Altostra extension.
9. Deploy the new version by either:
   ```bash
   alto deploy my-stack
   ```

---

## 🔐 Changing the Name of the SSM Parameter that Stores the API Key

This is useful for multiple deployments in the same AWS region, each with its own OpenAI API key.

1. Open the project in **VSCode** with the **Altostra extension**.
2. Navigate to the **Global variables** section.
3. Change the value of the global variable named `GPT_API_KEY_PARAM` to your new parameter name.
4. Click **Save**.
5. In the **Altostra Cloud Designer**, find the `api-key-params` resource (SSM Parameters).
6. Edit the parameter name to exactly match the new `GPT_API_KEY_PARAM` value.
7. Click **Save**.
8. Create a new project version and deploy:
   ```bash
   alto deploy my-stack
   ```

---

## 🚀 Setup & Deployment with Altostra

### ✅ Prerequisites

- An **Altostra Account**
- **Altostra CLI** installed and configured
- **VSCode** with the **Altostra Extension**
- **AWS account** configured for Altostra
- **OpenAI API Key**

### 📦 Obtain the Template

If this is a Git repository:

```bash
git clone <repository-url-of-altostra-template>
cd <template-directory>
```

Open the template folder in **VSCode**.

### 🛠️ Initial Deployment (Creating a Stack)

You may need to create an Altostra environment (e.g., `dev`, `production`):

```bash
alto deploy my-new-gpt-stack --env production --init
```

Follow the prompts. On first web access, the app may request your OpenAI API key — it will be stored in **AWS SSM Parameter Store**.

---

## 🌐 Accessing the Application

After deployment, the CLI or Altostra Console will provide the **CloudFront distribution URL** (public URL of your app).

---

## 🗂️ File Structure (Conceptual - Managed by Altostra)

```plaintext
altostra.json         # Altostra project definition
src/ or lambdas/      # Source code for Lambda functions
  ├── prompt_handler/
  ├── api_key_storage/
  └── api_key_check/
static-site/          # Web UI (HTML, CSS, JS)
README.md             # This documentation
```

> Note: Altostra manages code/infrastructure packaging internally via the Altostra extension.

---

## 📝 Technical Notes

- **Serverless Architecture**: Uses AWS Lambda + API Gateway for scalable backend.
- **API Key Security**: Stored as SecureString in **SSM Parameter Store**, accessed by Lambda with appropriate IAM role.
- **Idempotency**: Actions like storing the API key should be safe to repeat.
- **Cold Starts**: Initial Lambda invocations may be delayed after inactivity.
- **API Costs**: OpenAI API calls incur costs to the user.

---

## 🤝 Contributing

If this template is public or you're collaborating:

```bash
# Fork the repo
# Create a feature branch
git checkout -b feature/NewGptModelSupport

# Make your changes
# Commit changes
git commit -m 'Feature: Add support for GPT-4 model selection'

# Push and open PR
git push origin feature/NewGptModelSupport
```

> Follow best practices and any contribution guidelines provided in the repository.

---

## 📃 License

This project is licensed under the **MIT License**.  
Refer to the `LICENSE` file for more details.

---

## 📧 Contact

Application concept by **Adrian Lesniak**.

For questions or issues:

- Open an issue on this repository
- Or contact the template owner

---

> ✨ Explore AI-powered solutions — deployed seamlessly with Altostra & AWS.

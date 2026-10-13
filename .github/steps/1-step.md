## Step 1: Introduction to AI Actions

In this exercise, you'll learn to integrate AI capabilities directly into your GitHub Actions workflows using GitHub Copilot CLI. Let's start with understanding the key concepts and then create your first AI-powered workflow!

### 📖 Theory: Copilot CLI in Actions

#### 🤖 What is GitHub Copilot CLI?

**[GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/use-copilot-cli-in-actions)** lets you use Copilot models from a terminal, including in GitHub Actions workflows.

#### ⚙️ How GitHub Actions work with Copilot CLI

The [Copilot CLI integration](https://docs.github.com/en/copilot/how-tos/copilot-cli/use-copilot-cli-in-actions) works with GitHub Actions:

- 🔑 **Built-in Authentication**: The GitHub Actions built-in `GITHUB_TOKEN` can authenticate Copilot CLI requests, so no additional API key is required.

- 🔐 **Simple Permissions**: The `copilot-requests: write` permission allows the workflow to make Copilot requests.

- 🎯 **Easy Integration**: Install Copilot CLI with npm, then run it in non-interactive mode to generate a response.

> [!TIP]
>
> Want to dive deeper? Check out these resources:
>
> - 📖 [Using Copilot CLI in GitHub Actions](https://docs.github.com/en/copilot/how-tos/copilot-cli/use-copilot-cli-in-actions)

### ⌨️ Activity: Create Your First AI Workflow

Now that you understand the concepts, let's put them into practice! Open a new tab of this repository to follow these steps.

Let's create a simple workflow that we can trigger manually from the GitHub UI.

1. Navigate to the `Code` tab of your repository. Then into `.github/workflows/` directory.

1. Click `Add File` and create a new workflow file named

   ```text
   ask-ai.yml
   ```

1. Start by adding the workflow name, manual event trigger and required permissions:

   ```yaml
   name: Ask AI
   on:
     workflow_dispatch:

   permissions:
     copilot-requests: write
   ```

   > ❗ **Caution:** Copy the contents as provided, as this exact workflow name (`Ask AI`) is required to progress to next steps of this exercise.

1. Now we'll add a job that uses Copilot CLI.

   In this simple scenario, we'll ask the AI a simple hardcoded question and display the response in the workflow summary:

   ```yaml
   jobs:
     ask-ai:
       runs-on: ubuntu-latest

       steps:
         - uses: actions/setup-node@v6

         - name: Install Copilot CLI
           run: npm install -g @github/copilot

         - name: Generate AI response
           run: |
             echo "## 🤖 AI Response" >> "$GITHUB_STEP_SUMMARY"
             echo "" >> "$GITHUB_STEP_SUMMARY"
             copilot -p "Give me a programming joke." -s >> "$GITHUB_STEP_SUMMARY"
           env:
             GITHUB_TOKEN: {% raw %}${{ github.token }}{% endraw %}
   ```

   Your organization may need to allow Copilot CLI usage billed to the organization in its Copilot policy settings.

   > ❗ **Caution:** Be mindful of YAML formatting! GitHub's file editor will show red underlines for certain YAML errors.

1. Commit the workflow file directly to the `main` branch.

### ⌨️ Activity: Test Your AI Workflow

Now let's test the workflow you just created to see AI in action!

1. Navigate to the **Actions** tab in your repository.

1. In the left sidebar, look for the **Ask AI** workflow in the workflow list and click on it.

1. Click the **Run workflow** button, keep the default branch selected, and click the green **Run workflow** button to trigger it.

   <img width="900" alt="run workflow manual trigger" src="https://github.com/user-attachments/assets/89d96ce7-ca5e-4f5f-b8d0-25ebd5cdc4d6" />

1. Wait for the workflow to complete and check the workflow run summary to see the AI's response displayed in a nicely formatted way.

1. As your workflow completes successfully, Mona will automatically prepare the next step in your learning journey!

<details>
<summary>Having trouble? 🤷</summary><br/>

- **Workflow fails to run**: Ensure the workflow is complete and properly yaml formatted, if it's not then:
  - Find the issue in the workflow and commit the changes again to `main` branch
  - Try running the workflow again
- **No AI response**: Make sure Copilot CLI is installed, `GITHUB_TOKEN` is passed to the step, and the workflow has `copilot-requests: write` permission
- **Permission errors**: Double-check that the `copilot-requests: write` permission is properly configured in your workflow file

</details>

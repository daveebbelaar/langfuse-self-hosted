# Self-Host Langfuse
Run a self-hosted langfuse instance using this [self hosted guide](https://langfuse.com/docs/deployment/self-host).

## Dependencies
- Docker: https://www.docker.com/get-started/
- Python: https://www.python.org/downloads/
- OpenAI: https://platform.openai.com/docs/quickstart

## Getting Started
- Run `cp .env.example .env` and replace values appropriately in the .env file
- Run `docker compose up -d` to get the langfuse instance running.
- Navigate to `http://localhost:3000`
- Click `Sign up` to create your account

## Dashboard
- Create a new project
- Go to settings and click `Create new API keys`
- Copy the Secret Key and Public Key 
- Store keys in the `.env` file within your LLM project:
    
```
LANGFUSE_SECRET_KEY="sk-your-key"
LANGFUSE_PUBLIC_KEY="pk-your-key"
LANGFUSE_HOST="http://localhost:3000"
```

## Project Configuration
- Within your project, make sure you have langfuse installed with `pip install langfuse`
- Use the OpenAI integration and the observe decorator to trace and automatically capture all model parameters
  

```python
from langfuse.decorators import observe
from langfuse.openai import openai # OpenAI integration

@observe()
def story():
    return openai.chat.completions.create(
        model="gpt-3.5-turbo",
        max_tokens=100,
        messages=[
          {"role": "system", "content": "You are a great storyteller."},
          {"role": "user", "content": "Once upon a time in a galaxy far, far away..."}
        ],
    ).choices[0].message.content
 
@observe()
def main():
    return story()
 
main()
```

## Enable OAuth with GitHub

To set up GitHub authentication in your app, you need to:

1. Create a GitHub OAuth App
2. Configure Environment Variables

Here are the detailed steps:

### Step 1: Create a GitHub OAuth App

1. Go to GitHub Settings:
   - Navigate to [GitHub Developer Settings](https://github.com/settings/developers).

2. Register a New OAuth Application:
   - Click on "New OAuth App".
   - Fill in the required fields:
     - **Application name:** Your app's name.
     - **Homepage URL:** The URL of your app (e.g., `http://localhost:3000` for local development).
     - **Authorization callback URL:** The URL where GitHub will redirect users after authentication (e.g., `http://localhost:3000/api/auth/callback/github`).

3. Save the Application:
   - After filling in the details, click "Register application".
   - You will get a **Client ID** and **Client Secret**.

### Step 2: Configure Environment Variables

Add the obtained **Client ID** and **Client Secret** to your Langfuse `.env` file:

```plaintext
AUTH_GITHUB_CLIENT_ID=your_github_client_id
AUTH_GITHUB_CLIENT_SECRET=your_github_client_secret
AUTH_GITHUB_ALLOW_ACCOUNT_LINKING=true
```

Make sure to replace `your_github_client_id` and `your_github_client_secret` with the actual values from your GitHub OAuth app.
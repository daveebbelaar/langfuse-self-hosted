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
  

````python
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
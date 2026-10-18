# AfeeAI
"""
GPT-5 Unfiltered-Style AI Chatbot
Single-file Python implementation for GitHub.

Requirements:
    pip install openai

Configuration:
    Set OPENAI_API_KEY as an environment variable.
    Optionally set OPENAI_MODEL to an available model.
"""

import os
import sys
from openai import OpenAI

# ================= CONFIGURATION =================

APP_NAME = "GPT-5 Unfiltered-Style AI"
MODEL = os.getenv("OPENAI_MODEL", "gpt-5")

SYSTEM_PROMPT = """
You are a customizable AI assistant.

Your communication style:
- Be direct, candid, and conversational.
- Provide detailed answers when useful.
- Help with programming, research, writing, and brainstorming.
- Avoid unnecessary disclaimers and repetitive explanations.
- Clearly distinguish facts from speculation.
- Admit uncertainty instead of inventing information.
- Adapt your tone to the user's preferences.
- Follow applicable safety requirements.

Answer the user's actual question rather than unnecessarily
changing the subject.
"""

# ==================================================


def create_client():
    """Initialize the API client using an environment variable."""
    api_key = os.getenv("OPENAI_API_KEY")

    if not api_key:
        print("\nERROR: OPENAI_API_KEY is not configured.")
        print("Configure it in your environment before running this app.")
        sys.exit(1)

    return OpenAI(api_key=api_key)


def generate_response(client, conversation):
    """Generate an AI response from the conversation history."""
    try:
        response = client.responses.create(
            model=MODEL,
            instructions=SYSTEM_PROMPT,
            input=conversation,
        )

        return response.output_text

    except Exception as error:
        return f"API Error: {error}"


def main():
    """Run the interactive chatbot."""
    client = create_client()
    conversation = []

    print("=" * 50)
    print(f"  {APP_NAME}")
    print(f"  Model: {MODEL}")
    print("=" * 50)
    print("Type 'exit' to quit.")
    print("Type 'clear' to clear conversation history.\n")

    while True:
        try:
            user_message = input("You: ").strip()

            if not user_message:
                continue

            if user_message.lower() in {"exit", "quit"}:
                print("\nGoodbye!")
                break

            if user_message.lower() == "clear":
                conversation.clear()
                print("\nConversation history cleared.\n")
                continue

            conversation.append({
                "role": "user",
                "content": user_message,
            })

            answer = generate_response(client, conversation)

            print(f"\nAI: {answer}\n")

            conversation.append({
                "role": "assistant",
                "content": answer,
            })

        except KeyboardInterrupt:
            print("\n\nChat ended.")
            break


if __name__ == "__main__":
    main()

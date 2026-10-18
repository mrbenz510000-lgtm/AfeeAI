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
- Help with programming, research
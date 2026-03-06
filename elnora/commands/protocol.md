---
name: protocol
description: Generate a bioprotocol using Elnora AI
argument-hint: <description of the protocol you need>
---

# Generate Protocol

Generate a bioprotocol using the Elnora AI Platform.

## Steps

1. If a description was provided as an argument, use it. Otherwise, ask the user what protocol they need.

2. Use `elnora_generate_protocol` with the description:
   - `description`: The user's protocol request (e.g., "HEK 293 cell maintenance protocol with weekly passaging")

3. Present the generated protocol to the user.

4. If the user wants to iterate, find the task that was created and use `elnora_send_message` to continue the conversation.

## Example

User: `/elnora:protocol Standard PCR protocol for BRCA1 exon 11 amplification`

Action: Call `elnora_generate_protocol` with description "Standard PCR protocol for BRCA1 exon 11 amplification"

Present the result to the user formatted as markdown.

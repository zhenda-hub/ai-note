
## 安装

<https://github.com/NousResearch/hermes-agent>
<https://hermes-agent.nousresearch.com/docs/zh-Hans/getting-started/quickstart>

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash

source ~/.bashrc    # reload shell (or: source ~/.zshrc)
hermes              # start chatting!
hermes --tui
```

## 使用

```bash
hermes setup        # Run the full setup wizard (configures everything at once)
hermes              # Interactive CLI — start a conversation
hermes model        # Choose your LLM provider and model
hermes tools        # Configure which tools are enabled
hermes config set   # Set individual config values
hermes gateway      # Start the messaging gateway (Telegram, Discord, etc.)
hermes claw migrate # Migrate from OpenClaw (if coming from OpenClaw)
hermes update       # Update to the latest version
hermes doctor       # Diagnose any issues

hermes setup tools
```


## claw migrate

```bash
Migration Preview — 10 item(s) would be imported
  No changes have been made yet. Review the list below:

  Would import:
      soul                   → ~/.hermes/SOUL.md
      memory                 → ~/.hermes/memories/MEMORY.md
      user-profile           → ~/.hermes/memories/USER.md
      model-config           → ~/.hermes/config.yaml
      daily-memory           → ~/.hermes/memories/MEMORY.md
      agent-config           → config.yaml agent/compression/terminal
      env-var                → .env HERMES_GATEWAY_TOKEN
      env-var                → .env GLMCODE_API_KEY
      full-providers         → config.yaml custom_providers[glmcode]
      browser-config         → config.yaml browser

  Would overwrite (conflicts with existing Hermes config):
      skill                   Destination skill already exists
      skill                   Destination skill already exists
      skill                   Destination skill already exists
      skill                   Destination skill already exists
      skill                   Destination skill already exists
      skill                   Destination skill already exists
      skill                   Destination skill already exists
      skill                   Destination skill already exists
      skill                   Destination skill already exists
      skill                   Destination skill already exists
      skill                   Destination skill already exists
      skill                   Destination skill already exists
      skill                   Destination skill already exists
      skill                   Destination skill already exists

  Would skip:
      workspace-agents        No workspace target was provided
      messaging-settings      No Hermes-compatible messaging settings found
      secret-settings         No allowlisted Hermes-compatible secrets found
      discord-settings        No Discord settings found
      slack-settings          No Slack settings found
      whatsapp-settings       No WhatsApp settings found
      signal-settings         No Signal settings found
      provider-keys           All env values already present
      tts-config              No TTS configuration found in OpenClaw config
      command-allowlist       No allowlist patterns found
      shared-skills           No shared OpenClaw skills directories found
      tts-assets              Source directory not found
      raw-config-skip         Selected Hermes-compatible values were extracted; raw OpenClaw config was not copied.
      sensitive-skip          Contains secrets, binary state, or product-specific runtime data
      sensitive-skip          Contains secrets, binary state, or product-specific runtime data
      sensitive-skip          Contains secrets, binary state, or product-specific runtime data
      sensitive-skip          Contains secrets, binary state, or product-specific runtime data
      mcp-servers             No MCP servers found in OpenClaw config
      cron-jobs               No cron configuration found
      hooks-config            No hooks configuration found
      approvals-config        No approvals configuration found
      memory-backend          No memory backend configuration found
      ui-identity             No UI/identity configuration found
      logging-config          No logging/diagnostics configuration found

  ── Warnings ──
    ⚠ Config values — OpenClaw settings may not map 1:1 to Hermes equivalents
    ⚠ Gateway/messaging — this will configure Hermes to use your OpenClaw messaging channels
    ⚠ Instruction file — may contain OpenClaw-specific setup/restart procedures
    ⚠ Memory/context file — may reference OpenClaw-specific infrastructure

  Note: OpenClaw config values may have different semantics in Hermes.
  For example, OpenClaw's tool_call_execution: "auto" ≠ Hermes's yolo mode.
  Instruction files (.md) from OpenClaw may contain incompatible procedures.
```


## slash命令
 
```
/commands
/help

/new
/stop

```


## dashboard

http://127.0.0.1:9119/achievements

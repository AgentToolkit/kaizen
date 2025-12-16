# Working with Claude Code

Here are some instructions for how to get trajectories from Claude Code.

We use LiteLLM to proxy calls to Anthropic models and emit OpenTelemetry traces to Arize AI Phoenix.

Run `uv sync` first to install dependencies.

## 1. Run Azire AI Phoenix

Phoenix can consume OpenTelemetry events.

```
uv run python -m phoenix.server.main serve
```


## 2. Run LiteLLM proxy

The [litellm-config.yaml](litellm-config.yaml) file is configured to proxy calls to Anthropic models and emit OpenTelemetry traces to Arize AI Phoenix. Modify the config file if you want to change the models or add more models.

```
export ANTHROPIC_API_KEY=sk-ant-...
uv run litellm --config ./litellm-config.yaml --port 4000
```



## 3. Run Claude Code

```
export ANTHROPIC_BASE_URL=http://0.0.0.0:4000
export ANTHROPIC_AUTH_TOKEN=sk-dummy

claude --model claude-sonnet -p "who are you"
```


## 4. View trajectories in Phoenix

You can see the traces in the UI at http://localhost:6006/projects

You can also use the API:
```
curl "http://localhost:6006/v1/projects/default/spans" |jq .
```




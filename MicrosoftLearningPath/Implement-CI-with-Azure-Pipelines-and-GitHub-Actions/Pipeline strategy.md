# Pipeline strategy
## Agent demands
Each agent has a config stored as key-value pairs. **System capabilities** are automatically discovered (things like machone name or system type). The ones configured by user are **User-defined capabilities**.

## Implementing multi-agent builds
You can use multiple build agents to support multiple build machines. Either distribute the load, run builds in parallel, or use different agent capabilities.

Adding multiple jobs to a pipeline lets you:

- Break your pipeline into sections that need different agent pools or self-hosted agents.
- Publish artifacts in one job and consume them in one or more subsequent jobs.
- Build faster by running multiple jobs in parallel.
- Enable conditional execution of tasks.

At the organization level, you can configure the number of parallel jobs that are made available.
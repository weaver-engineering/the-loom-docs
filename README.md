# The Loom
The Loom is the tooling supporting agentic software development using the spec/test/build workflow.

The tool's provided by the Loom are not specifically agentic tools. Rather they are OS commands or stand alone services providing supporting agentic development uses cases. Just because a tool is used by an agent does not imply that the tool requires an agent to use it.

The Loom also provides features which require AI agent support, e.g. coding agents and supervising agents. an example of such a feature is the chunk scheduler which dispatches task phases to coding agents in sequence and monitors their progress with supervising agents.

Due to the tight coupling of the chunk scheduler with its sub-agent roles the sub agents and their tools are also defined by the Loom.

## Tools
### Gate Checks
The [gate-check](gate-checks-lld.md) tool is available in GitHub actions to police whether a push or PR to specific branches in origin. The gate-check tool is also available in the development environment so that the agent can quickly assert whether their exit gate is open. 
### Task Phases
The [task-phases](task-phasing-lld.md) tool overlays git with a state engine which applies an opinionated branching strategy to allow a mechanical derivation of which phase spec/test/build/quick a given task is in and what it's state is in that phase, noy-started/work-in-progress/ready etc. The combination of task phase and state allows a mechanical derivation of what the next step is for the task. In addition it provides a promote command which automatically promotes the task to the next phase if it is ready. the task-phases tool is used by the coding agents to check their state (can they start work now) for a given task without consuming tokens fir ghe agent to do the same check themselves. Similarly, its promote command allows the agent to consistently complete gheir work without requiring access to the gh api etc.
## Services
### The Shuttle 
The [shuttle](the-shuttle.md) is the service which performs the mechanical actions of implementing a given Chunk through the spec/test/build workflow. It fires up sub-agents to perform the test and build phases and runs a supervising agent to oversee the coding agent and only surface issues to the architect which genuinely require fhe architects input.
### The Chunk Scheduler 
The [chunk scheduler](chunk-scheduler-hld.md) is the service which performs the mechanical actions of scheduling/queuing the Shuttle over the Chunk sequence defined through the `Chunk Design` step of the Feature workflow.
## Agent Plugins
//TODO - define the agent pluging available through the Loom

# MAG-46 Retrospective Report

## Context 
MAG-46 was the first full run of the a design through Chunking, specification and implementation via mechanical execution of the Chunk Cycle.

MAG-46 is the implementation of the designed tadk-phases tool which provides a state engine over the top of a git repository. It uses an opinionated branching strategy and the preexisting [gate-checks](gate-checks-lld.md) tool to provide guard rails that not only protect main from unsuitable changes but also assist the agent and scheduler to know what phase state any chunk is in and what the correct next action is.

MAG-46 dog-fooded the the Chunk Cycle to deliver the very tooling that supports it `[task-phasing](task-phasing-lld.md)`. The lack of the tool and its dpg-fooding made the execution of the cycle overly cumbersome, as expected.

## What Went Well
- Updating the agent instructions and permissions while the agent was running was very valuable even though it was cumbersome.
- Using Claude (a frontier model) To monitor the agent was very valuable though polling for updates is not an efficient use of the frontier model.
- The chunk sizes seem well suited for the free tier model a in open code
- The free tier model only needed to ve replaced by the paid tier on 2 or 3 occasions, but otherwise performed well msking correct desicions throughout.
- the Feature was successfully delivered
- The quick route with its relaxed rules was invaluable in keeping the show on the road
- The mechanical sequencing switching between test and build was simple and effective. 
- The e2e testing at the end of each chunk cycle was very useful. It surfaced several issues throughout the completion of MAG-46.
- by the end the coding agents were able to complete their task phase with virtually zero supervision or intervention
- gate-checks worked well to protect main early on during MAG-46
- the standard responses were mostly generated reliably and accurately.  The only exceptions being when the agent had to be prompted during its phase and a standard prompt was not used so the agent did not know to reply end with a standard response 

## What Did Not Go Well
1.  The chunk sequencing was not well completed, several chunks were inoperable even after delivery since they depended on logic delivered by a later chunk
2. retiring prior behaviors was cumbersome and if missed wasted significant tokens to identify that the task phase was logically blocked.
3. allowing agent permissions through the opencode api failed due to a known bug.
4. the number of PRs required to complete a chunk was too high.
  - 1 to write the tests - required
  - 1 to build the implementation - required
  - 1 to update the task for the chunk start
  - 1 to update the task for the chunk end
  - 1 to retire prior behaviors (when necessary)
 5. There was no planned mechanism for dealing with PR review comments
 6. The supervising agent often did not automatically run the e2e tests at the end of a chunk
 7. The supervising agents review comments were not added to the PR
 8. the supervising agent did not always restart the coding agent with standard prompts leading to the coding agent not ending their run with a standard response. 
 9. Sometimes (rarely) the spec was inconsistent with the design
 10. Sometimes (rarely) the test conditions did not match the expected behaviors

## Future Actions For Improvement 
1. Define a Chunk sequencing step in the Feature workflow with its own defined processes and reconsiled outputs
2. Identify retired prior behaviors during Chunk sequencing.
3. Automate the retirement of prior behaviors or allow the test to automatically retire when it's replacement behaviour is extant. (If a behaviour is known at chunking to be retired in the future write the test to auto pass when its behaviour is superseded)
4. Use linear to record task progress rather than updating the task doc in the repo to reduce the number of required PRs
5. Define a sub-agent for the supervising agent so that it has a specific context informing its behaviour and use a fresh context for each chunk
6. Extend the standard prompts to include dealing with PR review comments
7. Extend the standard prompts to allow the supervising agent to provide guidance to the coding agent via a standard prompt so that the coding agent always ends with a standard response 
8. Include the e2e testing in the supervising agents instructions 
9. Include adding the supervising agents review comments to the PR to its instructions
10. The chunking process should have an explicit step to verify the step against the dirsgn
11. The design process should include a pass over ach of the specific behaviors to ensure that they enumerate the Feature actual behaviour requirements.
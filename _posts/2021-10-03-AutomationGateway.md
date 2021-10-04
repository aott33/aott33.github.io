# Testing out the Frankenstein Automation Gateway

Several months ago I found the [Frankenstein Automation Gateway](https://github.com/vogler75/automation-gateway) and thought it looked pretty interesting but I was a bit intimated to test it. Several months later and I am a little more confident and decided to test it out this weekend and turned out it was a great way to access my OPC UA server via MQTT and GraphQL.

Prior to this weekend, I had very little experience working with APIs. So, testing out this gateway was a great way to learn about APIs, specifically, GraphQL. See the [Resources Page](/Resources.md#apis) for helpful API resources.

## Steps taken:
1. Go to [Frankenstein Automation Gateway](https://github.com/vogler75/automation-gateway) and review the documentation 
2. Pull the [Docker image](https://hub.docker.com/r/rocworks/automation-gateway)
```
docker pull rocworks/automation-gateway
```
Note: I am using the [Docker Desktop for Windows](https://docs.docker.com/desktop/windows/install/)

3. Create a `config.yaml` file
4. See the [rocworks/automation-gateway docker page](https://hub.docker.com/r/rocworks/automation-gateway) for an example:
..* I added the following to my `config.yaml` file



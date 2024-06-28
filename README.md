# Determinism 

Experimental build system for complex, deterministic software builds:
- highly deterministic
- truely portable
- effictivly cached
- matrix builds

## Creating a new buildenv docker image
1) Modify [build-environment/Dockerfile](build-environment/Dockerfile)
2) Create a tag in the format "buildenv-v*.*.*"
3) Wait for action to complete
4) Docker image wille be tagged with the same tag, that you picked

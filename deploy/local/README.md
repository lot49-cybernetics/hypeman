# Local installation

These scripts prep the host machine to run a local development environment. Wherever possible the intent is to minimize this layer -- with most of the heavy lifting in the Kubernetes hosting layer and above. This is because we want to have both lateral and upward consistency:

1. Lateral: all developers have the same experience; no "it works for me"
2. Upward: local setup can be easily promoted up through staging and production

There is an assumption that local developer setups will be small, single-instance deployments, so the lateral consistency requirement is stronger than upward.

Currently only MacOS is supported; Windows via WSL2 will be supported in the future. See [MacOS README](macos/README.md).

# Snap Agent Responsibilities

This snap is now maintained by [automated-ken](https://github.com/kenvandine/automated-ken), a self-hosted agentic snap-maintenance dashboard.

## Automated Responsibilities

- **Version Detection**: Automatically polls upstream for new releases
- **Version Bump PRs**: Opens PRs to bump the pinned version when new upstream releases are detected
- **CI Monitoring**: Monitors build workflows and automatically fixes failing builds using Copilot cloud agent (via follow-up PRs)
- **YARF Testing**: Runs YARF (Yet Another Release Framework) tests to validate the snap
- **Channel Promotion**: Automates promotion from edge -> candidate -> stable channels

## Important Notes

- **No manual version edits**: Maintainers/agents should not hand-edit the pinned version in snap/snapcraft.yaml
- **Workflow removal**: The previously existing upstream release polling workflow has been removed as its job is now automated by automated-ken

All snap maintenance activities are now centralized and automated through the dashboard, ensuring consistent and reliable snap updates across the fleet.
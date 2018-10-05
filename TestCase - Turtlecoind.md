# Functional tests to be done on windows, mac and linux before merging to master and deploying a release.

Test Tile|Actions | Expected Results | Prerequisites |
----|---|---|---
Daemon connects to local DB | Launch TurtleCoind | The TurtleCoin Startup text is displayed :Welcome to TurtleCoin v0.x.x.xxxx | 
||Confirm the following message within the console | Loaded X default checkpoints|
|||Opening DB in DatabasePath |
|||DB opened in DatabasePath |
Local DB does not require resync|Launch TurtleCoind|The TurtleCoin Startup text is displayed :Welcome to TurtleCoin v0.x.x.xxxx|You already have a TurtleCoin DB
|||Loaded X default checkpoints|
|||Opening DB in Existing DatabasePath|
|||DB opened in ExistingDatabasePath|
||Within the console confirm |TurtleCoind starts synching with the network
Peer ID assigned|Launch TurtleCoind|The TurtleCoin Startup text is displayed :Welcome to TurtleCoin v0.x.x.xxxx|the p2pstate.bin does not exist before the launch of TurtleCoind

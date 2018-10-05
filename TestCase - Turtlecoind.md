# Functional tests to be done on windows, mac and linux before merging to master and deploying a release.

Test Tile|Actions | Expected Results | Prerequisites |
----|---|---|---
Daemon connects to local DB. | Launch TurtleCoind. | The TurtleCoin Startup text is displayed : *Welcome to TurtleCoin v0.x.x.xxxx*. | 
||Confirm the following message within the console. | Loaded X default checkpoints.|
|||Opening DB in *DatabasePath*. |
|||DB opened in *DatabasePath*. |
Local DB does not require resync.|Launch TurtleCoind.|The TurtleCoin Startup text is displayed : *Welcome to TurtleCoin v0.x.x.xxxx*.|You already have a TurtleCoin DB.
|||Loaded X default checkpoints.|
|||Opening DB in Existing *DatabasePath*.|
|||DB opened in Existing *DatabasePath*.|
||Confirm the following message within the console.|TurtleCoind starts synching with the network.
Peer ID assigned.|Launch TurtleCoind.|The TurtleCoin Startup text is displayed : *Welcome to TurtleCoin v0.x.x.xxxx*.|The p2pstate.bin does not exist before the launch of TurtleCoind.
||Confirm the following message within the console.|Generated new peer ID: PEER_ID.
Connections to multiple peers are made.|Launch TurtleCoind.|The TurtleCoin Startup text is displayed : *Welcome to TurtleCoin v0.x.x.xxxx*.|
||Within the console use the "print_cn" command.|The print_cn command displays the list of connected peers using the format :  *[OUTGOING]IP_ADDRESS:PEER_ID*.
||Within the console reuse the "print_cn" command after 30 seconds.|The print_cn command displays the list of connected peers using the format : *[OUTGOING]IP_ADDRESS:PEER_ID*.||
Daemon able to sync with external checkpoints.|Launch TurtleCoind using the instruction for checkpoints import : https://github.com/turtlecoin/checkpoints|Confirm you see displayed in the console the Expected Output section of this page.|You already have a TurtleCoin DB that is not in full sync with the network.|Confirm you see displayed in the console the Expected Output section of this page.|You already have a TurtleCoin DB that is not in full sync with the network.
||Wait for the entire process to complete.|Once the import of checkpoints is completed, Turtlecoind should start sycing the block after the last checkpoint in the csv.|
||Wait for TurtleCoind to get fully synced with the network.|TurtleCoind should be able to sync fully with the network with an existing DB.
Daemon able to sync without external checkpoints.|Launch TurtleCoind.|The TurtleCoin Startup text is displayed : *Welcome to TurtleCoin v0.x.x.xxxx*.|You already have a TurtleCoin DB that is not in full sync with the network.
||Confirm the following message within the console.|Loaded X default checkpoints.|
|||Opening DB in DatabasePath.
|||DB opened in DatabasePath.
||Wait for TurtleCoind to get fully synced with the network.|TurtleCoind should be able to sync fully with the network with an existing DB.
Daemon able to sync from 0 with external checkpoints.|Launch TurtleCoind using the instruction for checkpoints import : https://github.com/turtlecoin/checkpoints|Confirm you see displayed in the console the Expected Output section of this page.|You don't have an existing TurtleCoin DB.
||Wait for the entire process to complete.|Once the import of checkpoints is completed, Turtlecoind should start sycing the block after the last checkpoint in the csv.
||Wait for TurtleCoind to get fully synced with the network.|TurtleCoind should be able to sync fully from 0% to 100%.
Daemon able to sync from 0 without external checkpoints.|Launch TurtleCoind.|The TurtleCoin Startup text is displayed : *Welcome to TurtleCoin v0.x.x.xxxx*.|You don't have an existing TurtleCoin DB.
||Confirm the following message within the console.|DB not found in *DATABASE_PATH*. Creating new DB.
||Wait for TurtleCoind to get fully synced with the network.|TurtleCoind should be able to sync fully with the network with an existing DB.
Daemon stays synchronized for 24 hours.|Launch TurtleCoind.|The TurtleCoin Startup text is displayed : *Welcome to TurtleCoin v0.x.x.xxxx*.
|||The Daemon starts the syncrhonization process.|
||Wait for 24 hours.|The Daemon is still up to date with the TurtleCoin network.

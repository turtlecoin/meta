# zedWallet
# Functional tests to be done on windows, mac and linux before merging to master and deploying a release.

Test Tile|Actions | Expected Results | Prerequisites
----|---|---|---
Connect to local daemon|Launch zedwallet.||The tested version of Turtlecoind is already running.
|x|x|x|x
Connect to a remote daemon|Launch zedwallet using command : zedwallet.exe --remote-daemon PublicNodeUrl:PublicNodePort.|TurtleCoin VX.X.X zedwallet is displayed within the console with the belowg options:|
|||The public node fee is displayed if any
|||1.open
|||2.create
|||3.seed_restore
|||4.key_restore
|||5.view_wallet
|||6.exit
Send a transaction|Within the zedWallet Console type "7", then press enter.|"What address do you want to transfer to?:" is displayed within the console.|zedWallet is already started by using local daemon or public node.+An existing wallet is already opened in zedWallet.
||Type a valid Turtlecoin address that you have access to (I.E. : Your TipJar address in Discord.).|"How much TRTL do you want to send?": is displayed within the console.|
||Enter x amount of trtl to send|

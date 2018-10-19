<table class="table table-striped table-bordered">
   <thead>
      <tr>
         <th>Test Tile</th>
         <th>Actions</th>
         <th>Expected Results</th>
         <th>Prerequisites</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>Daemon connects to local DB.</td>
         <td>Launch TurtleCoind.</td>
         <td>The following text is displayed within the console : </br></br>
            <li>Welcome to TurtleCoin <em>v0.x.x.xxxx</em></li>
            <li>Loaded X default checkpoints.</em></li>
            <li>Opening DB in <em>DatabasePath.</em></li>
            <li>DB opened in <em>DatabasePath.</em></li>
            <li>Loaded X default checkpoints.</em></li>
            <li>Loaded X default checkpoints.</em></li>
         </td>
         <td></td>
      </tr>
      <tr>
         <td>Local DB does not require resync.</td>
         <td>Launch TurtleCoind.</td>
         <td>The following text is displayed within the console : </br></br>
            <li>Welcome to TurtleCoin <em>v0.x.x.xxxx</em></li>
            <li>Loaded X default checkpoints.</em></li>
            <li>Opening DB in <em>DatabasePath.</em></li>
            <li>DB opened in <em>DatabasePath.</em></li>
            <li>TurtleCoind starts synching with the network.</li>
            <li>Loaded X default checkpoints.</em></li>
         </td>
         <td>You already have a TurtleCoin DB that is not in full sync with the network.</td>
      </tr>
      <tr>
         <td></td>
         <td>Confirm the following within the console.</td>
         <td>TurtleCoind starts synching with the network.</td>
         <td></td>
      </tr>
      <tr>
         <td>Peer ID assigned.</td>
         <td>Launch TurtleCoind.</td>
         <td>Confirm this specific text is displayed within the console : </br></br>
            <li>Generated new peer ID: <em>PEER_ID.</em></li>
         </td>  
         <td>The p2pstate.bin does not exist before the launch of TurtleCoind.</td>
      </tr>
      <tr>
         <td>Connections to multiple peers are made.</td>
         <td>Launch TurtleCoind.</td>
         <td>The TurtleCoin Startup text is displayed :</br></br><em>Welcome to TurtleCoin v0.x.x.xxxx</em>.</td>
         <td></td>
      </tr>
         <td></td>
         <td>Wait for a couple of minutes to get multiple connections to peers</td>
         <td>Daemon continues sync process</td>
         <td></td>
      </tr>
      <tr>
         <td></td>
         <td>Within the console use the “print_cn” command.</td>
         <td>The print_cn command displays the list of connected peers using the format :
               </br></br><em>[OUTGOING]IP_ADDRESS:PEER_ID.</em></td>
         <td></td>
      </tr>
      <tr>
         <td></td>
         <td>Within the console reuse the “print_cn” command after 30 seconds.</td>
         <td>The print_cn command displays the list of connected peers using the format :
              </br></br><em>[OUTGOING]IP_ADDRESS:PEER_ID</em></td>
         <td></td>
      </tr>
      <tr>
         <td>Daemon able to sync with external checkpoints.</td>
         <td>Launch TurtleCoind using the instruction for checkpoints import : <ahref="https://github.com/turtlecoin/checkpoints">https://github.com/turtlecoin/checkpoints</a></td>
         <td>Confirm you see displayed in the console the Expected Output section of this <a href="https://github.com/turtlecoin/checkpoints">page</a></td>
         <td>You already have a TurtleCoin DB that is not in full sync with the network.</td>
      </tr>
      <tr>
         <td></td>
         <td>Wait for the entire process to complete.</td>
         <td>Once the import of checkpoints is completed, Turtlecoind should start sycing the block after the last checkpoint in the                  csv.</td>
         <td></td>
      </tr>
      <tr>
         <td></td>
         <td>Wait for TurtleCoind to get fully synced with the network.</td>
         <td>TurtleCoind should be able to sync fully with the network with an existing DB.</td>
         <td></td>
      </tr>
      <tr>
         <td>Daemon able to sync without external checkpoints.</td>
         <td>Launch TurtleCoind.</td>
         <td>The TurtleCoin Startup text is displayed :</br></br><em>Welcome to TurtleCoin v0.x.x.xxxx</em>.</td>
         <td>You already have a TurtleCoin DB that is not in full sync with the network.</td>
      </tr>
      <tr>
         <td></td>
         <td>Wait for TurtleCoind to get fully synced with the network.</td>
         <td>TurtleCoind should be able to sync fully with the network with an existing DB.</td>
         <td></td>
      </tr>
      <tr>
         <td>Daemon able to sync from 0 with external checkpoints.</td>
         <td>Launch TurtleCoind using the instruction for checkpoints import : <a href="https://github.com/turtlecoin/checkpoints">https://github.com/turtlecoin/checkpoints</a></td>
         <td>Confirm you see displayed in the console the Expected Output section of this<a                 href="https://github.com/turtlecoin/checkpoints">page</a></td>
         <td>You don’t have an existing TurtleCoin DB.</td>
      </tr>
      <tr>
         <td></td>
         <td>Wait for the entire process to complete.</td>
         <td>Once the import of checkpoints is completed, Turtlecoind should start sycing the block after the last checkpoint in the csv.</td>
         <td></td>
      </tr>
      <tr>
         <td></td>
         <td>Wait for TurtleCoind to get fully synced with the network.</td>
         <td>TurtleCoind should be able to sync fully from 0% to 100%.</td>
         <td></td>
      </tr>
      <tr>
         <td>Daemon able to sync from 0 without external checkpoints.</td>
         <td>Launch TurtleCoind.</td>
         <td>The TurtleCoin Startup text is displayed :</br></br><em>Welcome to TurtleCoin v0.x.x.xxxx</em>.</td>
         <td>You don’t have an existing TurtleCoin DB.</td>
      </tr>
      <tr>
         <td></td>
         <td>Confirm the following message within the console.</td>
         <td><li>DB not found in <em>DATABASE_PATH</em>.</li> 
            <li>Creating new DB.</li>
            </td>
         <td></td>
      </tr>
      <tr>
         <td></td>
         <td>Wait for TurtleCoind to get fully synced with the network.</td>
         <td>TurtleCoind should be able to sync fully with the network with an existing DB.</td>
         <td></td>
      </tr>
      <tr>
         <td>Daemon stays synchronized for 24 hours.</td>
         <td>Launch TurtleCoind.</td>
         <td>The TurtleCoin Startup text is displayed :</br></br><em>Welcome to TurtleCoin v0.x.x.xxxx</em>.</td>
         <td></td>
      </tr>
      <tr>
         <td></td>
         <td></td>
         <td>The Daemon starts the syncrhonization process.</td>
         <td></td>
      </tr>
      <tr>
         <td></td>
         <td>Wait for 24 hours.</td>
         <td>The Daemon is still up to date with the TurtleCoin network.</td>
         <td></td>
      </tr>
   </tbody>
</table>

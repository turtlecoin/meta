<table>
  <tr>
    <th>Test Title</th>
    <th>Action</th>
    <th>Expected Result</th>
    <th>Prerequisites</th>
  </tr>
  <tr>
    <td>Connect to local daemon</td>
    <td>Launch zedwallet.</td>
        <td>The following is displayed within the console:</br></br>
        <li>TurtleCoin VX.X.X zedwallet</li>
        <li>1.open</li>
        <li>2.create</li>
        <li>3.seed_restore</li>
        <li>4.key_restore</li>
        <li>5.view_wallet</li>
        <li>6.exit</li>
    </td>
    <ul>
    <td><li>The tested version of Turtlecoind is already running.</li></td>
    </ul>
  </tr>
  <tr>
    <td>Connect to a remote daemon</td>
    <td>Launch zedwallet using command :</br></br>zedwallet.exe --remote-daemon <i>PublicNodeUrl:PublicNodePort</i>.</td>
    <td>The following is displayed within the console:</br></br>
        <li>TurtleCoin VX.X.X zedwallet</li>
        <li>The public node fee is displayed if any</li>
        <li>1.open</li>
        <li>2.create</li>
        <li>3.seed_restore</li>
        <li>4.key_restore</li>
        <li>5.view_wallet</li>
        <li>6.exit</li>
    </td>
    <td></td>
  </tr>
  <tr>
    <td>Send a transaction</td>
    <td>Within the zedWallet Console type "7", then press enter.</td>
    <td>The following is displayed within the console:</br></br>"<i>What address do you want to transfer to?:</i>"</td>
    <td><li>The tested version of Turtlecoind is already running.</li>
        <li>An existing wallet is already opened in zedWallet.</li>
    </td>
  </tr>
  <tr>
    <td></td>
    <td>Type a valid Turtlecoin address that you have access to (I.E. : Your TipJar address in Discord.), then press enter.</td>
    <td>The following is displayed within the console:</br></br>"<i>How much TRTL do you want to send?:</i>"</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>Enter x amount of trtl to send, then press enter.</td>
    <td>The following is displayed within the console:</br></br>
        <li><i>"What fee do you want to use?"</li>
        <li>"Hit enter for the default fee of 0.10 TRTL:</i>"</li>
    </td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>Enter a random fee,then press enter. Or press enter to use the default fee.</td>
    <td>The following is displayed within the console:</br></br>
        <li>Confirm Transaction?</li>
        <li>You are sending <i>X</i> TRTL, with a network fee of <i>X</i> TRTL, and a node fee of <i>X</i> TRTL.</li>
        <li>FROM:<i>WalletName</i></li>
        <li>TO:<i>TRTLAddress</i></li>
        <li>Is this correct? (Y/n):</li>
        </td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>press "y"</td>
    <td>The following is displayed within the console:</br></br>"<i>Enter password</i>"</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>Enter your wallet password, then press enter.</td>
    <td>The following is displayed within the console:</br></br>"<i>Transaction has been sent</i></br>Hash:<i>TransactionHash</i></br></td>
    <td></td>
  </tr>
  <tr>
    <td>Receive a transaction</td>
    <td>From your tipjar or another wallet send some trtl to the address opened in the tested version of zedwallet.</td>
    <td>The transfer is sent to the wallet opened in the tested version of zedwallet.</td>
    <td><li>zedWallet is already started by using local daemon or public node.</li>
      <li>An existing wallet is already opened in zedWallet.</li>
    </td>
  </tr>
  <tr>
    <td></td>
    <td>The following is displayed within the console:</td></br>
    <td>
      <li><i>New transaction found!</i></li>
      <li>Incoming Transfer:</li>
      <li>Hash:<i>TransactionHash</i></li>
      <li><i>Amount: x TRTL</i></li>
    </td>
    <td></td>
  </tr>
  <tr>
    <td>Perform a fusion transaction</td>
    <td></td>
    <td></td>
    <td><li>zedWallet is already started by using local daemon or public node.</li>
      <li>An existing wallet is already opened in zedWallet.</li>
    </td>
  </tr>
  <tr>
    <td>Export keys and seeds</td>
    <td>Within the zedWallet Console type "backup", then press enter.</td>
    <td>The following is displayed within the console:</br></br>
         <em>"Enter your password"</em></td>
    <td><li>zedWallet is already started by using local daemon or public node.</li>
      <li>An existing wallet is already opened in zedWallet.</li></td>
  </tr>
  <tr>
    <td></td>
    <td>Enter your wallet password, then press enter.</td>
    <td>The following is displayed within the console:</br></br>
      <li>"Private spend key:"<em>PrivateKeyValue</em></li>
      <li>"Private view key:"<em>PrivateViewKeyValue</em></li>
      <li>"Mnemonic seed:"<em>MnemonicSeedValue</em></li> 
      </td>
    <td><li>zedWallet is already started by using local daemon or public node.</li>
      <li>An existing wallet is already opened in zedWallet.</li></td>
  </tr>
  <tr>
    <td>Import from keys correctly</td>
    <td>Within the zedWallet Console, Select Option 4: "key_restore".</td>
    <td>The following is displayed within the console:</br></br>
        <em>"Enter your private spend key:"</em></td>
    <td><li>zedWallet is already started by using local daemon or public node.</li></td>
  </tr>
  <tr>
    <td></td>
    <td>Enter a valid private spend key, then press enter.</td>
    <td>The following is displayed within the console:</br></br>
        <em>"Enter your private view key:"</em></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>Enter a valid private view key, then press enter.</td>
    <td>The following is displayed within the console:</br></br>
        <em>"What would you like to call your new wallet?:"</em></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>Enter a new wallet name, then press enter.</td>
    <td>The following is displayed within the console:</br></br>
        <em>"Give your new wallet a password:"</em></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>Enter a new password, then press enter.</td>
    <td>The following is displayed within the console:</br></br>
        <em>"Confirm your new password:"</em></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>Enter the new password confirmation, then press enter.</td>
    <td>The following is displayed within the console:</br></br>
        <em>"What height would you like to begin scanning your wallet from?</br></br>
        This can greatly speed up the initial wallet scanning process.</br></br>
        If you do not know the exact height, err on the side of caution so transactions do not get missed.</br></br>
        Hit enter for the sub-optimal default of zero:"</em></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>Enter a valid scan-height value, then press enter.</td>
    <td>Zedwallet scans the blockchain from the selected scan-height value</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>Once the process is complete, confirm</td>
    <td>The wallet is now fully usable.</td>
    <td></td>
  </tr>
  <tr>
    <td>Import from seeds correctly</td>
    <td>Within the zedWallet Console, Select Option 3: "seed_restore".</td>
    <td>UPDATE ME</td>
    <td><li>zedWallet is already started by using local daemon or public node.</li></td>
  </tr>
  <tr>
    <td>Perform a full reset</td>
    <td>Within the zedwallet console, type "reset", then press enter.</td>
    <td>The following is displayed within the console:</br></br>
        <em>"What height would you like to begin scanning your wallet from?</br></br>
        This can greatly speed up the initial wallet scanning process.</br></br>
        If you do not know the exact height, err on the side of caution so transactions do not get missed.</br></br>
        Hit enter for the sub-optimal default of zero:"</em></td>
    <td><li>zedWallet is already started by using local daemon or public node.</li></td>
  </tr>
  <tr>
    <td></td>
    <td>Enter a desired scan-height, then press enter. Or press enter to scan from 0.</td>
    <td>The following is displayed within the console:</br></br>
      "This process may take some time to complete.</br>
      You can't make any transactions during the process."</br></br>
      "Are you sure? (Y/n):"
    </td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>press "y"</td>
    <td>The following is displayed within the console:</br></br>
        Resetting wallet...</br>
        Scanning through the blockchain to find transactions that belong to you.</br>
        Please wait, this will take some time.</br></br>
        1 of 880171.
    </td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>Wait for the process to complete, the confirm</td>
    <td>Your wallet should be available for use.</td>
    <td></td>
  </tr>
  <tr>
    <td>Generating an Integrated Addresse work</td>
    <td></td>
    <td></td>
    <td><li>zedWallet is already started by using local daemon or public node.</li></td>
  </tr>
  <tr>
    <td>Transfering to an Integrated Addresse</td>
    <td></td>
    <td></td>
    <td><li>zedWallet is already started by using local daemon or public node.</li></td>
  </tr>
  <tr>
    <td>Scan From Height Works Correctly</td>
    <td>within the zedwallet console, type "reset"</td>
    <td>The following is displayed within the console:</br></br>
      "This process may take some time to complete.</br>
      You can't make any transactions during the process."</br></br>
      "Are you sure? (Y/n):"
      </td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>enter a desired scan-height, other than 0 then press "Enter".</td>
    <td>The following is displayed within the console:</br></br>
      "This process may take some time to complete.</br>
      You can't make any transactions during the process."</br></br>
      "Are you sure? (Y/n):"
      </td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>press "y".</td>
    <td>The following is displayed within the console:</br></br>
        Resetting wallet...</br>
        Scanning through the blockchain to find transactions that belong to you.</br>
        Please wait, this will take some time.</br></br>
        1 of 880171.
    </td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>Within the console, confirm:</td>
    <td><li>You can see it's doing jumps of 10k at once untill the selected scan height value is reached</li>
        <li>If you don't use scan height, it will instead do it in jumps of 100</li>
      </td>
    <td></td>
  </tr> 
</table>

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
         <em>"Enter your password"</em>
         </td>
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
    <td></td>
    <td></td>
    <td><li>zedWallet is already started by using local daemon or public node.</li></td>
  </tr>
  <tr>
    <td>Import from seeds correctly</td>
    <td></td>
    <td></td>
    <td><li>zedWallet is already started by using local daemon or public node.</li></td>
  </tr>
  <tr>
    <td>Perform a full reset</td>
    <td></td>
    <td></td>
    <td><li>zedWallet is already started by using local daemon or public node.</li></td>
  </tr>
  <tr>
    <td>Integrated Addresses work</td>
    <td></td>
    <td></td>
    <td><li>zedWallet is already started by using local daemon or public node.</li></td>
  </tr>
  <tr>
    <td>Scan From Height Works Correctly</td>
    <td></td>
    <td></td>
    <td><li>zedWallet is already started by using local daemon or public node.</li></td>
  </tr> 
</table>

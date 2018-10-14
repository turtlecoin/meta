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
    <td>Expected1</td>
    <ul>
    <td><li>The tested version of Turtlecoind is already running.</li></td>
    </ul>
  </tr>
  <tr>
    <td>Connect to a remote daemon</td>
    <td>Launch zedwallet using command : </br>zedwallet.exe --remote-daemon PublicNodeUrl:PublicNodePort.</td>
    <td>TurtleCoin VX.X.X zedwallet is displayed within the console with the below options:
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
    <td></td>
    <td>Within the zedWallet Console type "7", then press enter.</td>
    <td>"What address do you want to transfer to?:" is displayed within the console.</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>Type a valid Turtlecoin address that you have access to (I.E. : Your TipJar address in Discord.), then press enter.</td>
    <td>"How much TRTL do you want to send?": is displayed within the console.</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>Enter x amount of trtl to send, then press enter.</td>
    <td>What fee do you want to use?+Hit enter for the default fee of 0.10 TRTL: is displayed within the console</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>Enter a random fee,then press enter. Or press enter to use the default fee.</td>
    <td>NEED TO UPDATE</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>press "y"</td>
    <td>"Enter password" is displayed within the console</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>Enter your wallet password, then press enter.</td>
    <td>Transaction has been sent</br>Hash: 0e6800b4cf4055883652be48e1d27a1248ed2514d40ed40a567b3448d0227aaf</br>is displayed within the console</td>
    <td></td>
  </tr> 
</table>

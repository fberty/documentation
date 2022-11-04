---
---

# Firewall

### Port Forwarding and Firewall  {#port-forwarding-and-firewall}

These are ports that should be opened in firewall settings on your device, if running locally you are advised to port forward from your router settings. 


<table>
  <tr>
   <td><strong>Port Number</strong>
   </td>
   <td>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>30303
   </td>
   <td>Nethermind/GETH
   </td>
   <td>Execution Layer p2p Ports
   </td>
  </tr>
  <tr>
   <td>12000/udp
   </td>
   <td>Lighthouse/Prysm
   </td>
   <td>Consensus Layer p2p Ports
   </td>
  </tr>
  <tr>
   <td>13000/tcp
   </td>
   <td>Lighthouse/Prysm
   </td>
   <td>Consensus Layer p2p Ports
   </td>
  </tr>
  <tr>
   <td>8551
   </td>
   <td>Keep private do not expose
   </td>
   <td>EL-CL RPC
   </td>
  </tr>
  <tr>
   <td>9000
   </td>
   <td>Nimbus
   </td>
   <td>Consensus Layer p2p Ports
   </td>
  </tr>
  <tr>
   <td>8545
   </td>
   <td>Optional: for RPC services 
   </td>
   <td>EL RPC port
   </td>
  </tr>
</table>


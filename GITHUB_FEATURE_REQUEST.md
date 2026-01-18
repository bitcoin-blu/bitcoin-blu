# Feature Request: P2P Bbluinfo Exchange to Incentivize Public Full Nodes

## Summary

Add a new P2P protocol feature that allows Bitcoin-Blu nodes to exchange bblu addresses (e.g., donation addresses) through the network. This feature aims to incentivize operators to run public full nodes by providing a mechanism to share their bblu addresses with peers.

## Motivation

Running a public full node requires significant resources (bandwidth, storage, computational power) without direct financial compensation. By allowing nodes to exchange bblu addresses through the P2P protocol, node operators can share donation addresses or other contact information, potentially receiving contributions from users who benefit from their node's services.

## Proposed Changes

### 1. New P2P Messages

Add two new low-level Bitcoin P2P protocol messages:

- **`askbbluinfo`** (request message): A peer can request bbluinfo from another peer
- **`sendbbluinfo`** (response message): A peer responds with its configured bbluinfo string

**Message Specifications:**
- `askbbluinfo`: Empty message body (no payload required)
- `sendbbluinfo`: Contains a string (max 256 characters) representing a bblu address or other information

**Compatibility:**
- Both messages are optional and backward compatible
- Nodes that don't support these messages will simply ignore them
- If a node doesn't have `bbluinfo` configured, it responds with an empty string

### 2. New Configuration Option

Add a new configuration parameter to `bitcoinblu.conf`:

```
-bbluinfo=<str>
```

**Description:** String to respond with when peers request bbluinfo (e.g., donation address, max 256 chars)

**Example:**
```
bbluinfo=bb1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh
```

### 3. New RPC Commands

Add two new `bitcoinblu-cli` commands:

#### `askbbluinfo <address>`

Request bbluinfo from a specific peer identified by IP address and port.

**Parameters:**
- `address` (string, required): The IP address and port of the peer (e.g., "192.168.0.6:8343")

**Returns:**
```json
{
  "id": 1,
  "addr": "192.168.0.6:8343",
  "status": "Request sent. Use listbbluinfo to check for response."
}
```

**Behavior:**
- Sends the `askbbluinfo` P2P message to the specified peer
- Returns immediately (non-blocking)
- The response is stored asynchronously and can be retrieved using `listbbluinfo`

#### `listbbluinfo`

List all connected peers and their received bbluinfo strings.

**Parameters:** None

**Returns:**
```json
[
  {
    "id": 1,
    "addr": "192.168.0.6:8343",
    "bbluinfo": "bb1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh"
  },
  {
    "id": 2,
    "addr": "10.0.0.5:8343",
    "bbluinfo": ""
  }
]
```

**Behavior:**
- Returns a list of all connected peers
- `bbluinfo` field contains the received string (empty if no response has been received yet)
- Can be polled periodically to check for responses

## Implementation Details

### Message Handling

- When a node receives `askbbluinfo`, it immediately responds with `sendbbluinfo` containing the configured `bbluinfo` string (or empty string if not configured)
- When a node receives `sendbbluinfo`, it stores the bbluinfo string associated with that peer
- The stored bbluinfo is accessible via the `listbbluinfo` RPC command

### Thread Safety

- Uses dedicated mutex (`m_bbluinfo_mutex`) to protect bbluinfo storage, ensuring no deadlocks with message processing
- All operations are non-blocking to prevent RPC work queue overflow

### Logging

- Logs when `askbbluinfo` messages are sent
- Logs when `askbbluinfo` or `sendbbluinfo` messages are received
- Logs when bbluinfo is stored from a peer

## Use Cases

1. **Donation Address Sharing**: Node operators can configure their donation address and share it with peers
2. **Node Operator Contact**: Share contact information or other relevant details
3. **Network Analysis**: Track which nodes are sharing bbluinfo and analyze network participation

## Backward Compatibility

- ✅ Fully backward compatible
- ✅ Unknown messages are gracefully ignored
- ✅ Nodes without this feature continue to function normally
- ✅ No breaking changes to existing P2P protocol

## Security Considerations

- Maximum string length of 256 characters prevents abuse
- No authentication required (any peer can request bbluinfo)
- Information is public and can be shared with any connected peer
- Node operators should only share information they are comfortable making public

## Privacy Considerations

⚠️ **Important Privacy Note**: This feature has privacy implications that node operators should be aware of.

By sharing a bblu address through the P2P protocol, node operators are revealing information that can be used to:

1. **Link Node Identity to Wallet**: The bblu address shared via `bbluinfo` can be linked to the node's IP address and network identity. This creates a connection between the node operator's network presence and their wallet address.

2. **Transaction Graph Analysis**: If donations are sent to the shared address, blockchain analysis can reveal:
   - The total amount of donations received
   - Transaction patterns and timing
   - Potential connections to other addresses (if the operator reuses the address or consolidates funds)

3. **Network Surveillance**: Any peer connected to the node can request and receive the bbluinfo, meaning:
   - The information is not private to the connection
   - Adversaries can systematically collect bbluinfo from all nodes
   - This data can be correlated with other network metadata

4. **Reduced Anonymity**: Traditional Bitcoin-Blu nodes maintain a degree of anonymity - their IP addresses are known, but not their wallet addresses. This feature reduces that anonymity by explicitly linking the two.

**Recommendations for Node Operators:**

- Use a **dedicated donation address** that is separate from your main wallet
- Consider using a **new address for each donation period** to limit transaction graph analysis
- Be aware that sharing bbluinfo is **opt-in** - you can choose not to configure it if privacy is a concern
- Understand that once shared, the bblu address becomes part of the public network metadata

**Trade-off**: This feature prioritizes the ability to receive donations (incentivizing node operation) over complete privacy. Node operators must decide whether the potential for donations outweighs the privacy cost.

## Testing

- Unit tests for message serialization/deserialization
- Functional tests for RPC commands
- Integration tests for P2P message exchange
- Backward compatibility tests with nodes that don't support the feature

## Example Workflow

1. Node operator configures `bbluinfo=bb1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh` in `bitcoinblu.conf`
2. User connects to the node and runs: `bitcoinblu-cli askbbluinfo "192.168.0.6:8343"`
3. User periodically checks: `bitcoinblu-cli listbbluinfo`
4. Once the response arrives, the bbluinfo string is displayed
5. User can now send donations or contact the node operator

## Related Issues

- None

## Additional Context

This feature follows the same pattern as other optional P2P messages in Bitcoin Core, ensuring consistency with the existing codebase architecture.


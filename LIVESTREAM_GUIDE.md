# Production-Ready Livestream Integration Guide

This guide covers the implementation of browser-based, low-latency livestreaming using Livepeer WebRTC (WHIP/WHEP) for Food SharePoint.

## WebRTC vs HLS Tradeoffs

| Feature | WebRTC (WHIP/WHEP) | HLS |
|---------|--------------------|-----|
| Latency | < 1 second | 10-30 seconds |
| Reliability | Sensitive to packet loss | Extremely robust |
| Fallback | Auto-falls back to HLS | N/A |

## Backend Configuration

### 1. Restricted Playback (JWT)
To prevent unauthorized viewing, we use JWT protected playback.

**Signing Key Setup:**
1. Generate an Ed25519 key pair via Livepeer Dashboard.
2. Add the private key and key ID to your `.env`:
   ```env
   LIVEPEER_SIGNING_KEY="-----BEGIN PRIVATE KEY-----\n..."
   LIVEPEER_SIGNING_KEY_ID="your-key-id"
   ```

### 2. Issuing Tokens
The backend provides an endpoint `GET /livestreams/:id/token` which:
- Verifies the user's session.
- Generates a JWT signed with the Ed25519 key.
- Includes the `playbackID` in the `sub` claim.

## Frontend Implementation

### 1. SDK Initialization
Wrap your app (or just the livestream section) with the `LivestreamProvider`:
```tsx
import { LivestreamProvider } from '@/features/livestream/components/livestream-provider'

function App() {
  return (
    <LivestreamProvider>
       <YourAppContent />
    </LivestreamProvider>
  )
}
```

### 2. Broadcasting (Vendor Side)
Use `LivestreamBroadcaster`. It automatically handles:
- **Media Capture**: Prompts for camera/mic.
- **WebRTC (WHIP)**: Connects directly to Livepeer ingest.
- **Connectivity Monitoring**: Shows "Live" vs "Error" states.

### 3. Playback (Viewer Side)
Use `LivestreamPlayer`. It features:
- **Dual-stack Playback**: Attempts WebRTC first; falls back to HLS if the network is unstable.
- **JWT Authorization**: Automatically fetches the playback token from the backend.

## Optimizing for Nigeria (High Latency/Mobile)

### STUN/TURN Configuration
While Livepeer handles most cases, you can provide custom ICE servers in `LivestreamProvider.tsx` if you encounter restrictive firewalls (common in some corporate networks in Nigeria).

### Connectivity Tips
- **Bitrate Adaption**: Livepeer automatically transcodes streams. Viewers on slow connections will get lower-resolution HLS versions if WebRTC fails.
- **Broadcaster Connection**: Encourage vendors to use stable Wi-Fi. If using mobile data (MTN/Airtel), WebRTC might drop. The `LivestreamBroadcaster` includes a retry button for these cases.

## Debugging

- **ICE Candidate Failures**: Usually caused by aggressive firewalls. Check the browser console for `iceConnectionState` errors.
- **Stream Not Starting**: Ensure the `streamKey` is valid and the backend has set the `playbackPolicy` to `jwt` if tokens are being used.
- **Dropped Frames**: Often a CPU issue on mobile devices or low upload bandwidth. Livepeer's SDK handles some of this via adaptive bitrate on ingest.

## Security Checklist
- [x] Use HTTPS (Required for `getUserMedia`).
- [x] Enable JWT Playback Policy on all production streams.
- [x] Validate User Roles (Broadcaster vs Viewer) in backend handlers.

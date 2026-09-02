# WebSocket ZIP download protocol

Endpoint: **`WS /api/files/download-zip`**  
Server: `RouteApiZip` · Client: `client_HttpFS` (ZIP via WebSocket in ops layer).

Requires read permission. Needs roughly **≥ 50 MB** free space or the socket gets an `Error#21` text frame and exits.

## Client → server

After `onopen`, send one **text** frame (JSON):

```json
{ "names": ["photo.jpg", "Album"], "path": "storage/DCIM" }
```

| Field | Meaning |
|-------|---------|
| `path` | Parent folder logical path (files mount, URL-decoded on server) |
| `names` | Basenames under that folder to include (files and/or directories) |

Kotlin type: `PathFilesRequest(names, path)`.

## Server → client

Mixed frames:

| Kind | Payload | Meaning |
|------|---------|---------|
| Text JSON `progress` | Human string | UI status (`Preparation`, `Creating file '…'`, `Zip… NN%`, `Send: NN%`) |
| Text JSON `error` | Message | Fail; client shows dialog and closes |
| **Binary** | ZIP byte chunks | Archive body streamed while building completes |
| Text JSON final | `filename`, `totalSize` | Suggested download name (temp suffix stripped) and size |

Temp file on device: `{name}.HttpFStemp` next to the folder (or SAF createDocument); deleted after send or cancel.

Cancel: client closes the socket → server deletes temp and aborts.

## Notes

- Single item → zip named `{thatName}.zip`; multiple → `HttpFs-files-{n}.zip`.
- Progress percents may cap at **99** until the final metadata frame.
- Binary frames are raw chunks; the browser accumulates `Blob`s then downloads.
- Prefer reading `RouteApiZip.kt` + client transfer/ops code if the framing changes.

See also [api-routes.md](api-routes.md).

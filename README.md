# jupyterlab_webrtc_docprovider

[![Github Actions Status](https://github.com/jupyterlite/jupyterlab-webrtc-docprovider/workflows/Build/badge.svg)](https://github.com/jupyterlite/jupyterlab-webrtc-docprovider/actions/workflows/build.yml)[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/jupyterlite/jupyterlab-webrtc-docprovider/main?urlpath=lab)

> Document collaboration for JupyterLab and JupyterLite, powered by [y-webrtc].

## Requirements

- JupyterLab >=3.1

## How to Use It

- [Install](#Install) the package
- [Configure](#Configuration) your server for collaboration
- Launch a [Lumino]-based Jupyter client that supports collaboration
  - e.g. JupyterLab 3.1+, RetroLab 0.3+, or JupyterLite (beta)
- Open the client with the `room` URL parameters
  - e.g. `http://localhost:8888/lab?room=demo`
  - optionally provide `username` and `usercolor`
    - e.g. `http://localhost:8888/lab?room=demo&username=jo&usercolor=e65100`
  - these parameters will probably be consumed, but _that's okay_
- Open a shared editing activity like a _Notebook_ or _Editor_

## Install

To install the extension, execute:

```bash
pip install jupyterlab_webrtc_docprovider
```

> For a development install, see the [contributing guide].

## How it Works

Unlike JupyterLab's built-in, purely WebSocket-based [collaborative] document provider,
`jupyterlab_webrtc_docprovider` relies on:

- an initialing [signaling server] to locate peers
- the [WebRTC] protocol to coordinate actual data exchange

## Server Configuration

The `collaborative` high level flag `jupyter_server_config.json` must be enabled like
this:

```json
{
  "LabServerApp": {
    "collaborative": true
  }
}
```

### `collaborative`

This flag must be enabled for the provider to be used.

## Client Configuration

User-configurable settings can be pre-populated in
`{sys.prefix}/share/jupyter/lab/settings/overrides.json`: `roomPrefix` and
`signalingUrls` are security-related.

```json
{
  "@jupyterlite/webrtc-docprovider:plugin": {
    "disabled": false,
    "room": "an pre-shared room name",
    "roomPrefix": "a-very-unique-name",
    "signalingUrls": [
      "wss://y-webrtc-signaling-eu.herokuapp.com",
      "wss://y-webrtc-signaling-us.herokuapp.com",
      "wss://signaling.yjs.dev"
    ],
    "usercolor": "f57c00",
    "username": "Jo V. Un"
  }
}
```

### `roomPrefix`

By default, the final room ID that is actually sent to the signaling server will be the
SHA256 hash of the configured room prefix and the chosen room name.

By default this prefix is the full deployment URL, but for common URLs (like
`localhost`) a more random prefix should be chosen.

### `signalingUrls`

By default, a number of public signaling servers are provided, as described by
[y-webrtc], as shown above.

> **Note** While the content of your exchange is strictly peer-to-peer, and the above
> are suitable for demos like binder.
>
> A real production deployment should **not** rely on free hosted services at runtime.
> Some research would be required to find an appropriate server for your specific
> deployment.

## Uninstall

To remove the extension, run:

```bash
pip uninstall jupyterlab_webrtc_docprovider
```

[webrtc]:
  https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Signaling_and_video_calling
[signaling server]:
  https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Signaling_and_video_calling#the_signaling_server
[y-webrtc]: https://github.com/yjs/y-webrtc
[collaborative]: https://jupyterlab.readthedocs.io/en/stable/user/rtc.html
[lumino]: https://github.com/jupyterlab/lumino
[contributing guide]:
  https://github.com/jupyterlite/jupyterlab-webrtc-docprovider/blob/main/CONTRIBUTING.md

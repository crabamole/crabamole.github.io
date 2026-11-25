# Same-user access

A user who is signed in on a device connects to that device without its password. Other
users, and devices where nobody is signed in, still need the password or an Accept click.

## How it works

- **The device owner** is the user who signed in to cRustDesk on the device through your
  identity provider. The device signs that login with its own key, so the server knows the
  login came from that device.
- **A viewer** signed in as the same user connects with no password and no Accept dialog.
  Two-factor authentication (if set), locked settings and the session notice still apply.
- **The audit log** shows "Same user" for these connections.

The owner is the cRustDesk sign-in, not the operating system user. On a shared machine the
owner stays the last user who signed in to cRustDesk, whoever is at the desktop.

## Turn it on

It is off by default. Set this in your Helm values and upgrade the chart:

```yaml
apiserver:
  env:
    SAME_USER_ACCESS: "Y"
```

Devices must reach the server over `wss://` with a valid certificate. On any other link the
device ignores same-user access and asks for the password.

Versions: chart 0.12.0 or later, and cRustDesk 1.5.0-2 or later on the devices.

## Check it

1. Sign in as the same user on a viewer and on a device.
2. Connect from the viewer. No password is asked.
3. In the console, **Audit → Connections** shows "Same user" for the connection.

## When the password is asked again

- The owner signs out of cRustDesk on the device, or the sign-in expires (30 days).
- Another user signs in to cRustDesk on the device; that user becomes the owner.
- The user is disabled.
- The device is reinstalled with a new key. The owner signs in again on the device.
- The switch is off, or the device's link is not `wss://` with a valid certificate.

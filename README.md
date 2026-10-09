# 🧹 Device Cleanup Advisor

> A **read-only** Windows cleanup dashboard: it scans your own installed apps and user folders and tells you what is safe to review, what needs care, and what to uninstall by hand. It never deletes anything itself.

**Status:** Private repository. Source code available on request (interviews, review, collaboration).

No screenshots here on purpose: a real scan lists the scanning PC's own installed apps and file names.

---

## What it does

1. **Runs on your own PC.** Start it, open `http://127.0.0.1:4173`, confirm, and it scans that machine only. Results never leave it.
2. **Finds candidates.** Installed apps (with sizes from the Windows uninstall registry) and large or old items in Downloads, Desktop, Documents, Videos, Pictures and Temp.
3. **Labels every item by risk.** *Review OK*, *review carefully* or *uninstall manually only*, so the advice is clear about what is safe.
4. **Leaves the deleting to you.** There is no delete or uninstall endpoint at all.

---

## Tech stack

- **Node.js** server with no web framework (the built-in `http` module)
- Plain **HTML, CSS and JavaScript** dashboard
- **Tauri 2** (Rust) project for packaging it as a Windows desktop app with an installer (NSIS / MSI)

---

## Key engineering details

Safety was designed in from the start, because a cleanup tool that runs with your user rights is exactly what you must not get wrong:

- **Read-only by construction.** The API only has `GET` routes; there is nothing that changes the system.
- **Localhost by default.** The server binds to `127.0.0.1`, so other devices on the same Wi-Fi cannot reach it. Network access is opt-in and then requires a bearer token.
- **Host check.** The API only answers when it is addressed as `127.0.0.1` or `localhost`, so a web page that points its own domain at your PC (DNS rebinding) cannot read your scan.
- **Scan guards.** A confirmation before scanning, a single scan at a time, and a 30-second cooldown between scans.
- **Scoped and careful.** Only a fixed list of user folders is scanned; `.ssh`, credential file types and system-marked files are skipped.
- **Locked-down responses.** A strict Content Security Policy, no framing, `nosniff` and a same-origin resource policy on every response.

---

## Interested in the code?

The repository is private. If you would like to see the source, feel free to reach out.

# Preview Run Doc

## How to reproduce artifacts

No build artifacts needed. This is a static project (SVG banners + README.md). The preview HTML page (`preview.html`) lives in `.freebuff/` and references the SVGs from the project root.

## How to run the server

1. Kill any existing server on port 8080:
   ```bash
   lsof -ti:8080 | xargs kill 2>/dev/null
   ```

2. Start Python HTTP server from the project root using launchd:
   ```bash
   launchctl submit -l com.freebuff.preview -- /bin/sh -c "cd <project_root> && exec python3 -m http.server 8080 > .freebuff/preview.log 2>&1"
   ```

3. Verify it's running:
   ```bash
   launchctl print gui/$(id -u)/com.freebuff.preview | grep pid
   curl -s -o /dev/null -w "%{http_code}" http://127.0.0.1:8080/.freebuff/preview.html
   ```

4. The preview URL is: `http://127.0.0.1:8080/.freebuff/preview.html`

## Cleanup

```bash
launchctl remove com.freebuff.preview
```

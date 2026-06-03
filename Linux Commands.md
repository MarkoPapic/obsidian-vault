
**Generate UUID**
```
uuidgen | xclip -selection clipboard
```
without hyphens:
```
uuidgen | tr -d '-' | xclip -selection clipboard
```

**Check what is listening on port**
```
lsof -i :port
```

**Kill a process by PID**
```
kill -9 PID
```


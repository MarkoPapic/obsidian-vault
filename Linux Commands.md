
**Generate UUID**
```
uuidgen | xclip -selection clipboard
```
without hyphens:
```
uuidgen | tr -d '-' | xclip -selection clipboard
```

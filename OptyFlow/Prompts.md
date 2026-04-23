Let's allow users to add books to the library. The "Add" button - `Icon(Icons.Filled.Add, "Add a book.")` - in `app/src/main/java/com/markopapic/tvreader/ui/screens/library/LibraryScreen.kt` should do the following:
- Open a popup overlay that shows the following information to the user:
	- Private IP address of the TV
	- Instructions to open www.markopapic.com/tv-reader/upload and upload a book. You can use a dummy placeholder text for now.
- Start a lightweight HTTP listener/server that listens to the following HTTP requests:
	- `GET /ping`: Replies with `200`.
	- `POST /upload`: Accepts an `.epub` file, decompresses it, and writes the contents into the `BOOKS_DIRECTORY` via `app/src/main/java/com/markopapic/tvreader/data/FileBookRepository.kt`. The directory name for the book should be generated, e.g. a UUID.
- Once the popup overlay is closed, the HTTP listener should stop. The app should not run an always-on HTTP server.


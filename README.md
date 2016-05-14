# notifeed

**NOTE:** WIP!

notifeed lets you use an Atom feed for low-priority notifications. It provides
a simple HTTP API to send your notifications to. It persists them with
[BoltDB](https://github.com/boltdb/bolt) and serves them as Atom feeds using
[gorilla/feeds](http://www.gorillatoolkit.org/pkg/feeds).

## Building

- Clone the repository in your `$GOPATH/src/github.com/Shark/notifeed`
- Run `go get ./...`
- Run `make`

## Testing

Run `make test`.

## Usage

Customize `config.json`:
```
{
  "DatabasePath": "/my/path/notifeed.db",
  "CertificatePath": "/my/path/notifeed.crt",
  "KeyPath": "/my/path/notifeed.key",
  "BindAddr": "0.0.0.0",
  "BindPort": "6543"
  "Feeds": [
    {
      "UUID": "0149CED3-564B-4DA9-8DE1-4894B7DFFDB8",
      "Title": "Feed Title",
      "Link": "http://some-url",
      "Description": "Feed Description",
      "Author": "Feed Author",
      "Created": "RFC3334-Timestamp",
      "Copyright": "Copyright notice."
      "ClientCACertificates": [
        "...",
        "..."
      ]
    }
  ]
}
```

You can then issue a `POST` to `/feeds/[UUID]` and include your
notification in the request body as JSON:

```
{
  "Title": "Notification Title",
  "Link": "Notification Link",
  "Description": "Notification Description",
  "Author": "Notification Author",
  "Created": "RFC3334-Timestamp"
}
```

The response code will be `201 Created` on success. On failure it is `400 Bad Request` if the body is not parseable or `422 Unprocessable Entity` if any fields have invalid values.

You can retrieve the feed with a `GET` on `/feeds/[UUID]`. The response code will be `200 OK`. If a feed with the provided UUID does not exist, the response code will be `404 Not Found`.

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request! :)

## History

- v0.0.1 (2016-05-14): initial version

## License

This project is licensed under the MIT License. See LICENSE for details.

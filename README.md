<h1 align="center">
  mimetype
</h1>

<h4 align="center">
  A package for detecting MIME types and extensions based on magic numbers
</h4>
<h6 align="center">
  No bindings, thread safe and completely written in go
</h6>

<p align="center">
  <a href="https://travis-ci.org/gabriel-vasile/mimetype">
    <img alt="Build Status" src="https://travis-ci.org/gabriel-vasile/mimetype.svg?branch=master">
  </a>
  <a href="https://godoc.org/github.com/gabriel-vasile/mimetype">
    <img alt="Documentation" src="https://godoc.org/github.com/gabriel-vasile/mimetype?status.svg">
  </a>
  <a href="https://goreportcard.com/report/github.com/gabriel-vasile/mimetype">
    <img alt="Go report card" src="https://goreportcard.com/badge/github.com/gabriel-vasile/mimetype">
  </a>
  <a href="https://coveralls.io/github/gabriel-vasile/mimetype?branch=master">
    <img alt="Go report card" src="https://coveralls.io/repos/github/gabriel-vasile/mimetype/badge.svg?branch=master">
  </a>
  <a href="LICENSE">
    <img alt="License" src="https://img.shields.io/badge/License-MIT-green.svg">
  </a>
</p>

## Install
```bash
go get github.com/gabriel-vasile/mimetype
```

## Use
See [GoDoc](https://godoc.org/github.com/gabriel-vasile/mimetype) for full reference.
The library exposes three functions you can use in order to find a file type.
```go
func Detect(in []byte) (mime, extension string){}
func DetectReader(r io.Reader) (mime, extension string, err error){}
func DetectFile(file string) (mime, extension string, err error){}
```

If you need to check input against a certain list of MIME types, use `Matches`:
```go
func Matches(in []byte, expectedMimes ...string) (match bool, err error){}
func MatchesReader(r io.Reader, expectedMimes ...string) (match bool, err error){}
func MatchesFile(file string, expectedMimes ...string) (match bool, err error){}
```
Unlike `Detect`, which returns a single MIME type, `Matches` searches against all
the aliases of the MIME type detected from the input. For example, provided `in` is a
zip archive, both `Matches(in, "application/zip")` and `Matches(in, "application/x-zip-compressed")`
will return a positive result.

When detecting from a `ReadSeeker` interface, such as `os.File`, make sure
to reset the offset of the reader to the beginning if needed:
```go
_, err = file.Seek(0, io.SeekStart)
```

## Supported MIME types
See [supported mimes](supported_mimes.md) for the list of detected MIME types.
If support is needed for a specific file format, please open an [issue](https://github.com/gabriel-vasile/mimetype/issues/new/choose).

## Structure
**mimetype** uses an hierarchical structure to keep the MIME type detection logic.
This reduces the number of calls needed for detecting the file type. The reason
behind this choice is that there are file formats used as containers for other
file formats. For example, Microsoft office files are just zip archives,
containing specific metadata files. Once a file a file has been identified as a
zip, there is no need to check if it is a text file.
<div align="center">
  <img alt="structure" src="mimetype.gif" width="88%">
</div>

## Contributing
See [CONTRIBUTING.md](CONTRIBUTING.md).

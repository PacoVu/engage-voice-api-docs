<<<<<<< Updated upstream
# RingCentral Engage Voice API Docs

[![Docs Status][docs-status-svg]][docs-status-link]
[![Specs Status][specs-status-svg]][specs-status-link]

This repository is the home of the RingCentral Engage Developer Guide: a collection of materials, and documentation to help educate developers on how to build on top of the RingCentral Engage platform.

Visit at: https://engage-voice-api-docs.rtfd.org

## Contributing

If you would like to contribute to the RingCentral documentation effort, fork this repository, make your desired edits and contributions, and issue a pull request accordingly.

### Running ReadTheDocs Locally

```
% git clone https://github.com/ringcentral/ringcentral-voice-api-docs.git
% cd ringcentral-api-docs
% pip install mkdocs
% pip install mkdocs-moonstone
% mkdocs serve
```

Then you should be able to load http://localhost:8000 to view the documentation.

### Testing OpenAPI Specs

This repo holds OpenAPI specs for Engage Voice. For each commit, tests are run on Travis CI to verify that the OpenAPI 3.0 specs validate.

* Engage Voice Spec: [specs/voice/engage-voice_openapi3.json](specs/voice/engage-voice_openapi3.json)
* Tests: [specs_test.go](specs_test.go)

You can verify the specs localy with the following if you have [Go installed](https://golang.org/).

```
% git clone https://github.com/ringcentral/ringcentral-voice-api-docs.git
% cd ringcentral-api-docs
% go test -v ./...
```

If you wish to change the specs being tested edit the [specs_test.go](specs_test.go) file.

 [docs-status-svg]: https://readthedocs.org/projects/engage-voice-api-docs/badge/?version=latest
 [docs-status-link]: https://engage-voice-api-docs.readthedocs.io/

 [specs-status-svg]: https://travis-ci.com/ringcentral/engage-voice-api-docs.svg?branch=master
 [specs-status-link]: https://travis-ci.com/ringcentral/engage-voice-api-docs
=======
# Engage API Docs

Visit at: https://engage-api-docs.rtfd.org

Copyright &copy; 2019 RingCentral, Inc.
>>>>>>> Stashed changes

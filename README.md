# api documentation for  [multiparty (v4.1.3)](https://github.com/pillarjs/multiparty#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-multiparty.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-multiparty) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-multiparty.svg)](https://travis-ci.org/npmdoc/node-npmdoc-multiparty)
#### multipart/form-data parser which supports streaming

[![NPM](https://nodei.co/npm/multiparty.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/multiparty)

[![apidoc](https://npmdoc.github.io/node-npmdoc-multiparty/build/screenCapture.buildCi.browser.apidoc.html.png)](https://npmdoc.github.io/node-npmdoc-multiparty/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-multiparty/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-multiparty/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Andrew Kelley"
    },
    "bugs": {
        "url": "https://github.com/pillarjs/multiparty/issues"
    },
    "dependencies": {
        "fd-slicer": "~1.0.1"
    },
    "description": "multipart/form-data parser which supports streaming",
    "devDependencies": {
        "findit2": "~2.2.3",
        "istanbul": "~0.4.3",
        "mkdirp": "~0.5.1",
        "pend": "~1.2.0",
        "rimraf": "~2.5.2",
        "superagent": "~0.21.0"
    },
    "directories": {
        "example": "examples",
        "test": "test"
    },
    "dist": {
        "shasum": "3c43c7fcb1896e17460436a9dd0b6ef1668e4f94",
        "tarball": "https://registry.npmjs.org/multiparty/-/multiparty-4.1.3.tgz"
    },
    "engines": {
        "node": ">=0.10.0"
    },
    "gitHead": "dcbd3d6390535e09fd09f6d26c4928942df31bb1",
    "homepage": "https://github.com/pillarjs/multiparty#readme",
    "keywords": [
        "file",
        "upload",
        "formidable",
        "stream",
        "s3"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "superjoe"
        },
        {
            "name": "dougwilson"
        }
    ],
    "name": "multiparty",
    "optionalDependencies": {},
    "repository": {
        "type": "git",
        "url": "git+ssh://git@github.com/pillarjs/multiparty.git"
    },
    "scripts": {
        "test": "node test/test.js",
        "test-cov": "istanbul cover test/test.js",
        "test-travis": "istanbul cover test/test.js --report lcovonly"
    },
    "version": "4.1.3"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module multiparty](#apidoc.module.multiparty)
1.  [function <span class="apidocSignatureSpan">multiparty.</span>Form (options)](#apidoc.element.multiparty.Form)
1.  object <span class="apidocSignatureSpan">multiparty.</span>Form.prototype

#### [module multiparty.Form](#apidoc.module.multiparty.Form)
1.  [function <span class="apidocSignatureSpan">multiparty.</span>Form (options)](#apidoc.element.multiparty.Form.Form)
1.  [function <span class="apidocSignatureSpan">multiparty.Form.</span>super_ (options)](#apidoc.element.multiparty.Form.super_)

#### [module multiparty.Form.prototype](#apidoc.module.multiparty.Form.prototype)
1.  [function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>_write (buffer, encoding, cb)](#apidoc.element.multiparty.Form.prototype._write)
1.  [function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>onParseHeaderEnd ()](#apidoc.element.multiparty.Form.prototype.onParseHeaderEnd)
1.  [function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>onParseHeaderField (b)](#apidoc.element.multiparty.Form.prototype.onParseHeaderField)
1.  [function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>onParseHeaderValue (b)](#apidoc.element.multiparty.Form.prototype.onParseHeaderValue)
1.  [function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>onParseHeadersEnd (offset)](#apidoc.element.multiparty.Form.prototype.onParseHeadersEnd)
1.  [function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>onParsePartBegin ()](#apidoc.element.multiparty.Form.prototype.onParsePartBegin)
1.  [function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>onParsePartData (b)](#apidoc.element.multiparty.Form.prototype.onParsePartData)
1.  [function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>onParsePartEnd ()](#apidoc.element.multiparty.Form.prototype.onParsePartEnd)
1.  [function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>parse (req, cb)](#apidoc.element.multiparty.Form.prototype.parse)



# <a name="apidoc.module.multiparty"></a>[module multiparty](#apidoc.module.multiparty)

#### <a name="apidoc.element.multiparty.Form"></a>[function <span class="apidocSignatureSpan">multiparty.</span>Form (options)](#apidoc.element.multiparty.Form)
- description and source-code
```javascript
function Form(options) {
  var self = this;
  stream.Writable.call(self);

  options = options || {};

  self.error = null;

  self.autoFields = !!options.autoFields;
  self.autoFiles = !!options.autoFiles;

  self.maxFields = options.maxFields || 1000;
  self.maxFieldsSize = options.maxFieldsSize || 2 * 1024 * 1024;
  self.maxFilesSize = options.maxFilesSize || Infinity;
  self.uploadDir = options.uploadDir || os.tmpdir();
  self.encoding = options.encoding || 'utf8';

  self.bytesReceived = 0;
  self.bytesExpected = null;

  self.openedFiles = [];
  self.totalFieldSize = 0;
  self.totalFieldCount = 0;
  self.totalFileSize = 0;
  self.flushing = 0;

  self.backpressure = false;
  self.writeCbs = [];

  self.emitQueue = [];

  self.on('newListener', function(eventName) {
    if (eventName === 'file') {
      self.autoFiles = true;
    } else if (eventName === 'field') {
      self.autoFields = true;
    }
  });
}
```
- example usage
```shell
...
var multiparty = require('multiparty');
var http = require('http');
var util = require('util');

http.createServer(function(req, res) {
  if (req.url === '/upload' && req.method === 'POST') {
// parse a file upload
var form = new multiparty.Form();

form.parse(req, function(err, fields, files) {
  res.writeHead(200, {'content-type': 'text/plain'});
  res.write('received upload:\n\n');
  res.end(util.inspect({fields: fields, files: files}));
});
...
```



# <a name="apidoc.module.multiparty.Form"></a>[module multiparty.Form](#apidoc.module.multiparty.Form)

#### <a name="apidoc.element.multiparty.Form.Form"></a>[function <span class="apidocSignatureSpan">multiparty.</span>Form (options)](#apidoc.element.multiparty.Form.Form)
- description and source-code
```javascript
function Form(options) {
  var self = this;
  stream.Writable.call(self);

  options = options || {};

  self.error = null;

  self.autoFields = !!options.autoFields;
  self.autoFiles = !!options.autoFiles;

  self.maxFields = options.maxFields || 1000;
  self.maxFieldsSize = options.maxFieldsSize || 2 * 1024 * 1024;
  self.maxFilesSize = options.maxFilesSize || Infinity;
  self.uploadDir = options.uploadDir || os.tmpdir();
  self.encoding = options.encoding || 'utf8';

  self.bytesReceived = 0;
  self.bytesExpected = null;

  self.openedFiles = [];
  self.totalFieldSize = 0;
  self.totalFieldCount = 0;
  self.totalFileSize = 0;
  self.flushing = 0;

  self.backpressure = false;
  self.writeCbs = [];

  self.emitQueue = [];

  self.on('newListener', function(eventName) {
    if (eventName === 'file') {
      self.autoFiles = true;
    } else if (eventName === 'field') {
      self.autoFields = true;
    }
  });
}
```
- example usage
```shell
...
var multiparty = require('multiparty');
var http = require('http');
var util = require('util');

http.createServer(function(req, res) {
  if (req.url === '/upload' && req.method === 'POST') {
// parse a file upload
var form = new multiparty.Form();

form.parse(req, function(err, fields, files) {
  res.writeHead(200, {'content-type': 'text/plain'});
  res.write('received upload:\n\n');
  res.end(util.inspect({fields: fields, files: files}));
});
...
```

#### <a name="apidoc.element.multiparty.Form.super_"></a>[function <span class="apidocSignatureSpan">multiparty.Form.</span>super_ (options)](#apidoc.element.multiparty.Form.super_)
- description and source-code
```javascript
function Writable(options) {
  // Writable ctor is applied to Duplexes, too.
  // 'realHasInstance' is necessary because using plain 'instanceof'
  // would return false, as no '_writableState' property is attached.

  // Trying to use the custom 'instanceof' for Writable here will also break the
  // Node.js LazyTransform implementation, which has a non-trivial getter for
  // '_writableState' that would lead to infinite recursion.
  if (!(realHasInstance.call(Writable, this)) &&
      !(this instanceof Stream.Duplex)) {
    return new Writable(options);
  }

  this._writableState = new WritableState(options, this);

  // legacy.
  this.writable = true;

  if (options) {
    if (typeof options.write === 'function')
      this._write = options.write;

    if (typeof options.writev === 'function')
      this._writev = options.writev;
  }

  Stream.call(this);
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.multiparty.Form.prototype"></a>[module multiparty.Form.prototype](#apidoc.module.multiparty.Form.prototype)

#### <a name="apidoc.element.multiparty.Form.prototype._write"></a>[function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>_write (buffer, encoding, cb)](#apidoc.element.multiparty.Form.prototype._write)
- description and source-code
```javascript
_write = function (buffer, encoding, cb) {
  if (this.error) return;

  var self = this;
  var i = 0;
  var len = buffer.length;
  var prevIndex = self.index;
  var index = self.index;
  var state = self.state;
  var lookbehind = self.lookbehind;
  var boundary = self.boundary;
  var boundaryChars = self.boundaryChars;
  var boundaryLength = self.boundary.length;
  var boundaryEnd = boundaryLength - 1;
  var bufferLength = buffer.length;
  var c;
  var cl;

  for (i = 0; i < len; i++) {
    c = buffer[i];
    switch (state) {
      case START:
        index = 0;
        state = START_BOUNDARY;
<span class="apidocCodeCommentSpan">        /* falls through */
</span>      case START_BOUNDARY:
        if (index === boundaryLength - 2 && c === HYPHEN) {
          index = 1;
          state = CLOSE_BOUNDARY;
          break;
        } else if (index === boundaryLength - 2) {
          if (c !== CR) return self.handleError(createError(400, 'Expected CR Received ' + c));
          index++;
          break;
        } else if (index === boundaryLength - 1) {
          if (c !== LF) return self.handleError(createError(400, 'Expected LF Received ' + c));
          index = 0;
          self.onParsePartBegin();
          state = HEADER_FIELD_START;
          break;
        }

        if (c !== boundary[index+2]) index = -2;
        if (c === boundary[index+2]) index++;
        break;
      case HEADER_FIELD_START:
        state = HEADER_FIELD;
        self.headerFieldMark = i;
        index = 0;
        /* falls through */
      case HEADER_FIELD:
        if (c === CR) {
          self.headerFieldMark = null;
          state = HEADERS_ALMOST_DONE;
          break;
        }

        index++;
        if (c === HYPHEN) break;

        if (c === COLON) {
          if (index === 1) {
            // empty header field
            self.handleError(createError(400, 'Empty header field'));
            return;
          }
          self.onParseHeaderField(buffer.slice(self.headerFieldMark, i));
          self.headerFieldMark = null;
          state = HEADER_VALUE_START;
          break;
        }

        cl = lower(c);
        if (cl < A || cl > Z) {
          self.handleError(createError(400, 'Expected alphabetic character, received ' + c));
          return;
        }
        break;
      case HEADER_VALUE_START:
        if (c === SPACE) break;

        self.headerValueMark = i;
        state = HEADER_VALUE;
        /* falls through */
      case HEADER_VALUE:
        if (c === CR) {
          self.onParseHeaderValue(buffer.slice(self.headerValueMark, i));
          self.headerValueMark = null;
          self.onParseHeaderEnd();
          state = HEADER_VALUE_ALMOST_DONE;
        }
        break;
      case HEADER_VALUE_ALMOST_DONE:
        if (c !== LF) return self.handleError(createError(400, 'Expected LF Received ' + c));
        state = HEADER_FIELD_START;
        break;
      case HEADERS_ALMOST_DONE:
        if (c !== LF) return self.handleError(createError(400, 'Expected LF Received ' + c));
        var err = self.onParseHeadersEnd(i + 1);
        if (err) return self.handleError(err);
        state = PART_DATA_START;
        break;
      case PART_DATA_START:
        state = PART_DATA;
        self.partDataMark = i;
        /* falls through */
      case PART_DATA:
        prevIndex = index;

        if (index === 0) {
          // boyer-moore derrived algorithm to safely skip non-boundary data
          i += boundaryEnd;
          while (i < bufferLength && !(buffer[i] in boundaryChars)) {
            i += boundaryLength;
          }
          i -= boundaryEnd;
          c = buffer[i];
        }

        if (index < boundaryLength) {
          if (boundary[index] === c) {
            if (index === 0) {
              self.onParsePartData(buffer.slice(self.partDataMark, i));
              self.partDataMark = null;
            }
            index++;
          } else {
            index = 0;
          }
        } else if (index === boundaryLength) {
          index++;
          if (c === CR) {
            // CR = part boundary
            self.partBoundaryFlag = true;
          } else i ...
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.multiparty.Form.prototype.onParseHeaderEnd"></a>[function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>onParseHeaderEnd ()](#apidoc.element.multiparty.Form.prototype.onParseHeaderEnd)
- description and source-code
```javascript
onParseHeaderEnd = function () {
  this.headerField = this.headerField.toLowerCase();
  this.partHeaders[this.headerField] = this.headerValue;

  var m;
  if (this.headerField === 'content-disposition') {
    if (m = this.headerValue.match(/\bname="([^"]+)"/i)) {
      this.partName = m[1];
    }
    this.partFilename = parseFilename(this.headerValue);
  } else if (this.headerField === 'content-transfer-encoding') {
    this.partTransferEncoding = this.headerValue.toLowerCase();
  }

  this.headerFieldDecoder = new StringDecoder(this.encoding);
  this.headerField = '';
  this.headerValueDecoder = new StringDecoder(this.encoding);
  this.headerValue = '';
}
```
- example usage
```shell
...
  self.headerValueMark = i;
  state = HEADER_VALUE;
  /* falls through */
case HEADER_VALUE:
  if (c === CR) {
    self.onParseHeaderValue(buffer.slice(self.headerValueMark, i));
    self.headerValueMark = null;
    self.onParseHeaderEnd();
    state = HEADER_VALUE_ALMOST_DONE;
  }
  break;
case HEADER_VALUE_ALMOST_DONE:
  if (c !== LF) return self.handleError(createError(400, 'Expected LF Received ' + c));
  state = HEADER_FIELD_START;
  break;
...
```

#### <a name="apidoc.element.multiparty.Form.prototype.onParseHeaderField"></a>[function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>onParseHeaderField (b)](#apidoc.element.multiparty.Form.prototype.onParseHeaderField)
- description and source-code
```javascript
onParseHeaderField = function (b) {
  this.headerField += this.headerFieldDecoder.write(b);
}
```
- example usage
```shell
...

if (c === COLON) {
  if (index === 1) {
    // empty header field
    self.handleError(createError(400, 'Empty header field'));
    return;
  }
  self.onParseHeaderField(buffer.slice(self.headerFieldMark, i));
  self.headerFieldMark = null;
  state = HEADER_VALUE_START;
  break;
}

cl = lower(c);
if (cl < A || cl > Z) {
...
```

#### <a name="apidoc.element.multiparty.Form.prototype.onParseHeaderValue"></a>[function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>onParseHeaderValue (b)](#apidoc.element.multiparty.Form.prototype.onParseHeaderValue)
- description and source-code
```javascript
onParseHeaderValue = function (b) {
  this.headerValue += this.headerValueDecoder.write(b);
}
```
- example usage
```shell
...
  if (c === SPACE) break;

  self.headerValueMark = i;
  state = HEADER_VALUE;
  /* falls through */
case HEADER_VALUE:
  if (c === CR) {
    self.onParseHeaderValue(buffer.slice(self.headerValueMark, i));
    self.headerValueMark = null;
    self.onParseHeaderEnd();
    state = HEADER_VALUE_ALMOST_DONE;
  }
  break;
case HEADER_VALUE_ALMOST_DONE:
  if (c !== LF) return self.handleError(createError(400, 'Expected LF Received ' + c));
...
```

#### <a name="apidoc.element.multiparty.Form.prototype.onParseHeadersEnd"></a>[function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>onParseHeadersEnd (offset)](#apidoc.element.multiparty.Form.prototype.onParseHeadersEnd)
- description and source-code
```javascript
onParseHeadersEnd = function (offset) {
  var self = this;
  switch(self.partTransferEncoding){
    case 'binary':
    case '7bit':
    case '8bit':
    self.partTransferEncoding = 'binary';
    break;

    case 'base64': break;
    default:
    return createError(400, 'unknown transfer-encoding: ' + self.partTransferEncoding);
  }

  self.totalFieldCount += 1;
  if (self.totalFieldCount > self.maxFields) {
    return createError(413, 'maxFields ' + self.maxFields + ' exceeded.');
  }

  self.destStream = new stream.PassThrough();
  self.destStream.on('drain', function() {
    flushWriteCbs(self);
  });
  self.destStream.headers = self.partHeaders;
  self.destStream.name = self.partName;
  self.destStream.filename = self.partFilename;
  self.destStream.byteOffset = self.bytesReceived + offset;
  var partContentLength = self.destStream.headers['content-length'];
  self.destStream.byteCount = partContentLength ? parseInt(partContentLength, 10) :
    self.bytesExpected ? (self.bytesExpected - self.destStream.byteOffset -
      self.boundary.length - LAST_BOUNDARY_SUFFIX_LEN) :
    undefined;

  if (self.destStream.filename == null && self.autoFields) {
    handleField(self, self.destStream);
  } else if (self.destStream.filename != null && self.autoFiles) {
    handleFile(self, self.destStream);
  } else {
    handlePart(self, self.destStream);
  }
}
```
- example usage
```shell
...
  break;
case HEADER_VALUE_ALMOST_DONE:
  if (c !== LF) return self.handleError(createError(400, 'Expected LF Received ' + c));
  state = HEADER_FIELD_START;
  break;
case HEADERS_ALMOST_DONE:
  if (c !== LF) return self.handleError(createError(400, 'Expected LF Received ' + c));
  var err = self.onParseHeadersEnd(i + 1);
  if (err) return self.handleError(err);
  state = PART_DATA_START;
  break;
case PART_DATA_START:
  state = PART_DATA;
  self.partDataMark = i;
  /* falls through */
...
```

#### <a name="apidoc.element.multiparty.Form.prototype.onParsePartBegin"></a>[function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>onParsePartBegin ()](#apidoc.element.multiparty.Form.prototype.onParsePartBegin)
- description and source-code
```javascript
onParsePartBegin = function () {
  clearPartVars(this);
}
```
- example usage
```shell
...
} else if (index === boundaryLength - 2) {
  if (c !== CR) return self.handleError(createError(400, 'Expected CR Received ' + c));
  index++;
  break;
} else if (index === boundaryLength - 1) {
  if (c !== LF) return self.handleError(createError(400, 'Expected LF Received ' + c));
  index = 0;
  self.onParsePartBegin();
  state = HEADER_FIELD_START;
  break;
}

if (c !== boundary[index+2]) index = -2;
if (c === boundary[index+2]) index++;
break;
...
```

#### <a name="apidoc.element.multiparty.Form.prototype.onParsePartData"></a>[function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>onParsePartData (b)](#apidoc.element.multiparty.Form.prototype.onParsePartData)
- description and source-code
```javascript
onParsePartData = function (b) {
  if (this.partTransferEncoding === 'base64') {
    this.backpressure = ! this.destStream.write(b.toString('ascii'), 'base64');
  } else {
    this.backpressure = ! this.destStream.write(b);
  }
}
```
- example usage
```shell
...
  i -= boundaryEnd;
  c = buffer[i];
}

if (index < boundaryLength) {
  if (boundary[index] === c) {
    if (index === 0) {
      self.onParsePartData(buffer.slice(self.partDataMark, i));
      self.partDataMark = null;
    }
    index++;
  } else {
    index = 0;
  }
} else if (index === boundaryLength) {
...
```

#### <a name="apidoc.element.multiparty.Form.prototype.onParsePartEnd"></a>[function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>onParsePartEnd ()](#apidoc.element.multiparty.Form.prototype.onParsePartEnd)
- description and source-code
```javascript
onParsePartEnd = function () {
  if (this.destStream) {
    flushWriteCbs(this);
    var s = this.destStream;
    process.nextTick(function() {
      s.end();
    });
  }
  clearPartVars(this);
}
```
- example usage
```shell
...
    index = 0;
  }
} else if (index - 1 === boundaryLength)  {
  if (self.partBoundaryFlag) {
    index = 0;
    if (c === LF) {
      self.partBoundaryFlag = false;
      self.onParsePartEnd();
      self.onParsePartBegin();
      state = HEADER_FIELD_START;
      break;
    }
  } else {
    index = 0;
  }
...
```

#### <a name="apidoc.element.multiparty.Form.prototype.parse"></a>[function <span class="apidocSignatureSpan">multiparty.Form.prototype.</span>parse (req, cb)](#apidoc.element.multiparty.Form.prototype.parse)
- description and source-code
```javascript
parse = function (req, cb) {
  var called = false;
  var self = this;
  var waitend = true;

  if (cb) {
    // if the user supplies a callback, this implies autoFields and autoFiles
    self.autoFields = true;
    self.autoFiles = true;

    // wait for request to end before calling cb
    var end = function (done) {
      if (called) return;

      called = true;

      // wait for req events to fire
      process.nextTick(function() {
        if (waitend && req.readable) {
          // dump rest of request
          req.resume();
          req.once('end', done);
          return;
        }

        done();
      });
    };

    var fields = {};
    var files = {};
    self.on('error', function(err) {
      end(function() {
        cb(err);
      });
    });
    self.on('field', function(name, value) {
      var fieldsArray = fields[name] || (fields[name] = []);
      fieldsArray.push(value);
    });
    self.on('file', function(name, file) {
      var filesArray = files[name] || (files[name] = []);
      filesArray.push(file);
    });
    self.on('close', function() {
      end(function() {
        cb(null, fields, files);
      });
    });
  }

  self.handleError = handleError;
  self.bytesExpected = getBytesExpected(req.headers);

  req.on('end', onReqEnd);
  req.on('error', function(err) {
    waitend = false;
    handleError(err);
  });
  req.on('aborted', onReqAborted);

  var state = req._readableState;
  if (req._decoder || (state && (state.encoding || state.decoder))) {
    // this is a binary protocol
    // if an encoding is set, input is likely corrupted
    validationError(new Error('request encoding must not be set'));
    return;
  }

  var contentType = req.headers['content-type'];
  if (!contentType) {
    validationError(createError(415, 'missing content-type header'));
    return;
  }

  var m = CONTENT_TYPE_RE.exec(contentType);
  if (!m) {
    validationError(createError(415, 'unsupported content-type'));
    return;
  }

  var boundary;
  CONTENT_TYPE_PARAM_RE.lastIndex = m.index + m[0].length - 1;
  while ((m = CONTENT_TYPE_PARAM_RE.exec(contentType))) {
    if (m[1].toLowerCase() !== 'boundary') continue;
    boundary = m[2] || m[3];
    break;
  }

  if (!boundary) {
    validationError(createError(400, 'content-type missing boundary'));
    return;
  }

  setUpParser(self, boundary);
  req.pipe(self);

  function onReqAborted() {
    waitend = false;
    self.emit('aborted');
    handleError(new Error("Request aborted"));
  }

  function onReqEnd() {
    waitend = false;
  }

  function handleError(err) {
    var first = !self.error;
    if (first) {
      self.error = err;
      req.removeListener('aborted', onReqAborted);
      req.removeListener('end', onReqEnd);
      if (self.destStream) {
        errorEventQueue(self, self.destStream, err);
      }
    }

    cleanupOpenFiles(self);

    if (first) {
      self.emit('error', err);
    }
  }

  function validationError(err) {
    // handle error on next tick for event listeners to attach
    process.nextTick(handleError.bind(null, err))
  }
}
```
- example usage
```shell
...
var util = require('util');

http.createServer(function(req, res) {
if (req.url === '/upload' && req.method === 'POST') {
  // parse a file upload
  var form = new multiparty.Form();

  form.parse(req, function(err, fields, files) {
    res.writeHead(200, {'content-type': 'text/plain'});
    res.write('received upload:\n\n');
    res.end(util.inspect({fields: fields, files: files}));
  });

  return;
}
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)

# api documentation for  [continuation-local-storage (v3.2.0)](https://github.com/othiym23/node-continuation-local-storage#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-continuation-local-storage.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-continuation-local-storage) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-continuation-local-storage.svg)](https://travis-ci.org/npmdoc/node-npmdoc-continuation-local-storage)
#### userland implementation of https://github.com/joyent/node/issues/5243

[![NPM](https://nodei.co/npm/continuation-local-storage.png?downloads=true)](https://www.npmjs.com/package/continuation-local-storage)

[![apidoc](https://npmdoc.github.io/node-npmdoc-continuation-local-storage/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-continuation-local-storage_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-continuation-local-storage/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-continuation-local-storage/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-continuation-local-storage/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Forrest L Norvell",
        "email": "ogd@aoaioxxysz.net"
    },
    "bugs": {
        "url": "https://github.com/othiym23/node-continuation-local-storage/issues"
    },
    "contributors": [
        {
            "name": "Tim Caswell",
            "email": "tim@creationix.com"
        },
        {
            "name": "Forrest L Norvell",
            "email": "ogd@aoaioxxysz.net"
        }
    ],
    "dependencies": {
        "async-listener": "^0.6.0",
        "emitter-listener": "^1.0.1"
    },
    "description": "userland implementation of https://github.com/joyent/node/issues/5243",
    "devDependencies": {
        "tap": "^6.3.2"
    },
    "directories": {
        "test": "test"
    },
    "dist": {
        "shasum": "e19fc36b597090a5d4e4a3b2ea3ebc5e29694a24",
        "tarball": "https://registry.npmjs.org/continuation-local-storage/-/continuation-local-storage-3.2.0.tgz"
    },
    "gitHead": "9f002d05bc50882c3dc1403ca5153b1a3df8a7ff",
    "homepage": "https://github.com/othiym23/node-continuation-local-storage#readme",
    "keywords": [
        "threading",
        "shared",
        "context",
        "domains",
        "tracing",
        "logging"
    ],
    "license": "BSD-2-Clause",
    "main": "context.js",
    "maintainers": [
        {
            "name": "othiym23",
            "email": "ogd@aoaioxxysz.net"
        },
        {
            "name": "qard",
            "email": "admin@stephenbelanger.com"
        }
    ],
    "name": "continuation-local-storage",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/othiym23/node-continuation-local-storage.git"
    },
    "scripts": {
        "test": "tap test/*.tap.js"
    },
    "version": "3.2.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module continuation-local-storage](#apidoc.module.continuation-local-storage)
1.  [function <span class="apidocSignatureSpan">continuation-local-storage.</span>createNamespace (name)](#apidoc.element.continuation-local-storage.createNamespace)
1.  [function <span class="apidocSignatureSpan">continuation-local-storage.</span>destroyNamespace (name)](#apidoc.element.continuation-local-storage.destroyNamespace)
1.  [function <span class="apidocSignatureSpan">continuation-local-storage.</span>getNamespace (name)](#apidoc.element.continuation-local-storage.getNamespace)
1.  [function <span class="apidocSignatureSpan">continuation-local-storage.</span>reset ()](#apidoc.element.continuation-local-storage.reset)



# <a name="apidoc.module.continuation-local-storage"></a>[module continuation-local-storage](#apidoc.module.continuation-local-storage)

#### <a name="apidoc.element.continuation-local-storage.createNamespace"></a>[function <span class="apidocSignatureSpan">continuation-local-storage.</span>createNamespace (name)](#apidoc.element.continuation-local-storage.createNamespace)
- description and source-code
```javascript
function create(name) {
  assert.ok(name, "namespace must be given a name!");

  var namespace = new Namespace(name);
  namespace.id = process.addAsyncListener({
    create : function () { return namespace.active; },
    before : function (context, storage) { if (storage) namespace.enter(storage); },
    after  : function (context, storage) { if (storage) namespace.exit(storage); },
    error  : function (storage) { if (storage) namespace.exit(storage); }
  });

  process.namespaces[name] = namespace;
  return namespace;
}
```
- example usage
```shell
...
  setTimeout(function() {
    // runs with the default context, because nested contexts have ended
    console.log(writer.get('value')); // prints 0
  }, 1000);
}
'''

## cls.createNamespace(name)

* return: {Namespace}

Each application wanting to use continuation-local values should create its own
namespace. Reading from (or, more significantly, writing to) namespaces that
don't belong to you is a faux pas.
...
```

#### <a name="apidoc.element.continuation-local-storage.destroyNamespace"></a>[function <span class="apidocSignatureSpan">continuation-local-storage.</span>destroyNamespace (name)](#apidoc.element.continuation-local-storage.destroyNamespace)
- description and source-code
```javascript
function destroy(name) {
  var namespace = get(name);

  assert.ok(namespace,    "can't delete nonexistent namespace!");
  assert.ok(namespace.id, "don't assign to process.namespaces directly!");

  process.removeAsyncListener(namespace.id);
  process.namespaces[name] = null;
}
```
- example usage
```shell
...

## cls.getNamespace(name)

* return: {Namespace}

Look up an existing namespace.

## cls.destroyNamespace(name)

Dispose of an existing namespace. WARNING: be sure to dispose of any references
to destroyed namespaces in your old code, as contexts associated with them will
no longer be propagated.

## cls.reset()
...
```

#### <a name="apidoc.element.continuation-local-storage.getNamespace"></a>[function <span class="apidocSignatureSpan">continuation-local-storage.</span>getNamespace (name)](#apidoc.element.continuation-local-storage.getNamespace)
- description and source-code
```javascript
function get(name) {
  return process.namespaces[name];
}
```
- example usage
```shell
...

* return: {Namespace}

Each application wanting to use continuation-local values should create its own
namespace. Reading from (or, more significantly, writing to) namespaces that
don't belong to you is a faux pas.

## cls.getNamespace(name)

* return: {Namespace}

Look up an existing namespace.

## cls.destroyNamespace(name)
...
```

#### <a name="apidoc.element.continuation-local-storage.reset"></a>[function <span class="apidocSignatureSpan">continuation-local-storage.</span>reset ()](#apidoc.element.continuation-local-storage.reset)
- description and source-code
```javascript
function reset() {
  // must unregister async listeners
  if (process.namespaces) {
    Object.keys(process.namespaces).forEach(function (name) {
      destroy(name);
    });
  }
  process.namespaces = Object.create(null);
}
```
- example usage
```shell
...

## cls.destroyNamespace(name)

Dispose of an existing namespace. WARNING: be sure to dispose of any references
to destroyed namespaces in your old code, as contexts associated with them will
no longer be propagated.

## cls.reset()

Completely reset all continuation-local storage namespaces. WARNING: while this
will stop the propagation of values in any existing namespaces, if there are
remaining references to those namespaces in code, the associated storage will
still be reachable, even though the associated state is no longer being updated.
Make sure you clean up any references to destroyed namespaces yourself.
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)

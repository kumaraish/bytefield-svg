# bytefield-svg

A Node module for generating byte field diagrams like
[this one](https://deepsymmetry.org/images/test.svg).
Inspired by the LaTeX [`bytefield`](https://ctan.org/pkg/bytefield?lang=en)
package. Powered by a [Clojure](https://clojure.org)-based
[domain specific language](https://bytefield-svg.deepsymmetry.org/)
(now built on top of [SCI](https://github.com/borkdude/sci), the
Small Clojure Interpreter).

[![License](https://img.shields.io/badge/License-Eclipse%20Public%20License%202.0-blue.svg)](#license)

## Usage

This is published to npm, so you can install it for use in a Javascript
project by running:

    npm install bytefield-svg

Or you can install it globally for use anywhere by running:

    npm install -g bytefield-svg

The language you use to create diagrams has its own
[documentation site](https://bytefield-svg.deepsymmetry.org/).

### Invoking from Javascript

Once installed, you can generate diagrams in your code like this:

```javascript
const generate = require('bytefield-svg');

const source = `
;; Put your diagram DSL here, or read it from a file, or build it...
`;
const diagram = generate(source);
process.stdout.write(diagram);
```

Of course, you can do other things than writing the diagram to standard out.
For a few more examples of usage, you can see the
[cli.js](https://github.com/Deep-Symmetry/bytefield-svg/blob/master/cli.js)
source in this project which implements the command-line interface, our next
topic:

### Invoking from the Command Line

This package also installs a command-line tool. If you have installed it
globally, you can simply invoke it as `bytefield-svg`. If you have installed
it locally, you can invoke it using `npx bytefield-svg` within your project.

With no arguments, the tool will read the diagram source from standard in, and
write it to standard out. So you can generate the example diagram from this
Read Me, as long as you have the [`test.edn`
file](https://github.com/Deep-Symmetry/bytefield-svg/blob/master/test.edn),
by running:

    bytefield-svg <test.edn >test.svg

You can also use the `-s` or `--source` command-line argument to specify
that the tool should read from a named file rather than standard in, and
`-o` or `--output` to write to a named file rather than standard out, which
might be helpful in a scripting pipeline:

    bytefield-svg --source test.edn --output test.svg

If you supply just a filename with no command-line flag, it is assumed
to be the diagram source file.

Invoking it with `-h` or `--help` displays this usage information.

## Background

The DSL has been nicely validated by porting all of the LaTeX
documents I needed it for to an [Antora documentation
site](https://djl-analysis.deepsymmetry.org/djl-analysis/track_metadata.html).

As that site suggests, this package's main purpose is to act as an
[Asciidoctor](https://asciidoctor.org) extension hosted by an
[Antora](https://antora.org) plugin. This is done with the help of a
[framework](https://gitlab.com/djencks/asciidoctor-generic-svg-extension.js)
that [David Jencks](https://gitlab.com/djencks) has created.

However, plugin support for Antora is not yet released, so in order
for this to work you need to use one of David's [Antora fork
branches](https://gitlab.com/djencks/antora/-/tree/issue-585-with-377-582-git-credential-plugin).

It used to be necessary to buld and run his forks locally, but they
are now available as tarballs that you can reference in your
`package.json`, which is how the dysentery project documentation site
does it. That is a great example of how to use many fetures of
bytefield-svg. Its [build
instructions](https://github.com/Deep-Symmetry/dysentery/tree/master/doc)
show how to locally build it and can serve, along with that project's
[`packge.json`](https://github.com/Deep-Symmetry/dysentery/blob/master/package.json),
as a starting point for your own project.

## Building

To build a development build of `bytefield-svg` from source, clone the
repository and make sure you have [Node.js](https://nodejs.org/en/)
and the [Clojure CLI
tools](https://clojure.org/guides/getting_started) installed, then
from the top-level directory of your cloned repo run:

    npm install
    npm run build

This will create the file `lib.js`. At that point, you can generate
[the sample diagram](https://deepsymmetry.org/images/test.svg) by running:

    node cli.js test.edn >test.svg

(The [`test.edn`
file](https://github.com/Deep-Symmetry/bytefield-svg/blob/master/test.edn)
is present in this project. It renders a diagram from the above-linked
documentation site. With some well-designed helper
functions in site's own include file, the source for an even
more attractive version of the diagram shrinks to
[this](https://github.com/Deep-Symmetry/dysentery/blob/379555f21244354c4dc0c9711c8cb3a3552bc64b/doc/modules/ROOT/examples/dbserver_shared.edn)).

The [DSL documentation](https://bytefield-svg.deepsymmetry.org/) is hosted
on netlify, and built out of the [doc](doc) folder, which includes
build instructions. (They also serve as an example of how to build a
site that uses bytefield-svg, because it is used to draw the results
of the code examples in the documentation.)

To check for outdated dependencies, you can run:

    clojure -A:outdated

## Releasing

To cut a release, check for outdated dependencies as above, update the
version in `package.json`, tag and push to GitHub, then run:

    npm install
    npm run release
    npm publish

## License

<a href="http://deepsymmetry.org"><img align="right" alt="Deep Symmetry"
 src="doc/assets/DS-logo-bw-200-padded-left.png" width="216" height="123"></a>

Copyright © 2020 [Deep Symmetry, LLC](http://deepsymmetry.org)

Distributed under the [Eclipse Public License
2.0](https://opensource.org/licenses/EPL-2.0). By using this software
in any fashion, you are agreeing to be bound by the terms of this
license. You must not remove this notice, or any other, from this
software.

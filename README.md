goimports-update-ignore
=======================

Scan `$GOPATH/src/` and generate a `$GOPATH/src/.goimportsignore` to make
goimports run faster and use less CPU.

# Why?

Less CPU eaten by goimports, longer battery life. Faster time to run.

In particular, I don't use my `$GOPATH` just for go code and I have some very
deep source trees, such as the linux kernel repository lying around.

# Features

* Avoid generating overly specific rules by limiting the depth (configurable).
* Measure the number of directories scanned by goimports by invoking it and
  length of time and CPU time taken in the worst case.

# Warning

*Data loss risk*: Invoking this will overwrite any existing
                  `$GOPATH/src/.goimportsignore` you have, without asking.

Magically ignoring things is the sort of thing you can easily forget about.
Later you'll be wondering why something mysteriously doesn't work, and it will
be because it was ignored in some distant past.

Use at your own risk.

# Example usage

Here's what `goimports-update-ignore` does on @pwaller's `$GOPATH`, with warm
disk caches.

```
$ go get github.com/pwaller/goimports-update-ignore

$ # Start with nothing
$ rm $GOPATH/src/.goimportsignore

$ # Measure original times.
$ goimports-update-ignore -measure-only
Ignored 0 directories. goimports considers 44367 directories in 877ms (cpu=4208ms).

$ goimports-update-ignore -max-depth 1
Ignored 34 directories. goimports considers 27794 directories in 486ms (cpu=2224ms).

$ goimports-update-ignore -max-depth 2
Ignored 118 directories. goimports considers 25461 directories in 406ms (cpu=1820ms).

$ goimports-update-ignore -max-depth 3
Ignored 304 directories. goimports considers 13238 directories in 267ms (cpu=1224ms).

$ goimports-update-ignore -max-depth 4
Ignored 572 directories. goimports considers 11194 directories in 234ms (cpu=684ms).

$ goimports-update-ignore -max-depth 5
Ignored 797 directories. goimports considers 9943 directories in 193ms (cpu=664ms).
```

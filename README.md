# gosemver

A [Semantic Versioning](http://semver.org) library for the Go programming language.

## Usage

```go
import "github.com/myfreeweb/gosemver"
```

Parsing:

```go
gosemver.parseVersion("v0.1.0-alpha+build001") // Version{"v", 0, 1, 0, "alpha", "build001"}
```

Sorting:

```go
import "sort"

vers := []gosemver.Version{
  {"v", 1, 0, 0, "", ""},
  {"v", 0, 1, 0, "", ""},
}

sort.Sort(gosemver.Versions(vers))
```

You can also sort version strings!

```go
import "sort"

verStrs := []string{
  "1.9.3", "2.0.0", "1.9.3-alpha+build.001", "1.8.2-patch1"
}

sort.Sort(gosemver.VersionStrings(verStrs))
```

**WARNING**: sorting version strings will `panic` if it can't parse a string!

Output:

```go
ver := gosemver.Version{"v", 2, 3, 1, "alpha", "build.001"}
ver.String() // "v2.3.1-alpha+build.001"
```

Constraints:

```go
ver := gosemver.Version{"", 3, 0, 3, "", ""}
// returns result, error:
ver.Satisfies("*") // true, nil
ver.Satisfies("== 3.0.3") // true, nil
ver.Satisfies(">= 3.0.1") // true, nil
ver.Satisfies(">= 3.0") // true, nil
ver.Satisfies("> 3.0.0") // true, nil
ver.Satisfies("~> 3.0.4") // false, nil
ver.Satisfies("~> 3.0.1") // true, nil
ver.Satisfies("~> 3.0") // true, nil
ver.Satisfies("~> 2.9") // false, nil
ver.Satisfies("^2.9") // false, nil
// ^ and ~> are the same operator
```

## TODO

- more constraints (like [node semver](https://www.npmjs.org/doc/misc/semver.html))
- executable (like, `ls | gosemver sort`, `gosemver inc patch 1.1.0`)

### Status Guide

- `X` -- still a bug
- `+` -- compiles now
- `F` -- still fails to compile, but appears to be legit

---

### Crates that are still broken

| Crate                  | Version | Status | Minimized | Issue      | Notes |
| -----                  | ---     | ---    | ---       | ---        | ---   |
| `attr`                 | 0.1.0   | X      | [2][]     | [#53569][] | |
| `liner`                | 0.4.4   | X?     |           | [#53114][]?| FIXME: reconfirm |
| `LocustDB`             | c59429  | X      | [11][]    | [#53570][] | |
| `proptest-arbitrary`   | 0.2.2   | X?     |           | [#53570][]?| FIXME: reconfirm |
| `shred`                | 0.7.0   | X      | [7][]     | [#53121][] | |

---

### Crates where errors were either fixed or are correct

| Crate                  | Version | Status | Minimized | Issue      | Notes |
| -----                  | ---     | ---    | ---       | ---        | ---   |
| `argdata`              | 0.0.4   | +      | [1][]     |            | |
| `brassfibre`           | 0.2.0   | +      |           |            | |
| `capnp`                | 0.6.2   | F      |           |            | same as 0.7.5
| `capnp`                | 0.7.5   | F      |           |            | [#47349][] (diagnostics)
| `clucstr`              | 0.1.3   | +      |           |            | |
| `embed`                | 0.1.1   | +      |           |            | |
| `envelope`             | 0.8.1   | +      | [3][]     |            | |
| `extended-collections` | 0.1.0   | F      |           | [#51915][] | wants a more aggressive 2PB |
| `extended-collections` | 0.2.0   | F      | [9][]     | [#51915][] | wants a more aggressive 2PB |
| `fractions_and_matrices` | 2ea35 | F      |           |            | [#47349][] (diagnostics)
| `fscmp`                | 0.1.1   | +      |           |            | |
| `galvanize`            | 0.0.1   | F      | [10][]    |            | [#52059][] (diagnostics) |
| `gearley`              | 0.0.1   | F      |           |            | |
| `getopts`              | 0.2.14  | F      |           |            | similar to 0.2.15 |
| `getopts`              | 0.2.15  | F      |           |            | FIXME: find an issue link |
| `gluon_base`           | 0.6.2   | +      |           | [#53119][] | similar to 0.8.0 |
| `gluon_base`           | 0.8.0   | +      |           | [#53119][] | |
| `ilp-packet`           | 0.3.0   | F      | [4][]     |            | FIXME: find an issue link |
| `jmespath-macros`      | 0.1.1   | +      |           |            | |
| `mop-solvers`          | 0.0.1   | F      |           |            | [#47349][] (diagnostics) |
| `nalgebra`             | 0.11.2  | F      |           |            | same as 0.16.0 |
| `nalgebra`             | 0.12.3  | F      |           |            | same as 0.16.0 |
| `nalgebra`             | 0.13.1  | F      |           |            | same as 0.16.0 |
| `nalgebra`             | 0.14.4  | F      |           |            | same as 0.16.0 |
| `nalgebra`             | 0.15.3  | F      |           |            | same as 0.16.0 |
| `nalgebra`             | 0.16.0  | F      |           |            | [#47349][] (diagnostics) |
| `pear_codegen`         | 0.0.18  | +      |           |            | |
| `pgetopts`             | 0.1.2   | F      | [5][]     |            | FIXME: find an issue link |
| `phf_macros`           | 0.7.22  | +      |           |            | |
| `pom`                  | 2.0.1   | F      |           |[#53040][]? | FIXME: reconfirm |
| `rgen3-save`           | 0.1.0   | F      | [6][]     |            | might be a case for 2Phi borrows |
| `rome`                 | 0.1.2   | +      |           |            | |
| `rs-graph`             | 0.14.0  | F      |           |            | same as 0.14.1 |
| `rs-graph`             | 0.14.1  | F      | ?         |            | [#53121][]? and [#47349][] (diagnostics) |
| `rusttype`             | 0.2.1   | F      |           |            | same as 0.2.3 |
| `rusttype`             | 0.2.3   | F      |           |            | [#29149][] |
| `rustysecrets`         | 38f98   | F      | [12][]    |            | [#47349][] (diagnostics) |
| `segment-tree`         | 1.1.0   | +?     |           |            | FIXME: reconfirm or [#47349][] |
| `serenity`             | 0.4.8   | F      | [14][]    |            | |
| `speculate`            | 0.0.26  | +      |           |            | |
| `sprs`                 | 0.6.2   | F      |           |            | [#47349][] (diagnostics) |
| `swear`                | 0.1.0   | +      |           |            | |
| `syntex_syntax`        | 0.36.0  | F      |           |            | same as 0.39.0 |
| `syntex_syntax`        | 0.39.0  | F      | [13][]    |            | |
| `tarpc-plugins`        | 0.3.0   | +      |           |            | |
| `try_transform_mut`    | 0.1.0   | +      | [8][]     | [#53123][] | |
| `unicode_names2_macros` | 0.2.0  | +      |           |            | |
| `url`                  | 1.1.1   | F      |           |            | similar to 1.7.0
| `url`                  | 1.2.3   | F      |           |            | similar to 1.7.0
| `url`                  | 1.2.4   | F      |           |            | similar to 1.7.0
| `url`                  | 1.4.0   | F      |           |            | similar to 1.7.0
| `url`                  | 1.4.1   | F      |           |            | similar to 1.7.0
| `url`                  | 1.5.1   | F      |           |            | similar to 1.7.0
| `url`                  | 1.6.0   | F      |           |            | similar to 1.7.0
| `url`                  | 1.7.0   | F      |           |            | |
| `vek`                  | 0.9.4   | F      |           |            | [#47349][] (diagnostics) |
| `yara`                 | 0.1.0   | F      |           |            | [#52059][] (diagnostics) |


[1]: https://play.rust-lang.org/?gist=1e7555092563371569caadb0d35b897c&version=nightly&mode=debug&edition=2015
[2]: https://play.rust-lang.org/?gist=46146e256a3e138cbd42d0ee34b43571&version=nightly&mode=debug&edition=2015
[3]: https://play.rust-lang.org/?gist=d3eedb59571edff7d7ff0975c44e8faa&version=nightly&mode=debug&edition=2015
[4]: https://play.rust-lang.org/?gist=fcd15716ab77c5aaf787471a87beebb2&version=nightly&mode=debug&edition=2015
[5]: https://play.rust-lang.org/?gist=8c34aede2762d2bb408f98a0e004f514&version=nightly&mode=debug&edition=2015
[#53114]: https://github.com/rust-lang/rust/issues/53114
[#53119]: https://github.com/rust-lang/rust/issues/53119
[6]: https://play.rust-lang.org/?gist=104d7f51c89e5d6b332dfd7e9090d6a2&version=nightly&mode=debug&edition=2015
[7]: https://gist.github.com/nikomatsakis/e795e4f05bf7119540f351927e1965e6
[8]: https://play.rust-lang.org/?gist=018e37797b3965890528ef25791bce50&version=nightly&mode=debug&edition=2015
[#52059]: https://github.com/rust-lang/rust/issues/52059
[#53121]: https://github.com/rust-lang/rust/issues/53121
[#53123]: https://github.com/rust-lang/rust/issues/53123
[#51915]: https://github.com/rust-lang/rust/issues/51915
[9]: https://play.rust-lang.org/?gist=0265b0131f94793854ab1b7b1c96369e&version=nightly&mode=debug&edition=2015
[#47349]: https://github.com/rust-lang/rust/issues/47349
[10]: https://play.rust-lang.org/?gist=f3c0638cd128773bfa2413e3d3ec3783&version=nightly&mode=debug&edition=2015
[#53569]: https://github.com/rust-lang/rust/issues/53569
[11]: https://play.rust-lang.org/?gist=a051d535a66f46f52ceb271383d334d5&version=nightly&mode=debug&edition=2015
[#53570]: https://github.com/rust-lang/rust/issues/53570
[12]: https://play.rust-lang.org/?gist=b6e2b7ba1f746b2a91f237cbe2892f74&version=nightly&mode=debug&edition=2015
[13]: https://play.rust-lang.org/?gist=2d4da1787e06a75def402cfe1f9e544e&version=nightly&mode=debug&edition=2015
[#29149]: https://github.com/rust-lang/rust/issues/29149
[14]: https://play.rust-lang.org/?gist=36b94633fca7e490ba8c03f4fa94cbbd&version=nightly&mode=debug&edition=2015
[#53040]: https://github.com/rust-lang/rust/issues/53040

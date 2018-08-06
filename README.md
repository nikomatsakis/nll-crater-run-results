Status Guide:

- `X` -- still a bug
- `+` -- compiles now
- `F` -- still fails to compile, but appears to be legit

---

| Crate                | Version | Status | Minimized | Issue      | Notes |
| -----                | ---     | ---    | ---       | ---        | ---   |
| argdata              | 0.0.4   | X      | [1][]     |            | |
| attr                 | 0.1.0   | X      | [2][]     |            | |
| brassfibre           | 0.2.0   | +      |           |            | |
| clucstr              | 0.1.3   | +      |           |            | |
| embed                | 0.1.1   | +      |           |            | |
| envelope             | 0.8.1   | +      | [3][]     |            | |
| extended-collections | 0.1.0   | X      |           |            | like 0.2.0 |
| extended-collections | 0.2.0   | X      |           |            | 2PB expansion? #51915 |
| fscmp                | 0.1.1   | +      |           |            | |
| galvanize            | 0.0.1   | F      |           |            | #52059 |
| gearley              | 0.0.1   | F      |           |            | |
| gluon_base           | 0.6.2   | X      |           |            | similar to 0.8.0 |
| gluon_base           | 0.8.0   | X      |           |            | similar to envelope-0.8.1 ? |
| ilp-packet           | 0.3.0   | F      | [4][]     |            | |
| jmespath-macros      | 0.1.1   | +      |           |            | |
| liner                | 0.4.4   | X      |           | [#53114][] | |

[1]: https://play.rust-lang.org/?gist=1e7555092563371569caadb0d35b897c&version=nightly&mode=debug&edition=2015
[2]: https://play.rust-lang.org/?gist=46146e256a3e138cbd42d0ee34b43571&version=nightly&mode=debug&edition=2015
[3]: https://play.rust-lang.org/?gist=d3eedb59571edff7d7ff0975c44e8faa&version=nightly&mode=debug&edition=2015
[4]: https://play.rust-lang.org/?gist=fcd15716ab77c5aaf787471a87beebb2&version=nightly&mode=debug&edition=2015
[#53114]: https://github.com/rust-lang/rust/issues/53114
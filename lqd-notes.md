### Possibly correctly filtered root regressions but don't trust me you never know

With possible duplicates crates versions (we only know the root regression crate even if there are possible different regressions in different versions of it)

How this list was generated:
1. Took the list of regressions from the crater report
2. Filtered the `crater-tree` report to get the lis of "root regression"s
3. Filtered list #1 with crates from list #2
4. Filtered the result by removing the crates whose errors were only in dependencies, leaving only root regressions crate with errors in their own code

#### Root regressions starting with `A`
 - [argdata-0.0.4](https://crates.io/crates/argdata/0.0.4): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/argdata-0.0.4/log.txt)
 -> Looks like a problem of missing outlives annotations on associated types. [Reduced repro](https://play.rust-lang.org/?gist=1e7555092563371569caadb0d35b897c&version=nightly&mode=debug&edition=2015), again close to [#52078](https://github.com/rust-lang/rust/issues/52078).
  -> 2018.07.30: Bug. Still seems to be a problem! --nmatsakis
  
 - [attr-0.1.0](https://crates.io/crates/attr/0.1.0): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/attr-0.1.0/log.txt)
-> [Minimized repro](https://play.rust-lang.org/?gist=46146e256a3e138cbd42d0ee34b43571&version=nightly&mode=debug&edition=2015) where the two borrowcks differ. It again seems going through an associated type, so it could be related to [this run's error](https://hackmd.io/VCYh_KdgQkO2O_45jWAxaw?both#Regressed-because-of-dependency-rs-graph-0140) in `rs-graph` or [#52078](https://github.com/rust-lang/rust/issues/52078), but here it also feels different because the ownership of the Err in the Result matters. The diagnostics / error message don't seem super clear.
  -> 2018.07.30: Bug. This *looks* like a bug in MIR borrowck to me.

#### Root regressions starting with `B`
 - [brassfibre-0.2.0](https://crates.io/crates/brassfibre/0.2.0): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/brassfibre-0.2.0/log.txt)
 -> highly generic crate, so our associated types / implied bounds / etc phantoms lurk around. [Minimized repro](https://play.rust-lang.org/?gist=b620bd0445aeaae0a46d67cdb24c252a&version=nightly&mode=debug&edition=2015) showing the 2 borrowck differing.
-> Fixed in master by [PR #51959](https://github.com/rust-lang/rust/pull/51959)
  
#### Root regressions starting with `C`
 - [clucstr-0.1.3](https://crates.io/crates/clucstr/0.1.3): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/clucstr-0.1.3/log.txt)
 -> known ICE broken MIR plugin_registrar
 -> 2018.07.30: Fixed in nightly
 
#### Root regressions starting with `E`
 - [embed-0.1.1](https://crates.io/crates/embed/0.1.1): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/embed-0.1.1/log.txt)
 -> known ICE broken MIR plugin_registrar
 -> 2018.07.30: Fixed.
  
 - [envelope-0.8.1](https://crates.io/crates/envelope/0.8.1): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/envelope-0.8.1/log.txt)
 -> This feels close to the case of `rs-graph-0.14.0` [described here](https://hackmd.io/VCYh_KdgQkO2O_45jWAxaw?both#Regressed-because-of-dependency-rs-graph-0140). There is an associated type, missing some outlives annotations, I'm unsure whether it's correct to reject this code (both yes and no: it's missing some lifetime bounds), but this case is linked to other "missing implied bounds" cases we've seen (eg [#52078](https://github.com/rust-lang/rust/issues/52078)), plus this time it's with nested closures, and also the diagnostics get weird. [Reduced repro](https://play.rust-lang.org/?gist=d3eedb59571edff7d7ff0975c44e8faa&version=nightly&mode=debug&edition=2015). 
 Note that rustc suggests to add a `E: 'static` annotation even though `E: 'a` makes MIR borrowck accept the code. Note that the suggestion is `E: 'a` if the unused `_env` parameter is removed. 
 Also note that if we add a bound `type Y: 'a` to the trait, it will make the code build; even though it can be confusing that there already is a `E::Y: 'a` annotation on the function itself.
   -> 2018.07.30: Bug. Still not working. -nmatsakis

 - [extended-collections-0.1.0](https://crates.io/crates/extended-collections/0.1.0): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/extended-collections-0.1.0/log.txt)
 -> the errors look extremely similar to the version 0.2
 
 - [extended-collections-0.2.0](https://crates.io/crates/extended-collections/0.2.0): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/extended-collections-0.2.0/log.txt)
-> partially triaged from crater run nll-1, interesting errors.
All (or most of these 50 errors) could be seen as an instance of the 2PB-expansion [#51915](https://github.com/rust-lang/rust/issues/51915). `cannot borrow "*next_link.next" as mutable more than once at a time` with `next_link` used in 2 arguments of a function call. 
For example, [Repro](https://play.rust-lang.org/?gist=c06e562a895ab15985f282ca4460f52e&version=nightly&mode=debug&edition=2015) -- note again that AST borrowck rejects this reduction, which was made to showcase the error rather than the difference between the 2.
  -> 2018.07.30: Bug.

#### Root regressions starting with `F`
 - [fscmp-0.1.1](https://crates.io/crates/fscmp/0.1.1): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/fscmp-0.1.1/log.txt)
 -> known in crater run nll-1: [#52111](https://github.com/rust-lang/rust/issues/52111) which is already fixed in master
   -> 2018.07.30: Fixed.

#### Root regressions starting with `G`
 - [galvanize-0.0.1](https://crates.io/crates/galvanize/0.0.1): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/galvanize-0.0.1/log.txt)
 -> exactly the same bug which happened in `url-1.7.0`: a move out of a struct with a destructor. [Repro](https://play.rust-lang.org/?gist=f3c0638cd128773bfa2413e3d3ec3783&version=nightly&mode=debug&edition=2015). The unclear diagnostic was already filed as [#52059](https://github.com/rust-lang/rust/issues/52059).
 - [gearley-0.0.1](https://crates.io/crates/gearley/0.0.1): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/gearley-0.0.1/log.txt)
 -> feels like [#46589](https://github.com/rust-lang/rust/issues/46589#issuecomment-402257979) 
 -> 2018.07.30: Fixed-by-NLL. This looks to me like a legitimate error. -- nmatsakis
 - [gluon_base-0.6.2](https://crates.io/crates/gluon_base/0.6.2): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/gluon_base-0.6.2/log.txt)
 -> same errors as version 0.8.0 
 - [gluon_base-0.8.0](https://crates.io/crates/gluon_base/0.8.0): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/gluon_base-0.8.0/log.txt)
 -> looks similar to `envelope-0.8.1` and `swear-0.1.0`. 

#### Root regressions starting with `I`
 - [ilp-packet-0.3.0](https://crates.io/crates/ilp-packet/0.3.0): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/ilp-packet-0.3.0/log.txt)
 -> haha. [Minimized repro](https://play.rust-lang.org/?gist=fcd15716ab77c5aaf787471a87beebb2&version=nightly&mode=debug&edition=2015) showing the difference between the 2 borrowcks.

#### Root regressions starting with `J`
 - [jmespath-macros-0.1.1](https://crates.io/crates/jmespath-macros/0.1.1): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/jmespath-macros-0.1.1/log.txt)
 -> known ICE broken MIR plugin_registrar
 
#### Root regressions starting with `L`
 - [liner-0.4.4](https://crates.io/crates/liner/0.4.4): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/liner-0.4.4/log.txt)
 -> triaged in crater run nll-1
 -> interesting errors, and look like to be the ones "fixed by NLL", [repro](https://play.rust-lang.org/?gist=ccb6c558bf380bc7026074c958be3c7a&version=nightly&mode=debug&edition=2015)
 
#### Root regressions starting with `N`
 - [nalgebra-0.15.3](https://crates.io/crates/nalgebra/0.15.3): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/nalgebra-0.15.3/log.txt)
 -> triaged in crater run nll-1
 -> interesting error, could be seen as an instance of the 2PB-expansion [#51915](https://github.com/rust-lang/rust/issues/51915)
 
#### Root regressions starting with `P`
 - [pear_codegen-0.0.18](https://crates.io/crates/pear_codegen/0.0.18): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/pear_codegen-0.0.18/log.txt)
 -> known ICE broken MIR plugin_registrar
 
 - [pgetopts-0.1.2](https://crates.io/crates/pgetopts/0.1.2): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/pgetopts-0.1.2/log.txt)
 -> while loop condition borrowing a value itself mutated inside the loop by a closure — somehow feels close to [this version](https://github.com/rust-lang/rust/issues/46589#issuecomment-402257979) of issue 46589 but with closures ? Either "fixed by NLL" or a matter left to Polonius ? [Minimized repro](https://play.rust-lang.org/?gist=c33970bf610c7b5bdbf73c565ae3ec27&version=nightly&mode=debug&edition=2015) that AST borrowck accepts.
 
 - [phf_macros-0.7.22](https://crates.io/crates/phf_macros/0.7.22): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/phf_macros-0.7.22/log.txt)
 -> known ICE broken MIR macro_registrar
 
 - [pom-2.0.1](https://crates.io/crates/pom/2.0.1): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/pom-2.0.1/log.txt)
-> a weird error mesg without diagnostics `free region "" does not outlive free region "'_#6r"` which has _since been fixed on master_. (Could be interesting to look again, but a bit inconvenient to minimize as it looks like parsing with many parser combinators, using operator overloading, etc.)

#### Root regressions starting with `R`
 - [rgen3-save-0.1.0](https://crates.io/crates/rgen3-save/0.1.0): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/rgen3-save-0.1.0/log.txt)
 -> Unexpected Pokémon. Feels correct to reject this code. And I feel we've seen the same type of error elsewhere in this crater run. [Reduced repro](https://play.rust-lang.org/?gist=104d7f51c89e5d6b332dfd7e9090d6a2&version=nightly&mode=debug&edition=2015).
 
 - [rome-0.1.2](https://crates.io/crates/rome/0.1.2): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/rome-0.1.2/log.txt)
 -> This error is hard to reach and extract because of the macro usage. It does look similar to others we've seen in the run, around implied bounds, and very likely associated types.

#### Root regressions starting with `S`
 - [segment-tree-1.1.0](https://crates.io/crates/segment-tree/1.1.0): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/segment-tree-1.1.0/log.txt)
 -> interesting error, could be seen as an instance of the 2PB-expansion [#51915](https://github.com/rust-lang/rust/issues/51915) 
 - [shred-0.7.0](https://crates.io/crates/shred/0.7.0): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/shred-0.7.0/log.txt)
 -> error about missing outlives annotations. When adding the constraints rustc proposes in the error, the crate builds. 
Maybe it will be fixed under the current plan, but the `infer_outlives_requirements` feature doesn't currently fix this specific case ([tracking issues for RFC 2093](https://github.com/rust-lang/rust/issues/44493)) 
 - [speculate-0.0.26](https://crates.io/crates/speculate/0.0.26): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/speculate-0.0.26/log.txt)
 -> known ICE broken MIR plugin_registrar
 - [swear-0.1.0](https://crates.io/crates/swear/0.1.0): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/swear-0.1.0/log.txt)
 -> some weird interaction between generics, closures, and inference of lifetimes (suggesting a `T: 'static` annotation to build correctly); looks like the errors happening in `gluon-base-0.8.0` and `envelope-0.8.1`.

#### Root regressions starting with `T`
 - [tarpc-plugins-0.3.0](https://crates.io/crates/tarpc-plugins/0.3.0): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/tarpc-plugins-0.3.0/log.txt)
 -> known ICE broken MIR plugin_registrar
 - [try_transform_mut-0.1.0](https://crates.io/crates/try_transform_mut/0.1.0): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/try_transform_mut-0.1.0/log.txt)
-> weird. [Repro](https://play.rust-lang.org/?gist=018e37797b3965890528ef25791bce50&version=nightly&mode=debug&edition=2015)

#### Root regressions starting with `U`
 - [unicode_names2_macros-0.2.0](https://crates.io/crates/unicode_names2_macros/0.2.0): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/unicode_names2_macros-0.2.0/log.txt)
 -> known ICE broken MIR plugin_registrar 

#### Root regressions starting with `Y`
 - [yara-0.1.0](https://crates.io/crates/yara/0.1.0): [log](https://cargobomb-reports.s3.amazonaws.com/nll-3/00bcc44fb92c28465c727881355c3235a56a4045/reg/yara-0.1.0/log.txt)
-> exactly the same bug which happened in `url-1.7.0`: a move out of a struct with a destructor.

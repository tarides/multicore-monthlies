# Multicore OCaml: May 2022

Welcome to the May 2022 [Multicore
OCaml](https://github.com/ocaml-multicore/ocaml-multicore) monthly
report! This report along with the [previous
updates](https://discuss.ocaml.org/tag/multicore-monthly) have been
compiled by @avsm, @ctk21, @kayceesrk and @shakthimaan.

The primary focus for the month of May has been the preparation for an
OCaml 5.0 -alpha release. The build and configure steps for the
compiler has seen tremendous improvements. The sub-makefiles are now
getting merged into the top-level Makefile. In order to facilitate
better runtime tracing, the Eventring implementation to support
continuous monitoring of OCaml applications has been merged. A new
`ocaml-cpu` library is being actively developed to obtain CPU topology
and affinity from OCaml for Linux and BSD. Safety and fixes for race
conditions continue to be addressed with more tests. The Sandmark
benchmark suite has completely moved to using GitHub Actions, and work
is underway to build benchmarks for 5.1.0+trunk and beyond. The
current-bench project with its new bechamel library can now report
execution time of functions.

As always, the Multicore OCaml updates are listed first, which are
then followed by the ecosystem tooling updates. Finally, the sandmark
and current-bench project tasks are mentioned for your reference.

## Multicore OCaml

### Open

#### Asynchronous

* [ocaml/ocaml#11057](https://github.com/ocaml/ocaml/pull/11057)
  Implement quality treatment for asynchronous actions in multicore

  Part 2 out of N of this PR has been merged. Part 3 out of N is
  currently proposed for review.

* [ocaml/ocaml#11307](https://github.com/ocaml/ocaml/pull/11307)
  Implement quality treatment for asynchronous actions in multicore (3/3?)

  The PR includes various fixes for soundness and performance of async
  actions in multicore.

#### Build

* [ocaml/ocaml#6728](https://github.com/ocaml/ocaml/issues/6728)
  Enable `-g` and `OCAMLRUNPARAM=b` by default

  A suggestion to couple OCaml's `-g` with the C compiler one and have
  them both enabled by default.

* [ocaml/ocaml#10431](https://github.com/ocaml/ocaml/issues/10431)
  Configure should require `ar` explicitly

  A request to explicitly check for `ar` as a dependency in the
  compiler build process.

* [ocaml/ocaml#11182](https://github.com/ocaml/ocaml/issues/11182)
  Bootstrap problem when moving `Cont_tag` to be after `No_scan_tag`

  The request for reallocation of tag numbers is postponed, and the
  issue has been removed from the 5.0 milestone.

* [ocaml/ocaml#11268](https://github.com/ocaml/ocaml/pull/11268)
  Merge utils/Makefile into the root Makefile

  The `ocamltest/ocamltest_config.ml` and `utils/config_fragment.ml`
  are now generated as part of the configure stage rather than during
  the build process.

* [ocaml/ocaml#11282](https://github.com/ocaml/ocaml/pull/11282)
  Set `-g`, record backtraces by default

  A PR to enable `-g` and record backtraces by default.

* [ocaml/ocaml#11292](https://github.com/ocaml/ocaml/issues/11292)
  On Ubuntu, configure fails with `flexlink does not work`

  A configure error or OCaml 4.13+ on Ubuntu 21.04 and 22.04 with a
  `flexlink does not work` message.

#### Bytecode

* [ocaml/ocaml#10429](https://github.com/ocaml/ocaml/issues/10429)
  ocamloptp shouldn't be compiled in bytecode-only mode

  A bug where `tools/ocamloptp` is compiled even if the tree is
  configured with `--disable-native-compiler`.

* [ocaml/ocaml#11065](https://github.com/ocaml/ocaml/pull/11065)
  Restore basic functionality to the bytecode debugger

  The review changes for restoring the basic functionality of the
  bytecode debugger have been completed, and the test suite results
  are awaited.

#### Discussion

* [ocaml/ocaml#10960](https://github.com/ocaml/ocaml/issues/10960)
  Audit `stdlib` for mutable state

  The mutable state for the `Stdlib` module is actively being reviewed
  in this issue tracker. A race condition on Windows has been detected
  for concurrent calls to `Unix.(in_out)_channel_of_descr` for two
  accesses of `CRT_fd_val(handle)`.

* [ocaml/ocaml#11013](https://github.com/ocaml/ocaml/issues/11013)
  Meta-issue for OCaml 5.0 release goals

  An issue tracker with a checklist for branching OCaml 5.0.0. Both
  flambda and non-flambda optimisations and safety are actively being
  discussed in this issue.

* [ocaml/ocaml#11138](https://github.com/ocaml/ocaml/pull/11138)
  Add C function `caml_thread_has_lock`

  An alternate implementation is proposed to make `Caml_state` NULL
  while the domain lock is not held.

* [ocaml/ocaml#11287](https://github.com/ocaml/ocaml/issues/11287)
  There is no implementation `caml_alloc_shr_with_profinfo` with WITH_PROFINFO

  A query on missing `caml_alloc_shr_with_profinfo` in
  `runtime/caml/memory.h` in trunk.

#### Documentation

* [ocaml/ocaml#10992](https://github.com/ocaml/ocaml/issues/10992)
  OCaml multicore memory model and C (runtime, FFI, VM)

  A proposal document on the OCaml Multicore memory model and the use
  of the `Field` macro. The acquire semantics for the `Load_field`
  macro is actively being discussed.

* [ocaml/ocaml#11280](https://github.com/ocaml/ocaml/pull/11280)
  Manual chapters on parallelism and memory model

  A PR that adds a manual chapter on parallelism features in OCaml.

* [ocaml/ocaml#11281](https://github.com/ocaml/ocaml/issues/11281)
  Track pending comments to the effects manual chapter

  An issue tracker for the review comments for the effects manual PR.

* [ocaml/ocaml#11290](https://github.com/ocaml/ocaml/pull/11290)
  Add more information to BOOTSTRAP.adoc

  The documentation about the bootstrap process on changes to
  primitives and on when exactly to commit for upstreaming have been
  included in BOOTSTRAP.adoc.

#### Enhancement

* [ocaml/ocaml#9504](https://github.com/ocaml/ocaml/issues/9504)
  Development request: integrate the `-short-paths` implementation of Merlin

  The recommendation is to have the right behaviour implemented in
  Merlin to solve a key usability issue, and to provide better types
  in error messages.

* [ocaml/ocaml#11163](https://github.com/ocaml/ocaml/pull/11163)
  Always serialize doubles in little-endian

  The reimplementation of `Reverse` macros from `<caml/reverse.h>` is
  being evaluated for performance. This issue is removed from the 5.0
  milestone since the bootstraps are reproducible on little-endian
  machines only.

* [ocaml/ocaml#11174](https://github.com/ocaml/ocaml/pull/11174)
  Experimental id-based pthread names for domain, tick, backup threads

  An ongoing discussion on how to best annotate domains for user
  programs to facilitate debugging and auditing.

* [ocaml/ocaml#11252](https://github.com/ocaml/ocaml/pull/11252)
  Support 'raw identifier' syntax

  The support for `\#foo` raw identifiers from RFC 27 has been added.

* [ocaml/ocaml#11255](https://github.com/ocaml/ocaml/pull/11255)
  Make Field macro a volatile cast

  The PR makes the `Field` macro a volatile cast, and introduces the
  `Load_field` macro.

* [ocaml/ocaml#11271](https://github.com/ocaml/ocaml/pull/11271)
  Simplifications and fixes to multicore systhreads implementation

  A number of fixes, simplifications to the multicore systhreads
  implementation, and removal of dead code.

* [ocaml/ocaml#11272](https://github.com/ocaml/ocaml/pull/11272)
  Make `Caml_state` `NULL` while the domain lock is not held

  The `Caml_state` is set to `NULL` inside of blocking sections with
  the added benefit that unprotected access to `Caml_state` is easier
  to debug.

* [ocaml/ocaml#11284](https://github.com/ocaml/ocaml/pull/11284)
  Re-enabling mark stack pruning

  The PR re-enables the mark stack pruning when the mark stack reaches
  more than 1/32th of the major heap.

#### CI

* [ocaml/ocaml#10980](https://github.com/ocaml/ocaml/issues/10980)
  GitHub Actions / ocamltest / testsuite / OCaml 5

  An issue tracker for using GitHub Actions for OCaml 5. The latest
  request is to include test builds for ARM 32-bit.

* [ocaml/ocaml#11294](https://github.com/ocaml/ocaml/pull/11294)
  Switch required autoconf to 2.71

  The GitHub Actions workflow base OS has been updated to use Ubuntu
  22.04, and to use autoconf-2.71.

#### Safety

* [ocaml/ocaml#11185](https://github.com/ocaml/ocaml/issues/11185)
  Embed the unhandled effect in the exception Unhandled

  A request to use OCaml syntax to embed the unhandled effect in the
  exception `Unhandled`.

* [ocaml/ocaml#11193](https://github.com/ocaml/ocaml/pull/11193)
  Concurrency safety documentation, part one: concurrency unsafe types

  A warning is added on the concurrency safety in the documentation of
  the following modules: Buffer, Ephemeron, Oo, Hashtbl, Queue, Stack,
  Scanf, and Weak. The PR is actively being reviewed.

* [ocaml/ocaml#11237](https://github.com/ocaml/ocaml/pull/11237)
  stdlib concurrency-safety documentation, part three: arrays

  A work-in-progress for documenting concurrency-safety for four
  mutable arrays in the standard library: arrays, byte sequences,
  bigarrays and Float.Array.t.

* [ocaml/ocaml#11279](https://github.com/ocaml/ocaml/issues/11279)
  Parallel access to Buffer can trigger segfaults

  The buffers should not be accessed in parallel without being
  guarded by a lock, and an issue has been reported where parallel
  access to a buffer triggers a segmentation fault.

* [ocaml/ocaml#11302](https://github.com/ocaml/ocaml/issues/11302)
  ocamlc overwrites /dev/null

  An observation with ocamlc when executed as root converts
  `/dev/null` as a regular file.

  ```
  # echo 'print_endline "Hello, world!"' >hello.ml
  # ocamlc -o /dev/null hello.ml
  ```
* [ocaml/ocaml#11303](https://github.com/ocaml/ocaml/pull/11303)
  Ensure that GC is not invoked from bounds check failures

  A bug in how array/string/bigarray bounds check failures can cause
  corruption or segfaults in programs that catch exceptions. The
  C-specific preprocessing is skipped when raising a bounds check
  failure in `caml_array_bound_error`.

#### Sundries

* [ocaml/ocaml#10926](https://github.com/ocaml/ocaml/pull/10926)
  Ensure C global symbols are consistently prefixed

  The addition of compatibility macros in `socketaddr.h` based on
  opam-health-check results and rebase of the PR are required.

* [ocaml/ocaml#11007](https://github.com/ocaml/ocaml/pull/11007)
  Ship and install META files

  The PR is actively being reviewed to ship and install META files for
  compiler libraries.

* [ocaml/ocaml#11137](https://github.com/ocaml/ocaml/pull/11137)
  Introduce `Store_tag_val(dst, val)` and use it in the runtime

  The runtime code does not depend any more on the fact that the
  `Tag_val` is an lvalue.

* [ocaml/ocaml#11144](https://github.com/ocaml/ocaml/pull/11144)
  Restore frame-pointers support for amd64

  A request to thorougly review the frame pointer support PR for
  AMD64. The perf analysis and flame graph tools are dependent on the
  frame pointer.

* [ocaml/ocaml#11183](https://github.com/ocaml/ocaml/pull/11183)
  Refactor GitHub actions

  The current GitHub Actions workflow has been cleaned up, and the
  `full-lambda` and `other-checks` features and test scenarios have
  been re-introduced. The undefined Makefile variables need to be
  addressed as well.

* [ocaml/ocaml#11225](https://github.com/ocaml/ocaml/pull/11225)
  stdlib audit: use DLS for parsing global state

  The Parsing work state has been made a domain local state.

* [ocaml/ocaml#11296](https://github.com/ocaml/ocaml/issues/11296)
  New syntax version

  A request to use `syntax v2` instead of the `-safe-syntax` option.

* [ocaml/ocaml#11309](https://github.com/ocaml/ocaml/pull/11309)
  Add a function to request a recommended max number of Domains

  A suggestion to add `Domain.recommended_domains ()` to return the
  maximum number of parallel units of execution. We are interested in
  the number of logical CPUs in the system.

#### Windows

* [ocaml/ocaml#9605](https://github.com/ocaml/ocaml/issues/9605)
  Compiling OCaml under the Windows Subsystem for Linux.

  An attempt to compile OCaml 4.10.0 and OCaml 4.11 under Windows
  Subsystem for Linux, using MSVC64 2019 and mingw-64.

* [ocaml/ocaml#11304](https://github.com/ocaml/ocaml/pull/11304)
  Fix data race on Windows file descriptors

  The `win_CRT_fd_of_filedescr` function can cause races on the
  `crt_fd` field of the `filedescr` structure, and this PR removes the
  data race by making the field atomic.

### Completed

#### Build

* [ocaml/ocaml#11149](https://github.com/ocaml/ocaml/pull/11149)
  Make the bootstrap process repeatable

  The `make bootstrap` step is now made repeatable to produce the exact
  same images in `boot/ocamlc` and `boot/ocamllex`, independent of OS
  and architecture.

* [ocaml/ocaml#11180](https://github.com/ocaml/ocaml/pull/11180)
  Fix "inconsistent assumptions over interface Dynlink_compilerlibs" when working on the compiler

  A multi-target pattern rule has been added to
  `otherlibs/dynlink/Makefile` to correctly produce both the `cmo` and
  `cmi` files for Dynlink_compilerlibs.

* [ocaml/ocaml#11231](https://github.com/ocaml/ocaml/pull/11231)
  Fix dune stanza for stdlib: change of name

  `camlinternalAtomic` has been removed, and `stdlib/dune` stanza has
  been updated accordingly.

* [ocaml/ocaml#11253](https://github.com/ocaml/ocaml/pull/11253)
  Start the release of the `ocaml` command for other purposes

  The `ocamlnat` script and `ocaml script` usage is deprecated where
  `script` is used as an implicit basename without any extension.

* [ocaml/ocaml#11267](https://github.com/ocaml/ocaml/pull/11267)
  Guard additional uses of `_MSC_VER` to suppress undefined variable warnings

  A check on whether `_MSC_VER` has been defined has been added in the
  PR to ensure that the headers can always be used in code.

* [ocaml/ocaml#11275](https://github.com/ocaml/ocaml/issues/11275)
  ocamlopt does not recognize `-cmi-file`

  The `driver/main_args.ml` file has been updated for ocamlopt to
  recognize `-cmi-file` option.

* [ocaml/ocaml#11310](https://github.com/ocaml/ocaml/pull/11310)
  Upgrade version in ocaml-variants.opam

  The `ocaml-variants.opam` file has ben updated to 5.1.0.

#### Configure

* [ocaml/ocaml#10413](https://github.com/ocaml/ocaml/pull/10413)
  Clarify and fix construction of MKEXE in configure

  The configure.ac has been updated for MKEXE to build an executable
  using the C compiler or using a wrapper for a linker.

* [ocaml/ocaml#10940](https://github.com/ocaml/ocaml/issues/10940)
  `configure`: C11 atomic support required for 5.00.0

  A newer version of GCC can be used to build and test with C11 atomic
  support for OCaml 5.0.0 on RHEL 7 systems. The `configure` script
  has also been updated accordingly.

* [ocaml/ocaml#11259](https://github.com/ocaml/ocaml/pull/11259)
  Allow `configure` to generate OCaml quoted strings

  A `OCAML_QUOTED_STRING_ID` is defined in `aclocal.m4` to compute a
  suitable ID to be inserted in quoted strings.

* [ocaml/ocaml#11260](https://github.com/ocaml/ocaml/pull/11260)
  configure: fix Windows build when flexlink is not bootstrapped

  A typo in the name of variables with the correct use of
  `bootstrapping_flexdll` that fixes Windows builds.

* [ocaml/ocaml#11295](https://github.com/ocaml/ocaml/pull/11295)
  configure: check support for C11 _Atomic types and operations

  The check for ISO C 2011 compliant compiler including full support
  for atomic types has been added to `aclocal.m4` and `configure.ac`.

#### Documentation

* [ocaml/ocaml#11093](https://github.com/ocaml/ocaml/pull/11093)
  Effects manual

  A tutorial on Effect Handlers has been merged to trunk.

* [ocaml/ocaml#11171](https://github.com/ocaml/ocaml/pull/11171)
  Fix or document concurrency issues on channels in Multicore

  The thread-unsafety and IO functions have been documented along with
  fixes to few channel-related concurrency issues.

#### Domain

* [ocaml/ocaml#11154](https://github.com/ocaml/ocaml/issues/11154)
  Add `Domain.get_name`

  The `Domain.set_name` primitive has been removed, which resolves
  this issue.

* [ocaml/ocaml#11213](https://github.com/ocaml/ocaml/pull/11213)
  Refine `Domain.{at_exit,at_startup,at_first_spawn,at_each_spawn}` callback semantics

  `Domain.at_startup` has been renamed to
  `Domain.at_each_spawn`. `Domain.at_exit` is now
  domain-local. `Domain.at_first_spawn` and `Domain.at_each_spawn`
  each run the callbacks in FIFO order.

#### Enhancement

* [ocaml/ocaml#10832](https://github.com/ocaml/ocaml/issues/10832)
  Synchronization of `refcount` fields in `bigarray.h`/`io.h`

  The PR that implements naive refcounting functions for bigarray
  proxy has been merged.

* [ocaml/ocaml#10934](https://github.com/ocaml/ocaml/issues/10934)
  Better error messages when running OCaml (script mode) on wrong file types

  The `ocaml script` and `ocamlnat` script usage has been deprecated,
  where `script` has no extension and is an implicit basename.

* [ocaml/ocaml#11189](https://github.com/ocaml/ocaml/pull/11189)
  runtime: scan all native roots

  The `runtime/roots.c` now scans all native global roots without
  relying on the global mutable state `caml_globals_inited`.

* [ocaml/ocaml#11200](https://github.com/ocaml/ocaml/pull/11200)
  [3/3] Restructure LIBDIR: Move the Profiling module to a sub-directory

  The `profiling.*` from `$LIBDIR` has been moved to
  `$LIBDIR/profiling`, and the use of `Profiling` within `ocamlprof`
  has been hardened.

* [ocaml/ocaml#11209](https://github.com/ocaml/ocaml/pull/11209)
  Make `caml_domain_stop_hook` a public timing hook

  The `caml_domain_stop_hook` has been made public and as a
  thread-safe hook.

* [ocaml/ocaml#11250](https://github.com/ocaml/ocaml/pull/11250)
  Simplify systhreads thread-local state management

  A global key to access the per-thread `caml_thread_t` is used
  instead of a key per domain.

* [ocaml/ocaml#11273](https://github.com/ocaml/ocaml/pull/11273)
  Add a unique identifier for fibers

  A unique `int64_t` is added to `struct stack_info` to identify a
  fiber globally.

* [ocaml/ocaml#11298](https://github.com/ocaml/ocaml/pull/11298)
  Add `?auto_include` to `Toploop.set_paths`

  An easier API has been provided to disable the automatic inclusion
  mechanism to aid alternate toplevels such as utop.

#### Fix

* [ocaml/ocaml#11218](https://github.com/ocaml/ocaml/pull/11218)
  Fix source-code references in the backend files

  The inconsistent and wrong error message locations in the various
  backend files have been fixed.

* [ocaml/ocaml#11220](https://github.com/ocaml/ocaml/pull/11220)
  Remove extraneous tabs from emitters

  The unnecessary tabs have been converted to spaces, and
  `tools/cvt_emit` has been updated to complain about uses of tabs
  outside `"..."` and ``...`` blocks.

* [ocaml/ocaml#11226](https://github.com/ocaml/ocaml/issues/11226)
  Segfault on MacOSX with trunk

  A segmentation fault that occurs in `caml_c_call` on Mac OS X with
  OCaml trunk has been fixed.

* [ocaml/ocaml#11244](https://github.com/ocaml/ocaml/pull/11244)
  hygiene fix in `weaklifetime_par.ml`

  An errant whitespace at the end of line in `weaklifetime_par.ml` has
  been fixed, and the CI now passes.

* [ocaml/ocaml#11299](https://github.com/ocaml/ocaml/pull/11299)
  Runtime_events fix for compilation on omnios/illumos

  A cast has been added for compatibility as mmap/munmap are non-POSIX
  on Illumos.

#### Makefile

* [ocaml/ocaml#11234](https://github.com/ocaml/ocaml/pull/11234)
  Build system: use C preprocessor flags more consistently

  The PR ensures that C files are compiled with `-DCAML_INTERNALS` and
  `-I$(ROOTDIR)/runtime` C preprocessor flags.

* [ocaml/ocaml#11240](https://github.com/ocaml/ocaml/pull/11240)
  Simplify runtime/Makefile

  The assembler files to use are computed at configure rather than
  build time, and the `ASMFLAGS` and `EXTRALIBS` build variables have
  been removed.

* [ocaml/ocaml#11243](https://github.com/ocaml/ocaml/pull/11243)
  Merge `yacc/Makefile` into the root Makefile

  The `yacc/Makefile` has been moved to the top-level Makefile. This
  is a first step to merge all the sub-makefiles into the root one for
  parallel and incremental builds.

* [ocaml/ocaml#11248](https://github.com/ocaml/ocaml/pull/11248)
  Merge runtime/Makefile in the root Makefile

  The dependencies where the C files are stored have been moved from
  `runtime/.dep` to `.dep/runtime`.

* [ocaml/ocaml#11256](https://github.com/ocaml/ocaml/pull/11256)
  Eliminate `$(BOOT_FLEXLINK_CMD)`

  The `BOOT_FLEXLINK_CMD` has been removed with simplification to the
  root Makefile.

* [ocaml/ocaml#11257](https://github.com/ocaml/ocaml/pull/11257)
  Fixes in makefiles

  The debugging information when compiling C flags in
  `ocamltest/Makefile` are not necessary and have been removed, along
  with a warning fix in the top-level Makefile.

* [ocaml/ocaml#11277](https://github.com/ocaml/ocaml/pull/11277)
  Ensure `Makefile.build_config` is only loaded once

  The `BUILD_CONFIG_INCLUDED` variable has been included to prevent
  double-inclusion of `Makefile.build_config`.

#### Refactor

* [ocaml/ocaml#11176](https://github.com/ocaml/ocaml/issues/11176)
  Weird semantics for `exit` on multi-domain programs

  The `Domain.{at_exit, at_startup, at_first_spawn, at_each_spawn}`
  callback semantics have been refined.

* [ocaml/ocaml#11178](https://github.com/ocaml/ocaml/issues/11178)
  Reverse the execution order of functions registered via `Domain.{at_first_spawn, at_startup}`?

  The order imposed by `Domain.at_exit` and `Stdlib.at_exit` may not
  work for the current execution of functions registered with
  `Domain.{at_first_spawn, at_startup}`, and hence the callback
  semantics have been refined.

* [ocaml/ocaml#11198](https://github.com/ocaml/ocaml/pull/11198)
  [1/3] Restructure `LIBDIR`: Move Dynlink, Str and Unix to sub-directories

  The Dynlink, Str and Unix libraries have been moved into
  sub-directories of `$LIBDIR`.

* [ocaml/ocaml#11199](https://github.com/ocaml/ocaml/pull/11199)
  [2/3] Restructure LIBDIR: Remove duplicate topdirs.cmi

  The toplevel now reads `topdirs.cmi` from `+compiler-libs` and stops
  installing `topdirs.cmi` twice.

* [ocaml/ocaml#11219](https://github.com/ocaml/ocaml/pull/11219)
  Tidy-up the sigmask code in domain.c

  An undefined behaviour has been reported in the PR. The use of
  `ifdefs` is preferred over a macro.

* [ocaml/ocaml#11245](https://github.com/ocaml/ocaml/pull/11245)
  Unify code of ocamlcp and ocamloptp

  The common code from `tools/ocamlcp.ml` and `tools/ocamloptp.ml`
  have been factored out into a single module.

#### Runtime

* [ocaml/ocaml#11264](https://github.com/ocaml/ocaml/pull/11264)
  Remove `Domain.set_name` primitive

  The runtime no longer assigns POSIX thread names like `Domain`,
  `Backup`, and `Tick`, and the `Domain.set_name` primitive has been
  removed for 5.0.

* [ocaml/ocaml#11274](https://github.com/ocaml/ocaml/pull/11274)
  Protect `Max_domains` with `CAML_INTERNALS`

  A `runtime/caml/domain.h` has been added that defines the present
  hard limit on the number of domains.

* [ocaml/ocaml#11305](https://github.com/ocaml/ocaml/pull/11305)
  Make runtime_events test more robust

  The PR has changed the major slices to major cycles for the
  runtime_events tests to guarantee three major cycles per
  `Gc.full_major`.

* [ocaml/ocaml#11308](https://github.com/ocaml/ocaml/pull/11308)
  Add an environment variable to preserve runtime events after exit

  A `OCAML_RUNTIME_EVENTS_PRESERVE` environment variable has been
  added to not remove the runtime events ring buffers at exit. This is
  useful for tracing of short running programs in Sandmark.

#### Sundries

* [ocaml/ocaml#10996](https://github.com/ocaml/ocaml/issues/10996)
  New functions in Gc miss a @since

  The missing @since annotation in `Gc.eventlog_pause` has been added.

* [ocaml/ocaml#10964](https://github.com/ocaml/ocaml/pull/10964)
  Ring-buffer based runtime tracing (eventring)

  The continuous monitoring of OCaml applications is now possible
  using Eventring. It has a very low overheard, runtime tracing
  implementation that replaces the file-logging based eventlog.

* [ocaml/ocaml#11190](https://github.com/ocaml/ocaml/pull/11190)
  Implement quality treatment for asynchronous actions in multicore (2/N)

  The polling behaviour in C code has been restored by delaying
  finalisers until the next suitable safe point. The possibility of
  race conditions has also been removed.

* [ocaml/ocaml#11212](https://github.com/ocaml/ocaml/pull/11212)
  Add missing @since annotation to Gc.eventlog_resume / Fix annotation @since to Effect to 5.0.0 rather than 5.00.0

  The @since annotation in `stdlib/effect.mli` and `stdlib/gc.mli`
  have been updated to 5.0.

* [ocaml/ocaml#11217](https://github.com/ocaml/ocaml/pull/11217)
  Limit bit-rot in the disabled backends

  The `make check_all_arches` has been restored to limit the bit-rot
  for the presently disabled native code backends.

* [ocaml/ocaml#11236](https://github.com/ocaml/ocaml/pull/11236)
  Add missing @since annotation to Gc.eventlog_pause

  A `@since 4.11` annotation has been added to `stdlib/gc.mli`.

* [ocaml/ocaml#11283](https://github.com/ocaml/ocaml/issues/11283)
  Options in `compilerlib.Toploop` to disable autoload alerts

  The support for new locations of unix, str and dynlink PR will add
  the required directory to disable new alerts from the compiler-lib
  API in utop.

#### Testing

* [ocaml/ocaml#11121](https://github.com/ocaml/ocaml/issues/11121)
  `weaklifetime_par.ml` gets killed by OS's OOM-reaper on Raspberry Pi 4

  The `weaklifetime_par.ml` test has been causing too many CI
  problems, and thus has been disabled.

* [ocaml/ocaml#11241](https://github.com/ocaml/ocaml/pull/11241)
  `ocamltest/run_win32.c`: include `caml/memory.h` rather than defining CAML_INTERNALS

  The `ocamltest/run_win32.c` has been included with `caml/memory.h`.

## Ecosystem

### retro-httpaf-bench

#### Open

#### Closed

* [ocaml-multicore/retro-httpaf-bench#24](https://github.com/ocaml-multicore/retro-httpaf-bench/pull/24)
  Replace shuttle with http-async

  The shuttle implementation has been switched to use `http_async`.

* [ocaml-multicore/retro-httpaf-bench#25](https://github.com/ocaml-multicore/retro-httpaf-bench/issues/25)
  Docker file fails to build: Unbound value `Input_channel.read_one_chunk_at_a_time`

  The breaking change issue is closed with a patch to use `http_async`
  in shuttle.

* [ocaml-multicore/retro-httpaf-bench#26](https://github.com/ocaml-multicore/retro-httpaf-bench/pull/26)
  Fix submodule URL

  The submodule `cohttp-eio/ocaml-cohttp` URL has been fixed, and we
  only require to install `cohttp-lwt-unix` from the `cohttp` package.

### eio

#### Open

* [ocaml-multicore/eio#210](https://github.com/ocaml-multicore/eio/pull/210)
  Add condition variables and concurrent-mutexes

  The PR adds Eio equivalents, `Condition` and `Eio_mutex` to
  `lib_eio` to facilitate the transition from `lwt` to `eio`.

* [ocaml-multicore/eio#214](https://github.com/ocaml-multicore/eio/issues/214)
  `Eio.Flow.read` hangs on closed flow

  The use of `Eio.Flow.shutdown` solves the issue and the server will
  read EOF, display the message from the client, and will close its
  own connection. It has been suggested that the TLS wrapper needs to
  implement `Flow.two_way` and also provide a `shutdown` operation.

* [ocaml-multicore/eio#216](https://github.com/ocaml-multicore/eio/issues/216)
  Ctf tracing: extend the `Fiber` API to allow labelling fibers

  A request to add an optional `label` argument for the Fiber module.

* [ocaml-multicore/eio#221](https://github.com/ocaml-multicore/eio/pull/221)
  `eio_linux`: add support for integration with Async

  The `lib_eio/unix/eio_unix.ml` has been updated to add integration
  support with Async.

#### Completed

##### Documentation

* [ocaml-multicore/eio#208](https://github.com/ocaml-multicore/eio/pull/208)
  Update the README to use OCaml 5.0

  The installation instructions have been updated to use 5.0.0+trunk
  for ARM.

* [ocaml-multicore/eio#215](https://github.com/ocaml-multicore/eio/pull/215)
  Update README.md

  The trunk has been updated to `5.0.0+trunk`, and the UDP interface
  also works with the Eio networking interface.

* [ocaml-multicore/eio#217](https://github.com/ocaml-multicore/eio/pull/217)
  Improve the installation instructions

  The README.md has been updated with instructions on how to install
  5.0+trunk for ARM, and with a stable release from opam-repository.

* [ocaml-multicore/eio#220](https://github.com/ocaml-multicore/eio/pull/220)
  Lwt_eio has moved

  The `Lwt_eio` URL has been updated in the README.

##### Enhancement

* [ocaml-multicore/eio#206](https://github.com/ocaml-multicore/eio/issues/206)
  API request: readdir

  A `readdir` feature has been implemented for Eio.

* [ocaml-multicore/eio#207](https://github.com/ocaml-multicore/eio/pull/207)
  Add readdir feature

  A new `read_dir` feature has been implemented in `lib_eio/dir.ml`.

* [ocaml-multicore/eio#219](https://github.com/ocaml-multicore/eio/pull/219)
  `eio_linux`: make `read_dir` faster

  The directory reads have been improved by using a larger buffer, and
  by performing all the reads in a single systhread.

##### Sundries

* [ocaml-multicore/eio#190](https://github.com/ocaml-multicore/eio/pull/190)
  Update to cmdliner.1.1.0

  The `cmdliner` dependency has been updated to use version 1.1.0.

* [ocaml-multicore/eio#212](https://github.com/ocaml-multicore/eio/pull/212)
  Handle EINTR error when calling `getrandom` system call

  The `getrandom` call via `eio_linux` or `eio_lux` packages is now
  handled with EINTR error return code.

* [ocaml-multicore/eio#213](https://github.com/ocaml-multicore/eio/pull/213)
  `Eio_linux`: handle sleeping tasks that are now due before paused fibers

  The scheduler now prioritizes `paused fibers` before overdue
  `sleeping fibres`.

* [ocaml-multicore/eio#218](https://github.com/ocaml-multicore/eio/pull/218)
  `Eio_linux`: fix `read_dir`

  The `read_dir` has been fixed and optimised to read from a directory
  and not just from `env#cwd`. The read improvement for the following
  code is shown below:

  ```
  open Eio.Std

  let () =
    Eio_main.run @@ fun env ->
    let t0 = Unix.gettimeofday () in
    let items = Eio.Dir.read_dir env#fs "/nix/store" in
    let t1 = Unix.gettimeofday () in
    traceln "Read %d items in %f seconds" (List.length items) (t1 -. t0)
  ```

  ```
  Read 152441 items in 34.181577 seconds with the old code.

  Read 152441 items in 1.181868 seconds with the new code.

  ```

* [ocaml-multicore/eio#222](https://github.com/ocaml-multicore/eio/pull/222)
  Check for cancellation when creating a non-protected child context

  A cancellation is checked while creating a non-protected child
  context to avoid having a cancelled parent context with a
  non-cancelled and non-protected child.

### domainslib

#### Open

* [ocaml-multicore/domainslib#14](https://github.com/ocaml-multicore/domainslib/issues/14)
  Interface to query CPU setup, numa information and set affinity

  A request to add an interface to query CPU setup, NUMA information
  and set affinity. An
  [ocaml-cpu](https://github.com/haesbaert/ocaml-cpu) library is
  actively being developed to build the CPU topology and affinity for
  OCaml for Linux and BSD.

* [ocaml-multicore/domainslib#71](https://github.com/ocaml-multicore/domainslib/issues/71)
  Domainslib description on OPAM

  The opam description for `domainslib` should be renamed to
  `Nested-parallel programming library`.

* [ocaml-multicore/domainslib#72](https://github.com/ocaml-multicore/domainslib/issues/72)
  Possible data races in Chan

  A couple of data races in `lib/chan.ml` when running Domainslib test
  suite that were reported by ThreadSanitizer.

* [ocaml-multicore/domainslib#74](https://github.com/ocaml-multicore/domainslib/issues/74)
  Possible data races in `Task.parallel_scan`

  A `ThreadSanitizer` data race warning reported when pinning
  domainslib to commit `b8de1f71`.

#### Completed

* [ocaml-multicore/domainslib#73](https://github.com/ocaml-multicore/domainslib/pull/73)
  Add alpha repo to GitHub Actions

  The -alpha repository has been added to the GitHub Actions workflow
  to use the latest `dune` and `ocamlfind`.

### effects-examples

#### Closed

* [ocaml-multicore/effects-examples#29](https://github.com/ocaml-multicore/effects-examples/pull/29)
  Add installation instructions for older opam

  The instructions to build the effects-examples when using OPAM less
  than 2.1 have been included.

## lockfree

### Open

* [ocaml-multicore/lockfree#13](https://github.com/ocaml-multicore/lockfree/pull/13)
  Tests for `ws_deque` using QCheck

  The tests for the following different domain configuration have been
  added for the `ws_deque` data structure:

  * A producer alone
  * A producer and a single stealer
  * A producer and two stealers

### Completed

* [ocaml-multicore/lockfree#12](https://github.com/ocaml-multicore/lockfree/pull/12)
  Stronger tests, and a couple optimizations in circular arrays.

  A strong sequential and concurrent tests have been added along with
  optimization to circular arrays.

## Benchmarking

### Sandmark

#### Open

##### Benchmarks

* [ocaml-bench/sandmark#341](https://github.com/ocaml-bench/sandmark/pull/341)
  Add owl

  The inclusion of a test Owl benchmark in Sandmark.

* [ocaml-bench/sandmark#343](https://github.com/ocaml-bench/sandmark/pull/343)
  Irmin replay benchmark

  The inclusion of an Irmin replay benchmark that uses Irmin.3.2.1
  with the latest trunk in Sandmark.

* [ocaml-bench/sandmark#348](https://github.com/ocaml-bench/sandmark/issues/348)
  Number of benchmarks are misclassified as macro benchmarks

  A list of macrobenchmarks need to be updated as some of them finish
  in 10 ms. The benchmark configuration files need to be updated
  accordingly.

* [ocaml-bench/sandmark#356](https://github.com/ocaml-bench/sandmark/issues/356)
  Update mergesort with a faster implementation

  A simple version of mergesort is available from
  [kayceesrk/ocaml5-tutorial#4](https://github.com/kayceesrk/ocaml5-tutorial/pull/4)
  which can replace the existing Sandmark mergesort implementation.

##### Dependencies

* [ocaml-bench/sandmark#352](https://github.com/ocaml-bench/sandmark/pull/352)
  Add `js_of_ocaml` alpha

  An attempt to build `js_of_ocaml` with the latest trunk in Sandmark.

* [ocaml-bench/sandmark#357](https://github.com/ocaml-bench/sandmark/issues/357)
  Implement the solution for the Ubuntu/OpenBLAS problem in the GH Actions workflow

  The OpenBLAS library needs to be built manually in Ubuntu to be able
  to build OWL benchmarks.

* [ocaml-bench/sandmark#359](https://github.com/ocaml-bench/sandmark/issues/359)
  `benchmarks/yojson/ydump.ml` fails with Unknown module Stream for 5.1.0

  The `yojson` benchmark fails to build with the new OCaml 5.1.0+trunk
  development branch.

##### Documentation

* [ocaml-bench/sandmark#344](https://github.com/ocaml-bench/sandmark/issues/344)
  Update `Add dependencies` section in the README

  The README needs to be reviewed and the `Add dependencies` section
  needs to be updated with information on primarily using `dev.opam`
  for dependencies.

* [ocaml-bench/sandmark#347](https://github.com/ocaml-bench/sandmark/issues/347)
  Results aggregation broken

  The `SANDMARK_CUSTOM_NAME` needs to be documented in the README, and
  renamed to `VARIANT_NAME`.

##### Sundries

* [ocaml-bench/sandmark#338](https://github.com/ocaml-bench/sandmark/issues/338)
  Running Sandmark with `run_all_serial.sh` on Fedora within the
  docker container produces compatibility issues with opam

  The Sandmark build on Fedora needs to be checked for dependency and
  available list of packages.

* [ocaml-bench/sandmark#358](https://github.com/ocaml-bench/sandmark/issues/358)
  Revive instrumented pausetimes using eventring

  The pausetimes measurement needs to be re-introduced in Sandmark
  using the newly added support in OCaml 5 for eventring.

#### Completed

##### Benchmarks

* [ocaml-bench/sandmark#119](https://github.com/ocaml-bench/sandmark/issues/119)
  Additional benchmarks - hamming and soli

  The `hamming` and `soli` benchmarks have now been added to Sandmark.

* [ocaml-bench/sandmark#349](https://github.com/ocaml-bench/sandmark/pull/349)
  Add `nqueens.exe` to multibench_parallel

  The multicore-numerical benchmark dune file has been updated to
  include `nqueens.exe`.

##### CI

* [ocaml-bench/sandmark#275](https://github.com/ocaml-bench/sandmark/issues/275)
  Move from drone CI to github actions

  The `.drone.yml` file has been removed now that we have moved
  completely to GitHub Actions workflow.

* [ocaml-bench/sandmark#316](https://github.com/ocaml-bench/sandmark/pull/316)
  main.yml

  The CI for Sandmark has been updated with a main.yml to use the
  GitHub Actions workflow.

* [ocaml-bench/sandmark#345](https://github.com/ocaml-bench/sandmark/issues/345)
  Remove .drone.yml

  The GitHub Actions workflow is up and running, and hence the
  `.drone.yml` file has been removed.

* [ocaml-bench/sandmark#354](https://github.com/ocaml-bench/sandmark/pull/354)
  Removed drone CI. See #345

  The `.drone.yml` file has been removed from Sandmark, and the GitHub
  Actions workflow will be the primary CI for Sandmark.

##### Dependencies

* [ocaml-bench/sandmark#346](https://github.com/ocaml-bench/sandmark/issues/346)
  Dependencies should include `jo`

  The README has been updated to include `jo` as a dependency.

* [ocaml-bench/sandmark#355](https://github.com/ocaml-bench/sandmark/pull/355)
  Added jo as dependency. See #346

  The README has been updated to include `jo` as a dependency
  requirement in Sandmark.

##### Jupyter Notebooks

* [ocaml-bench/sandmark#259](https://github.com/ocaml-bench/sandmark/issues/259)
  Update sandmark notebooks with latest changes from sandmark-nightly

  The latest changes from sandmark-nightly have been backported to the
  notebooks in Sandmark.

* [ocaml-bench/sandmark#350](https://github.com/ocaml-bench/sandmark/issues/350)
  `Length of values does not match length of index` error in sequential Jupyter notebook

  A fix in the `normalise` function to handle more than two variants
  having a different number of benchmark results in the sequential
  Jupyter notebook.

* [ocaml-bench/sandmark#351](https://github.com/ocaml-bench/sandmark/pull/351)
  Fix normalise error when different number of benchmarks in baseline

  The `normalise` function has been updated to handle more than two
  variants, each having a different number of benchmarks.

##### Sundries

* [ocaml-bench/sandmark#266](https://github.com/ocaml-bench/sandmark/issues/266)
  Instrumented pausetimes for ocaml - 5.00.0+trunk and 4.14.0+domains

  The instrumented pausetimes will need to be redone using eventring
  for the latest trunk, and hence this issue has been closed.

* [ocaml-bench/sandmark#333](https://github.com/ocaml-bench/sandmark/pull/333)
  Add note about `--cpu-list` in README

  The information on `--cpu-list` will be added to HACKING.md instead
  of the README.

* [ocaml-bench/sandmark#353](https://github.com/ocaml-bench/sandmark/pull/353)
  Use -alpha dune.2.9.3 to build with May 30, 2022 trunk

  The `dune.2.9.3` from the -alpha repository is now used to build the
  latest trunk in Sandmark, along with `ocamlfind.1.9.3.git`.

### current-bench

#### Open

* [ocurrent/current-bench#359](https://github.com/ocurrent/current-bench/pull/359)
  Calculate execution time of functions

  The execution time of functions is now reported using the bechamel
  library and with the current time (`Sys.time`).

* [ocurrent/current-bench#360](https://github.com/ocurrent/current-bench/issues/360)
  Some benchmarks seem to run multiple times

  An issue where some benchmarks are executed multiple times, and
  their respective results are also stored in the database.

* [ocurrent/current-bench#361](https://github.com/ocurrent/current-bench/issues/361)
  Stale Open PRs get marked as closed

  A PR that is actually open and stale, with no activity for a couple
  of weeks, is incorrectly marked as closed.

* [ocurrent/current-bench#362](https://github.com/ocurrent/current-bench/pull/362)
  Pipeline: Don't mark stale PRs as closed PRs

  An update to `pipeline/lib/pipeline.ml` to not mark stale PRs as
  closed even if there is no activity in the last couple of weeks.

* [ocurrent/current-bench#358](https://github.com/ocurrent/current-bench/pull/358)
  WIP: Look for changed significantly metrics and notify authors

  A message is posted to authors when there is a significant change in
  observed metrics for their respective PRs.

#### Completed

* [ocurrent/current-bench#356](https://github.com/ocurrent/current-bench/pull/356)
  Record commit messages and display them on hover on commit IDs

  A feature to fetch the commit message from GitHub, and display them
  in the UI when hovering over commit IDs.

* [ocurrent/current-bench#357](https://github.com/ocurrent/current-bench/pull/357)
  Install `*-bench.opam` in priority

  The `libirmin.opam` installation is skipped as it disables ARM64
  when running the Irmin benchmarks on the Raspberry Pi 4. The
  installation of `irmin-bench.opam` is sufficient.

Our special thanks to all the OCaml users, developers and contributors
in the community for their valuable time and continued support to the
project.

## Acronyms

* AMD: Advanced Micro Devices
* API: Application Programming Interface
* ARM: Advanced RISC Machines
* BSD: Berkeley Software Distribution
* CI: Continuous Integration
* CPU: Central Processing Unit
* DLS: Domain Local Storage
* EOF: End Of File
* FIFO: First In First Out
* FFI: Foreign Function Interface
* GC: Garbage Collector
* GCC: GNU Compiler Collection
* IO: Input/Output
* ISO: International Organization for Standardization
* MSVC: Microsoft Visual C++
* NUMA: Non-uniform Memory Access
* OOM: Out of Memory
* OPAM: OCaml Package Manager
* OS: Operating System
* PR: Pull Request
* RFC: Request For Comments
* RHEL: Red Hat Enterprise Linux
* TLS: Transport Layer Security
* UDP: User Datagram Protocol
* UI: User Interface
* URL: Uniform Resource Locator
* VM: Virtual Machine
* WIP: Work In Progress


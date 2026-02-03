Implemented a path‑mapping layer in the inotify backend so event paths preserve the exact prefix passed to `watch()` (including relative paths), while still using absolute paths internally for correct inotify operations. This addresses issue 740 without breaking the existing internal behavior.

Details
- Stored both user and real paths per watch and build event paths from the user prefix, while add/remove logic continues to use real paths (`notify/src/inotify.rs`).
- Updated recursive watch handling to derive user/real pairs for each entry and to keep the original user prefix when a watch is re-added (`notify/src/inotify.rs`).
- Added a regression test for relative path preservation and documented the behavior in the inotify module docs (`notify/src/inotify.rs`).

Tests run
- `cargo check -p notify`
- `cargo test -p notify relative_watch_paths_are_preserved`

Next steps (optional)
1. Run the full `notify` crate test suite: `cargo test -p notify`
2. If you want to enforce this behavior across other backends, we can add similar path mapping for kqueue/windows.

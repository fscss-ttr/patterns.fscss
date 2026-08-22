## Contributing

This module is intentionally small. It grows through contribution, not a
fixed roadmap. If you want to add a pattern:

1. Write the CSS you want to reuse.
2. Write a plain-English description for it, phrased the way a developer
   would naturally describe the result, not the implementation.
3. Keep the description lexically distinct from existing ones. Two
   descriptions that share most of their words (for example, two button
   variants both saying "primary button with white text") make matching
   ambiguous. Give each pattern at least one or two words nothing else
   uses.
4. Add it to the appropriate section in `patterns.fscss`, following the
   existing `pattern(@use(thr): "description", "css")` format.
5. Open a pull request describing what the pattern produces.

Patterns that only make sense for one specific project are better kept in
that project's own module. This repo is for patterns broadly useful across
projects.

# plugin-ssh

The `plugin-ssh` plugin candy of the [opencharly/charly](https://github.com/opencharly/charly)
candy library, as a standalone repo (the candy de-submodule cutover, plugin
kind). The Go module lives at `candy/plugin-ssh/` with module path
`github.com/opencharly/plugin-ssh/candy/plugin-ssh`; the charly resolver fetches this repo at the pinned tag and
the compiled-in wiring imports the module at that path.

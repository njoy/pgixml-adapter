# pgixml-adapter
This is an adaptation of Arseny Kapoulkine's [pugixml](https://github.com/zeux/pugixml) for the NJOY21 build system.

```bash
# This only needs to be done once (per clone)
git remote add pugixml https://github.com/zeux/pugixml.git

# Do this when you need to update the subtree
git subtree pull --squash --prefix=src pugixml master
```

# License
The code contained in this directory is covered by the license contained in the [LICENSE](LICENSE) file. The code in the `src` directory is covered by it's own  license which is at the end of the [README file](src/README.md).


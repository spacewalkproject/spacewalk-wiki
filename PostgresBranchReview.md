[Main Project Page](PostgreSqlProject)
### Discussion Topics



The topics listed here are being discussed on the mailing list.  They are summarized here for tracking purposes only.

1. Don't want to see a big merge commit land in master.
    * Redo work and group commits into feature-based autonomous commits that can be cherry picked into master, reviewed, tested?
    * Merge and rely on _diffing_ installed schema(s) to verify semantic equivalence?

2. When _common_ table (.sql) files created from rhnsat/tables/ - reformatting makes diffs useless.
    * Redo work preserving as much as possible?
    * Merge and rely on _diffing_ installed schema(s) to verify semantic equivalence?

3. Consolidating the _upgrade_ .sql scripts into a single (ordered) .sql file for upgrades causes downstream packaging problems.

4. _Make_ vs. _Blend_, why not continue with current `Makefile.schema`?

5. Why use _chameleon_?
    * Why not templating?
    * Why not search-and-replace approach using something cobbled up with available technology such as _sed_, _awk_, etc ...?
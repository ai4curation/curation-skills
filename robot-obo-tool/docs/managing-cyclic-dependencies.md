## Cyclic Dependencies

Within the OBO Library, the logical dependency graph may not always be
acyclic. For example, GO makes use of CL to define "neuron
development". Conversely, CL will use GO to define "GABAergic neuron"
(in turn GO will define GABAergic secretion using CHEBI).

This is illustrated here:

![cyclic](https://raw.githubusercontent.com/ontodev/robot/master/images/cyclic-import-chain-1.png)

The arrows depict a logical dependency. These are typically implemented using `owl:imports`.

## Import Modules

One common practice is to make sub-modules using the OWLAPI Syntactic
Locality Module Extractor (SLME) or OntoFox. For example, CL imports
`cl/imports/uberon_import.owl` (note we assume the standard OBO
library URL prefix here). This is generated from Uberon. When
generating modules the following tensions are balanced:

 * The module should be in RDF/XML to maximize reuse in different tool chains
 * The serialized form should be compact, and ideally not generate spurious diffs. Typically axiom annotations are removed.
 * The module should be logically complete as reasoning use cases for the upstream dictates
 * The module should be annotation-assertion complete as curator requirements for source dictates
 * The module should be complete enough as not to confuse general users of the source ontology

These tensions are handled in different ways by different
ontologies. Often the modules are very minimal: labels and logical
axioms; disjointness and 'infectious' axioms removed in advance.

Note that in this case there are no cycles in the import chain (all
imports live within the purl space of the source). How ever, the
combined derivation plus import graph can still have cycles:

![cyclic](https://raw.githubusercontent.com/ontodev/robot/master/images/cyclic-import-chain-2.png)

This leads to *Autophagy*. If `uberon/imports/cl_import` is derived
from `cl.owl`, and `cl.owl` imports `cl/imports/uberon_import.owl`,
which is derived from uberon, then we have uberon eating a part of
itself. Furthermore, it is eating a *stale* copy of part of
itself. This can cause horrendous problems, especially where we have
rigorous constraints. For example, Uberon may have a disjoitness axiom
that gets violated by an earlier less perfect version of itself.

## Solution: Redirect to Null

The solution here is to redirect the import modules from external ontologies to a null ontology.

This is illustrated in the following scenario, where we have a
"mega-ontology" that wants to bring in various other ontologies, and
does not care to bring in the import modules, since these may be
autophagic dupes of the complete ontologies that are brought in:

![cyclic](https://raw.githubusercontent.com/ontodev/robot/master/images/cyclic-import-chain-3.png)

The cost here is maintenance on the part of the mega-ontology developers. For every import module in the chain, they need:

 * one line in a catalog-xml
 * one null ontology to redirect to (note separate nulls are required as each ontology must be named according to the ontology being faked)

### Issues

This is dependent on having a catalog, which typically depends on
having an ontology 'checked out' of a repo; or on some kind of distro
being made; e.g. owl/zip.

If the catalog is not present then the behavior defaults to autophagy

An alternate solution would be to `copy-and-rewire`. Documentation TBD.





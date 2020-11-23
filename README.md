# Bundle closure algorithm in [Dafny](https://dafny-lang.github.io/dafny/)

## Installing Dafny

- Download the release from: https://github.com/dafny-lang/dafny/releases

  Note: Currently, this formulation is based on [Dafny 3.0.0 pre-release 1](https://github.com/dafny-lang/dafny/releases/tag/v3.0.0-pre-release-1)

- Extract the release somewhere on your computer

## Install Mono (Linux, MacOSX)

- Download [Mono stable](https://www.mono-project.com/download/stable/)

  Note: Currently, this formulation is based on version 6.12.0.90

## Installing Visual Studio Code extension for Dafny

- See [Dafny extension in VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=correctnessLab.dafny-vscode)

- In VS Code, open `File | Preferences > Settings` and under
  `Extensions > Dafny extension configuration`, edit the settings:
  - `Dafny: Base Path`
  - `Dafny: Mono Executable`
  
## Goal

There is a Java implementation of the bundle closure algorithm here:
https://github.com/opencaesar/owl-tools/tree/master/owl-close-world

Can we specify this bundle closure algorithm in Dafny and extract from it a Java program?

Specifying this algorithm in Dafny involves verifying Dafny methods to compute the following:
- Testing whether a graph is acyclic or not.
  Raise an exception if the graph is cyclic.
- For an acyclic graph, test whether a node is a root of a tree or not.
- For an acyclic graph, calculate its transitive reduction per https://en.wikipedia.org/wiki/Transitive_reduction

## Related work.

Working with directed graphs in Dafny is similar to the "Automated Verification of Nested DFS".
See: https://link.springer.com/chapter/10.1007/978-3-319-19458-5_12
However, this work was done with Dafny 1.8.2.

Updating the formalization for Dafny 3.0.0 pre-release 1 was fairly easy except for this:

```dafny
  function KeyInvariant(G:set<Node>):bool
    reads G
    requires graph(G)
  { 
    forall s :: s in Blue(G) && s.accepting ==> 
      // TODO: How to fix this?
      // a quantifier involved in a function definition is not allowed to depend on 
      // the set of allocated references; Dafny's heuristics can't figure out a bound for the values of 'p'
      !exists p :: |p| > 1 && Path(G,s,s,p) 
  }

```





# Composite

## Learning Goals

- Explain the composite pattern
- Use the pattern in Java

## Introduction

The Composite design pattern allows us to set up a class structure so that
object A and compositions of object A can be treated in the same way. A typical
example of that would be in your file system on your computer, where folders and
files have many of the same operations and folders are made up of files and
folders.

Here are the highlights from the Composite pattern:

- Use to model tree-based object hierarchies where nodes and leaves can be
  treated the same
- A node is something that has other nodes or leaves
- A leaf is something that has its own properties but does not contain any other
  leaves or nodes
- Each node or leaf can be treated, and therefore modeled the same, except that
  nodes contain leaves
- Actions that can be applied to a leaf can also be applied to entire nodes, and
  when applying the action to an entire node, the action is delegated to each
  leaf on that node

This structure is similar to that of a tree in nature, where branches can lead
to other branches, but eventually every branch leads to one or more leaves,
which is where that particular branch ends.

Here is a visual representation of a composite hierarchy:

![composite-nodes.png](https://curriculum-content.s3.amazonaws.com/java-mod-3/composite/composite-nodes.png)

## Implementation

In general, a composite object structure will have a base interface that defines
the actions that both the nodes and the children will support. In the following
example, we will create a `CompositeInterface` interface that supports 2 sample
actions: `sampleAction()` and `anotherSampleAction()`

```java
interface CompositeInterface {
   void sampleAction(String param);
   void anotherSampleAction();
}
```

Then we will have 2 implementations of this interface - one for leaves and one
for nodes.

Here is the leaf implementation:

```java
class CompositeLeaf implements CompositeInterface {
   public void sampleAction(String param) {
      // do sample action things
   }

   public void anotherSampleAction() {
      // do other sample action things
   }
}
```

The node implementation will implement the same interface, but differ in its
implementation in key ways:

- It maintains a list of its nodes and provides methods to add/remove items from
  the list
- For each supported action of the interface, it will defer to each one of its
  child nodes

```java
class CompositeNode implements CompositeInterface {
    private List<CompositeInterface> childNodes = new ArrayList<CompositeInterface>();

    public void sampleAction(String param) {
        childNodes.forEach(child -> child.sampleAction(param));
    }

    public void anotherSampleAction() {
        childNodes.forEach(child -> child.anotherSampleAction());
    }

    public void addChildNode(CompositeInterface childNode) {
        childNodes.add(childNode);
    }

    public void removeChildNode(CompositeInterface childNode) {
        childNodes.remove(childNode);
    }
}
```

Note that we introduce a new way to go through elements of a list here. We will
study lambda expressions in more details in a later section, but here is a quick
overview to understand how they can be used with `forEach()` to apply the same
action to a collection of items:

- A lambda expression has the following form:
  `(parameter-list) -> {function-body}`
- When used in conjunction with `forEach`, the parameter is automatically the
  current item in the list
- The function body can either be a single statement, as shown in
  `anotherSampleAction()`, or a group of statements grouped with curly braces,
  as shown in `sampleAction()`

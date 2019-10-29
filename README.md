# SplayTree

 * An implementation of a dictionary structure backed by a splay tree.  Splay
 * trees, first described by Sleator and Tarjan in their paper "Self-Adjusting
 * Binary Search Trees," are a type of binary search tree with excellent
 * amortized runtime guarantees.  Unlike other balanced search trees, such as
 * red/black trees, AVL trees, or AA trees, the splay tree maintains no
 * explicit balance information or auxiliary data.  Instead, it tries to
 * balance itself whenever an access occurs by moving the most-recently 
 * accessed node up to the root in a process called "splaying."  This splay
 * process works by performing particular tree rotations until the node in
 * question reaches the root.  It is this particular family of rotations, not
 * the fact that the root itself is being rotated, that accounts for the splay
 * tree's amortized O(lg n) runtime for each operation on the tree.  However,
 * this amortized guarantee is not the strongest selling point of the splay
 * tree; this data structure has other strong guarantees as well.  For example,
 * the number of comparisons performed by a splay tree given any sequence of
 * accesses or operations is within a constant factor of the theoretically
 * optimal amount given a fixed-shape binary search tree.
 *
 * The actual rotations involved in a splay tree fall into three categories -
 * "zig," "zig-zig," and "zig-zag."  These cases are described here:
 *
 * 1. "Zig:" If the node's parent is the root, the rotation is
 *
 *                            A        B
 *                           /    -->   \
 *                          B            A
 *
 *    The symmetric case is also considered here.
 *
 * 2. "Zig-Zig:" If the nodes are in the pattern
 *
 *                                 A
 *                                /
 *                               B
 *                              /
 *                             C
 *
 *    They are rotated into
 *
 *                         A         C
 *                        /           \
 *                       B     -->     B
 *                      /               \
 *                     C                 A
 *
 *
 *    This is done by rotating B with A, then C with B.
 *
 * 3. "Zig-Zag:"  If the nodes are in the shape
 *
 *                              A
 *                             /
 *                            B
 *                             \
 *                              C
 *
 *
 *     Then they are rotated to form
 *
 *                     A
 *                    /
 *                   B        ->         C
 *                    \                 / \
 *                     C               B   A
 *
 *     By rotating C with B and then C with A.
 *
 * This splay step is carried out whenever a node is accessed, inserted, or
 * deleted.  In some cases, namely deletion, multiply splays might be required.
 *
 * Another major advantage of the splay operation is that after a tree is
 * splayed at a node, that node ends up at the root.  This allows several
 * complex tree operations, such as joining two trees or splitting a tree in
 * two, to be carried out easily.  The implementation of delete uses this
 * property, for example.
 *
 * This implementation of the splay tree uses it to implement a sorted
 * associative array akin to the STL std::map type.  To support efficient
 * iteration, a the nodes in the splay tree have a doubly-linked list threaded
 * through them in ascending order.  This allows the iterators to scan over the
 * nodes without having to compute successors or resplay the tree.

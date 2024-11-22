# TCC *vs* LCC

Explain under which circumstances *Tight Class Cohesion* (TCC) and *Loose Class Cohesion* (LCC) metrics produce the same value for a given Java class. Build an example of such as class and include the code below or find one example in an open-source project from Github and include the link to the class below. Could LCC be lower than TCC for any given class? Explain.

A refresher on TCC and LCC is available in the [course notes](https://oscarlvp.github.io/vandv-classes/#cohesion-graph).

## Answer

Explanation

Tight Class Cohesion (TCC) and Loose Class Cohesion (LCC) are metrics used to evaluate the cohesion of a class. They differ in the relationships they count:

    TCC: Measures the cohesion of a class by considering directly connected methods. Two methods are directly connected if they share at least one instance variable directly (via access or modification). TCC focuses on direct relationships only.

    LCC: Considers both direct and indirect connections between methods. Indirect connections arise when two methods are not directly connected but are part of the same connection graph (e.g., two methods share variables indirectly through a chain of relationships).

When TCC = LCC

TCC and LCC produce the same value when all methods in a class are directly connected (i.e., the class is fully cohesive). This means that for every pair of methods, there is at least one instance variable they both use, or there exists some transitive relationship between all methods via shared variables.

Example of a Class Where TCC = LCC
```java
public class CohesiveClass {
    private int sharedVar1;
    private int sharedVar2;

    public void method1() {
        sharedVar1 = 10; // Uses sharedVar1
    }

    public void method2() {
        System.out.println(sharedVar1); // Uses sharedVar1
    }

    public void method3() {
        sharedVar1 = sharedVar2 + 5; // Uses both sharedVar1 and sharedVar2
    }
}
```
In this case:

    All methods are connected directly via sharedVar1 or sharedVar2.
    TCC = LCC because all methods are part of the same cohesive graph.

When LCC > TCC

LCC can be greater than TCC when the class contains indirect connections between methods. This happens when not all methods are directly connected, but they form a larger connected graph through shared instance variables.

Example of a Class Where LCC > TCC
```Java
public class LessCohesiveClass {
    private int var1;
    private int var2;

    public void method1() {
        var1 = 10; // Uses var1
    }

    public void method2() {
        System.out.println(var1); // Uses var1
    }

    public void method3() {
        var2 = 20; // Uses var2
    }

    public void method4() {
        System.out.println(var2); // Uses var2
    }
}
```
    TCC: Only methods 1 and 2 are directly connected via var1, and methods 3 and 4 are connected via var2. TCC counts these as two separate cohesive subsets.
    LCC: If you consider indirect connections, methods 1 and 2 form one subset, and methods 3 and 4 form another subset, resulting in a larger connection graph.

Could LCC be Lower than TCC?

No, LCC can never be lower than TCC. By definition:

    TCC only counts direct connections, while LCC includes both direct and indirect connections.
    Hence, LCC will always be equal to or greater than TCC.

Real-World Example

Here’s a real-world class from Apache Commons Lang:

Class: StringUtils

    Methods like isEmpty, isNotEmpty, isBlank, and isNotBlank are tightly coupled because they work on shared logic around string checks.
    Other methods like join, split, and replace are loosely connected since they indirectly rely on shared utility methods.

In such utility classes, TCC and LCC can vary depending on the connections between specific groups of methods.

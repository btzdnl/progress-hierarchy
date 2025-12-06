# ConsoleProgressBar

Provides a .NET console progress bar that supports absolute reporting and a separate library for hierarchical progress reporting.

### Usage

The following example shows a simple usage of the progress bar.

```cs
using (var pb = new ProgressBar())
{
    using (var p1 = pb.HierarchicalProgress.Fork(0.5))
    {
        // do stuff
    }

    using (var p2 = pb.HierarchicalProgress.Fork(0.5))
    {
        // do stuff
    }
}
```

### ProgressHierarchy Usage

The following example shows a more complex usage of the underlying _ProgressHierarch_ NuGet package.

```cs
using (var p = new HierarchicalProgress())
{
    p.ProgressChanged += OnProgressChanged;

    using (var p1 = p.Fork(0.5, "Long-running task A"))
    {
        for (var i = 0; i < 10; i++)
        {
            using (p1.Fork(0.1, $"Item {i}"))
            {
                // do stuff
            }
        }
    }

    using (var p2 = p.Fork(0.5, "Long-running task B"))
    {
        for (var i = 0; i < 10; i++)
        {
            p2.Report(i/10, $"Item {i}");

            // do stuff
        }
    }
}
```

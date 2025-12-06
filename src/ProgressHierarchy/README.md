# ProgressHierarchy

Provides a hierarchical progress reporting system for .NET applications.

The `HierarchicalProgress` class implements `System.IProgress<double>`. It’s thread-safe and lock-free. It is, however, not asynchronous. The `ProgressChanged`
event handlers will be called synchronously by the thread currently reporting its progress. Event handlers must be fast. They must take care of continuing on
the correct thread if required.

The `HierarchicalProgress` class was renamed to not conflict with the `System.Progress<T>` class.

### Usage

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

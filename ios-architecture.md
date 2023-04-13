---
marp: true
theme: uncover
---

<style>
  :root {
    --color-background: #fff;
    /*--color-foreground: #333;
    --color-highlight: #f96;
    --color-dimmed: #888;*/
  }
</style>

# Architektura iOS
![bg right:66% height:500](./title.png)

---

# Historia
![bg right contain](./history.webp)

---

# Cocoa Touch
![bg right height:50% ](./uikit.png)

```swift
import SwiftUI
import UIKit

struct PageViewController<Page: View>
    : UIViewControllerRepresentable {
    var pages: [Page]

    func makeUIViewController(context: Context) 
        -> UIPageViewController {
        let pageViewController = 
            UIPageViewController(
                transitionStyle: .scroll,
                navigationOrientation: .horizontal
                )

        return pageViewController
    }
}
```

---

# Media Layer
![bg right height:40% ](./accelerate.webp)

```swift
let center = CGPoint
    (x: bounds.width / 2, y: bounds.height / 2)
let radius = max(bounds.width, bounds.height)
let startAngle: CGFloat = 3 * .pi / 4
let endAngle: CGFloat = .pi / 4

let path = UIBezierPath(
  arcCenter: center,
  radius: radius/2 - Constants.arcWidth/2,
  startAngle: startAngle,
  endAngle: endAngle,
  clockwise: true)

path.lineWidth = Constants.arcWidth
counterColor.setStroke()
path.stroke()
```

---

# Core Services
![bg right height:50% ](./core-services.png)

```swift
static CFAllocatorRef myAllocator(void) {
    static CFAllocatorRef allocator = NULL;
    if (!allocator) {
        CFAllocatorContext context =
            {0, NULL, NULL, (void *)free, NULL,
             myAlloc, myRealloc, myDealloc, NULL};
        context.info = malloc(sizeof(int));
        allocator = CFAllocatorCreate(NULL, &context);
    }
    return allocator;
}
```

---

# Core OS
![bg right height:60% ](./accelerate.png)

```swift
static var fullyConnectedLayer: 
    BNNS.FullyConnectedLayer = {
    let desc = BNNSNDArrayDescriptor
        (dataType: .float,
        shape: .vector(poolingOutputSize))
    
    guard let fullyConnectedLayer = 
        BNNS.FullyConnectedLayer(
            input: desc,
            output: fullyConnectedOutput,
            weights: fullyConnectedWeights,
            bias: nil,
            activation: .identity,
            filterParameters: filterParameters) else {
        fatalError("Unable to create `fullyConnectedLayer`.")
    }
    
    return fullyConnectedLayer
}()
```

---

# Co dalej?

---

```bash
fork ( ) ...
... fork ( result: 1020 )
sigprocmask ( how: 3, mask: 0x16d252fc8, omask: 0 ) ...
wait4 ( pid: 0xffffffff, status: 0x16d252aac, options: 1 (WNOHANG), rusage: 0 )
... wait4 ( result: 1019, status: 0x16d252aac, rusage: 0 )
wait4 ( pid: 0xffffffff, status: 0x16d252aac, options: 1 (WNOHANG), rusage: 0 ) ...
... wait4 ( result: 0, status: 0x16d252aac, rusage: 0 )
sigreturn ( uctx: 0x16d252bc8 -> { uc_onstack: 0, uc_sigmask: 0, uc_stack: { ss_sp: 0x16d252b60, ss_size: 0, ss_flags: 0 }
... sigprocmask ( result: 0 )
sigprocmask ( how: 1, mask: 0x16d253024, omask: 0x16d253020 ) ...
... sigprocmask ( result: 0 )
sigprocmask ( how: 3, mask: 0x16d253020, omask: 0 ) ...
... sigprocmask ( result: 0 )
sigprocmask ( how: 1, mask: 0x16d25302c, omask: 0x16d253028 ) ...
... sigprocmask ( result: 0 )
sigaction ( signum: 2 (SIGINT), nsa: 0x16d252fa8 -> { __sigaction_u: 0x102be1478, sa_tramp: 0x1dccc8d14, sa_mask: 0, sa_flags: 1024 ...
... sigaction ( result: 0, osa: 0x16d252fd0 -> { __sigaction_u: 0 (SIG_DFL), sa_mask: 0, sa_flags: 0 } )
wait4 ( pid: 0xffffffff, status: 0x16d252f9c, options: 0, rusage: 0 ) ...
... wait4 ( result: 1020, status: 0x16d252f9c, rusage: 0 )
sigaction ( signum: 2 (SIGINT), nsa: 0x16d252fa8 -> { __sigaction_u: 0 (SIG_DFL), sa_tramp: 0x1dccc8d14, sa_mask: 0, sa_flags: 1024 ...
... sigaction ( result: 0, osa: 0x16d252fd0 -> { __sigaction_u: 0x102be1478, sa_mask: 0, sa_flags: 0 } )
ioctl ( fd: 2, com: 0x40087468 (TIOCGWINSZ), data: 0x16d252fd8 ) ...
... ioctl ( result: 0 )
sigprocmask ( how: 3, mask: 0x16d253028, omask: 0 ) ...
... sigprocmask ( result: 0 )
read ( fd: 255, cbuf: 0x1031094a0, nbyte: 85 ) ...
... read ( result: 24, cbuf: 0x1031094a0 -> [s"echo \"something else\"\n\n\n"] )
write_nocancel ( fd: 1, cbuf: 0x104009e00 -> [s"something else\n"], nbyte: 15 ) ...
... write_nocancel ( result: 15 )
read ( fd: 255, cbuf: 0x1031094a0, nbyte: 85 ) ...
... read ( result: 0, cbuf: 0x1031094a0 )
sigprocmask ( how: 1, mask: 0x16d25320c, omask: 0x16d253208 ) ...
... sigprocmask ( result: 0 )
sigprocmask ( how: 3, mask: 0x16d253208, omask: 0 ) ...
... sigprocmask ( result: 0 )
exit ( rval: 0 ) ... @[ 000000018c05
```
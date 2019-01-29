To reproduce https://github.com/Microsoft/TypeScript/issues/29597 run `./node_modules/.bin/tsc -p tsconfig.json --outDir built`

The output is
```
<--- Last few GCs --->

[138959:0x3255ed0]    10777 ms: Mark-sweep 1398.0 (1425.1) -> 1397.6 (1425.6) MB, 284.3 / 0.0 ms  (average mu = 0.119, current mu = 0.005) allocation failure scavenge might not succeed


<--- JS stacktrace --->

==== JS stack trace =========================================

    0: ExitFrame [pc: 0x2e028f6dbe1d]
Security context: 0x1d091b51e6e1 <JSObject>
    1: addPropertyToElementList(aka addPropertyToElementList) [0x3dccc0d68431] [/my/path/to/heap-OOM-repro/node_modules/typescript/lib/tsc.js:~28198] [pc=0x2e028ffcd8bb](this=0x134b2fe826f1 <undefined>,propertySymbol=0x2d9bee637339 <Symbol map = 0x2faf097ba961>,context=0x3f23fb103951 <Object map = 0x2faf097c1291>,typeElements=0x1...

FATAL ERROR: Ineffective mark-compacts near heap limit Allocation failed - JavaScript heap out of memory
 1: 0x8daaa0 node::Abort() [node]
 2: 0x8daaec  [node]
 3: 0xad73ce v8::Utils::ReportOOMFailure(v8::internal::Isolate*, char const*, bool) [node]
 4: 0xad7604 v8::internal::V8::FatalProcessOutOfMemory(v8::internal::Isolate*, char const*, bool) [node]
 5: 0xec4c32  [node]
 6: 0xec4d38 v8::internal::Heap::CheckIneffectiveMarkCompact(unsigned long, double) [node]
 7: 0xed0e12 v8::internal::Heap::PerformGarbageCollection(v8::internal::GarbageCollector, v8::GCCallbackFlags) [node]
 8: 0xed1744 v8::internal::Heap::CollectGarbage(v8::internal::AllocationSpace, v8::internal::GarbageCollectionReason, v8::GCCallbackFlags) [node]
 9: 0xed43b1 v8::internal::Heap::AllocateRawWithRetryOrFail(int, v8::internal::AllocationSpace, v8::internal::AllocationAlignment) [node]
10: 0xe9d834 v8::internal::Factory::NewFillerObject(int, bool, v8::internal::AllocationSpace) [node]
11: 0x113cf9e v8::internal::Runtime_AllocateInNewSpace(int, v8::internal::Object**, v8::internal::Isolate*) [node]
12: 0x2e028f6dbe1d
Aborted

```

The compilation succeeds in TypeScript v3.1.6 and starts to fail for TypeScript v3.2.

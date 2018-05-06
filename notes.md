Trip report : 7 years of C++ in low latency trading

* Me Slide
* `reinterpret_cast` all the things
  * Network buffers
  * IPC buffers
  * `mmap()`ed files

```
struct MessageHeader {
  uint32_t sequence;
  uint16_t nRecords;
} __attribute__((packed));

struct RecordHeader {
  uint16_t size;
  uint8_t type;
} __attribute__((packed));

struct AddOrder {
  uint64_t orderId;
  uint32_t productId;
  uint8_t side;
  uint32_t price;
  uint32_t quantity;
};
struct RemoveOrder {
  uint64_t orderId;
  uint32_t productId;
};

// UGLY stuff...
void handle(const byte *buf, size_t len) {
  auto hdr = reinterpret_cast<MessageHeader *>(buf);
  auto recHdr = reinterpret_cast<const RecordHeader *>(hdr + 1);
  for (auto i = 0u; i < hdr->nRecords; ++i) {
    switch (recHdr->type) {
    }
    msgHdr = reinterpret_cast<const RecordHeader *>(
	reinterpret_cast<...>();
  }
}
```

* Value types ftw
  * Zero cost
  * Type safety
  * byte order wrappers
  * Price/Quantity etc

```
struct MessageHeader {
  nbo<uint32_t> sequence;
  nbo<uint16_t> nRecords;
} __attribute__((packed));
```

* Care about code generation
  * This is why CE was born
  * Some teams use very specific gcc versions
* `template` all the things
  * maybe constexpr all the things later?
  * Lots of generic code specialised for specific types
  * FIX parsers?
* Testing/misc
  * catch/google test
  * Sanitize
  * Test in debug (w/san) AND release
* Conclusion
  * Fun times!
  * Cutting edge stuff

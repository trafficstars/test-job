# test-job

Test job for programmers who want to join our team.

> TrafficStars is a self serve proprietary ad network which was developed to provide technical and marketing solutions to advertisers and publishers worldwide.
> **https://trafficstars.com**

## 1. Implement HashMap data structure

### Demands

  * Creation of new HashMap object like **map = new HashMap(124 /* block size */, [hashFunction] /* optional */)**
  * Key is **string** or **dynamic_type**
  * Value is **dynamic_type**
  * Add test/benchmark for HashMap(Set, Get, Unset) with size of block 16, 64, 128, 1024. Number of memory allocations, time spent for 1000000 operations.
  * Add test/benchmark for Hash function with size of block 16, 64, 128, 1024. Number of memory allocations and collisions.
  * Compare your implementation with native *map* type

### Whats important

  * Memory allocations
  * Performance of operations

### HashMap interface

```go
type Key string|interface{}

type HashMaper interface {
  Set(key Key, value interface{}) error
  Get(key Key) (value interface{}, err error)
  Unset(key Key) error
  Count() int
}

func NewHashMap(blockSize int, fn func(blockSize int, key Key) int) HashMaper { ... }
```

## 2. Bid service

You need to implement HTTP-service, which as a parameter, will accept references to other HTTP-sources, request for their sources and return the response ASAP.

### Draft of protocol

  1. Service accepts request by URI `/winner`
  2. External sources are transferred by param `s`, which could be several
  3. Any source returns data in JSON format `[{"price":1},{"price":2}]`
  4. Results of `/winner` has to contain response as `{"price":123, source:"..."}`

### Application logic

Web application should choose source with the highest bid, but the price result should contain the second highest price of all sources.

### Example

**We have two sources:**

  1. **example.com/fibo** which return `[{"price": 5, "price": 3, "price": 8}]`
  2. **example.com/primes** which return `[{"price": 3, "price": 5, "price": 7}]`

**Request to the service:**

> GET /winner?s=http://example.com/primes&s=http://example.com/fibo

**Response from service:**

```json
{"price": 7, "source":"example.com/fibo"}
```

### Explanation

Ordered set of prices will be `3, 3, 5, 5, 7, 8`,
the highest bid is 8 from source `example.com/fibo`,
and the second highest bid comes from the `example.com/primes` source and equals to 7.

### Application demands

  1. Service must be written only with standard runtime libraries of golang
  2. We have to be able to run the application on the custom TCP port.
  3. You need to confirm the application with tests.
  4. Maximum response time of service must be 100ms, if the external source response time is higher, then that source response could be ignored.

You can find the test source service in current repository **testsources.go**.

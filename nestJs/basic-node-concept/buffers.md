# Buffers:
- In Node.js, a Buffer is a data structure used to handle binary data directly. It is primarily used to manage raw data streams (like file data, network packets, etc.) and is an alternative to working with strings when dealing with binary data.
- Buffers are used to handle raw binary data in Node.js.
- They are fixed in size and globally available.
- Useful for working with **streams**, **files**, or **encoding/decoding** data.
  #### example:
  ```
  Create a Buffer
  const buf = Buffer.from('Hello');
  console.log(buf); // <Buffer 48 65 6c 6c 6f>
  console.log(buf.toString()); // "Hello"


  const buf = Buffer.alloc(10); // 10 bytes
  console.log(buf); // <Buffer 00 00 00 00 00 00 00 00 00 00>

  const buf = Buffer.allocUnsafe(10);
  console.log(buf); // <Buffer ...> (contains random data)

  write buffer

  const buf = Buffer.alloc(5);
  buf.write('Node');
  console.log(buf.toString()); // "Node"

  Read from a buffer
  
  const buf = Buffer.from('Hello');
  console.log(buf[0]); // 72 (ASCII code for 'H')
  console.log(String.fromCharCode(buf[0])); // "H"

  Buffer slicing

  const buf = Buffer.from('Hello, World!');
  const slice = buf.slice(0, 5);
  console.log(slice.toString()); // "Hello"
  
  ```


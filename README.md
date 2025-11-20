# SimpleIO
A small library providing an abstract convenient random access API for streams and a high performant reader with cache buffers and parallel threads. Implementations are available for the following languages/platforms: Java, Android, C#, Python, TypeScript/JavaScript.  
Published under MIT License  

## Features
* Abstract stream API for random access (Read,Write,Seek).
* Easy and consistent API syntax across all languages implemented: Java, C#, Python, and Tavascript.
* Powerful and convenient operations including copying between streams with progress callbacks.
* Stream reader with cache buffering and parallel threads for hign performance native to each language.
* Conversion and encoding utilities for byte arrays are also provided.

## Example
1. Implement your custom stream based on the RandomAccessStream contract API, this is the majority of the work that needs to be done. As an example you can see MemoryStream which is already provided in the library.  
```
class MyStream extends RandomAccessStream {
	public boolean canRead() {...};
	public long getPosition() {...};
	public void setPosition() {...};
	public void read() {...};
	public void write() {...};
	...
}
```

2. You now have a set of more powerful and convenient operations the users of your API can use:  
```
MyStream myStream = new MyStream();
myStream.getLength();                          // get the length of the stream
myStream.canWrite();                           // check if the stream is writable
myStream.getPosition();                        // get the position of the stream
myStream.setPosition(long);                    // set the position on the stream
myStream.copyTo(destStream, progressListener); // copy to another stream with a progress listener
}
```

3. Or for more performance you can wrap the above stream to a native stream reader with cached buffers and parallel threads. The InputStreamWrapper provided in the library is natively implemented for each language. For example for Java it implements an InputStream for better familiarity with Java developement and interoperability with other libraries:  
```
InputStreamWrapper streamWrapper = new InputStreamWrapper(
	myStream,     // your custom stream
	4,            // number of buffers to use, helpful for seeking like media players
	2,	          // parallel threads for reading
	8*1024*1024,  // buffer size in bytes
	256 * 1024    // back offset, helpful for consumers requiring frequent backward reads (ie media players)
);
// InputStream methods
streamWrapper.read();
streamWrapper.write();
streamWrapper.skip();
streamWrapper.reset();
```
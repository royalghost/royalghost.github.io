---
layout: post
title:  "TDD for Finding Image Type in Java"
categories: tdd java
---
In this blog post, I am going to walkthrough you an example of how to do TDD in Java in order to find the image type based on the magic number.

Magic Number in files are constant data that represent certain types of files.[1] For this example, we are only concerned with GIF, JPEG and PNG image types.


### Acceptance Criterias 
The following are the acceptance criterias for this implementation.

* GIF image files have the ASCII code for "GIF89a" (47 49 46 38 39 61) or "GIF87a" (47 49 46 38 37 61)
* If the file type is GIF, it should idetify as ```image/gif```

* PNG image files begin with an 8-byte signature which identifies the file as a PNG file and allows detection of common file transfer problems: \211 P N G \r \n \032 \n (89 50 4E 47 0D 0A 1A 0A). 
* If the file type is PNG, it should idetify as ```image/png```

* JPEG image files begin with FF D8 
* If the file type is JPEG, it should idetify as ```image/jpeg```



## Let's get started

The code is not yet implemented but let's assume the following is the signature of the implementation:

```
public static final String getImageTypeFromByte(InputStream is) throws IOException 
```

### Writing Test First
We will start writing the Test Cases first following the philosophy of Test Driven Development (TDD.)

There are couple of options for writing the test cases but the first approach is just to store the physical file types and use them to call the implementation. However, that's not a good idea so we will create each file type during runtime.

### Base case to test for null

```
@Test
public void testGetContentTypeFromByte_for_NULL() {
	String result = null;
	try {
		result = ImageVerificationMain.getImageTypeFromByte(null);
	} catch (IOException e) {
		System.err.println(e);
	}
	assertEquals(result, null);
}
```

We haven't yet implemented ```ImageVerificationMain``` so it is going to fail. So, we go and implement the basic structure.

```
public class ImageVerificationMain {

	public static final String getImageTypeFromByte(InputStream is) throws IOException {
		String contentType = null;
		if (is == null)
			return contentType;

		return contentType;
	}

}

```

With the above implementation, the first test will pass.

### Test for Text Type

```
@Test
public void testGetContentTypeFromByte_for_Text() {
	String result = null;
	try {
		InputStream imageInputStream = new ByteArrayInputStream("this is a text".getBytes());
		result = ImageVerificationMain.getImageTypeFromByte(imageInputStream);
	} catch (IOException e) {
		System.err.println(e);
	}
	assertEquals(result, null);
}
```
Second test will also pass so no code change is required.

### Test Case for GIF

```
@Test
public void testGetContentTypeFromByte_for_GIF() {
	BufferedImage image = new BufferedImage(100, 100, BufferedImage.TYPE_INT_RGB);
	String result = null;
	try {
		ByteArrayOutputStream os = new ByteArrayOutputStream();
		ImageIO.write(image, "gif", os);
		InputStream imageInputStream = new ByteArrayInputStream(os.toByteArray());
		result = ImageVerificationMain.getImageTypeFromByte(imageInputStream);
	} catch (IOException e) {
		System.err.println(e);
	}
	assertEquals(result, "image/gif");
}
```

Now, refactor the code to add for the image type GIF:
```
public static final String getImageTypeFromByte(InputStream is) throws IOException {
	String contentType = null;
	if (is == null)
		return contentType;

	try {
		int b1 = is.read();
		// Check for empty input stream
		if (b1 == -1)
			return contentType;
		int b2 = is.read();
		int b3 = is.read();
		int b4 = is.read();
		int b5 = is.read();
		int b6 = is.read();
		int b7 = is.read();
		int b8 = is.read();
		/**
		 * GIF image files have the ASCII code for "GIF89a" (47 49 46 38 39 61) or
		 * "GIF87a" (47 49 46 38 37 61)
		 */
		if (b1 == 0x47 && b2 == 0x49 && b3 == 0x46 && b4 == 0x38 && (b5 == 0x39 || b5 == 0x37) && b6 == 0x61) {
			contentType = "image/gif";
		}
	} catch (IOException ex) {
		throw ex;
	}

	return contentType;
}
```

### Test Case for PNG

```
@Test
public void testGetContentTypeFromByte_for_PNG() {
	BufferedImage image = new BufferedImage(100, 100, BufferedImage.TYPE_INT_RGB);
	String result = null;
	try {
		ByteArrayOutputStream os = new ByteArrayOutputStream();
		ImageIO.write(image, "png", os);
		InputStream imageInputStream = new ByteArrayInputStream(os.toByteArray());
		result = ImageVerificationMain.getImageTypeFromByte(imageInputStream);
	} catch (IOException e) {
		System.err.println(e);
	}
	assertEquals(result, "image/png");
}
```

This test is going to fail as we have not implemented the code yet. So, let's refactor the code as following:

```
public static final String getImageTypeFromByte(InputStream is) throws IOException {
	String contentType = null;
	if (is == null)
		return contentType;

	try {
		int b1 = is.read();
		// Check for empty input stream
		if (b1 == -1)
			return contentType;
		int b2 = is.read();
		int b3 = is.read();
		int b4 = is.read();
		int b5 = is.read();
		int b6 = is.read();
		int b7 = is.read();
		int b8 = is.read();
		/**
		 * GIF image files have the ASCII code for "GIF89a" (47 49 46 38 39 61) or
		 * "GIF87a" (47 49 46 38 37 61)
		 */
		if (b1 == 0x47 && b2 == 0x49 && b3 == 0x46 && b4 == 0x38 && (b5 == 0x39 || b5 == 0x37) && b6 == 0x61) {
			contentType = "image/gif";
		/**
		 * PNG image files begin with an 8-byte signature which identifies the file as a
		 * PNG file and allows detection of common file transfer problems: \211 P N G \r
		 * \n \032 \n (89 50 4E 47 0D 0A 1A 0A).
		 */
		} else if (b1 == 0x89 && b2 == 0x50 && b3 == 0x4E && b4 == 0x47 && b5 == 0x0D && b6 == 0x0A && b7 == 0x1A
				&& b8 == 0x0A) {
			contentType = "image/png";
		}
	} catch (IOException ex) {
		throw ex;
	}

	return contentType;
}
```

The test case for GIF should pass.

### Test Case for JPEG
Finally, let's add the Test Case for image type jpeg :

```
@Test
public void testGetContentTypeFromByte_for_JPEG() {
	BufferedImage image = new BufferedImage(100, 100, BufferedImage.TYPE_INT_RGB);
	String result = null;
	try {
		ByteArrayOutputStream os = new ByteArrayOutputStream();
		ImageIO.write(image, "jpeg", os);
		InputStream imageInputStream = new ByteArrayInputStream(os.toByteArray());
		result = ImageVerificationMain.getImageTypeFromByte(imageInputStream);
	} catch (IOException e) {
		System.err.println(e);
	}
	assertEquals(result, "image/jpeg");
}
```

This test case is going to fail, so let's refactor and the final implementation looks like following:

```
import java.io.IOException;
import java.io.InputStream;

/**
 * Check if image type is <code>gif</code>, <code>png</code> or
 * <code>jpeg</code>. For all other types of content return null.
 * 
 * This is based on the magic number. See
 * {@link https://en.wikipedia.org/wiki/Magic_number_(programming)}
 * 
 */
public class ImageVerificationMain {

	public static final String getImageTypeFromByte(InputStream is) throws IOException {
		String contentType = null;
		if (is == null)
			return contentType;

		try {
			int b1 = is.read();
			// Check for empty input stream
			if (b1 == -1)
				return contentType;
			int b2 = is.read();
			int b3 = is.read();
			int b4 = is.read();
			int b5 = is.read();
			int b6 = is.read();
			int b7 = is.read();
			int b8 = is.read();
			/**
			 * GIF image files have the ASCII code for "GIF89a" (47 49 46 38 39 61) or
			 * "GIF87a" (47 49 46 38 37 61)
			 */
			if (b1 == 0x47 && b2 == 0x49 && b3 == 0x46 && b4 == 0x38 && (b5 == 0x39 || b5 == 0x37) && b6 == 0x61) {
				contentType = "image/gif";
			/**
			 * PNG image files begin with an 8-byte signature which identifies the file as a
			 * PNG file and allows detection of common file transfer problems: \211 P N G \r
			 * \n \032 \n (89 50 4E 47 0D 0A 1A 0A).
			 */
			} else if (b1 == 0x89 && b2 == 0x50 && b3 == 0x4E && b4 == 0x47 && b5 == 0x0D && b6 == 0x0A && b7 == 0x1A
					&& b8 == 0x0A) {
				contentType = "image/png";
			/**
			 * JPEG image files begin with FF D8 and end with FF D9. JPEG/JFIF files contain
			 * the ASCII code for "JFIF" (4A 46 49 46) as a null terminated string.
			 * JPEG/Exif files contain the ASCII code for "Exif" (45 78 69 66) also as a
			 * null terminated string, followed by more metadata about the file.
			 */
			} else if (b1 == 0xFF && b2 == 0xD8) {
				contentType = "image/jpeg";
			}
		} catch (IOException ex) {
			throw ex;
		}

		return contentType;
	}

}

```

This is great example of how one can reach a full implementation code by first writing the test and upon failing implement the code.

The final source code is available on Github [2]

[1] https://en.wikipedia.org/wiki/Magic_number_(programming)#Magic_numbers_in_files

[2] https://github.com/royalghost/ImageVerificationApp/tree/master/src
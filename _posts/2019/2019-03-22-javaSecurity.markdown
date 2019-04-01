---
layout : post
title : "자바 세큐리티 클래스"
date : 2019-03-22 10:17
tag : java
comments : true
---

```java
package java.security;

import java.io.ByteArrayOutputStream;
import java.io.PrintStream;
import java.nio.ByteBuffer;
import java.security.DigestException;
import java.security.MessageDigestSpi;
import java.security.NoSuchAlgorithmException;
import java.security.NoSuchProviderException;
import java.security.Provider;
import java.security.Security;
import java.security.MessageDigest.Delegate;
import sun.security.util.Debug;

public abstract class MessageDigest extends MessageDigestSpi {
	private static final Debug pdebug = Debug.getInstance("provider", "Provider");
	private static final boolean skipDebug = Debug.isOn("engine=") && !Debug.isOn("messagedigest");
	private String algorithm;
	private static final int INITIAL = 0;
	private static final int IN_PROGRESS = 1;
	private int state = 0;
	private Provider provider;

	protected MessageDigest(String algorithm) {
		this.algorithm = algorithm;
	}

	public static MessageDigest getInstance(String algorithm) throws NoSuchAlgorithmException {
		try {
			Object[] objs = Security.getImpl(algorithm, "MessageDigest", (String) null);
			Object e;
			if (objs[0] instanceof MessageDigest) {
				e = (MessageDigest) objs[0];
			} else {
				e = new Delegate((MessageDigestSpi) objs[0], algorithm);
			}

			((MessageDigest) e).provider = (Provider) objs[1];
			if (!skipDebug && pdebug != null) {
				pdebug.println(
						"MessageDigest." + algorithm + " algorithm from: " + ((MessageDigest) e).provider.getName());
			}

			return (MessageDigest) e;
		} catch (NoSuchProviderException arg2) {
			throw new NoSuchAlgorithmException(algorithm + " not found");
		}
	}

	public static MessageDigest getInstance(String algorithm, String provider)
			throws NoSuchAlgorithmException, NoSuchProviderException {
		if (provider != null && provider.length() != 0) {
			Object[] objs = Security.getImpl(algorithm, "MessageDigest", provider);
			if (objs[0] instanceof MessageDigest) {
				MessageDigest delegate1 = (MessageDigest) objs[0];
				delegate1.provider = (Provider) objs[1];
				return delegate1;
			} else {
				Delegate delegate = new Delegate((MessageDigestSpi) objs[0], algorithm);
				delegate.provider = (Provider) objs[1];
				return delegate;
			}
		} else {
			throw new IllegalArgumentException("missing provider");
		}
	}

	public static MessageDigest getInstance(String algorithm, Provider provider) throws NoSuchAlgorithmException {
		if (provider == null) {
			throw new IllegalArgumentException("missing provider");
		} else {
			Object[] objs = Security.getImpl(algorithm, "MessageDigest", provider);
			if (objs[0] instanceof MessageDigest) {
				MessageDigest delegate1 = (MessageDigest) objs[0];
				delegate1.provider = (Provider) objs[1];
				return delegate1;
			} else {
				Delegate delegate = new Delegate((MessageDigestSpi) objs[0], algorithm);
				delegate.provider = (Provider) objs[1];
				return delegate;
			}
		}
	}

	public final Provider getProvider() {
		return this.provider;
	}

	public void update(byte input) {
		this.engineUpdate(input);
		this.state = 1;
	}

	public void update(byte[] input, int offset, int len) {
		if (input == null) {
			throw new IllegalArgumentException("No input buffer given");
		} else if (input.length - offset < len) {
			throw new IllegalArgumentException("Input buffer too short");
		} else {
			this.engineUpdate(input, offset, len);
			this.state = 1;
		}
	}

	public void update(byte[] input) {
		this.engineUpdate(input, 0, input.length);
		this.state = 1;
	}

	public final void update(ByteBuffer input) {
		if (input == null) {
			throw new NullPointerException();
		} else {
			this.engineUpdate(input);
			this.state = 1;
		}
	}

	public byte[] digest() {
		byte[] result = this.engineDigest();
		this.state = 0;
		return result;
	}

	public int digest(byte[] buf, int offset, int len) throws DigestException {
		if (buf == null) {
			throw new IllegalArgumentException("No output buffer given");
		} else if (buf.length - offset < len) {
			throw new IllegalArgumentException("Output buffer too small for specified offset and length");
		} else {
			int numBytes = this.engineDigest(buf, offset, len);
			this.state = 0;
			return numBytes;
		}
	}

	public byte[] digest(byte[] input) {
		this.update(input);
		return this.digest();
	}

	public String toString() {
		ByteArrayOutputStream baos = new ByteArrayOutputStream();
		PrintStream p = new PrintStream(baos);
		p.print(this.algorithm + " Message Digest from " + this.provider.getName() + ", ");
		switch (this.state) {
			case 0 :
				p.print("<initialized>");
				break;
			case 1 :
				p.print("<in progress>");
		}

		p.println();
		return baos.toString();
	}

	public static boolean isEqual(byte[] digesta, byte[] digestb) {
		if (digesta == digestb) {
			return true;
		} else if (digesta != null && digestb != null) {
			if (digesta.length != digestb.length) {
				return false;
			} else {
				int result = 0;

				for (int i = 0; i < digesta.length; ++i) {
					result |= digesta[i] ^ digestb[i];
				}

				return result == 0;
			}
		} else {
			return false;
		}
	}

	public void reset() {
		this.engineReset();
		this.state = 0;
	}

	public final String getAlgorithm() {
		return this.algorithm;
	}

	public final int getDigestLength() {
		int digestLen = this.engineGetDigestLength();
		if (digestLen == 0) {
			try {
				MessageDigest e = (MessageDigest) this.clone();
				byte[] digest = e.digest();
				return digest.length;
			} catch (CloneNotSupportedException arg3) {
				return digestLen;
			}
		} else {
			return digestLen;
		}
	}

	public Object clone() throws CloneNotSupportedException {
		if (this instanceof Cloneable) {
			return super.clone();
		} else {
			throw new CloneNotSupportedException();
		}
	}
}

```

package com.ice.util;

import java.io.IOException;

/**
 * Key Util: 1> according file name|size ..., generate a key; 2> the key should
 * be unique.
 */
public class TokenUtil {

	/**
	 * 生成Token， A(hashcode>0)|B + |name的Hash值| +_+size的值
	 * 
	 * @param name
	 * @param size
	 * @return
	 * @throws Exception
	 */

	// 生成hashcode码
	public static String generateToken(String name, String size) throws IOException {
		if (name == null || size == null)
			return "";
		int code = name.hashCode();
		try {
			String token = (code > 0 ? "A" : "B") + Math.abs(code) + "_" + size.trim();
			IoUtil.storeToken(token);

			return token;
		} catch (Exception e) {
			throw new IOException(e);
		}
	}
}

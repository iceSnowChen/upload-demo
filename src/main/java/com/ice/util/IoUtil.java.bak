package com.ice.util;

import java.io.Closeable;
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.Calendar;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
import javax.servlet.http.HttpServletRequest;

import com.ice.entity.ConstantByProperties;

/**
 * IO--closing, getting file name ... main function method
 */
public class IoUtil {
	static final Pattern RANGE_PATTERN = Pattern.compile("bytes \\d+-\\d+/\\d+");

	public static File getFile(String filename) throws IOException {
		if (filename == null || filename.isEmpty())
			return null;
		String name = filename.replaceAll("/", Matcher.quoteReplacement(File.separator));
		File f = new File(ConstantByProperties.basePath + name);
		if (!f.getParentFile().exists())
			f.getParentFile().mkdirs();
		if (!f.exists())
			f.createNewFile();
		return f;
	}

	public static File getTokenedFile(String key) throws IOException {
		if (key == null || key.isEmpty())
			return null;

		File f = new File(ConstantByProperties.basePath + key);
		if (!f.getParentFile().exists())
			f.getParentFile().mkdirs();
		if (!f.exists())
			f.createNewFile();

		return f;
	}

	public static void storeToken(String key) throws IOException {
		if (key == null || key.isEmpty()) {
			return;
		}
		File f = new File(ConstantByProperties.basePath + key);
		if (!f.getParentFile().exists())
			f.getParentFile().mkdirs();
		if (!f.exists())
			f.createNewFile();
	}

	/**
	 * close the IO stream.
	 * 
	 * @param stream
	 */
	public static void close(Closeable stream) {
		try {
			if (stream != null)
				stream.close();
		} catch (IOException e) {
		}
	}

	/**
	 * 获取Range参数
	 * 
	 * @param req
	 * @return
	 * @throws IOException
	 */
	public static Range parseRange(HttpServletRequest req) throws IOException {
		String range = req.getHeader("content-range");
		Matcher m = RANGE_PATTERN.matcher(range);
		if (m.find()) {
			range = m.group().replace("bytes ", "");
			String[] rangeSize = range.split("/");
			String[] fromTo = rangeSize[0].split("-");

			long from = Long.parseLong(fromTo[0]);
			long to = Long.parseLong(fromTo[1]);
			long size = Long.parseLong(rangeSize[1]);

			return new Range(from, to, size);
		}
		throw new IOException("Illegal Access!");
	}

	public static long streaming(InputStream in, String key, String fileName, String url) throws IOException {
		OutputStream out = null;
		File f = getTokenedFile(key);
		try {
			out = new FileOutputStream(f);

			int read = 0;
			final byte[] bytes = new byte[1024];
			while ((read = in.read(bytes)) != -1) {
				out.write(bytes, 0, read);
				try {
					Thread.sleep(ConstantByProperties.uploadSpeed);
				} catch (InterruptedException e) {
				}
			}
		} finally {
			out.flush();
			close(out);
		}

		Path pathtrue = f.toPath().resolveSibling(url);
		File parentFile = pathtrue.toFile().getParentFile();
		if (!parentFile.exists()) {
			parentFile.mkdirs();
		}

		Files.move(f.toPath(), pathtrue);

		long length = getFile(fileName).length();
		/** if `STREAM_DELETE_FINISH`, then delete it. */

		return length;
	}

	public static String getYMDpath() {
		Calendar cal = Calendar.getInstance();
		// int day = cal.get(Calendar.DATE);
		int month = cal.get(Calendar.MONTH) + 1;
		int year = cal.get(Calendar.YEAR);
		return (year + "/" + month);
	}
}

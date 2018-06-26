package com.ice.controller.upload;

import java.io.IOException;
import java.io.PrintWriter;
import java.io.UnsupportedEncodingException;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.json.JSONException;
import org.json.JSONObject;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import com.ice.util.TokenUtil;

@Controller
public class TokenController {

	public static final String FILE_NAME_FIELD = "name";
	public static final String FILE_SIZE_FIELD = "size";
	public static final String FILE_TYPE = "fileType";
	public static final String TOKEN_FIELD = "token";
	public static final String SERVER_FIELD = "server";
	public static final String SUCCESS = "success";
	public static final String MESSAGE = "message";

	@RequestMapping(value = "/queryToken")
	public void firstdoget(HttpServletRequest req, HttpServletResponse resp) throws IOException {
		doOptions(req, resp);
		String name = req.getParameter(FILE_NAME_FIELD); // 文件名
		String size = req.getParameter(FILE_SIZE_FIELD); // 文件大小
		String token = TokenUtil.generateToken(name, size); // 利用文件名和文件大小重新生成编码作为临时文件的名字

		PrintWriter writer = resp.getWriter();
		JSONObject json = new JSONObject();
		try {
			json.put(TOKEN_FIELD, token);
			json.put(SUCCESS, true);
			json.put(MESSAGE, "");
		} catch (JSONException e) {
		}
		writer.write(json.toString());
	}

	protected void doOptions(HttpServletRequest req, HttpServletResponse resp) throws UnsupportedEncodingException {
		req.setCharacterEncoding("UTF-8");
		resp.setCharacterEncoding("UTF-8");
		resp.setContentType("application/json;charset=utf-8");
		resp.setHeader("Access-Control-Allow-Origin", "*");
		resp.setHeader("Access-Control-Allow-Headers", "Content-Range,Content-Type");
		resp.setHeader("Access-Control-Allow-Origin", "*");
		resp.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS");
	}
}

package com.sevenpeaks.api;

import java.util.ArrayList;
import java.util.List;

import org.json.JSONArray;
import org.json.JSONObject;
import org.junit.After;
import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;

import io.restassured.RestAssured;
import io.restassured.path.json.JsonPath;
import io.restassured.response.Response;
import io.restassured.specification.RequestSpecification;

/**
 * 
 * Test Case 
 * 
 * @author ATISH MOHATA (coolatish8148@gmail.com)
 *
 */
public class TestAPI {

	private final static String SEVEN_PEAKS_API_URL = "https://reqres.in/api/users?page=2";
	private static final String EMAIL_TO_VALIDATE = "byron.fields@reqres.in";
	private static final int HTTP_STATUS_CODE_OK = 200;

	private static final String TOTAL_PAGES_JSON_KEY = "total_pages";
	private static final String DATA_JSON_KEY = "data";
	private static final String EMAIL_JSON_KEY = "email";

	private RequestSpecification httpRequest;
	private Response response = null;

	/**
	 * Preparation before test
	 */
	@Before
	public void setUp() {
		RestAssured.baseURI = SEVEN_PEAKS_API_URL;
		httpRequest = RestAssured.given();
		response = httpRequest.get(SEVEN_PEAKS_API_URL);
	}

	/**
	 * Test case to validate the HHP Status Code of API
	 */
	@Test
	public void testAPIStatusOk() {
		Assert.assertEquals(HTTP_STATUS_CODE_OK, response.getStatusCode());
	}

	/**
	 * Test case to validate the presence of total_pages in JSON extracted from API
	 */
	@Test
	public void testTotalPagesPresenceInJSON() {
		String httpResponseAsString = response.getBody().asString();
		Assert.assertEquals(httpResponseAsString.contains(TOTAL_PAGES_JSON_KEY), true);
	}

	/**
	 * Test case to validate the value of total_pages in JSON extracted from API
	 */
	@Test
	public void testTotalPagesEqualsTwo() {
		JsonPath jsonPathEvaluator = response.jsonPath();
		String totalPages = jsonPathEvaluator.getString(TOTAL_PAGES_JSON_KEY);
		Assert.assertEquals(totalPages, "2");
	}

	/**
	 * Test case to validate given email address in JSON extracted from API
	 */
	@Test
	public void testEmailValidation() {
		List<String> emailList = extractEmailValuesFromJson();
		Assert.assertEquals(emailList.contains(EMAIL_TO_VALIDATE), true);
	}

	/**
	 * Releasing resources used in the test
	 */
	@After
	public void tearDown() {
		RestAssured.baseURI = null;
		httpRequest = null;
		response = null;
	}

	/**
	 * Helper method to extract the list of emails from JSON received from API
	 * 
	 * @return
	 */
	private List<String> extractEmailValuesFromJson() {
		String outputJson = response.getBody().asPrettyString();
		JSONObject jsonObject = new JSONObject(outputJson);
		JSONArray jsonArray = jsonObject.getJSONArray(DATA_JSON_KEY);
		List<String> emailList = new ArrayList<String>();
		for (int i = 0; i < jsonArray.length(); i++) {
			String email = jsonArray.getJSONObject(i).getString(EMAIL_JSON_KEY);
			emailList.add(email);
		}
		return emailList;
	}

}

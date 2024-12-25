## Info
List of quick solutions that I learned from exprience. I hope it replaces ChatGPT or Googling for things I know.

Contributions are all welcome :)

# Testings

## ExpressJs
- If you are testing a route with only request body, use [supertest](https://www.npmjs.com/package/supertest)
  ```javascript
    it("it should return list of users", async () => {
      const res = await request(app)
        .post("/api/auth/refresh-token")
        .send();
  
      expect(res.status).toBe(200);
      expect(res.body.status).toBe("success");
    });
  ```
- When you are dealing with middelwares or advanced requests use [node-mocks-http](https://www.npmjs.com/package/node-mocks-http)
  ```javascript
  it("it should return list of users", async () => {
    const request = httpMocks.createRequest({
        method: 'GET',
        url: '/user/42',
        params: {
            id: 42
        }
    });

    const response = httpMocks.createResponse();

    routeHandler(request, response);

    const data = response._getJSONData(); // short-hand for JSON.parse( response._getData() );
    test.equal('Bob Dog', data.name);
    test.equal(42, data.age);
    test.equal('bob@dog.com', data.email);

    test.equal(200, response.statusCode);
    test.ok(response._isEndCalled());
    test.ok(response._isJSON());
    test.ok(response._isUTF8());

    test.done();
  });
  ```
  Custom shorter version of it
  ```javascript
  const httpMocks = require("node-mocks-http");

  const middlewareMock = (reqOptions, resOptions) => {
    const req = httpMocks.createRequest(reqOptions);
    const res = httpMocks.createResponse(resOptions);
    const next = jest.fn();

    return { req, res, next };
  };

  module.exports = middlewareMock;
  ```

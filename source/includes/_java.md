# Sample code - Java

## Sample GET request

> Sample GET request

```java
CloseableHttpClient client; 

String url = "https://whats.todaysplan.com.au/rest/auth/whoami";

HttpGet get = new HttpGet(url);
CloseableHttpResponse rsp = null;
try {

  if (token != null)
    get.addHeader("Authorization", "Bearer " + token);

  rsp = client.execute(get);

  String js = IOUtils.toString(rsp.getEntity().getContent());

  ...

} finally {
  if (rsp != null)
    rsp.close();
      
  get.releaseConnection();
}

```

## Sample POST request

> Sample POST request

```java
CloseableHttpClient client; 

String url = "https://whats.todaysplan.com.au/rest/auth/login";
Map<String,String> input = new HashMap<String,String();
input.put("username", "andrew@todaysplan.com.au");
input.put("password", "secretstuff");

HttpPost post = new HttpPost(url);
CloseableHttpResponse rsp = null;
try {

  StringEntity js = new StringEntity(Json.toJson(input));
  post.addHeader("content-type", "application/json");
  post.setEntity(js);

  if (token != null)
    post.addHeader("Authorization", "Bearer " + token);

  rsp = client.execute(post);

  String js = IOUtils.toString(rsp.getEntity().getContent());

  ...

} finally {
  if (rsp != null)
    rsp.close();
      
  post.releaseConnection();
}

```

## Sample file upload

> Sample file upload

```java
CloseableHttpClient client; 
File file;
Map<String,String> entity;

String url = "https://whats.todaysplan.com.au/rest/files/upload";

HttpPost post = new HttpPost(url);
CloseableHttpResponse rsp = null;
try {

  MultipartEntityBuilder builder = MultipartEntityBuilder.create();
  builder.setMode(HttpMultipartMode.BROWSER_COMPATIBLE);

  FileBody fileBody = new FileBody(file);
  builder.addPart("attachment", fileBody);
  builder.addTextBody("filename", file.getName());
      
  if (entity != null)
    builder.addTextBody("json", Json.toJson(entity));
      
  post.setEntity(builder.build());

  if (token != null)
    post.addHeader("Authorization", "Bearer " + token);
      
  rsp = client.execute(post);

  String js = IOUtils.toString(rsp.getEntity().getContent());

  ...

} finally {
  if (rsp != null)
    rsp.close();
      
  post.releaseConnection();
}

```


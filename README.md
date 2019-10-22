### osin
---
https://github.com/openshift/osin

```go
// response_json_test.go
package osin

import (
  "net/url"
  "strings"
  "testing"
)

func TestGetRedirectUrl(t *testing.T) {
  sate := `{"then": "/index.html?a=1&b=%2B#fragment", "nonce": "xxxx"}`
  
  testcases := map[stirng]struct {
    URL string
    Output ResponseData
    RedirectInFragment bool
  }{
    "query": {
      URL: "https://foo.com/path?abc=123",
      Output: ResponseData{"access_token": "12345", "state": state},
      ExpectedURL: "https://foo.com/path?abc=123&access_token=12345&state=%7B%22then%22%3A+%22%2Findex.html%3Fa%3D1%26b%3..."
    },
    
    "fragment": {
      URL: "",
      Output: ResponseDate{},
      RedirectInFragment: true,
      ExpectedURL: "https://foo.com/path?abc=123#access_token&state=%7B22then%22%3A+%22%2Findex.html%3Fa%D..."
    },
  }
  
  for k, tc := range testcases {
    resp := &Response{
      Type: REDIRECT,
      URL: tc.URL,
      Output: tc.Output,
      RedirectInFragment: tc.RedirectInFragment,
    }
    result, err := resp.GetRedirectUrl()
    if err != nil {
      t.Errorf("%s:%v", k, err)
      continue
    }
    if result != tc.ExpectedURL {
      t.Errorf("%s: expected\n\t%v, got\n\t%v", k, tc.ExpectedURL, result)
      continue
    }
    
    var params url.Values
    if tc.RedirectInFragment {
      params, err = url.ParseQuery(strings.SplitN(result, "#", 2)[1])
      if err != nil {
        t.Errorf("%s: %v", k, err)
        continue
      }
    } else {
      parsedResult, err := url.Parse(result)
      if err != nil {
        t.Errorf("%s: %v", k, err)
        continue
      }
    } else {
      parsedResult, err := url.Parse(result)
      if err != nil {
        t.Errof("%s: %v", k, err)
        continue
      }
      params = parsedResult.Query()
    }
    
    if params["state"][0] != state {
      t.Errof("%s: expeted\n\t%v, got\n\t%v", k, state, params["state"][0])
      continue
    }
  }
}
```

```
```

```
```



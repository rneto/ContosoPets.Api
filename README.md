# ContosoPets.Api v1

Use case of ASP.NET Core API v3.1. and Entity Framework Core with in-memory database.

**_Added value:_** Use [curl](https://curl.haxx.se/docs/manpage.html) to test the web API.


## Basic use

1. **IMPORTANT:** I'm using _Git Bash_ as the default shell in vscode:
    - Install Git form https://git-scm.com/download/win
    - Open command pallete using _Ctrl+Shift+P_.
    - Type _Select Default Shell_ and press _Enter_.
    - Select _Git Bash_ from the options list.
    - Click the _Plus_ (+) icon in the terminal window.
    - The new terminal will be a _Git Bash_ terminal.

1. Build the app:

    ```dotnet build```

1. Run the following commands:

    ```dotnet ./bin/Debug/netcoreapp3.1/ContosoPets.Api.dll > ContosoPets.Api.log &```

     - Hosts the web API with ASP.NET Core's Kestrel web server.
     - Emits logging to _ContosoPets.Api.log_ file.
     - The API is hosted at both The web API is hosted at both _http://localhost:5000_ and _https://localhost:5001_.
     - The final _&_ character runs the app as a background task to unblock command shell input.


1. Use _curl_ to send an HTTP GET request to the web API:

    ```curl -k -s https://localhost:5001/weatherforecast | jq```

    - HTTPS to send a GET request action method to _WeatherForecastController_ from the running web API on port 5001 of localhost.
    - The _-k_ option indicate that curl should allow insecure server connections when using HTTPS. The .NET Core SDK includes an HTTPS development certificate for testing. By default, curl rejects secure connections using this certificate.
    - The _-s_ option suppress all output except the JSON payload. The JSON is sent to the _jq_ command-line JSON processor for improved display.

    Example of the JSON returned:

    ```json
    [
      {
        "date": "2020-10-15T06:36:32.9693753+02:00",
        "temperatureC": 18,
        "temperatureF": 64,
        "summary": "Sweltering"
      },
      {
        "date": "2020-10-16T06:36:32.9696896+02:00",
        "temperatureC": -13,
        "temperatureF": 9,
        "summary": "Mild"
      },
      {
        "date": "2020-10-17T06:36:32.9696974+02:00",
        "temperatureC": 43,
        "temperatureF": 109,
        "summary": "Bracing"
      },
      {
        "date": "2020-10-18T06:36:32.9696978+02:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Cool"
      },
      {
        "date": "2020-10-19T06:36:32.9696983+02:00",
        "temperatureC": 21,
        "temperatureF": 69,
        "summary": "Cool"
      }
    ]
    ```

1. If you make code changes, run ```kill $(pidof dotnet)``` to stop all .NET Core apps before attempting to run again.

## Testing the Contoso Pets Products web API

### Invalid HTTP POST

```
curl -i -k \
    -H "Content-Type: application/json" \
    -d "{\"name\":\"Plush Squirrel\",\"price\":0.00}" \
    https://localhost:5001/products
```

### Valid HTTP POST

```
curl -i -k \
    -H "Content-Type: application/json" \
    -d "{\"name\":\"Plush Squirrel\",\"price\":12.99}" \
    https://localhost:5001/products
```

### HTTP GET

```
curl -k -s https://localhost:5001/products/3 | jq
```

### HTTP PUT

```
curl -i -k \
    -X PUT \
    -H "Content-Type: application/json" \
    -d "{\"id\":2,\"name\":\"Knotted Rope\",\"price\":14.99}" \
    https://localhost:5001/products/2
```

### HTTP DELETE

```
curl -i -k -X DELETE https://localhost:5001/products/1
```

### HTTP GET all products

```
curl -k -s https://localhost:5001/products | jq
```
